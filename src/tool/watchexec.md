# `watchexec`

Simple, standalone tool that watches a path and runs a command whenever it detects
modifications.

- [Official Repository](https://github.com/watchexec/watchexec)

## Examples

```bash
# Watch: all `.js`, `.css`, `.html` files
# Run: `make` command
watchexec  -e js,css,html  make

# Watch: all files
# Run: `cargo run --release` command
# Restart: kill existing process
watchexec --restart  cargo run --release
```
