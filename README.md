# markterm

A lightweight command-line tool that renders markdown files with VT100/ANSI terminal formatting and displays them in a built-in `more`-style pager.

## Features

- **Markdown rendering** — headings, bold, italic, strikethrough, inline code, code blocks, lists, tables, block quotes, links, horizontal rules, task lists
- **Built-in pager** — page through documents with spacebar, scroll line-by-line, jump to top/bottom
- **Word wrapping** — respects terminal width with proper line breaking
- **No-wrap mode** — truncate long lines with ellipsis instead of wrapping
- **Single binary** — no runtime dependencies, compiles to a standalone executable
- **Pipe-friendly** — auto-detects TTY; dumps plain output when piped

## Installation

```bash
cargo install --path .
```

Or build from source:

```bash
cargo build --release
./target/release/markterm README.md
```

### macOS PATH Setup

After installing with `cargo install`, the binary is placed in `~/.cargo/bin/`. If `markterm` isn't found, add Cargo's bin directory to your PATH:

```bash
source "$HOME/.cargo/env"
```

To make it permanent, add it to your shell profile:

```bash
# For zsh (default on macOS)
echo 'source "$HOME/.cargo/env"' >> ~/.zshrc

# For bash
echo 'source "$HOME/.cargo/env"' >> ~/.bash_profile
```

Then restart your terminal or run `source ~/.zshrc`.

## Usage

```
markterm [OPTIONS] <FILE>

Arguments:
  <FILE>    Markdown file to display (use - for stdin)

Options:
  -w, --width <COLS>     Override terminal width
  -t, --theme <THEME>    Color theme: auto, dark, light, none (default: auto)
      --no-pager         Dump rendered output to stdout without paging
      --no-wrap          Truncate long lines with ellipsis instead of wrapping
  -h, --help             Show help
  -V, --version          Show version
```

### Examples

```bash
# View a markdown file with paging
markterm README.md

# Pipe from stdin
cat README.md | markterm -

# Dump without pager (useful for piping)
markterm --no-pager README.md

# Override terminal width
markterm -w 60 README.md

# Truncate long lines instead of wrapping
markterm --no-wrap README.md
```

## Pager Controls

| Key | Action |
|-----|--------|
| `Space` | Next page |
| `b` | Previous page |
| `Enter` / `j` / `Down` | Next line |
| `k` / `Up` | Previous line |
| `d` | Half page down |
| `u` | Half page up |
| `g` / `Home` | Go to top |
| `G` / `End` | Go to bottom |
| `q` / `Esc` | Quit |

## Rendering

markterm converts markdown elements to styled terminal output using ANSI/VT100 escape codes:

- **H1** — bold, underlined, bright white
- **H2** — bold, bright cyan
- **H3** — bold, bright yellow
- **H4–H6** — bold
- **Bold/Italic/Strikethrough** — native ANSI attributes
- **Inline code** — reverse video background
- **Code blocks** — bordered with box-drawing characters
- **Lists** — `•` `◦` `▪` bullets for unordered, numbered for ordered
- **Tables** — full box-drawing borders with bold headers
- **Block quotes** — `│` left border
- **Task lists** — `[✓]` / `[ ]` with color
- **Links** — text with URL shown in parentheses

## License

MIT
