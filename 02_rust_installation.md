# Installing RUST

## first of all you run this command in your terminal (cmd)

`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`

### the options i will chose personally if i am learning.
### (custom > default > stable > complete)
for me the command is
`curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --default-toolchain stable --profile complete`
```
   default host triple:                     x86_64-unknown-linux-gnu
     default toolchain: stable
               profile: complete
  modify PATH variable: yes
```

### When the installer runs, you’ll be offered options.

— Proceed with the standard installation (recommended).
This is the default — just press Enter to accept it.
Note: you can skip ahead to file 03\_ if you only want to follow examples and projects.

— Choose custom installation if you want more control.
Pick custom if you want to change installation details (toolchain channel, components like rustfmt or clippy, default path). This is useful if you prefer a non-standard setup or want to learn what each option does.

If you don’t care about installation details and just want Rust working quickly, choose the standard option. If you’re like me and want to learn the “why” behind choices, choose custom and read each prompt before confirming.

Custom install: "Default host triple? [x86_64-unknown-linux-gnu]"
You may see a prompt like:
Default host triple? [x86_64-unknown-linux-gnu] ?

What it means:

The “host triple” identifies your target platform in the form CPU-VENDOR-OS (for example, x86_64-unknown-linux-gnu).

Rust installs a toolchain built for a specific target. The installer guesses the correct one for your system and shows it as the default.

Accept the default if you’re installing on your machine and you didn’t change your OS or CPU type.

Choose a different triple only if you know you need a different target (for example, cross-compiling for ARM or Windows from Linux).

When to change it:

You are cross-compiling (building for a different architecture or OS).

You have a nonstandard setup and know the correct triple for your environment.

Otherwise, press Enter to accept the default.

- Default toolchain? (stable/beta/nightly/none) [stable]  
what is tool chain ? and what in the v stable || beta || nightly and || ??

## What is a toolchain?

A toolchain is a complete installation of the Rust compiler and its related tools. It includes:

- The compiler (`rustc`)
- The package manager and build tool (`cargo`)
- The documentation generator (`rustdoc`)
- The standard library and supporting tools like `rustfmt` and `clippy`

You can have multiple toolchains (for example, different versions or channels) installed and switch between them with `rustup`.

---

## Default toolchain? (stable/beta/nightly/none) — what each option means

Rust publishes three official channels:

| Channel   | What it is                                  | When to use it                                | Stability                |
| --------- | ------------------------------------------- | --------------------------------------------- | ------------------------ |
| `stable`  | The current released, production-ready Rust | Normal development, production projects       | Very stable, recommended |
| `beta`    | Preview of the next stable release          | CI testing, preview upcoming changes          | Stable, but previewing   |
| `nightly` | Builds from the latest code every night     | Experimenting with unstable features          | Less stable, may break   |
| `none`    | No default toolchain set                    | You always specify per-project or per-command | N/A (no default chosen)  |

for more informations about `(rustc, cargo, rustdoc, rustfmt, clippy)` acces folder tools_rust_use.

- you will be asked Profile (which tools and data to install)? (minimal/default/complete) [default]  
you should ask what each one includes ?


---
 
## Components Table
 
| Component | What It Is | Minimal | Default | Complete |
|-----------|-----------|:-------:|:-------:|:--------:|
| `rustc` | The Rust **compiler** — turns your `.rs` source files into executable programs | ✅ | ✅ | ✅ |
| `cargo` | The Rust **package manager** — builds projects, manages dependencies, runs tests (like `npm` for JS or `pip` for Python) | ✅ | ✅ | ✅ |
| `rust-std` | The Rust **standard library** — built-in types, functions, and utilities every Rust program can use | ✅ | ✅ | ✅ |
| `rustfmt` | **Code formatter** — automatically formats your code to follow Rust style conventions (`cargo fmt`) | ❌ | ✅ | ✅ |
| `clippy` | **Linter** — analyzes your code and warns about common mistakes, bad habits, and unidiomatic patterns (`cargo clippy`) | ❌ | ✅ | ✅ |
| `documentation` | **Offline docs** — the full official Rust documentation available locally without internet access (`rustup doc`) | ❌ | ❌ | ✅ |
| `rust-analyzer` | **Language server** — powers IDE features like autocomplete, go-to-definition, inline errors, and code suggestions in editors like VS Code or NeoVim | ❌ | ❌ | ✅ |
| `sources` | **Standard library source code** — the actual Rust source files; useful for debugging, understanding internals, and advanced IDE features | ❌ | ❌ | ✅ |
 
---

one thing you install also by default is the ``rustup`` — The Rust toolchain installer and version manager that installs, updates, and controls all components above.