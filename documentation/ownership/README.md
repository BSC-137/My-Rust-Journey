# Rust Ownership: Core Concepts

> Definition: Ownership is a set of rules that governs how a Rust program manages memory. It ensures memory safety without needing a garbage collector or manual memory management (like malloc/free).

---

## 1. The Three Rules of Ownership

These are the fundamental laws of Rust. If your code breaks these, it will not compile.

1. Each value in Rust has an **owner**.
2. There can only be **one owner** at a time.
3. When the owner goes **out of scope**, the value is dropped (memory is freed).

---

## 2. Memory: Stack vs. Heap

Understanding ownership requires understanding where data lives.

| **Feature** | **The Stack** | **The Heap** |
| --- | --- | --- |
| **Storage Type** | Fixed size, known at compile time. | Unknown or changing size. |
| **Order** | Last In, First Out (LIFO). | Less organized; requires an allocator. |
| **Access Speed** | Fast (no pointer following). | Slower (requires following a pointer). |
| **Example** | `i32`, `bool`, `char`. | `String`, `Vec`. |

---

## 3. Variable Scope

A scope is the range within a program for which an item is valid.

- **Start:** Variable is valid from the point of declaration.
- **End:** Remains valid until the closing curly bracket `}`.

```rust
{                      // s is not valid here
    let s = "hello";   // s is valid from this point forward
    // do stuff with s
}                      // scope is over, s is no longer valid
```

---

## 4. Memory Allocation (The `String` Type)

Unlike string literals (stored on the stack), the `String` type is allocated on the heap to allow for mutability and dynamic sizing.

### How Rust handles Deallocation:

- In C/C++, you must manually `free` memory.
- In Java/Python, a Garbage Collector (GC) handles it.
- **In Rust:** Memory is automatically returned once the variable that owns it goes out of scope. Rust calls a special function called `drop` at the end of the scope.

---

## 5. Moving, Cloning, and Copying

How variables interact with the same data depends on the data type.

### A. The Move (Heap Data)

When you assign one `String` to another, Rust copies the **pointer, length, and capacity** (stack data) but **not** the actual contents on the heap. To prevent "double-free" errors, Rust invalidates the first variable.

```rust
let s1 = String::from("hello");
let s2 = s1; // s1 is MOVED to s2.

// println!("{s1}"); // ERROR: s1 is no longer valid!
```

### B. The Clone (Deep Copy)

If you want to copy the heap data as well, you must use the `.clone()` method. This is visually explicit because it is a performance-heavy operation.

```rust
let s1 = String::from("hello");
let s2 = s1.clone(); 

println!("s1 = {s1}, s2 = {s2}"); // Both are valid
```

### C. The Copy (Stack Data)

Types with a known size at compile time (integers, booleans) implement the `Copy` trait. They don't "move"; they are simply duplicated on the stack.

```rust
let x = 5;
let y = x; // x is still valid!
```

**Types that are `Copy`:**

- All integers (`u32`, `i64`, etc.)
- Booleans (`bool`)
- Floating point (`f64`, etc.)
- Characters (`char`)
- Tuples (only if they contain `Copy` types)

---

## 6. Ownership in Functions

Passing a value to a function behaves just like an assignment: it will either **move** or **copy**.

```rust
fn main() {
    let s = String::from("hello");
    takes_ownership(s);             // s moves into the function
    // println!("{s}");             // ERROR: s is no longer valid here

    let x = 5;
    makes_copy(x);                  // x is copied
    println!("{x}");                // Valid: x is still here
}

fn takes_ownership(some_string: String) { 
    println!("{some_string}"); 
} // drop is called here, memory is freed.

fn makes_copy(some_integer: i32) { 
    println!("{some_integer}"); 
}
```

---

## 7. Return Values and Scope

Returning values also transfers ownership.

- **Returning a value:** Moves ownership out of the function to the calling variable.
- **The Problem:** Moving ownership back and forth is tedious.
- **The Solution:** Use **References** (to be covered in the next section) or return a **Tuple** if you need multiple values back.

```rust
fn calculate_length(s: String) -> (String, usize) {
    let length = s.len();
    (s, length) // Return ownership and the result
}
```

---

### Quick Summary for Notion Callout

> [!TIP]
> 
> Key Takeaway: Ownership's main job is to manage heap data. By ensuring only one owner exists at a time, Rust can safely clean up memory the moment that owner disappears, preventing leaks and crashes without needing a background garbage collector.
