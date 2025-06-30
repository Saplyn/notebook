# `ast-grep`

Fast and polyglot tool for code structural search, lint, rewriting at large scale.

- [Official Website](https://ast-grep.github.io/)

## Examples

```bash
# Pattern: `println!($$$)`
sg -p "println!(\$\$\$)"
```

## Pattern

`sg` uses pattern code to construct Abstract Syntax Tree and match that against
target code:

```javascript
// `a+1` will match the following (expressions in) code:
const b = a + 1;
funcCall(a + 1);
deeplyNested({
  target: a + 1,
});
```

We can match dynamic content with meta variables:

- **Meta variable**: `$` matches one single AST node, can be named.
- **Multi-meta variable**: `$$$` matches zero or more AST node, can be named.

Named variable can be captured like capture group in regular expressions.

```javascript
// `$A == $A` will match the following code:
a == a;
1 + 1 == 1 + 1;

// `function $FUNC($$$ARGS) { $$$ }` will match the following code:
function foo(bar) {
  return bar;
}
function noop() {}
function add(a, b, c) {
  return a + b + c;
}
```
