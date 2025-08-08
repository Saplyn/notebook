# `gdb` With `gef`

The GNU Debugger with Enhanced Features.

## `gdb` Control Commands

- `file [FILE]`: use `[File]` as the executable to debug, reading its symbols.
- `target remote [HOST]:[PORT]`: connect to a remote target for debugging.
- `add-symbol-file [FILE] [ADDRESS]`: add a symbol file at the specified address.

## Debug Control Commands

- `break [LOCATION]`/`b [LOCATION]`: set a breakpoint at the specified location,
  which can be a function name, line number, or address.
- `delete [BREAKPOINT]`/`d [BREAKPOINT]`: delete the specified breakpoint, or all
  breakpoints if no argument is given.
- `stepi`/`si`: steps through the program one machine instruction at a time.
- `step`/`s`: steps through the program one line of source code at a time,
  stepping into function calls.
- `nexti`/`ni`: similar to `si`, but it will treat any function call as one
  instruction, so will not step into the function.
- `next`/`n`: steps through the program one line of source code at a time.
- `continue`/`c`: continue execution until the next breakpoint.

## Debug Inspection Commands

- `print [EXPRESSION]`/`p [EXPRESSION]`: evaluate and print the value of the
  specified expression.
- `set [VARIABLE]=[VALUE]`: set the value of a variable to a new value.
- `backtrace`/`bt`: print the stack trace of the current thread.
- `info local`: list all local variables in the current stack frame

## Information Commands

- `info function`: list names and data types of all defined functions
- `info sources`: list all source files used in the debugging session
- `info break`: list all breakpoints
