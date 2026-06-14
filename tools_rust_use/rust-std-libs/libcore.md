# libcore

libcore is the most fundamental built-in library in Rust.
It is the foundation that everything else (liballoc, libstd) is built on top of.

## Key rule:
libcore needs NOTHING to run:
- no OS
- no memory manager
- no heap
It only needs a CPU — that's it.

## How it connects to hardware:
libcore does not talk to hardware directly.
Rust compiler turns libcore code into CPU instructions:

your code → Rust compiler → CPU instructions → CPU executes directly

## What it provides:

### Types:
- integers    → i8, i16, i32, i64, i128, u8, u16, u32, u64, u128
- floats      → f32, f64
- bool        → true / false
- char        → single character
- str         → string slice (not growable — that's liballoc)
- tuple       → (i32, bool)
- array       → [i32; 5] (fixed size — not growable)

### Operations:
- math        → + - * /
- bitwise     → & | ^ << >>
- comparison  → == != < > <= >=

### Core concepts:
- Option      → value that may or may not exist
- Result      → success or failure
- Iterator    → looping over data
- panic!      → crash the program

## How to use it alone (no OS / chip programming):
#![no_std]  ← removes std, you only get libcore

## When you use it:
- Normal code    → you use it without knowing, std includes it
- Chip/no OS     → you use it directly with #![no_std]
- Building OS    → only thing available at the start

## What it does NOT have:
- No String      → that's liballoc (needs heap)
- No Vec         → that's liballoc (needs heap)
- No println!    → that's libstd (needs OS)
- No files       → that's libstd (needs OS)
- No network     → that's libstd (needs OS)

## Layers:
libcore         ← needs nothing
   ↑
liballoc        ← needs memory manager
   ↑
libstd          ← needs OS