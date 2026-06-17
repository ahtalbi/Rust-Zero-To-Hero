# rust-analyzer is (The Rust Language Server).

When you install Rust tooling, rust-analyzer is the brain behind your editor's Rust support.
It is a **language server** — a background program that talks to your editor (VSCode, Neovim, etc.)
and provides smart features: completions, errors, go-to-definition, and more.

---

## What rust-analyzer Does

rust-analyzer reads your Rust code **without compiling it** — it builds its own internal model of your
project so it can answer questions instantly, without waiting for `cargo build`.

| Feature | What it does |
|---------|-------------|
| Diagnostics | Shows errors and warnings as you type |
| Completions | Suggests code as you type (IntelliSense) |
| Go to Definition | Jump to where a function/type is defined |
| Find References | See everywhere a symbol is used |
| Hover Docs | Show docs when you hover over a symbol |
| Inlay Hints | Show inferred types inline in your code |
| Code Actions | Quick fixes, auto-imports, refactors |
| Rename | Rename a symbol everywhere safely |
| Expand Macro | See what a macro expands into |
| Syntax Tree | Inspect the raw AST of your file |
| Run / Debug | Lens buttons to run tests or binaries |
| Cargo Integration | Understands your `Cargo.toml` and workspaces |

---

## How rust-analyzer Works (Internals)

rust-analyzer is built on a few key concepts:

```
YOUR CODE
    │
    ▼
[ Parsing ]           → Builds a Syntax Tree (CST) from raw text
    │
    ▼
[ Name Resolution ]   → Figures out what each name refers to
    │
    ▼
[ Type Inference ]    → Infers types for every expression
    │
    ▼
[ IDE Layer ]         → Answers editor questions (completions, hints, etc.)
    │
    ▼
[ LSP Layer ]         → Sends answers to your editor via the LSP protocol
```

The key insight: it uses **incremental computation** (via the `salsa` library).
When you change one line, only the parts that depend on that line are recomputed — not the whole project.

---

## rust-analyzer Components

| Component | What it does |
|-----------|-------------|
| `parser` | Turns raw Rust text into a syntax tree |
| `syntax` | Represents and traverses the syntax tree |
| `hir` | High-level Intermediate Representation (semantic model) |
| `hir_def` | Definitions: structs, functions, traits, modules |
| `hir_ty` | Type inference and trait solving |
| `ide` | Core IDE features (completions, hover, etc.) |
| `ide_assists` | Code actions and quick fixes |
| `ide_diagnostics` | Error/warning messages |
| `ide_ssr` | Structural Search Replace |
| `proc_macro_srv` | Runs procedural macros |
| `project_model` | Reads `Cargo.toml` and workspace structure |
| `vfs` | Virtual File System — tracks file changes |
| `lsp_server` | Speaks the LSP protocol with your editor |
| `flycheck` | Runs `cargo check` in the background |

---

## Controlling rust-analyzer

## A) Configure via `rust-analyzer.toml` or `settings.json`

rust-analyzer is **highly configurable**. You control it through:
- A `rust-analyzer.toml` file in your project root (newer, recommended)
- Your editor's settings file (e.g. `.vscode/settings.json`)

```toml
# rust-analyzer.toml

# Control which features to enable
[check]
command = "clippy"          # use clippy instead of cargo check

[cargo]
features = "all"            # enable all Cargo features
allTargets = true           # check tests and examples too

[inlayHints]
typeHints.enable = true     # show inferred types inline
parameterHints.enable = true

[completion]
autoimport.enable = true    # auto-add use statements

[diagnostics]
enable = true
experimental.enable = true  # show extra experimental warnings
```

---

## B) Install / Update / Remove via `rustup`

```bash
# install rust-analyzer as a rustup component (recommended)
rustup component add rust-analyzer

# update rust-analyzer (runs with rustup update)
rustup update

# remove rust-analyzer
rustup component remove rust-analyzer

# check if it's installed
rustup component list | grep rust-analyzer

# run it manually (starts the LSP server on stdin/stdout)
rust-analyzer
```

You can also install it as a **standalone binary**:
```bash
# via cargo
cargo install rust-analyzer

# or download from GitHub releases
# https://github.com/rust-lang/rust-analyzer/releases
```

---

## C) Replace / Extend Parts With Your Own Setup

**1 — Use a different check command:**
```toml
# instead of cargo check, use cargo clippy
[check]
command = "clippy"
extraArgs = ["--", "-W", "clippy::all"]
```

**2 — Point to a custom Rust toolchain:**
```toml
# force rust-analyzer to use a specific toolchain
[rustc]
source = "rustup"   # or path to a custom rustc
```

**3 — Use a custom proc-macro server:**
```toml
# if your macros need a different server
[procMacro]
enable = true
server = "/path/to/custom/proc-macro-server"
```

**4 — Disable rust-analyzer for parts of a workspace:**
```toml
# in a specific crate's Cargo.toml
[package.metadata.rust-analyzer]
rustc_private = true   # for compiler-internal crates
```

---

## D) Debug / Inspect rust-analyzer Itself

```bash
# see what rust-analyzer sees as your project structure
rust-analyzer analysis-stats .

# dump the syntax tree of a file
rust-analyzer parse < src/main.rs

# show all config options with docs
rust-analyzer --print-config-schema

# enable logging to see what it's doing
env RA_LOG=info rust-analyzer

# check the LSP logs inside VSCode
# → View → Output → rust-analyzer (channel)
```

---

## The Full Control Map

```
rust-analyzer CONTROL
│
├── A) CONFIGURE
│     ├── rust-analyzer.toml      → project-level config
│     ├── settings.json (editor)  → editor-level config
│     ├── check.command           → swap cargo check → clippy
│     ├── inlayHints.*            → control inline type display
│     └── procMacro.enable        → toggle macro expansion
│
├── B) MANAGE (rustup)
│     ├── component add           → install
│     ├── component remove        → uninstall
│     ├── update                  → upgrade
│     └── component list          → see installed state
│
├── C) REPLACE / EXTEND
│     ├── custom check command    → swap the checker
│     ├── custom proc-macro srv   → swap macro expansion
│     ├── custom toolchain path   → use a different rustc
│     └── standalone binary       → run outside rustup
│
└── D) INSPECT / DEBUG
      ├── analysis-stats          → project analysis info
      ├── parse                   → dump syntax tree
      ├── RA_LOG=info             → enable internal logging
      └── --print-config-schema   → see all config options
```

---

## rust-analyzer vs rustc — Key Difference

| | `rustc` | `rust-analyzer` |
|---|---------|-----------------|
| Purpose | Compile code to a binary | Understand code for editors |
| Speed | Slow (full compilation) | Fast (incremental, no codegen) |
| Output | Binary / errors | IDE features |
| Runs when | `cargo build` | Every keypress |
| Errors | 100% accurate | Best-effort (can work with broken code) |

rust-analyzer intentionally **tolerates broken/incomplete code** so it can still give you
completions and hints even while you're in the middle of typing.

---

Last thing — rust-analyzer internally uses **`salsa`** (a query-based incremental computation
framework) and **`rowan`** (a lossless syntax tree library). These are the hidden engines
that make it fast. You can find them in the rust-analyzer repo under `lib/`.