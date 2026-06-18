# Variables in Rust

after i will finish this file i need to solve at least 20 problems in rust easy or mid in rust

> Rust's variable system is one of its most unique features.  
> It enforces memory safety at compile time — no garbage collector, no runtime cost.

---

## ❓ How do you declare a variable — and is there more than one way?

Yes! There are **3 ways** to declare a variable in Rust:

---

### 1️⃣ `let` — the standard way

```rust
let x = 5;               // type inferred as i32
let y: f64 = 3.14;       // explicit type annotation
let name = "Ferris";     // type inferred as &str
```

---

### 2️⃣ `let mut` — mutable variable

By default, **all variables in Rust are immutable**.  
You must explicitly opt into mutability with `mut`:

```rust
let mut score = 0;
score += 10;   // ✅ works
score = 99;    // ✅ works

let lives = 3;
lives = 2;     // ❌ ERROR: cannot assign twice to immutable variable
```

---

### 3️⃣ `const` — compile-time constant

Always immutable, must have an explicit type, computed at **compile time**.  
Can be declared in any scope including global scope.

```rust
const MAX_POINTS: u32 = 100_000;
const PI: f64 = 3.14159265358979;
const APP_NAME: &str = "MyApp";
```

---

### 4️⃣ `static` — global variable

Lives for the **entire duration** of the program.  
Unlike `const`, has a fixed memory address.

```rust
static GREETING: &str = "Hello, world!";
static mut COUNTER: u32 = 0;  // mutable statics are unsafe to access
```

---

### 🔁 Shadowing — redeclaring the same name

Rust lets you **shadow** a variable — declare a new variable with the same name.  
This is different from mutation: you can even change the type!

```rust
let x = 5;
let x = x + 1;       // shadows previous x, now x = 6
let x = x * 2;       // shadows again, now x = 12

let spaces = "   ";       // &str
let spaces = spaces.len(); // usize — type changed! shadowing allows this
```

---

## ❓ What are ALL the types of variables?

Rust has two categories of types: **Scalar** (single value) and **Compound** (multiple values).

---

## 🔢 Scalar Types

### Integer Types

Signed integers can be negative. Unsigned cannot.

| Type | Size | Range |
|------|------|-------|
| `i8` | 8-bit | −128 to 127 |
| `i16` | 16-bit | −32,768 to 32,767 |
| `i32` | 32-bit | −2.1B to 2.1B *(default)* |
| `i64` | 64-bit | very large |
| `i128` | 128-bit | enormous |
| `isize` | arch | pointer-sized (64-bit on modern systems) |
| `u8` | 8-bit | 0 to 255 |
| `u16` | 16-bit | 0 to 65,535 |
| `u32` | 32-bit | 0 to 4.2B |
| `u64` | 64-bit | very large |
| `u128` | 128-bit | enormous |
| `usize` | arch | pointer-sized, used for indexing |

```rust
let a: i8   = -100;
let b: u8   = 255;
let c: i32  = -2_000_000;   // underscores for readability
let d: u64  = 18_446_744_073_709_551_615;
let e: isize = -9999;

// different literal formats
let decimal     = 1_000_000;
let hex         = 0xFF;          // 255
let octal       = 0o77;          // 63
let binary      = 0b1111_0000;   // 240
let byte        = b'A';          // 65  (u8 only)
```

---

### Float Types

```rust
let x: f32 = 3.14;    // 32-bit, less precision
let y: f64 = 3.14159265358979;  // 64-bit, default, more precise

let result = 10.0 / 3.0;   // 3.3333333333333335  (f64 by default)
let rounded = (result * 100.0).round() / 100.0;  // 3.33
```

---

### Boolean

```rust
let is_active: bool = true;
let is_done        = false;      // type inferred

// used in conditions
if is_active && !is_done {
    println!("still running");
}

let val = if is_active { "yes" } else { "no" };
```

---

### Character `char`

Rust's `char` is **4 bytes** — a full Unicode scalar value, not just ASCII.

```rust
let letter: char = 'A';
let emoji         = '🦀';   // yes, this works!
let arabic        = 'ب';
let chinese       = '中';

println!("{} {} {} {}", letter, emoji, arabic, chinese);
// A 🦀 ب 中
```

---

## 📦 Compound Types

### Tuple

Fixed-length, can hold **different types**.  
Access elements by index with `.0`, `.1`, etc.

```rust
let point: (i32, i32) = (10, 20);
let person: (&str, u8, bool) = ("Alice", 30, true);

// destructuring
let (name, age, active) = person;
println!("{} is {} years old", name, age);  // Alice is 30 years old

// index access
println!("{}", point.0);  // 10
println!("{}", point.1);  // 20

// unit tuple — returned by functions with no return value
let nothing: () = ();
```

---

### Array

Fixed-length, **same type** for all elements.  
Stored on the stack — very fast.

```rust
let nums: [i32; 5] = [1, 2, 3, 4, 5];
let zeros = [0; 10];   // [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]

// access by index
println!("{}", nums[0]);   // 1
println!("{}", nums[4]);   // 5
println!("{}", nums.len()); // 5

// iterate
for n in &nums {
    print!("{} ", n);  // 1 2 3 4 5
}
```

---

### Slice `&[T]`

A **view into** an array or vector — no ownership, just a reference.

```rust
let arr = [1, 2, 3, 4, 5];
let slice: &[i32] = &arr[1..4];   // [2, 3, 4]

println!("{:?}", slice);   // [2, 3, 4]
println!("{}", slice.len()); // 3

// string slice
let sentence = "hello world";
let word: &str = &sentence[0..5];  // "hello"
```

---

### String Types

Rust has two main string types:

| Type | Owned? | Mutable? | Where |
|------|--------|----------|-------|
| `&str` | No (borrowed) | No | Stack / binary |
| `String` | Yes | Yes | Heap |

```rust
// &str — string slice, borrowed, immutable
let greeting: &str = "Hello, Ferris!";
let slice = &greeting[0..5];   // "Hello"

// String — owned, heap-allocated, growable
let mut owned = String::from("Hello");
owned.push_str(", world!");
owned.push('!');
println!("{}", owned);   // Hello, world!!

// converting between them
let s: String = greeting.to_string();
let r: &str   = &owned;           // deref coercion

// useful String methods
let upper  = owned.to_uppercase();
let len    = owned.len();
let has    = owned.contains("world");
let replaced = owned.replace("world", "Rust");
println!("{}", replaced);  // Hello, Rust!!
```

---

## 🧱 Struct Types

Structs let you group related data under one name.

### Regular Struct

```rust
struct User {
    username: String,
    email:    String,
    age:      u8,
    active:   bool,
}

let user = User {
    username: String::from("ferris"),
    email:    String::from("ferris@rust.dev"),
    age:      3,
    active:   true,
};

println!("{} ({})", user.username, user.email);
// ferris (ferris@rust.dev)
```

---

### Tuple Struct

```rust
struct Point(f64, f64);
struct Color(u8, u8, u8);

let origin = Point(0.0, 0.0);
let red    = Color(255, 0, 0);

println!("({}, {})", origin.0, origin.1);  // (0, 0)
println!("rgb({}, {}, {})", red.0, red.1, red.2);  // rgb(255, 0, 0)
```

---

### Unit Struct

```rust
struct Marker;   // no fields — used as a type marker or for traits

let m = Marker;
```

---

## 🎭 Enum Types

Enums let a variable be **one of several variants**.  
Much more powerful than enums in other languages — variants can hold data.

### Basic Enum

```rust
enum Direction {
    North,
    South,
    East,
    West,
}

let dir = Direction::North;

match dir {
    Direction::North => println!("Going north"),
    Direction::South => println!("Going south"),
    Direction::East  => println!("Going east"),
    Direction::West  => println!("Going west"),
}
```

---

### Enum with Data

```rust
enum Shape {
    Circle(f64),            // holds radius
    Rectangle(f64, f64),    // holds width and height
    Triangle { base: f64, height: f64 },  // named fields
}

let c = Shape::Circle(3.0);
let r = Shape::Rectangle(4.0, 5.0);
let t = Shape::Triangle { base: 6.0, height: 8.0 };

fn area(shape: &Shape) -> f64 {
    match shape {
        Shape::Circle(r)             => std::f64::consts::PI * r * r,
        Shape::Rectangle(w, h)       => w * h,
        Shape::Triangle { base, height } => 0.5 * base * height,
    }
}

println!("{:.2}", area(&c));  // 28.27
println!("{:.2}", area(&r));  // 20.00
println!("{:.2}", area(&t));  // 24.00
```

---

### Built-in Enums: `Option` and `Result`

These are used **everywhere** in Rust:

```rust
// Option<T> — a value that might or might not exist (replaces null)
let some_number: Option<i32> = Some(42);
let no_number:   Option<i32> = None;

if let Some(n) = some_number {
    println!("Got: {}", n);  // Got: 42
}

let value = some_number.unwrap_or(0);   // 42
let double = some_number.map(|n| n * 2); // Some(84)

// Result<T, E> — either success or an error
let good: Result<i32, &str> = Ok(100);
let bad:  Result<i32, &str> = Err("something went wrong");

match bad {
    Ok(v)  => println!("value: {}", v),
    Err(e) => println!("error: {}", e),  // error: something went wrong
}

// ? operator — propagate errors automatically
fn parse_number(s: &str) -> Result<i32, std::num::ParseIntError> {
    let n = s.trim().parse::<i32>()?;  // returns Err early if it fails
    Ok(n * 2)
}
```

---

## 📚 Collection Types

### Vector `Vec<T>`

Dynamic array — growable, heap-allocated.

```rust
let mut nums: Vec<i32> = Vec::new();
nums.push(1);
nums.push(2);
nums.push(3);

// or with macro
let mut fruits = vec!["apple", "banana", "cherry"];

println!("{:?}", fruits);     // ["apple", "banana", "cherry"]
println!("{}", fruits[0]);    // apple
println!("{}", fruits.len()); // 3

fruits.push("date");
fruits.remove(1);             // removes "banana"
println!("{:?}", fruits);     // ["apple", "cherry", "date"]

// iterate
for f in &fruits {
    println!("- {}", f);
}
```

---

### HashMap `HashMap<K, V>`

Key-value store. Keys must implement `Hash` and `Eq`.

```rust
use std::collections::HashMap;

let mut scores: HashMap<String, u32> = HashMap::new();

scores.insert(String::from("Alice"), 95);
scores.insert(String::from("Bob"),   87);
scores.insert(String::from("Carol"), 92);

// access
println!("{:?}", scores.get("Alice"));  // Some(95)

// insert only if key doesn't exist
scores.entry(String::from("Dave")).or_insert(80);

// iterate
for (name, score) in &scores {
    println!("{}: {}", name, score);
}

// check existence
if scores.contains_key("Bob") {
    println!("Bob is in the map");
}
```

---

### HashSet `HashSet<T>`

Unique values, no duplicates, no guaranteed order.

```rust
use std::collections::HashSet;

let mut set: HashSet<i32> = HashSet::new();
set.insert(1);
set.insert(2);
set.insert(3);
set.insert(2);  // duplicate — ignored

println!("{}", set.len());           // 3
println!("{}", set.contains(&2));    // true

let a: HashSet<i32> = [1, 2, 3].iter().cloned().collect();
let b: HashSet<i32> = [2, 3, 4].iter().cloned().collect();

let union:        Vec<_> = a.union(&b).collect();
let intersection: Vec<_> = a.intersection(&b).collect();
let difference:   Vec<_> = a.difference(&b).collect();
```

---

## 🧠 Special Variable Concepts

### Ownership & Move

When you assign a heap value to another variable, the original is **moved** (no copy):

```rust
let s1 = String::from("hello");
let s2 = s1;          // s1 is MOVED into s2
// println!("{}", s1); // ❌ ERROR: s1 was moved

// to keep both, clone:
let s3 = String::from("world");
let s4 = s3.clone();  // deep copy
println!("{} {}", s3, s4);  // world world
```

---

### Copy Types

Primitive types implement `Copy` — they're copied automatically, no move:

```rust
let x = 5;
let y = x;        // x is COPIED (not moved)
println!("{} {}", x, y);  // 5 5  — both valid!

// Copy types: i32, f64, bool, char, tuples of Copy types, arrays of Copy types
```

---

### References & Borrowing

Borrow a variable without taking ownership:

```rust
let s = String::from("hello");

let r1 = &s;    // immutable borrow
let r2 = &s;    // another immutable borrow — OK, can have many
println!("{} {}", r1, r2);

let mut name = String::from("Ferris");
let r3 = &mut name;  // mutable borrow — only ONE at a time
r3.push_str(" the Crab");
println!("{}", name);  // Ferris the Crab
```

---

## 📋 Quick Reference

| Declaration | Mutable? | Scope | Notes |
|-------------|----------|-------|-------|
| `let x = 5` | ❌ No | Local | Default, immutable |
| `let mut x = 5` | ✅ Yes | Local | Explicitly mutable |
| `const X: i32 = 5` | ❌ No | Any | Compile-time, needs type |
| `static X: i32 = 5` | ❌ No | Global | Fixed address, whole program |
| `static mut X: i32 = 5` | ✅ Yes | Global | `unsafe` to access |

| Category | Types |
|----------|-------|
| **Integer** | `i8` `i16` `i32` `i64` `i128` `isize` `u8` `u16` `u32` `u64` `u128` `usize` |
| **Float** | `f32` `f64` |
| **Boolean** | `bool` |
| **Character** | `char` |
| **Text** | `&str` `String` |
| **Compound** | `(T, U)` tuple · `[T; N]` array · `&[T]` slice |
| **Struct** | regular · tuple struct · unit struct |
| **Enum** | basic · with data · `Option<T>` · `Result<T, E>` |
| **Collections** | `Vec<T>` · `HashMap<K,V>` · `HashSet<T>` |