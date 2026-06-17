# rustfmt is (The Rust Code Formatter).

When you install Rust, rustfmt comes by default via rustup.
It is a **code formatter** — a tool that automatically rewrites your Rust code
to follow a consistent style, so you never argue about spacing, brackets, or line length again.

---

## What rustfmt Does

rustfmt reads your `.rs` files and **rewrites them in place** to match the official Rust style guide.
It does NOT change what your code does — only how it looks.

| Feature | What it does |
|---------|-------------|
| Indentation | Fixes spaces/tabs consistently (4 spaces by default) |
| Line length | Wraps long lines (default max: 100 chars) |
| Brace style | Enforces where `{` goes |
| Import sorting | Sorts and groups `use` statements |
| Trailing commas | Adds/removes trailing commas in lists |
| Blank lines | Normalizes empty lines between items |
| Comment alignment | Aligns inline comments |
| String formatting | Normalizes string literals where possible |
| Attribute formatting | Formats `#[derive(...)]` and other attributes |
| Match arm alignment | Aligns `=>` in match blocks |

---

## How rustfmt Works (Internals)

```
YOUR .rs FILE
    │
    ▼
[ Parsing ]           → Parses code into a syntax tree (uses rustfmt's own parser)
    │
    ▼
[ Visiting ]          → Walks every node in the tree (fn, struct, block, expr...)
    │
    ▼
[ Rewriting ]         → Rewrites each node according to style rules
    │
    ▼
[ Shaping ]           → Decides: does this fit on one line or needs wrapping?
    │
    ▼
[ Output ]            → Writes the formatted text back to the file (or stdout)
```

Key insight: rustfmt works on the **syntax tree**, not raw text — so it understands
the structure of your code, not just the characters.

---

## rustfmt Style Rules (What It Controls)

| Rule | Default | Example |
|------|---------|---------|
| `indent_style` | Block (4 spaces) | functions, match arms |
| `max_width` | 100 chars | wraps lines longer than this |
| `tab_spaces` | 4 | spaces per indent level |
| `brace_style` | SameLineWhere | `fn foo() {` on same line |
| `imports_granularity` | Preserve | how `use` items are grouped |
| `imports_layout` | Mixed | flat vs nested use trees |
| `trailing_comma` | Vertical | comma after last item in multi-line |
| `trailing_semicolon` | true | keeps `;` at end of blocks |
| `reorder_imports` | true | sorts `use` alphabetically |
| `reorder_modules` | true | sorts `mod` declarations |
| `use_small_heuristics` | Default | auto-collapses short structs/fns |
| `newline_style` | Auto | LF / CRLF handling |
| `edition` | 2015 | Rust edition (set to match your project) |

---

## Controlling rustfmt

## A) Configure via `rustfmt.toml`

Create a `rustfmt.toml` (or `.rustfmt.toml`) file in your project root:

```toml
# rustfmt.toml

edition = "2021"            # match your Cargo.toml edition
max_width = 100             # max line length
tab_spaces = 4              # indent size
trailing_comma = "Always"   # Always / Never / Vertical
reorder_imports = true      # sort use statements
imports_granularity = "Crate"  # group imports by crate
group_imports = "StdExternalCrate"  # std → external → local

# style choices
brace_style = "SameLine"
where_single_line = true    # keep short where clauses on one line
use_small_heuristics = "Max" # collapse short things aggressively
```

**Skip formatting for a block:**
```rust
#[rustfmt::skip]
fn messy_but_intentional() {
    let matrix = [
        1, 0, 0,
        0, 1, 0,
        0, 0, 1,
    ];
}
```

**Skip an entire file:**
```rust
// at the top of the file
#![rustfmt::skip]
```

---

## B) Install / Update / Remove via `rustup`

```bash
# install rustfmt (usually pre-installed)
rustup component add rustfmt

# remove rustfmt
rustup component remove rustfmt

# update rustfmt (comes with rustup update)
rustup update

# check if installed
rustup component list | grep rustfmt

# check version
rustfmt --version
```

---

## C) Run rustfmt

```bash
# format a single file
rustfmt src/main.rs

# format the whole project (recommended)
cargo fmt

# CHECK only — don't change files, just report what would change
cargo fmt --check        # exits with error if anything needs formatting
rustfmt --check src/main.rs

# format and show the diff (what changed)
rustfmt --emit stdout src/main.rs

# format with a specific edition
rustfmt --edition 2021 src/main.rs

# format using a specific config file
rustfmt --config-path /path/to/rustfmt.toml src/main.rs

# print all available config options with docs
rustfmt --print-config default .
```

---

## D) Use in CI / Git Hooks

**In CI (GitHub Actions example):**
```yaml
- name: Check formatting
  run: cargo fmt --check
```

**As a git pre-commit hook** (`.git/hooks/pre-commit`):
```bash
#!/bin/sh
cargo fmt --check
if [ $? -ne 0 ]; then
  echo "Please run cargo fmt before committing."
  exit 1
fi
```

---

## The Full Control Map

```
rustfmt CONTROL
│
├── A) CONFIGURE
│     ├── rustfmt.toml            → project-level style rules
│     ├── #[rustfmt::skip]        → skip a specific block
│     ├── #![rustfmt::skip]       → skip an entire file
│     └── --config-path           → point to a custom config
│
├── B) MANAGE (rustup)
│     ├── component add           → install
│     ├── component remove        → uninstall
│     ├── update                  → upgrade
│     └── component list          → check installed state
│
├── C) RUN
│     ├── cargo fmt               → format whole project
│     ├── rustfmt <file>          → format one file
│     ├── cargo fmt --check       → CI mode (no changes, just check)
│     ├── --emit stdout           → preview changes without writing
│     └── --edition               → specify Rust edition
│
└── D) AUTOMATE
      ├── CI pipeline             → enforce fmt on every PR
      ├── git pre-commit hook     → block unformatted commits
      └── editor on-save          → format automatically on save
```

---

## rustfmt vs clippy vs rust-analyzer — Key Difference

| | `rustfmt` | `clippy` | `rust-analyzer` |
|---|-----------|----------|-----------------|
| Purpose | Format style | Catch bad patterns | Editor intelligence |
| Changes code? | YES (rewrites) | Suggests fixes | NO (read-only) |
| Runs when | `cargo fmt` | `cargo clippy` | Every keypress |
| Cares about logic? | NO | YES | YES |
| Output | Formatted file | Warnings / hints | IDE features |

Think of them as a team:
- **rustfmt** — makes your code *look* right
- **clippy** — makes your code *work* right
- **rust-analyzer** — helps you *write* right

---

Last thing — rustfmt has two modes of style rules: **stable** (on by default)
and **unstable** (opt-in, requires nightly). Unstable options give you much finer
control but may change between Rust versions. To use them:

```toml
# rustfmt.toml — requires nightly rustfmt
unstable_features = true
format_code_in_doc_comments = true   # format code inside /// comments
imports_granularity = "One"          # one use item per line
```

```bash
# run with nightly to unlock unstable options
rustup run nightly rustfmt src/main.rs
```