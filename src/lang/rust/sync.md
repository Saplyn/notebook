# Sync & Locks

## Threading

New threads are spawned using `std::thread::spawn()` function, it takes the callable
it would execute as an argument, and stops once the it returns.

```rust,noplayground
fn main() {
    let remember = std::thread::spawn(f);
    let forget = std::thread::spawn(f);

    println!("Hello from the main thread.");

    remember.join().unwrap(); // Wait for thread `remember` to finish
    // `forget` would be terminated immediately after this line, regardless of
    // it finished or not.
}

fn f() {
    println!("Hello from another thread!");

    let id = std::thread::current().id();
    println!("This is my thread id: {:?}", id);
}
```

Scoped threads are threads that are spawned within a scope, It allows us to spawn
threads that cannot outlive the scope of the closure we pass to that function, making
it possible to safely borrow local variables.

```rust,noplayground
let numbers = vec![1, 2, 3];

thread::scope(|s /* the scope */| {
    s.spawn(|| {
        println!("length: {}", numbers.len());
    });
    s.spawn(|| {
        for n in &numbers {
            println!("{n}");
        }
    });
});
```

## Sharing State

There are several ways to share state that can be accessed by multiple threads:

- **Statics**: Lives for the entire duration of the program.

```rust,noplayground
static X: [i32; 3] = [1, 2, 3];

std::thread::spawn(|| dbg!(&X));
std::thread::spawn(|| dbg!(&X));
```

- **Leaks**: Leaking a value, promising to never drop it.

```rust,noplayground
let x: &'static [i32; 3] = Box::leak(Box::new([1, 2, 3]));

std::thread::spawn(move || dbg!(x));
std::thread::spawn(move || dbg!(x));
```

- **Reference Counting**: Keep track of the number of owners, effectively sharing
  ownership of the value. The value is dropped when the last owner is dropped.

```rust,noplayground
let a = std::rc::Rc::new([1, 2, 3]); // reference counted (non-thread-safe)
let b = a.clone();
assert_eq!(a.as_ptr(), b.as_ptr()); // Same allocation!

let c = std::sync::Arc::new([1, 2, 3]); // atomic rc (thread-safe, immutable)
let d = c.clone();
thread::spawn(move || dbg!(c));
thread::spawn(move || dbg!(d));
```

## Interior Mutability

- **`Cell<T>`**: A wrapper which allows replacement (mutation) through a shared
  reference for types that implement `Copy`.

```rust,noplayground
use std::cell::Cell;

fn f(a: &Cell<i32>, b: &Cell<i32>) {
    let before = a.get();
    b.set(b.get() + 1);
    let after = a.get();
    if before != after {
        x(); // might happen
    }
}
```

- **`RefCell<T>`**: A wrapper which allows borrowing (mutable or immutable) through
  a shared reference, enforcing the borrowing rules at runtime. It keeps track of
  any outstanding borrows and **panics** if the rules are violated.

```rust,noplayground
use std::cell::RefCell;

fn f(v: &RefCell<Vec<i32>>) {
    v.borrow_mut().push(1); // We can modify the `Vec` directly.
}
```

- **`Mutex<T>` & `RwLock<T>`**: Exclusive lock and read-write lock. It blocks the
  current threads on conflicting borrows, waiting for the lock to be released.

- **Atomics**: Types that can be modified atomically, without locks. They require
  support from the processor to avoid data races.

## `Send` & `Sync`

- **`Send`**: A type is `Send` if it can be safely sent to another thread. In other
  words, if ownership of a value of that type can be transferred to another thread.
- **`Sync`**: A type is `Sync` if it can be safely shared with another thread. In
  other words, a type `T` is `Sync` if and only if a shared reference to that type,
  `&T`, is Send.

All primitive types and most standard library types are `Send` and `Sync`, as
they are auto traits that are implemented automatically by the compiler. One way
to opt out of them is to use `std::marker::PhantomData<T>`, which is a zero-sized
marker type.

```rust,noplayground
use std::marker::PhantomData;

struct X {
    handle: i32,
    _not_sync: PhantomData<Cell<()>>,
}
```

## Locking

- **Poisoning**: If a thread panics while holding a lock, the lock is poisoned, and
  any subsequent attempts to acquire the lock will fail with an error. The error
  contains a guard that can be used to correct the inconsistency.
- **Guard**: When acquiring a lock, a guard is returned, which is a smart pointer
  that automatically releases the lock when it goes out of scope. So it is
  recommended that the user keeps the guard in a variable, instead of using it
  in place as a temporary, to avoid releasing the lock unexpectedly.

```rust,noplayground
let mut num_guard = num.lock().unwrap();
for _ in 0..100 {
    *num_guard += 1;
}
drop(num_guard); // Manually drop the guard, releasing the lock
```

## Waiting

- **Thread parking**: A thread can park itself, which puts it to sleep, stopping
  it from consuming any CPU cycles. Another thread can then wake it up by unparking.

```rust
use std::collections::VecDeque;

fn main() {
    let queue = Mutex::new(VecDeque::new());

    thread::scope(|s| {
        // Consuming thread
        let t = s.spawn(|| loop {
            let item = queue.lock().unwrap().pop_front();
            if let Some(item) = item {
                dbg!(item);
            } else {
                thread::park(); // Nothing to consume, park the thread
            }
        });

        // Producing thread
        for i in 0.. {
            queue.lock().unwrap().push_back(i);
            t.thread().unpark(); // New item, unpark, wake up the consuming thread
            thread::sleep(Duration::from_secs(1));
        }
    });
}
```

- **Condition variables**: A condition variable is a synchronization primitive
  that has two main operations: `wait()` and `notify()`. One for waiting for a
  condition to be met, and the other for notifying one or more threads that are
  waiting for that condition. It is often used in conjunction with a `Mutex<T>`.

```rust,noplayground
use std::sync::Condvar;

let queue = Mutex::new(VecDeque::new());
let not_empty = Condvar::new();

thread::scope(|s| {
    s.spawn(|| {
        loop {
            let mut q = queue.lock().unwrap();
            let item = loop {
                if let Some(item) = q.pop_front() {
                    break item;
                } else {
                    q = not_empty.wait(q).unwrap();
                }
            };
            drop(q);
            dbg!(item);
        }
    });

    for i in 0.. {
        queue.lock().unwrap().push_back(i);
        not_empty.notify_one();
        thread::sleep(Duration::from_secs(1));
    }
});
```

- **Barrier**: A barrier is a synchronization primitive that allows multiple threads
  to wait for each other at a certain point in their execution. When all threads
  reach the barrier, they are all released to continue execution.

```rust,noplayground
use std::sync::Barrier;
use std::thread;

let n = 10;
let barrier = Barrier::new(n);
thread::scope(|s| {
    for _ in 0..n {
        // The same messages will be printed together.
        // You will NOT see any interleaving.
        s.spawn(|| {
            println!("before wait");
            barrier.wait();
            println!("after wait");
        });
    }
});
```
