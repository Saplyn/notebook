# Saplyn's Notebook

A personal programming notebook built with [mdBook](https://rust-lang.github.io/mdBook/), containing notes on languages, frameworks, tools, and useful config snippets.

## Contents

The book content lives in `src/` and is organized into:

- **Language** notes (C, C++, Rust, Zig, Bash, Go, Java, Kotlin, Vue, SQLite)
- **Framework** notes (Nuxt, Nest, Relm4, Makepad)
- **Tools** notes (GitHub Actions, ast-grep, watchexec, sed, gdb)
- **Emergency Copy** snippets (`.vimrc`, `.bashrc`, `.tmux.conf`, etc.)

Navigation is defined in:

- `src/SUMMARY.md`

Project settings are defined in:

- `book.toml`

## Local Development

1. Install `mdbook`.
2. From the repository root, run:

```bash
mdbook serve
```

To build static output:

```bash
mdbook build
```

Generated files are written to `book/`.

## Deployment

GitHub Actions builds and deploys the book to GitHub Pages using:

- `.github/workflows/mdbook.yml`

The configured site URL is `/notebook/` (see `book.toml`).
