# Rust: References and Borrowing

> Key Concept: A Reference is like a pointer—it contains an address you can follow to access data—but unlike pointers, it is guaranteed to point to a valid value for the duration of its life. The act of creating a reference is called Borrowing.

---

## 1. Why Use References?

In the previous section, we saw that passing a value to a function moves ownership. If we want to keep using the value afterward, we have to return it back in a tuple. References solve this by letting us "borrow" a value without taking ownership.

### Example: Borrowing vs. Moving

```rust
fn main() {
    let s1 = String::from("hello");

    // We pass &s1 (a reference) instead of s1 (the value)
    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}."); // s1 is still valid here!
}

fn calculate_length(s: &String) -> usize { // s is a reference to a String
    s.len()
} // s goes out of scope, but since it doesn't own the data, nothing is dropped.
```

---

## 2. Mutable References (`&mut`)

By default, references are **immutable**. You cannot modify something you are borrowing unless you explicitly use a mutable reference.

### The Syntax:

1. Variable must be mutable: `let mut s = ...`
2. Pass a mutable reference: `&mut s`
3. Accept a mutable reference: `some_string: &mut String`

```rust
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

---

## 3. The "Big Restriction" of Mutable References

To prevent data races, Rust enforces a strict rule regarding mutable references:

> [!CAUTION]
> 
> The Rule: If you have a mutable reference to a value, you can have no other references (mutable or immutable) to that same value at the same time.

### Why this rule exists?

It prevents **Data Races**. A data race occurs when:

1. Two or more pointers access the same data simultaneously.
2. At least one pointer is writing to the data.
3. There is no mechanism to synchronize access.

---

## 4. Reference Scoping (Non-Lexical Lifetimes)

A reference’s scope starts where it is introduced and ends **at the last time that reference is used**, not necessarily at the end of the curly brackets.

```rust
let mut s = String::from("hello");

let r1 = &s; // No problem
let r2 = &s; // No problem
println!("{r1} and {r2}"); 
// Scope of r1 and r2 ends HERE (last usage)

let r3 = &mut s; // No problem! r1 and r2 are no longer in use.
println!("{r3}");
```

---

## 5. Dangling References

In Rust, the compiler guarantees that you will never have a dangling pointer (a pointer to memory that has been freed).

### The "Dangle" Error

If you try to return a reference to a local variable, the code will not compile because the variable is dropped at the end of the function, leaving the reference pointing to "garbage" memory.

```rust
// This will NOT compile
fn dangle() -> &String {
    let s = String::from("hello");
    &s // ERROR: returns reference to local variable
} 

// The Fix: Return the String directly (Move ownership)
fn no_dangle() -> String {
    let s = String::from("hello");
    s 
}
```

---

## Summary: The Rules of References

| **Rule** | **Description** |
| --- | --- |
| **Rule #1** | At any given time, you can have **either** one mutable reference **OR** any number of immutable references. |
| **Rule #2** | References must **always be valid** (no dangling references). |
| **Immutability** | You cannot change a value through an `&` reference; you must use `&mut`. |
| **Safety** | These rules are checked at **compile-time**, preventing runtime bugs and data races. |
