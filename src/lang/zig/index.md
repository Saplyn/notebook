# Zig

- [Language Reference](https://ziglang.org/documentation)
- [Standard Library](https://ziglang.org/documentation/master/std/)

```zig
// Hello world program
const std = @import("std");

pub fn main() void {
    std.debug.print("Hello world!\n", .{});
}
```

## Types

- Primitives:
  - `u8`, `u16`, `u32`, `u64`, `u128`, `usize`
  - `i8`, `i16`, `i32`, `i64`, `i128`, `isize`

## Operators

- Arithmetic: `+`, `-`, `*`, `/`, `%`
- Relational: `==`, `!=`, `<`, `<=`, `>`, `>=`
- Array manipulation: `++` (concatenation), `**` (repeat)

## Branching

```zig
// if-else statement
if (condition) {
    std.debug.print("Condition is true!\n", .{});
} else {
    std.debug.print("Condition is not true!\n", .{});
}

// if-else expression
const price: u8 = if (discount) 17 else 20;
```

## Looping

```zig
// while loop
while (n < 1024) {
    std.debug.print("{} ", .{n});
    n *= 2;
}

// while loop with continue expression
// (the continue expression invoked at the end of the loop
//  or when an explicit 'continue' is invoked)
while (n < 1024) : (n *= 2) {
    std.debug.print("{} ", .{n});
}

continue; // skips the current iteration
break; // exits the loop

// for loop
for (array) |item| {
    std.debug.print("{} ", .{item});
}
for (array, 0..) |item, index| {
    std.debug.print("{}: {} ", .{index, item});
}
```
