# Installing RUST
## first of all you run this command in your terminal (cmd)
```curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh```

### me options i chose personally 
### (custom > default > )

### When the installer runs, you’ll be offered options.
 — Proceed with the standard installation (recommended).
This is the default — just press Enter to accept it.
Note: you can skip ahead to file 03_ if you only want to follow examples and projects.

2 — Choose custom installation if you want more control.
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

2 - Default toolchain? (stable/beta/nightly/none) [stable]  
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

| Channel   | What it is                              | When to use it                              | Stability               |
|-----------|------------------------------------------|---------------------------------------------|-------------------------|
| `stable`  | The current released, production-ready Rust | Normal development, production projects      | Very stable, recommended |
| `beta`    | Preview of the next stable release        | CI testing, preview upcoming changes         | Stable, but previewing   |
| `nightly` | Builds from the latest code every night   | Experimenting with unstable features         | Less stable, may break   |
| `none`    | No default toolchain set                  | You always specify per-project or per-command | N/A (no default chosen)  |

for more informations about ``(rustc, cargo, rustdoc, rustfmt, clippy)`` acces folder tools_rust_use.

