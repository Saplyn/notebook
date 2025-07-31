# `sed`

GNU stream editor for filtering and transforming text.

## Examples

```bash
# Substitute 'from' with 'to' in a file (first occurrence only)
sed 's/from/to/' file.txt

# Substitute 'from' with 'to' globally in a file, modify in-place
sed -i 's/from/to/g' file.txt

# Different delimiter  (useful for paths)
sed 's|/from/path|/to/path|' file.txt
sed 's /from/path /to/path ' file.txt
```
