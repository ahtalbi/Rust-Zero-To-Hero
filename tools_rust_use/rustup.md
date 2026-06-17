# rustup is (The Rust Toolchain Manager).

rustup is the **first thing you install** when you start with Rust.
It is not a Rust tool — it is the tool that **installs and manages** all other Rust tools.
Think of it as the "app store" for your entire Rust environment.

---

## What rustup Manages

rustup does not compile code. It manages the things that do.

| Thing it manages | What it is |
|-----------------|------------|
| `rustc` | The Rust compiler |
| `cargo` | The build system and package manager |
| `rustfmt` | The code formatter |
| `clippy` | The linter |
| `rust-analyzer` | The language server (editor support) |
| `rust-std` | The standard library |
| `rust-docs` | Offline Rust documentation |
| `rust-src` | Source code of the standard library |
| `llvm-tools` | Low-level tools for binary inspection |
| `miri` | Interpreter for catching undefined behavior |
| Toolchains | Entire versioned sets of all the above |
| Targets | Cross-compilation support (other OS/chip) |
| Profiles | Preset groups of components to install |

---

## How rustup Works (Internals)

```
YOU RUN: rustup install stable
         │
         ▼
[ Registry Check ]     → contacts static.rust-lang.org to find latest stable version
         │
         ▼
[ Download ]           → downloads compiler + std + tools as .tar.gz packages
         │
         ▼
[ Unpacking ]          → extracts to ~/.rustup/toolchains/<name>/
         │
         ▼
[ Shims ]              → rustup places thin wrapper scripts in ~/.cargo/bin/
         │                 (rustc, cargo, rustfmt... all point back to rustup)
         ▼
[ Active Toolchain ]   → rustup reads rust-toolchain.toml to decide which
                          version to forward commands to
```

Key insight: when you type `rustc`, you are not calling rustc directly —
you are calling a **rustup shim** that then calls the right rustc version.
This is how rustup can manage multiple versions at once.

---

## rustup Concepts

| Concept | What it means |
|---------|--------------|
| **Toolchain** | A complete set: rustc + cargo + std + tools, all one version |
| **Channel** | Which release track: `stable`, `beta`, or `nightly` |
| **Component** | A single piece inside a toolchain (rustfmt, clippy, etc.) |
| **Target** | A machine/OS you want to compile FOR (cross-compilation) |
| **Profile** | A preset group of components (minimal, default, complete) |
| **Override** | Force a specific toolchain for one directory/project |
| **Shim** | A tiny wrapper in `~/.cargo/bin/` that forwards to rustup |

---

## Controlling rustup

## A) Toolchains — Install, Switch, Remove Rust Versions

```bash
# install a channel
rustup install stable       # latest stable Rust
rustup install beta         # next release candidate
rustup install nightly      # cutting-edge, updated daily

# install a specific version
rustup install 1.78.0       # exact version
rustup install nightly-2024-01-15  # specific nightly date

# set the default (used everywhere)
rustup default stable
rustup default nightly

# list all installed toolchains
rustup toolchain list

# remove a toolchain
rustup toolchain uninstall nightly

# update all installed toolchains
rustup update
rustup update stable        # update only stable
```

---

## B) Components — Install Individual Tools

```bash
# add a component to your active toolchain
rustup component add rustfmt
rustup component add clippy
rustup component add rust-analyzer
rustup component add rust-src        # std source (needed by some tools)
rustup component add rust-docs       # offline docs

# add to a specific toolchain (not just the active one)
rustup component add rustfmt --toolchain nightly

# remove a component
rustup component remove rustfmt

# see all available and installed components
rustup component list
rustup component list --installed
```

---

## C) Targets — Cross Compilation

A **target** lets you compile Rust code that runs on a DIFFERENT machine than yours.

```bash
# add a target (install std for that platform)
rustup target add x86_64-unknown-linux-gnu     # 64-bit Linux
rustup target add aarch64-apple-ios            # iPhone
rustup target add wasm32-unknown-unknown       # WebAssembly
rustup target add thumbv7em-none-eabihf        # embedded ARM (no OS)
rustup target add x86_64-pc-windows-gnu       # Windows from Linux

# list all installed targets
rustup target list --installed

# list ALL available targets
rustup target list

# remove a target
rustup target remove wasm32-unknown-unknown

# build FOR a target
cargo build --target wasm32-unknown-unknown
```

---

## D) Overrides — Per-Project Toolchain

You can pin a specific Rust version to one project:

**Option 1 — `rust-toolchain.toml` file (recommended):**
```toml
# rust-toolchain.toml (in your project root)
[toolchain]
channel = "1.78.0"           # exact version
# or
channel = "nightly-2024-01-15"

components = ["rustfmt", "clippy", "rust-analyzer"]
targets = ["wasm32-unknown-unknown"]
```
rustup reads this file automatically when you `cd` into the project.

**Option 2 — command line override:**
```bash
# set override for current directory
rustup override set nightly
rustup override set 1.75.0

# remove override
rustup override unset

# see all active overrides
rustup override list
```

---

## E) Profiles — Control What Gets Installed

Profiles are presets that control how many components get installed:

| Profile | What it includes |
|---------|-----------------|
| `minimal` | rustc, cargo, rust-std only — nothing else |
| `default` | minimal + rustfmt + clippy + rust-docs |
| `complete` | everything available |

```bash
# set profile before installing
rustup set profile minimal
rustup set profile default
rustup set profile complete

# check current profile
rustup show
```

---

## F) Other Useful Commands

```bash
# show everything about your current setup
rustup show

# show active toolchain for current directory
rustup show active-toolchain

# run a command with a specific toolchain (without switching default)
rustup run nightly rustc --version
rustup run 1.78.0 cargo build

# +toolchain shorthand (same as rustup run)
cargo +nightly build
rustc +stable --version

# check for updates without installing
rustup check

# install rustup itself (fresh machine)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# uninstall everything (rustup + all toolchains)
rustup self uninstall

# update rustup itself
rustup self update
```

---

## The Full Control Map

```
rustup CONTROL
│
├── A) TOOLCHAINS
│     ├── install stable/beta/nightly   → get a channel
│     ├── install 1.x.x                 → get exact version
│     ├── default <name>                → set global default
│     ├── update                        → upgrade all
│     └── toolchain uninstall           → remove
│
├── B) COMPONENTS
│     ├── component add <name>          → install a tool
│     ├── component remove <name>       → uninstall a tool
│     └── component list                → see what's installed
│
├── C) TARGETS
│     ├── target add <triple>           → enable cross-compilation
│     ├── target remove <triple>        → remove it
│     └── target list                   → see all available
│
├── D) OVERRIDES
│     ├── rust-toolchain.toml           → pin version per project
│     ├── override set <toolchain>      → pin current directory
│     └── override unset                → remove pin
│
├── E) PROFILES
│     ├── set profile minimal           → compiler only
│     ├── set profile default           → compiler + common tools
│     └── set profile complete          → install everything
│
└── F) SELF
      ├── self update                   → update rustup itself
      ├── self uninstall                → remove everything
      ├── show                          → inspect current setup
      └── run <toolchain> <cmd>         → run with specific version
```

---

## Where rustup Stores Everything

```
~/.rustup/                          ← rustup's home
│
├── toolchains/
│     ├── stable-x86_64-unknown-linux-gnu/
│     │     ├── bin/rustc
│     │     ├── bin/cargo
│     │     └── lib/rustlib/...
│     └── nightly-x86_64-unknown-linux-gnu/
│
├── downloads/                      ← cached downloads
└── tmp/                            ← temp during install

~/.cargo/                           ← cargo's home
│
├── bin/                            ← rustup SHIMS live here
│     ├── rustc      → shim → rustup → actual rustc
│     ├── cargo      → shim → rustup → actual cargo
│     └── rustfmt    → shim → rustup → actual rustfmt
│
└── registry/                       ← downloaded crates cache
```

---

## rustup vs cargo — Key Difference

| | `rustup` | `cargo` |
|---|---------|---------|
| Purpose | Manages Rust itself | Manages your project |
| Installs | rustc, cargo, tools | crates (libraries) |
| Config file | `rust-toolchain.toml` | `Cargo.toml` |
| Registry | rust-lang.org | crates.io |
| Think of it as | "app store for Rust tools" | "npm/pip for Rust libs" |

---

Last thing — rustup uses **target triples** to identify platforms.
A triple looks like: `<arch>-<vendor>-<os>-<abi>`
Example: `x86_64-unknown-linux-gnu`
- `x86_64` → chip architecture (64-bit Intel/AMD)
- `unknown` → vendor (generic)
- `linux` → operating system
- `gnu` → C library (glibc)

Run `rustup target list` to see all ~100+ platforms Rust can compile for.