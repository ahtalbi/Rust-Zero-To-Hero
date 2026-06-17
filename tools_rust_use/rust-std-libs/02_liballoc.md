# liballoc

Notes: you need to have knolege about hardware and ruststd first read them they are available in this docs.

liballoc is a built-in Rust library that controls 
the HEAP section of RAM only and to manage this the default is libc cause every os have libc installed by default its not used only when we use.

Stack is automatic — controlled by the CPU itself.
Heap is dynamic — needs liballoc to manage it.

## What it gives you (types that live on heap):
- String   → growable text
- Vec      → growable list
- Box      → single value on heap
- HashMap  → key-value store
- Arc      → shared value between threads
- Rc       → shared value single thread

## How it works:
1. You create a String or Vec
2. liballoc asks the global allocator for space in heap
3. Allocator finds space and returns memory address
4. liballoc tracks it
5. When you are done → ownership system frees it automatically

## Who is the global allocator:
- On PC    → OS provides it automatically
- On chip  → YOU build your own allocator

## Why its special vs other languages:
- C   → you manually malloc() and free() → easy to forget = memory leak 💀
- Rust → liballoc + ownership = automatic, no leaks ✅

## Source to read later:
https://www.reddit.com/r/rust/comments/15dsswd/alloc_vs_corealloc/