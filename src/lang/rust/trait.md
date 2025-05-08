# Trait & Generic

## Generic

Generic in Rust is implemented as type parameters. It allows us to define
functions, structs, enums, and methods that work with arbitrary types.
Generic happens at compile time.

```rust,noplayground
// generic struct
struct Point<T> {
    x: T,
    y: T,
}

// generic method
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}
// generic enum
enum Either<L, R> {
    Left(L),
    Right(R),
}

// generic function
fn swap<T>(pair: (T, T)) -> (T, T) {
    (pair.1, pair.0)
}
```

## Trait

A trait defines functionality a particular type has and can share with other types.

```rust,noplayground
// define a trait
trait Animal {
    // method blueprint
    fn eat(&mut self);
    // method with default implementation
    fn info(&self) -> String {
        String::from("I'm an animal")
    }
}

// implement a trait
struct Cat;
impl Animal for Cat {
    // implement method
    fn eat(&mut self) {
        println!("I'm eating fish");
    }
    // override default implementation
    fn info(&self) -> String {
        String::from("I'm a cat")
    }
}
```

When combined with generic, we can use trait bounds to specify that a type
parameter has specific behaviors.

```rust,noplayground
// generic function with trait bound
use std::fmt::Display;
fn print<T: Display>(val: T) {  // `T` implement `Display` trait (`T` is displayable)
    println!("{}", val);
}

use std::fmt::Debug;
use std::clone::Clone;
fn clone_and_print<T>(val: T)
where
    T: Debug + Clone // `T` implement `Debug` and `Clone` trait
{
    let cloned = val.clone();
    println!("{:?}", cloned);
}
```

They are similar two ways to achieve polymorphism in Rust: `impl Trait` which
happens at compile time, and `dyn Trait` which happens at runtime.

```rust,noplayground
// `impl Trait` (static dispatch)
fn print_info(animal: impl Animal) {
    println!("{}", animal.info());
}

// `&dyn Trait` (dynamic dispatch)
fn print_info_dyn(animal: &dyn Animal) {
    println!("{}", animal.info());
}

// `Box<dyn Trait>` (dynamic dispatch)
fn print_info_box(animal: Box<dyn Animal>) {
    println!("{}", animal.info());
}
```

Traits can also be generic, they may also have associated types.

```rust,noplayground
// generic trait
trait Into<T> {
    // type associated with the trait
    type Output = T;
    // method blueprint
    fn Into(self) -> Self::Output;
}

// implement generic trait
struct Cat;
impl Into<String> for Cat {
    fn Into(self) -> String {
        String::from("I'm a cat!")
    }
}
```

## Lifetime

Lifetimes are another kind of generic, but for references. Rather than ensuring
that a type has the behavior we want, lifetimes ensure that references are
valid as long as we need them to be.

Note that lifetime annotations do not define the lifetime of references, but
bridge the relationship between them. The actual lifetime is determined by the
value and reference flow.

```rust,noplayground
// lifetime annotation
&str         // a reference
&'a str      // a reference with an explicit lifetime
&'a mut str  // a mutable reference with an explicit lifetime
&'static str // a reference with the 'static lifetime (the entire program)

// function with lifetime
// `x` and `y` have the same lifetime `'a`, so does the return value
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

## Type State Pattern

Type state pattern is a design pattern that allows a type to change its behavior
based on its associated state.

```rust,noplayground
use std::marker::PhantomData; // A marker type that takes no space

// state type
struct Locked;
struct Unlocked;

// stateful type (state default to `Locked`)
struct Safe<State = Locked> {
    treasure: String,
    _state: PhantomData<State>, // state marker to satisfy the type system
}

// `Locked` specialized impl
impl Safe<Locked> {
    pub fn unlock(self, _pswd: String) -> Safe<Unlocked> {
        Safe {
            treasure: self.treasure,
            _state: PhantomData,
        }
    }
}

// `Locked` specialized impl (state elided)
impl Safe {
    pub fn new() -> Self {
        Self {
            treasure: "treasure".to_string(),
            _state: PhantomData,
        }
    }
}

// `Unlocked` specialized impl
impl Safe<Unlocked> {
    pub fn lock(self) -> Safe<Locked> {
        Safe {
            treasure: self.treasure,
            _state: PhantomData,
        }
    }
    pub fn treasure(&self) -> String {
        self.treasure.clone()
    }
}

// state independent impl (shared)
impl<State> Safe<State> {
    pub fn version(&self) {}
    pub fn encryption(&self) {}
}

fn main() {
    let safe = Safe::new();
    safe.version();
    safe.encryption();
    // no method named `treasure` found for struct `Safe` in the current scope
    // safe.treasure();

    let safe = safe.unlock("secret".to_string());
    safe.version();
    safe.encryption();
    safe.treasure();

    let safe = safe.lock();
}
```
