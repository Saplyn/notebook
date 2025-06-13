# Zig

- [Language Reference](https://ziglang.org/documentation)
- [Standard Library](https://ziglang.org/documentation/master/std/)
- [Zig 圣经](https://course.ziglang.cc/)

```zig
// Hello world program
const std = @import("std");

pub fn main() !void {
    const stdout = std.io.getStdOut().writer();
    try stdout.print("Hello world!\n", .{});
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

// switch statement
switch (value) {
    1 => std.debug.print("Value is 1!\n", .{}),
    2 => std.debug.print("Value is 2!\n", .{}),
    // a switch must be exhaustive, `else` is used as "default" case
    else => std.debug.print("Value is something else!\n", .{}),
}

// switch expression
const output = switch (value) {
    1 => "Value is 1!",
    2 => "Value is 2!",
    else => "Value is something else!",
};

// unreachable
if (false) { unreachable; }

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

## Compound Types

### Enum

```zig
// `Ops` is a `type`
const Ops = enum { inc, pow, dec };
const operations = [_]Ops{ // an array of Ops
    Ops.inc,
    Ops.pow,
    Ops.dec,
};
switch (op) {
    Ops.inc => { current_value += 1; },
    Ops.dec => { current_value -= 1; },
    Ops.pow => { current_value *= current_value; },
    // exhaustive switch, no need for `else`
}

const Color = enum(u32) { // using u32 as underlying type
    red = 0xff0000,
    green = 0x00ff00,
    blue = 0x0000ff,
};
std.debug.print( // multi-line string
    \\<p>
    \\  <span style="color: #{x:0>6}">Red</span>
    \\  <span style="color: #{x:0>6}">Green</span>
    \\  <span style="color: #{x:0>6}">Blue</span>
    \\</p>
    \\
, .{
    @intFromEnum(Color.red),
    @intFromEnum(Color.green),
    @intFromEnum(Color.blue), // Oops! We're missing something!
});
```

### Struct

```zig
// `Point` is a `type`
const Point = struct {
    x: f64,
    y: f64,
};
var point = Point{ .x = 1.0, .y = 2.0 }; // create a Point instance
point.x = 3.0; // access and modify fields

// default value
const Character = struct {
    level: u8,
    health: u16 = 100, // default health is 100
};
```

## Pointer

```zig
// declare `u8` ptr: *u8
// reference `num1`: &num1
var num1: u8 = 5;
const num1_pointer: *u8 = &num1;

var num2: u8 = undefined;
num2 = num1_pointer.*; // dereference `num1_pointer`: num1_pointer.*
num1_pointer.* = 10;   // modify the value pointed by `num1_pointer`

// pointer types
var mutable: u8 = 5; // `&mutable` is of type `*u8`
const value: u8 = 5; // `&value` is of type `*const u8`

// const pointer
const p1: *u8 = &locked; // cannot reassign, point to a specific value
var   p2: *u8 = &free;   // can be reassigned, point to other
```
