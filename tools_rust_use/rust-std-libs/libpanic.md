# libpanic

libpanic is the Rust library that handles when something goes WRONG in your program.

---

## Why Rust needs it:

In C — no protection:
```c
int arr[3] = {1, 2, 3};
arr[5] = 99;  // ← accesses garbage memory 💀 no error!
```

In Rust — libpanic protects you:
```rust
let arr = [1, 2, 3];
arr[5];  // ← libpanic triggers immediately ✅
```

### The logic behind it:
```rust
// what you write:
arr[5]

// what Rust actually does automatically:
if 5 >= arr.len() {
    panic!("index out of bounds");
}
arr[5]  // ← only runs if index is safe
```

---

## What triggers libpanic:
- accessing array index out of bounds
- dividing by zero
- calling `.unwrap()` on `None` or `Err`
- explicitly calling `panic!()` yourself
- integer overflow in debug mode

---

## The 2 default behaviors:

```
panic triggers
      ↓
   2 choices:
   ├── unwind  ← default on PC
   └── abort   ← default on chip
```

### 1. `unwind` — default on PC:
```
panic triggers
      ↓
Rust walks back through the stack
      ↓
cleans up every variable, every memory
      ↓
then stops the program cleanly
```
Like leaving a room and cleaning everything before you close the door. 🚪

### 2. `abort` — default on chip:
```
panic triggers
      ↓
stops IMMEDIATELY
      ↓
no cleanup at all
      ↓
OS cleans up (on PC) or endless loop (on chip)
```
Like leaving a room and slamming the door without cleaning. 💥

---

## What libpanic does when triggered:

### On OS (PC / server):
1. stops the program immediately
2. prints error message + where it happened
3. OS cleans up memory and resources
```
thread 'main' panicked at 'index out of bounds', src/main.rs:5
```

### On chip (no OS):
1. no OS to stop the program
2. runs endless loop → freezes CPU
3. this is called "halt"
4. stops CPU from accessing garbage memory

---

## How to control panic behavior:

### In `Cargo.toml` — change for whole project:
```toml
[profile.release]
panic = "unwind"  # ← default on PC

[profile.release]
panic = "abort"   # ← default on chip
```

### In code — fully custom panic handler:
```rust
#[panic_handler]
fn panic(info: &PanicInfo) -> ! {
    // option 1 — endless loop (halt)
    loop {}

    // option 2 — log the error somewhere
    log::error!("{}", info);
    loop {}

    // option 3 — restart the chip
    system::reset();
}
```

---

## All panic behaviors:

| Behavior | What it does | When to use |
|----------|-------------|-------------|
| `unwind` | clean up stack then stop | PC / server |
| `abort` | stop immediately no cleanup | chip / OS dev |
| `loop {}` | freeze CPU endless loop | chip no OS |
| `reset` | restart the chip | embedded systems |
| `log + loop` | save error then freeze | chip with logging |

---

## libpanic vs C:

| | C | Rust |
|--|---|------|
| Out of bounds | accesses garbage memory 💀 | panic triggers ✅ |
| Divide by zero | undefined behavior 💀 | panic triggers ✅ |
| Null pointer | crash or garbage 💀 | doesn't exist in Rust ✅ |

---

## Full control map:

```
panic triggers
      │
      ├── WHERE?
      │     ├── PC      → unwind (default)
      │     └── chip    → abort / loop (default)
      │
      ├── HOW TO CHANGE?
      │     ├── Cargo.toml → panic = "abort" / "unwind"
      │     └── #[panic_handler] → fully custom behavior
      │
      └── WHAT TO DO?
            ├── unwind  → clean up and stop
            ├── abort   → stop immediately
            ├── loop {} → freeze CPU
            ├── reset   → restart chip
            └── log     → save error then stop
```

---

## One line:
libpanic is Rust's safety net — when something goes wrong
it stops everything before bad things happen to memory. ✅

---

## Source to read later:
https://doc.rust-lang.org/nomicon/panic-handler.html