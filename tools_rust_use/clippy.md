# clippy

clippy is Rust's official linter — it analyzes your code and warns about
common mistakes, bad habits, slow code, and unidiomatic patterns.

It has 600+ rules organized into categories.
Most developers only know 10 of them.

---

## How clippy works under the hood:

```
your code
    ↓
rustc parses it into AST (Abstract Syntax Tree)
    ↓
clippy reads the AST
    ↓
checks against 600+ rules
    ↓
reports warnings or errors
```

---

## The 3 levels of control:

| Level | What it does |
|-------|-------------|
| `warn` | shows a warning — code still compiles |
| `deny` | treats warning as ERROR — blocks compilation |
| `allow` | silences a specific rule you don't want |

---

## All categories:

| Category | What it checks | Default |
|----------|---------------|---------|
| `clippy::all` | all base rules combined | warn |
| `clippy::correctness` | code that is WRONG | always on |
| `clippy::style` | code that works but looks bad | warn |
| `clippy::complexity` | code that is too complicated | warn |
| `clippy::performance` | code that is slow unnecessarily | warn |
| `clippy::pedantic` | very strict rules | off |
| `clippy::nursery` | experimental rules | off |
| `clippy::cargo` | checks your Cargo.toml | off |

---

## What each category catches:

### `clippy::correctness` — WRONG code:
```rust
// bad
let x = 1.0 / 0.0;         // division by zero
let v: Vec<i32> = vec![];
v.iter().filter(|x| true);  // filter that does nothing
```

### `clippy::style` — BAD style:
```rust
// bad                        // good
if x == true { }          →   if x { }
return x;                 →   x
let _ = vec.iter().count() →  vec.len()
```

### `clippy::complexity` — TOO COMPLEX:
```rust
// bad                        // good
if x { true } else { false } → x
match x { true => 1, false => 0 } → x as i32
```

### `clippy::performance` — SLOW code:
```rust
// bad                        // good
vec.iter().collect::<Vec<_>>()  → unnecessary collect
"hello".to_string()             → use &str when possible
clone() when not needed         → remove it
```

### `clippy::pedantic` — VERY STRICT:
```rust
// catches things like:
// - missing docs on public functions
// - implicit integer casting
// - must use Result / Option
```

### `clippy::nursery` — EXPERIMENTAL:
```rust
// new rules still being tested
// may change or be removed
// use when you want maximum strictness
```

### `clippy::cargo` — CARGO.TOML checks:
```toml
# catches things like:
# - missing license field
# - missing description
# - duplicate dependencies
```

---

## How to set up — in main.rs (top of file):

### Starter setup (recommended for beginners):
```rust
#![deny(clippy::all)]           // all base rules — errors not warnings
#![deny(clippy::correctness)]   // no wrong code — most important
#![deny(clippy::style)]         // no bad style
#![deny(clippy::complexity)]    // no unnecessary complex code
#![deny(clippy::performance)]   // no slow code
```

### Full setup (when you are comfortable):
```rust
#![deny(clippy::all)]
#![deny(clippy::correctness)]
#![deny(clippy::style)]
#![deny(clippy::complexity)]
#![deny(clippy::performance)]
#![deny(clippy::pedantic)]      // very strict
#![deny(clippy::nursery)]       // experimental
#![deny(clippy::cargo)]         // check Cargo.toml
```

### Silence a specific rule you disagree with:
```rust
#![allow(clippy::needless_return)]   // allow this specific rule
#![allow(clippy::must_use_candidate)] // allow this specific rule
```

---

## How to set up — in Cargo.toml (affects whole project):

### Starter setup:
```toml
[lints.clippy]
all = "deny"
correctness = "deny"
style = "deny"
complexity = "deny"
performance = "deny"
```

### Full setup:
```toml
[lints.clippy]
all = "deny"
correctness = "deny"
style = "deny"
complexity = "deny"
performance = "deny"
pedantic = "deny"
nursery = "deny"
cargo = "deny"
```

---

## How to run clippy:

```bash
# basic run
cargo clippy

# run with all rules
cargo clippy -- -W clippy::all -W clippy::pedantic -W clippy::nursery

# auto fix what clippy can fix automatically
cargo clippy --fix

# auto fix with all rules
cargo clippy --fix -- -W clippy::all -W clippy::pedantic
```

---

## Difference between warn and deny:

```bash
# warn — code compiles but shows warning
cargo clippy
warning: use `x` instead of `x == true`

# deny — code does NOT compile
cargo clippy
error: use `x` instead of `x == true`
```

---

## Suggested learning path:

```
Week 1-2:
#![deny(clippy::all)]
#![deny(clippy::correctness)]
#![deny(clippy::style)]

Week 3-4: add
#![deny(clippy::complexity)]
#![deny(clippy::performance)]

When comfortable: add
#![deny(clippy::pedantic)]
#![deny(clippy::nursery)]
#![deny(clippy::cargo)]
```

---

## One line:
clippy is Rust's official code analyzer with 600+ rules —
it catches wrong code, bad style, slow code, and complexity
before it becomes a problem. ✅

---
