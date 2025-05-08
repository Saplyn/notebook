# Fundamental

## Error Handling

In Zig, errors are represented as values, and is supported with native language features.

```zig
// define an error set
const NumberError = error{
    TooBig,
    TooSmall,
};

// failable function, returns an error union
// on error, it's `NumberError` variant
// on success, it's `i32` variant
fn failable() NumberError!i32 {
    if (number > 100) {
        return NumberError.TooBig; // specify error type
    } else if (number < 0) {
        return error.TooSmall;     // infer error type
    }
    return number;
}

// ignore error with default value
const fallback = failable() catch 50; // on error, fallback to 50

// handle error
const handle = failable() catch |err| {
    switch (err) {
        NumberError.TooBig => std.debug.print("Number is too big!\n", .{}),
        NumberError.TooSmall => std.debug.print("Number is too small!\n", .{}),
    }
};

// propagate error
fn anyhow() NumberError!void {
  const res = try failable();  // early return on error
}

// handle error with `if-else` & `switch`
const res = failable();
if (res) |val| { // <~ if success, `val` is the value
    std.debug.print("Value: {}\n", .{val});
} else |err| switch (err) { // <~ if error, `err` is the error. switch is not required
    NumberError.TooBig => std.debug.print("Number is too big!\n", .{}),
    NumberError.TooSmall => std.debug.print("Number is too small!\n", .{}),
}

```

## Defer Execution

The `defer` keyword can be used to schedule a function to be executed when the
current scope exits. The `errdefer` keyword can be used to schedule a function to
be executed when the scope exits with an error.

```zig
// When a function may return in many places, `defer` can be used to ensure
// that a "tail" function is called at the end of the function, regardless of how
// the function exits.
fn printClosedWord(cond: bool) void {
    std.debug.print("(", .{});

    defer std.debug.print(") ", .{}); // <~ deferred

    if (cond) {
        std.debug.print("True", .{});
        return;
    } else {
        std.debug.print("False", .{});
        return;
    }
}

// When a function may early return with an error, `errdefer` can be used to
// ensure that a "error tail" function is called when the function exits with an
// error.
fn example() !void {
    errdefer std.debug.print("failed!\n", .{}); // <~ deferred (on error)
    const res = try failable(); // <~ possible early return
}
```
