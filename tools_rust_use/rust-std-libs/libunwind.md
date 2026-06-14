# libunwind

libunwind is the library that cleans up the stack after a panic happens.
It is the bridge between libpanic and the OS.
 
It is only used when:
- you have `std` (PC / server)
- panic behavior is set to `unwind` (default on PC)
If you use `#![no_std]` (chip / no OS) → libunwind is NOT available.
You handle panic yourself with `loop {}` or `abort`.
 
---
 
## How it connects to libpanic:
 
```
panic triggers (libpanic)
      ↓
libunwind takes over
      ↓
walks backwards through stack
      ↓
cleans every function frame
      ↓
program stops cleanly
```
 
---
 
## What "unwinding the stack" means:
 
```
main()               ← 4th: cleaned by libunwind
  └── a()            ← 3rd: cleaned by libunwind
       └── b()       ← 2nd: cleaned by libunwind
            └── c()  ← 1st: panic starts here
```
 
libunwind goes from bottom to top cleaning everything:
- variables
- memory
- open files
- anything allocated
---
 
## Who handles cleanup:
 
| Behavior | Who handles it |
|----------|---------------|
| `unwind` | libunwind ✅ |
| `abort` | OS ✅ |
| `loop {}` on chip | nobody — CPU freezes |
 
---
 
## When libunwind is used:
 
| Situation | libunwind? |
|-----------|-----------|
| PC with std | ✅ YES — default |
| chip with #![no_std] | ❌ NO — no OS exists |
| panic = "abort" | ❌ NO — OS handles it |
| panic = "unwind" | ✅ YES |
 
---
 
## unwind vs abort:
 
```
unwind (libunwind):         abort:
panic triggers              panic triggers
      ↓                           ↓
clean every function        stop IMMEDIATELY
      ↓                           ↓
free all memory             OS cleans up
      ↓                           ↓
stop cleanly ✅             faster but no Rust cleanup 💥
```
 
## Why not always use unwind:
 
| | unwind | abort |
|--|--------|-------|
| Memory cleanup | ✅ full | ❌ OS only |
| Speed | slower | faster |
| Binary size | bigger | smaller |
| On chip | ❌ no OS | ✅ perfect |
| On PC | ✅ default | optional |
 
---
 
## Simple rule:
 
```
have OS?
  ├── YES → unwind available  → libunwind cleans up
  └── NO  → unwind impossible → loop {} or abort only
```
 
---
 
## One line:
libunwind walks backwards through your stack and cleans 
everything up after a panic — but only when an OS exists. ✅
 
---
 
## Source to read later:
https://doc.rust-lang.org/nomicon/unwinding.html