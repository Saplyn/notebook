# Function & Closure

## Function Item

Function items are zero-sized. Every none generic function has a unique,
unnameable function item type.

```rust,noplayground
fn callable() {}
let fn_item = callable; // function item: fn() {callable}

fn generic<T>() {}
let mut fn_i32 = generic::<i32>; // fn() {generic::<i32>}
// fn_i32 = generic::<u32>; // ERROR: mismatched types, fn() {generic::<u32>}

// zero-sized
println!("{}", std::mem::size_of(fn_item))
println!("{}", std::mem::size_of(fn_i32))
```

## Function Pointer: `fn`

Function pointer are pointers that points to a function, referring to a
function whose identity is not known at compile time.

```rust,noplayground
// a function that takes two `i32` and returns one `i32`
type BinOp = fn(i32, i32) -> i32;

fn add(a: i32, b: i32) -> i32 { a + b }
fn minus(a: i32, b: i32) -> i32 { a - b }

// coerce: from "fn item" to "fn pointer"
let mut bo: BinOp = add;
bo = minus;
```

## Closure

Closure is a function that is able to captures its environment.

```rust,noplayground
// none-capturing closure
let add = |a: i32, b: i32| a + b;

// capturing closure (immutable)
let x = 10;
let read_x = |a: i32| a + x;

// capturing closure (mutable)
let mut x = 10;
let mut_x = |a: i32| { x += a; };

// capturing closure (move)
let x = 10;
let drop_x = || { drop (x); };
```

At compiler implementation level, closures are divided into two types:

- None-capturing: They are like unnamed function, aka. instructions written in
  the code.
- Capturing: They are like a struct that contains the captured variables, which
  also implements the corresponding function traits.

```rust,noplayground
// consider a capturing closure like this
let x = 10;
let read_x = |a: i32| a + x;
read_x(5);

// it would be interpreted as something like this
let x = 10;
struct ReadX {
    x: i32,
}
impl Fn(i32) -> i32 for ReadX {
    fn call(&self, a: i32) -> i32 {
        a + self.x
    }
}
impl FnMut(i32) -> i32 for ReadX { /* ... */ }
impl FnOnce(i32) -> i32 for ReadX { /* ... */ }
let read_x = ReadX { x };
read_x.call(5);
```

## Function Traits: `Fn`, `FnMut` & `FnOnce`

`Fn`, `FnMut` and `FnOnce` are traits that are implemented on function types.

- `Fn`: The function can be called immutably, modifying nothing.
- `FnMut`: The function can be called mutably, modifying captured data.
- `FnOnce`: The function can be called only once, moving the captured value.

```rust,noplayground
fn add(a: i32, b: i32) -> i32 { a + b }
fn call_it<F: Fn(i32, i32) -> i32>(f: F, a: i32, b: i32) -> i32 {
    f(a, b)
}

call_it(add, 1, 2);
```

Note that `Fn` is a super trait of `FnMut`, and `FnMut` is a super trait of
`FnOnce`, because every "shared" function can be called "exclusively", and
every "exclusive" function can be called "at least once".

```rust,noplayground
fn add(a: i32, b: i32) -> i32 { a + b }
fn call_once<F: FnOnce(i32, i32) -> i32>(f: F, a: i32, b: i32) -> i32 {
    f(a, b)
}
fn call_mut<F: FnMut(i32, i32) -> i32>(f: F, a: i32, b: i32) -> i32 {
    f(a, b)
}

call_once(add, 1, 2);
call_mut(add, 1, 2);
```
