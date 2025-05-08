# Rust

- [Standard Library](https://doc.rust-lang.org/std/)
- [Official Crate Registry](https://crates.io/)
- [Community Crate Catalog](https://lib.rs/)

```rust,noplayground
// Guess the number game
// https://doc.rust-lang.org/book/ch02-00-guessing-game-tutorial.html
use rand::Rng; // external crate
use std::cmp::Ordering;
use std::io;

fn generate_secret_number(min: u32, max: u32) -> u32 {
    rand::thread_rng().gen_range(min..=max)
}

fn main() {
    println!("Guess the number!");

    let secret_number = generate_secret_number(1, 100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

## Types

- Primitives:
  - `u8`, `u16`, `u32`, `u64`, `u128`, `usize`
  - `i8`, `i16`, `i32`, `i64`, `i128`, `isize`
  - `f32`, `f64`; `char`; `bool`
- Constants: `const`, `static`
- Mutability: `let mut x = 5;`
- Reference: `&x`, `&mut x`
- Casting: `5 as u8`
- Arrays: `[i32; 5]`
- Tuples: `(i32, f64, char)`

## Operators

- Arithmetic: `+`, `-`, `*`, `/`, `%`
- Relational: `==`, `!=`, `<`, `<=`, `>`, `>=`
- Logical: `&&`, `||`, `!`
- Bitwise: `&`, `|`, `^`, `<<`, `>>`
- Assignment: `=`, `+=`, `-=`, `*=`, `/=`, `%=`

## Branching

```rust,noplayground
// if-else expression
if condition { /* ... */ }
else if condition { /* ... */ }
else { /* ... */ }

// match expression
match value {
    pattern1 => /* ... */,
    pattern2 => /* ... */,
    _ => /* ... */,  // catch-all (wildcard)
}

// if let expression
if let Some(x) = compute() {
    println!("got a number: {}", x);
} else {
    println!("got nothing");
}

// let else statement
let x = Some(5) else {
    println!("got nothing");
    return;
};
```

## Looping

```rust,noplayground
// loop expression (while true)
loop { /* ... */ }

// while loop
while condition { /* ... */ }

// for loop
for elem in iter { /* ... */ }

// range
1..=5 // 1, 2, 3, 4, 5
1..5  // 1, 2, 3, 4

// break and continue
loop {
    if condition { break; }
    if condition { continue; }
}

// `break` with label.
'outer: loop {
    'inner: loop {
        break 'outer;
    }
}

// `break` with value
let result = loop {
    if condition { break 1; }
    else { break 2; }
};

// while let loop
while let Some(top) = stack.pop() {
    println!("{}", top);
}
```

## Pattern Matching

```rust,noplayground
// Destructuring a tuple
let (x, y, z) = (1, 2, 3);
let (first, .., last) = (2, 4, 8, 16, 32);
for (index, value) in v.iter().enumerate() {
    println!("{} is at index {}", value, index);
}

// Destructuring a struct
let Point { x, y } = Point::new(1, 2);   // extract x and y
let Vec3 { z, .. } = Vec3::new(4, 5, 6); // extract z, ignore others

// Destructuring an enum
let Some(x) = compute();

// matching a enum
match compute() {
    Some(x) => println!("got a number: {}", x),
    None => println!("got nothing"),
}

// various match patterns
match x {
    1 => println!("one"),
    2 => println!("two"),
    3 | 4 => println!("three or four"),
    5..=10 => println!("five through ten"),
    _ => {
        print!("it ");
        print!("can ");
        print!("be ");
        println!("anything!");
    },
}

// match guards
match x {
    Some(0) => println!("zero"),
    Some(n) if n < 0 => println!("negative"),
    Some(n) if n > 0 => println!("positive"),
    _ => println!("nothing"),
}

// @ bindings
match x {
    Some(n @ 0) => println!("zero: {}", n),
    Some(n) => println!("non-zero: {}", n),
    None => println!("nothing"),
}

// @ bindings with renaming
let msg = Message::Hello { id: 5 };
match msg {
    Message::Hello {
        id: id_variable @ 3..=7,
    } => println!("Found an id in range: {}", id_variable),
    Message::Hello { id: 10..=12 } => {
        println!("Found an id in another range")
    }
    Message::Hello { id } => println!("Found some other id: {}", id),
}
```

## Struct

```rust,noplayground
struct Point {
    // fields are private by default
    x: i32,
    // `pub` makes the field public
    pub y: i32,
}

// methods
impl Point {
    // associated function, also constructor
    fn new(x: i32, y: i32) -> Point {
        Point { x, y }
    }
    // captures `self` by immutable reference
    fn get_x(&self) -> i32 {
        self.x
    }
    // captures `self` by mutable reference
    fn set_x(&mut self, x: i32) {
        self.x = x;
    }
}

// method implementation can be separated
impl Point {
    // consumes `self`
    fn move_to(self, x: i32, y: i32) -> Point {
        Point { x, y }
    }
}

// construction
let p0 = Point::new(0, 0);
let x = p.get_x();
let p1 = Point {
    x, // field init shorthand
    y: 4
};
let p2 = Point { y: 4, ..p1 }; // struct update syntax

// tuple struct
struct Color(i32, i32, i32);
let black = Color(0, 0, 0); // construction
let r = black.0; // accessing fields by index
let g = black.1;
let b = black.2;

// unit-like struct (no fields, marker)
struct PlayerMarker;
```

## Enum

```rust,noplayground
// marker enum
enum IpAddrKind {
    V4,
    V6,
}
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;

// with data
enum AppError {
    NoContext,
    IoError(io::Error),
    InvalidInput {
        encountered: String,
        expected: String
    },
}
let unknown = AppError::NoContext;
let io_err = AppError::IoError(/* ... */);
let invalid = AppError::InvalidInput {
    encountered: "sapling".to_string(),
    expected: "saplyn".to_string()
};

// methods
impl AppError {
    fn is_io_error(&self) -> bool {
        match self {
            AppError::IoError(_) => true,
            _ => false,
        }
    }
}
```
