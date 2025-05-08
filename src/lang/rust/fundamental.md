# Fundamental

## Macros

Macros in rust are "code that writes code". For simplicity, they can be thought
of as:

- a function call, like: `println!()`
- an attribute to add metadata to some item, like: `#[derive(Debug)]`
- a way to define new syntax, like: `vec![]`

```rust,noplayground
// function-like macro
println!("Hello, world!");             // print to stdout
eprintln!("An error occurred: {}", e); // print to stderr
dbg!(Object);                          // debug print

// attribute-like macro
#[derive(Debug)] // derive `Debug` trait for the struct
struct Object;
```

## Common Types

Rust has a powerful ans expressive type system. Here are some commonly used
non-primitive types.

```rust,noplayground
String       // growable UTF-8 encoded text
Vec<T>       // growable array
Option<T>    // optional value. Rust's version of `null` or something
Result<T, E> // failable value
Box<T>       // heap-allocated value
&str         // string slice
&[T]         // array slice
```

## Ownership & Borrowing

- **Ownership**
  - Every value in Rust has a variable that's its owner.
  - When the owner goes out of scope, the value gets dropped.
  - When copying a value, the ownership is transferred. (if type not `Copy`)
- **Borrowing**
  - A reference to a value, without taking ownership.
  - At any given time, there can be either one mutable reference or any number of
    immutable references.
  - References must always be valid.

## Error Handling

In Rust, there're two ways to handle errors: `panic!()` and `Result<T, E>`.
`panic!()` terminates the program, indicating a unrecoverable error; while
`Result<T, E>` is used to handle recoverable errors. For when to `panic!()`,
see [The Book](https://doc.rust-lang.org/book/ch09-03-to-panic-or-not-to-panic.html).

```rust,noplayground
enum AppError {
    SaplynIsNotCat,
}

// failable function, returns `Result<T, E>`
fn failable() -> Result<String, AppError> {
    if saplyn.is_cat() {
        Ok(String::from("I'm a cat!"))
    } else {
        Err(AppError::SaplynIsNotCat)
    }
}

fn main() {
    let result = failable(); // call failable function
    match result {
        // is value? compute value
        Ok(val) => println!("Got value: {}", val),
        // is error? handle error
        Err(err) => {
            // unrecoverable error, panic!
            panic!("What?! I should be a cat!")
        },
    }
}
```

Sometimes, we are not handling the error right away, but passing it to the
caller. In this case, we use `?` to propagate the error.

```rust,noplayground
// https://lib.rs/crates/anyhow

fn compute() -> anyhow::Result<i32> { Ok(114) }

fn action() -> anyhow::Result<i32> {
    let val = compute()?;  // propagate error or extracting value
    Ok(val * 1000 + 514)
}

fn long_chain() -> anyhow::Result<()> {
    let a = action()?;
    let b = action()?;
    let c = action()?;
    Ok(a + b + c)
}
```
