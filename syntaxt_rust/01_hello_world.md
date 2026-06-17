# Hello World! (First Program)

Now after we understood the big picture of what tools Rust uses and we installed everything correctly, we are going to start learning the syntax.

In this example we are going to see the classic "hello world" program:

```rust
fn main() {
    println!("hello world!");
}
```

After you read this code you may have these questions:

- `what is fn ?`
- `why main exactly ?`
- `what is ! after println ?`
- `why did we use {} ?`

---

## The Answers:

### What is `fn` ?

`fn` stands for **function** — it is a keyword that tells Rust you are about to define a function.
A function is a named block of code that does one or more things in your program.
You write it once and can call it whenever you need it.

---

### Why `main` exactly ?

`main` is the name of this function. Normally you decide the name yourself,
but `main` is special — it is the **entry point** of every Rust program.
When you run your program, Rust looks for a function named `main` and runs it first.
No `main` = your program has no starting point and will not compile.

---

### What is `!` after `println` ?

The `!` means you are calling a **macro**, not a regular function.

The difference:
- A **function** is a normal block of code. You define it, you call it at runtime, it runs.
- A **macro** is code that **generates code** — it runs at **compile time**, before your program even starts.

Example — when you write:

```rust
println!("hello world!");
```

The compiler expands it into something like this before compiling:

```rust
{
    ::std::io::_print(format_args!("{}\n", "hello world!"));
}
```

By the time your program runs, `println!` is already gone — replaced by the real instructions.

**Why can't a normal function do this?**

```rust
// if println was a normal function it would need a fixed signature like:
fn println(value: ???) { }

// but we call it in many different ways:
println!("hello");            // one string
println!("{}", name);         // string + variable
println!("{} {}", a, b);      // string + two variables
```

A normal function needs to know exactly how many arguments and what types ahead of time.
A macro does not — it figures it all out at compile time and generates the right code each time.

> For now just remember: `!` = macro. You will use macros written by others all the time.
> Writing your own macros is an advanced topic — save it for much later.

---

### Why did we use `{}` ?

The `{}` defines a **block** — it groups lines of code together and tells the compiler
"everything inside here belongs to this function."

```rust
fn main() {         // ← function starts here
    println!(...);  // ← this line belongs to main
}                   // ← function ends here
```

Every function in Rust needs a `{}` block. Whatever is inside runs when the function is called.

---

## The Full Picture

```
fn       → keyword: "I am defining a function"
main     → name:    "this is the entry point — run this first"
()       → inputs:  "this function takes no arguments"
{  }     → body:    "everything inside here is the function's code"
println! → macro:   "generate code to print a line to the terminal"
```

in 04 we are gonna talk about print at all good cause we are gonna use it brifely when using the rust language.