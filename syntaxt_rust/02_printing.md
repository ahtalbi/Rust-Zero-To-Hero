# Printing

In Rust there is not just one way to print — there are several, each built for a different purpose.
This file covers everything: the macros, the format flags, and when to use each one.

---

## 1 — Standard Output (normal printing)

```rust
print!("hello");          // print to terminal, NO newline at end
println!("hello");        // print to terminal, WITH newline at end
```

```
// output of print!
hello▌   ← cursor stays on same line

// output of println!
hello
▌        ← cursor moves to next line
```

---

## 2 — Standard Error (printing errors)

```rust
eprint!("something went wrong");    // print to stderr, no newline
eprintln!("something went wrong");  // print to stderr, with newline
```

**Why two separate streams?**

Your terminal has two output channels:

```
your program
    │
    ├── stdout  → normal output   → print! / println!
    └── stderr  → errors          → eprint! / eprintln!
```

They look the same in your terminal by default, but tools like pipes and redirects treat them differently:

```bash
cargo run > output.txt        # stdout goes to file, errors still show in terminal
cargo run 2> errors.txt       # stderr goes to file, normal output shows in terminal
cargo run > out.txt 2> err.txt # both separated into different files
```

Rule: **never mix errors into stdout** — always use `eprint!` for errors.

---

## 3 — Format Without Printing (build a String)

```rust
let name = "ali";
let age = 25;

let s = format!("name: {} age: {}", name, age);
// s is now a String → "name: ali age: 25"
// nothing printed yet
```

Use `format!` when you want to:
- build a string to store in a variable
- pass a string to a function
- combine strings before printing or saving

```rust
// example: build then print
let message = format!("welcome back, {}!", name);
println!("{}", message);

// example: build then save to file
std::fs::write("log.txt", format!("user {} logged in", name)).unwrap();
```

---

## 4 — Write to Anything (files, buffers, sockets)

```rust
use std::io::Write;

write!(destination, "hello");      // no newline
writeln!(destination, "hello");    // with newline
```

`write!` and `writeln!` are the most flexible — they work on anything that implements the `Write` trait:

```rust
use std::io::Write;
use std::fs::File;

// write to a file
let mut file = File::create("output.txt").unwrap();
writeln!(file, "hello from rust!");

// write to a buffer in memory
let mut buffer = Vec::new();
writeln!(buffer, "hello");

// write to stderr manually
writeln!(std::io::stderr(), "error happened");
```

You will use these when you work with files, network sockets, or building your own output systems.

---

## 5 — Logging (for real applications)

The standard library has no built-in logger.
The Rust ecosystem has a standard logging interface via the `log` crate:

```toml
# Cargo.toml
[dependencies]
log = "0.4"
env_logger = "0.11"   # the backend that actually prints the logs
```

```rust
use log::{info, warn, error, debug, trace};

fn main() {
    env_logger::init();  // initialize the logger

    info!("server started on port 8080");
    warn!("memory usage is high");
    error!("database connection failed");
    debug!("request payload: {:?}", payload);
    trace!("entering function process_request");
}
```

**Log levels from most to least severe:**

```
error  → something broke, needs immediate attention
warn   → something suspicious, not broken yet
info   → normal events worth knowing about
debug  → detailed info for development
trace  → extremely detailed, every small step
```

```bash
# control what level shows at runtime — no recompile needed
RUST_LOG=info cargo run        # show info and above
RUST_LOG=debug cargo run       # show debug and above
RUST_LOG=error cargo run       # show errors only
```

---

## 6 — The `{}` Format System

This is where Rust printing gets powerful.
Every `{}` inside a format string is a **placeholder** with optional formatting instructions.

### Basic placeholders

```rust
println!("{}", value);      // Display      → clean, human-readable
println!("{:?}", value);    // Debug        → raw developer view
println!("{:#?}", value);   // Pretty Debug → debug but nicely indented
println!("{:p}", &value);   // Pointer      → memory address
```

**Display vs Debug:**

```rust
let v = vec![1, 2, 3];

println!("{}", v);     // ERROR — Vec does not implement Display
println!("{:?}", v);   // OK    → "[1, 2, 3]"
println!("{:#?}", v);  // OK    → 
                       // [
                       //     1,
                       //     2,
                       //     3,
                       // ]
```

Rule: `{}` needs `Display` — not everything has it. `{:?}` needs `Debug` — almost everything has it.

---

### Number formats

```rust
println!("{:b}", 42);       // binary           → "101010"
println!("{:o}", 42);       // octal            → "52"
println!("{:x}", 255);      // hex lowercase    → "ff"
println!("{:X}", 255);      // hex uppercase    → "FF"
println!("{:e}", 1000000.0);// scientific lower → "1e6"
println!("{:E}", 1000000.0);// scientific upper → "1E6"
```

With `#` prefix — shows the format type explicitly:

```rust
println!("{:#b}", 10);    // "0b1010"   ← you know it's binary
println!("{:#x}", 255);   // "0xff"     ← you know it's hex
println!("{:#o}", 8);     // "0o10"     ← you know it's octal
```

---

### Precision (decimal places)

```rust
println!("{:.2}", 3.14159);     // "3.14"
println!("{:.4}", 3.14159);     // "3.1416"
println!("{:.0}", 3.7);         // "4"  ← rounds

// real use: money
let price = 19.9999;
println!("${:.2}", price);      // "$20.00"

// real use: measurements
let temp = 36.6666;
println!("{:.1}°C", temp);      // "36.7°C"
```

---

### Width and alignment

```rust
println!("{:10}", "hi");        // "hi        "  → width 10, left aligned (strings default)
println!("{:<10}", "hi");       // "hi        "  → explicitly left
println!("{:>10}", "hi");       // "        hi"  → right aligned
println!("{:^10}", "hi");       // "    hi    "  → centered
```

With fill character:

```rust
println!("{:*<10}", "hi");      // "hi********"  → left,  fill with *
println!("{:*>10}", "hi");      // "********hi"  → right, fill with *
println!("{:*^10}", "hi");      // "****hi****"  → center, fill with *
println!("{:-^20}", "title");   // "-------title--------"
```

Real use — building a clean table:

```rust
println!("{:<15} {:>10} {:>10}", "name",    "score", "rank");
println!("{:<15} {:>10} {:>10}", "ali",     1500,    1);
println!("{:<15} {:>10} {:>10}", "mohammed",900,     2);
println!("{:<15} {:>10} {:>10}", "bob",     750,     3);

// output:
// name                score       rank
// ali                  1500          1
// mohammed              900          2
// bob                   750          3
```

---

### Zero padding

```rust
println!("{:06}", 42);          // "000042"
println!("{:08.2}", 3.14);      // "00003.14"

// real use: timestamps
let h = 9; let m = 5; let s = 3;
println!("{:02}:{:02}:{:02}", h, m, s);   // "09:05:03"

// real use: fixed-length IDs
let id = 7;
println!("ID-{:06}", id);       // "ID-000007"
```

---

### Sign

```rust
println!("{:+}", 42);           // "+42"
println!("{:+}", -42);          // "-42"

// real use: showing changes
let change = 150;
let loss = -30;
println!("today: {:+}", change);   // "today: +150"
println!("today: {:+}", loss);     // "today: -30"
```

---

### Positional and named arguments

```rust
// positional — reference by index
println!("{0} + {0} = {1}", 5, 10);
// "5 + 5 = 10"

// reuse without repeating
println!("{0} is {1} and {0} likes {2}", "ali", 25, "rust");
// "ali is 25 and ali likes rust"

// named arguments
println!("{name} scored {score} points", name="ali", score=150);

// named from variables (Rust 2021+)
let name = "ali";
let score = 150;
println!("{name} scored {score} points");   // no need for , name=name
```

---

## The Full Format String Anatomy

```
{ argument : fill align sign # 0 width .precision type }

  argument  → which value to use (index or name). empty = next in order
  :         → "formatting starts here"
  fill      → character to pad with (any char: 0 * - space...)
  align     → < left   > right   ^ center
  sign      → + always show sign
  #         → alternate form (0b 0x 0o prefixes)
  0         → pad with zeros
  width     → minimum total width
  .precision→ decimal places for floats
  type      → b o x X e E ? #? p
```

Examples reading left to right:

```rust
{:0>10.2}   → fill=0  align=>  width=10  precision=.2
{:*^20}     → fill=*  align=^  width=20
{:#010x}    → alt=#   zero=0   width=10  type=x
{:+.3}      → sign=+  precision=.3
```

---

## Comparison to printf (C / Go)

| printf | Rust | What |
|--------|------|------|
| `%d` / `%i` | `{}` | integer |
| `%f` | `{}` | float |
| `%s` | `{}` | string |
| `%c` | `{}` | char |
| `%p` | `{:p}` | pointer |
| `%b` | `{:b}` | binary |
| `%o` | `{:o}` | octal |
| `%x` | `{:x}` | hex lowercase |
| `%X` | `{:X}` | hex uppercase |
| `%e` | `{:e}` | scientific notation |
| `%10d` | `{:10}` | width |
| `%-10d` | `{:<10}` | left align |
| `%010d` | `{:010}` | zero pad |
| `%.2f` | `{:.2}` | precision |
| `%10.2f` | `{:10.2}` | width + precision |

**Key difference:** in C/Go you use `%d` for int, `%f` for float, `%s` for string.
In Rust `{}` works for any type — Rust knows the type at compile time so you never get a type mismatch.

---

## The Full Picture — All Macros

| Macro | Goes to | Newline | Returns | Use for |
|-------|---------|---------|---------|---------|
| `print!` | stdout | no | nothing | normal output |
| `println!` | stdout | yes | nothing | normal output |
| `eprint!` | stderr | no | nothing | errors |
| `eprintln!` | stderr | yes | nothing | errors |
| `format!` | — | — | String | build a string |
| `write!` | any writer | no | Result | files, buffers, sockets |
| `writeln!` | any writer | yes | Result | files, buffers, sockets |
| `log::info!` | logger | — | nothing | app info events |
| `log::warn!` | logger | — | nothing | app warnings |
| `log::error!` | logger | — | nothing | app errors |
| `log::debug!` | logger | — | nothing | development details |
| `log::trace!` | logger | — | nothing | fine-grained tracing |

---

## When You Will Actually Use Each Format Option

| Option | You need it when |
|--------|-----------------|
| `{}` | printing anything — default |
| `{:?}` | type has no Display, or you are debugging |
| `{:#?}` | inspecting complex nested data |
| `{:.2}` | floats, money, measurements |
| `{:b}` `{:x}` `{:o}` | bits, memory, colors, low-level work |
| `{:#x}` `{:#b}` | same but want the 0x / 0b prefix visible |
| `{:<}` `{:>}` `{:^}` | tables, CLI output, aligned reports |
| `{:0>N}` | IDs, timestamps, fixed-length codes |
| `{:+}` | showing positive/negative changes |
| `{0}` `{name}` | reusing values, long format strings |
| `format!` | building a string to store or pass around |
| `eprint!` | anything that is an error |
| `write!` | output to files, buffers, or sockets |
| `log::*` | production applications, not just scripts |

---

> For the complete list of every format option:
> https://doc.rust-lang.org/std/fmt