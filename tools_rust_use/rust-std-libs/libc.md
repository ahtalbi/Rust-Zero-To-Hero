# libc

libc is a C library that libstd and liballoc depend on.
It is not a Rust library — it is written in C.
It comes installed by default on most operating systems.

It is the bridge between Rust and the OS kernel —
because every OS (Linux, Windows, Mac) was built in C
and only speaks C natively.

## What libc provides:

### Memory:
malloc   → allocate memory on heap
realloc  → resize allocated memory
calloc   → allocate + zero it out
free     → free memory

### Other:
files    → open, read, write, close
process  → fork, exec, exit
network  → socket, connect, send, recv
time     → get current time
threads  → create and manage threads

## How it connects:
libstd / liballoc
      ↓
libc (C functions)
      ↓
OS System Calls
      ↓
OS Kernel
      ↓
Hardware