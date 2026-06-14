# rust-std is (The Rust Standard Library).

TIP: in every thing we use to compile or need in the language we are learning we need to understand is it customizable or no and how to controle it, and how it work for sure.

When you install Rust, it comes by default.
It contains built-in modules with types, functions 
and tools you can use in any Rust program.

## rust-std modules
`std` is organized into **modules** (sections), each responsible for something:
 
| Module | What it does |
|--------|-------------|
| `std::io` | Input / Output (printing, reading) |
| `std::fs` | File system (read/write files) |
| `std::collections` | Data structures (HashMap, Vec...) |
| `std::string` | Text handling |
| `std::net` | Networking (internet connections) |
| `std::thread` | Running things in parallel |
| `std::time` | Time and dates |
| `std::math` | Math operations |
| `std::env` | System environment variables |
| `std::os` | OS-specific features |
| `std::path` | Working with file paths |
| `std::process` | Run & control system processes |
| `std::sync` | Shared data between threads safely |
| `std::cell` | Interior mutability |
| `std::fmt` | Text formatting |
| `std::error` | Error handling traits |
| `std::iter` | Iterators (looping over data) |
| `std::slice` | Working with slices of data |
| `std::vec` | The Vec type specifically |
| `std::hash` | Hashing data |
| `std::cmp` | Comparing values |
| `std::clone` | Cloning/copying values |
| `std::convert` | Converting between types |
| `std::ops` | Operator overloading (+, -, *, ...) |
| `std::ptr` | Raw pointers (advanced/unsafe) |
| `std::mem` | Memory manipulation |

# Controlling `rust-std`
 
## A) Configure / Customize std
 
**1 — Remove it completely:**
```rust
#![no_std]  // remove entire std
```
 
**2 — Replace only parts behavior:**
```rust
// replace the default allocator (memory manager)
#[global_allocator]
static A: MyAllocator = MyAllocator;
```
 
**3 — Replace panic behavior:**
```rust
// control what happens when your program crashes
#[panic_handler]
fn panic(info: &PanicInfo) -> ! {
    loop {} // your custom crash behavior
}
```
 
---
 
## B) Install / Update / Remove via `rustup`
 
```bash
# install rust-std for your current machine
rustup component add rust-std
 
# install rust-std for a DIFFERENT machine/chip
rustup target add x86_64-unknown-linux-gnu
 
# remove rust-std
rustup component remove rust-std
 
# update rust-std to latest version
rustup update
 
# see all installed components
rustup component list
```
 
---
 
## C) Replace Parts With Your Own Implementation
 
**1 — Replace memory allocator:**
```rust
// use a custom/lighter allocator instead of std's default
use my_custom_allocator::MyAllocator;
 
#[global_allocator]
static GLOBAL: MyAllocator = MyAllocator::new();
```
 
**2 — Replace std with a lighter alternative:**
```bash
# community built alternatives to std
hashbrown      # replaces std::HashMap
smol           # replaces std::thread / async
no-std-compat  # bridge between std and no_std
```
 
**3 — Build your OWN std (extreme case):**
```bash
# Rust allows you to compile a custom version of std
cargo build -Z build-std=core,alloc,std
```
 
---
 
## The Full Control Map
 
```
rust-std CONTROL
│
├── A) CONFIGURE
│     ├── #![no_std]          → remove everything
│     ├── #[global_allocator] → replace memory manager
│     └── #[panic_handler]    → replace crash behavior
│
├── B) MANAGE (rustup)
│     ├── add                 → install
│     ├── remove              → uninstall
│     ├── update              → upgrade
│     └── target add          → install for different chip/OS
│
└── C) REPLACE
      ├── custom allocator    → swap memory management
      ├── community crates    → swap individual modules
      └── build-std           → compile your own version
```

last thing i almost forgot about rust is inside it there is some thing that called libcore and liballoc.
you can check theme in folder rust-std-libs for more infos.