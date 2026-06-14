# infomrations about cargo the (package manager for rust)

cargo means like some thing shipped example product in hour case its packages dependencies.

What is a crate?
A crate is just a compilation unit — basically one Cargo project with its own Cargo.toml. Every time you do cargo new something you made a crate.
and you can push your crate like npm publish to crates like this 
https://crates.io/


note Cargo.toml is like the files where dependecies in theme
fun fact toml means [Tom's Obvious Minimal Language]

## Commands that i will use for most of time
Daily coding:     cargo check
Run your app:     cargo run
Before commit:    cargo test
Release:          cargo build --release
Add dep:          cargo add serde --features derive
Free disk space:  cargo clean
See errors deep:  cargo build -vv
Explain error:    cargo --explain E0502

## All commands 
### `cargo build`
```bash
cargo build
  -p / --package <name>       # build specific package in workspace
  --workspace                 # build all packages in workspace
  --exclude <name>            # exclude a package from workspace build
  --lib                       # build only the library
  --bin <name>                # build specific binary
  --bins                      # build all binaries
  --example <name>            # build a specific example
  --examples                  # build all examples
  --test <name>               # build specific test target
  --tests                     # build all test targets
  --bench <name>              # build specific benchmark
  --benches                   # build all benchmarks
  --all-targets               # build ALL targets (lib+bins+tests+benches+examples)
  --release                   # optimized build (release profile)
  --profile <name>            # use a custom profile from Cargo.toml
  --features <list>           # enable specific features (comma separated)
  --all-features              # enable ALL features
  --no-default-features       # disable default features
  --target <triple>           # cross-compile to a different target
  --target-dir <dir>          # custom output directory (instead of target/)
  --out-dir <dir>             # copy final artifacts here
  --manifest-path <path>      # path to Cargo.toml (if not in current dir)
  --ignore-rust-version       # ignore rust-version field in Cargo.toml
  --message-format <fmt>      # human | json | json-diagnostic-short | json-render-diagnostics
  --timings[=<fmts>]          # show how long each step took
  -j / --jobs <n>             # number of parallel jobs (default = CPU count)
  --keep-going                # keep building even if one target fails
  --future-incompat-report    # show report of future breaking changes
```
 
> **Output goes to:**
> ```
> target/debug/your-project    ← cargo build
> target/release/your-project  ← cargo build --release
> ```
 
### `cargo check`
```bash
cargo check
  --keep-going                # don't stop at first error
  # accepts all the same flags as cargo build
```
 
### `cargo run`
```bash
cargo run
  --bin <name>                # run a specific binary (if multiple exist)
  --example <name>            # run a specific example
  -- <args>                   # everything after -- is passed to YOUR program
  # also accepts: --release, --profile, --features, --target, -p, etc.
```
 
### `cargo test`
```bash
cargo test
  <test_name>                 # run only tests matching this name
  --lib                       # test only library
  --bin <name>                # test specific binary
  --bins                      # test all binaries
  --test <name>               # run specific integration test file
  --tests                     # run all integration tests
  --bench <name>              # test specific benchmark
  --benches                   # test all benchmarks
  --doc                       # run documentation tests only
  --all-targets               # test all targets
  --no-run                    # compile but don't run tests
  --no-fail-fast              # keep running after a test fails
  -- --nocapture              # show println! output during tests
  -- --test-threads=1         # run tests single-threaded
  -- --ignored                # run only #[ignore]d tests
  -- --include-ignored        # run all tests including ignored ones
  # also accepts: --release, --features, --target, -p, etc.
```
 
### `cargo bench`
```bash
cargo bench
  <bench_name>                # run only benchmarks matching this name
  --no-run                    # compile but don't run
  --bench <name>              # specific benchmark target
  --benches                   # all benchmark targets
  # also accepts: --features, --target, -p, etc.
```
 
### `cargo clean`
```bash
cargo clean
  -p / --package <name>       # clean only specific package
  --release                   # clean only release artifacts
  --profile <name>            # clean specific profile artifacts
  --target <triple>           # clean only for specific target
  --target-dir <dir>          # clean a custom target directory
  --doc                       # clean only documentation artifacts
  -n / --dry-run              # show what would be deleted, don't delete
```
 
### `cargo doc`
```bash
cargo doc
  --open                      # open docs in browser after building
  --no-deps                   # don't build docs for dependencies
  --document-private-items    # include private items in docs
  --lib                       # document only the library
  --bin <name>                # document specific binary
  --bins                      # document all binaries
  # also accepts: --release, --features, --target, -p, etc.
```
 
### `cargo fetch`
```bash
cargo fetch
  --target <triple>           # fetch dependencies for specific target
```
 
### `cargo fix`
```bash
cargo fix
  --edition                   # migrate code to next Rust edition
  --edition-idioms            # apply edition-specific style fixes
  --allow-dirty               # fix even with uncommitted git changes
  --allow-staged              # fix even with staged git changes
  --allow-no-vcs              # fix even without a git repo
  --broken-code               # apply fixes even to broken code
  # also accepts: --bin, --lib, --tests, --features, etc.
```
 
### `cargo rustc`
```bash
cargo rustc
  -- <rustc_flags>            # pass flags directly to rustc
  --crate-type <types>        # override crate type (bin, lib, dylib...)
  --print <info>              # print compiler info and exit
  # also accepts all cargo build flags
```
 
### `cargo rustdoc`
```bash
cargo rustdoc
  -- <rustdoc_flags>          # pass flags directly to rustdoc
  # also accepts all cargo doc flags
```
 
---
 
## 📋 Manifest Commands
 
### `cargo add`
```bash
cargo add <crate>
  --dev                       # add as [dev-dependencies]
  --build                     # add as [build-dependencies]
  --target <triple>           # add as target-specific dependency
  -F / --features <list>      # enable specific features of the added crate
  --no-default-features       # disable default features of added crate
  --optional                  # make it an optional dependency
  --rename <name>             # rename the dependency in Cargo.toml
  --git <url>                 # add from a git repo instead of crates.io
  --branch <branch>           # use specific branch (with --git)
  --tag <tag>                 # use specific tag (with --git)
  --rev <sha>                 # use specific commit (with --git)
  --path <path>               # add from a local path
  --registry <name>           # use a specific registry
  --dry-run                   # show what would change, don't write
```
 
### `cargo remove`
```bash
cargo remove <crate>
  --dev                       # remove from [dev-dependencies]
  --build                     # remove from [build-dependencies]
  --target <triple>           # remove from target-specific section
  --dry-run                   # show what would change, don't write
```
 
### `cargo update`
```bash
cargo update
  <crate>                     # update only a specific crate
  --precise <version>         # update to this exact version
  --recursive                 # update transitive dependencies too
  --dry-run                   # show what would change, don't write
  -w / --workspace            # only update workspace members
```
 
### `cargo tree`
```bash
cargo tree
  -p / --package <name>       # show tree for specific package
  -i / --invert <crate>       # show what depends ON this crate (reverse)
  --prune <crate>             # hide this crate from the tree
  --depth <n>                 # limit tree depth
  --prefix <mode>             # none | indent | depth
  -e / --edges <kinds>        # normal | dev | build | all | features | no-dev
  --duplicates                # show only duplicate packages
  --charset <charset>         # utf8 | ascii
  --target <triple>           # show dependencies for specific target
```
 
### `cargo vendor`
```bash
cargo vendor
  <path>                      # where to put vendored sources (default: vendor/)
  --no-delete                 # don't delete old vendored sources
  --versioned-dirs            # name dirs by crate-version instead of crate
  -s / --sync <toml>          # sync another manifest's dependencies too
```
 
### `cargo metadata`
```bash
cargo metadata
  --no-deps                   # exclude dependency info
  --format-version <n>        # metadata format version (currently 1)
  --filter-platform <triple>  # only include platform-specific deps
```
 
---
 
## 📦 Package Commands
 
### `cargo new`
```bash
cargo new <name>
  --lib                       # create a library (lib.rs) instead of binary
  --bin                       # create a binary (main.rs) — default
  --edition <year>            # 2015 | 2018 | 2021 | 2024
  --name <name>               # package name (if different from directory)
  --vcs <vcs>                 # git | hg | pijul | fossil | none
```
 
### `cargo init`
```bash
cargo init [path]
  --lib                       # initialize as library
  --bin                       # initialize as binary — default
  --edition <year>            # 2015 | 2018 | 2021 | 2024
  --name <name>               # package name
  --vcs <vcs>                 # git | hg | pijul | fossil | none
```
 
### `cargo install`
```bash
cargo install <crate>
  --version <ver>             # install specific version
  --git <url>                 # install from git repo
  --branch / --tag / --rev    # git ref options
  --path <path>               # install from local path
  --list                      # list all installed binaries
  --force                     # reinstall even if already installed
  --bin <name>                # install only specific binary
  --bins                      # install all binaries
  --example <name>            # install specific example
  --root <dir>                # install to a different directory
  --features <list>           # enable features
  --all-features              # enable all features
  --no-default-features       # disable default features
  --target <triple>           # install for specific target
  -j / --jobs <n>             # parallel jobs
```
 
### `cargo uninstall`
```bash
cargo uninstall <crate>
  --root <dir>                # uninstall from specific directory
  --bin <name>                # uninstall only specific binary
```
 
### `cargo search`
```bash
cargo search <query>
  --limit <n>                 # number of results (default 10, max 100)
  --registry <name>           # use named registry
```
 
---
 
## 🚀 Publishing Commands
 
### `cargo login`
```bash
cargo login [token]
  --registry <name>           # login to specific registry
```
 
### `cargo logout`
```bash
cargo logout
  --registry <name>           # logout from specific registry
```
 
### `cargo package`
```bash
cargo package
  -l / --list                 # list files included in the package
  --no-verify                 # don't build to verify it works
  --allow-dirty               # include uncommitted git changes
  --features <list>           # enable features
  --all-features              # enable all features
  --no-default-features       # disable default features
```
 
### `cargo publish`
```bash
cargo publish
  --dry-run                   # do everything except the actual upload
  --no-verify                 # skip building to verify
  --allow-dirty               # publish with uncommitted changes
  --registry <name>           # publish to specific registry
  --token <token>             # API token (instead of stored one)
  --features <list>           # enable features
  --all-features              # all features
  --no-default-features       # no default features
```
 
### `cargo yank`
```bash
cargo yank <crate>
  --version <ver>             # which version to yank (required)
  --undo                      # un-yank a previously yanked version
  --registry <name>           # specific registry
  --token <token>             # API token
```
 
### `cargo owner`
```bash
cargo owner
  -a / --add <user>           # add an owner
  -r / --remove <user>        # remove an owner
  -l / --list                 # list current owners
  --token <token>             # API token
  --registry <name>           # specific registry
```
 
---
 
## 📊 Report Commands
 
```bash
cargo report future-incompatibilities
  --id <id>                   # show report for specific build ID
  -p / --package <name>       # filter by package
```
 
---
 
## ℹ️ General Commands
 
```bash
cargo --version / -V          # print cargo version
cargo --list                  # list ALL installed cargo subcommands
cargo --explain E0004         # explain a rust error code in detail
cargo help <command>          # same as cargo <command> --help
cargo info <crate>            # show info about a crate from crates.io
```
 
---
 
## 🌍 Global Flags (work with ANY command)
 
```bash
-v / --verbose                # verbose output
-vv                           # VERY verbose (shows dependency warnings too)
-q / --quiet                  # suppress all cargo log messages
--color <when>                # auto | always | never
--locked                      # use exact versions from Cargo.lock (great for CI)
--offline                     # no network access at all
--frozen                      # --locked + --offline combined
--config KEY=VALUE            # override a cargo config value inline
+stable / +nightly            # use a specific rustup toolchain
+1.70.0                       # use a specific Rust version
-Z <flag>                     # nightly-only unstable flags
-h / --help                   # help for any command
```
 

---
 
## ✈️ Cross Compilation Targets
 
```bash
cargo build --target x86_64-pc-windows-gnu      # Windows
cargo build --target aarch64-apple-darwin        # Mac M1/M2
cargo build --target wasm32-unknown-unknown      # WebAssembly
cargo build --target i686-unknown-linux-gnu      # Linux 32-bit
```
 
---
 
## 🗺️ Speed Comparison
 
```
FASTEST  →  cargo check          (errors only, no binary)
         →  cargo build          (debug binary)
         →  cargo run            (build + run)
SLOWEST  →  cargo build --release (fully optimized)
```
 
---
 
## 🎉 Fun Facts
 
- 🦀 **Ferris the Crab** is Rust's mascot — crabs have hard shells (safety) and are fast
- 📦 **Cargo** was added in 2014, a year before Rust 1.0 (2015)
- 🚢 The whole theme: you ship **crates** of **cargo** from the **crates.io** port
- 🔒 Rust has been the **#1 most loved language** on Stack Overflow for 9 years straight
- 🐧 The **Linux kernel** added Rust in 2022 — only C had been allowed for 30+ years
- 🏗️ Rust was originally written in **OCaml**, then rewrote itself in Rust in 2011
- ⚡ `cargo fix` can **automatically fix** your compiler warnings — no other language does this built-in