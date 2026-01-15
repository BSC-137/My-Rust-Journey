# Rust Collections: Hash Maps (HashMap<K, V>)

A `HashMap<K, V>` stores a mapping of keys of type `K` to values of type `V`. It uses a **hashing function** to determine how to place these keys and values into memory.

---

## 1. Key Characteristics

- **Key-Value Pairs:** Useful for looking up data by a unique identifier (key) rather than an index.
- **Heap Allocated:** Data is stored on the heap.
- **Homogeneous:** All keys must be the same type, and all values must be the same type.
- **Not in Prelude:** You must explicitly import it: `use std::collections::HashMap;`.

---

## 2. Basic Operations

### Creating and Inserting

There is no built-in macro for Hash Maps (like `vec!`). You use `HashMap::new()` and `.insert()`.

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

### Accessing Values

The `.get()` method returns an `Option<&V>`.

```rust
let team_name = String::from("Blue");
let score = scores.get(&team_name).copied().unwrap_or(0);
```

- `.get(&key)` returns `Some(&value)` or `None`.
- `.copied()` changes `Option<&i32>` to `Option<i32>`.
- `.unwrap_or(0)` provides a default value if the key doesn't exist.

### Iterating

Iteration happens in an **arbitrary order**.

```rust
for (key, value) in &scores {
    println!("{key}: {value}");
}
```

---

## 3. Ownership and Hash Maps

How ownership is handled depends on whether the type implements the `Copy` trait.

| **Type Category** | **Behavior** | **Example** |
| --- | --- | --- |
| **Copy Types** | Values are **copied** into the map. | `i32`, `f64`, `bool` |
| **Owned Types** | Values are **moved**; Map becomes the owner. | `String`, `Vec<T>` |
| **References** | Values are not moved; requires **Lifetimes**. | `&str`, `&i32` |

---

## 4. Updating a Hash Map

There are three main strategies for handling a key that might already exist:

### A. Overwriting the Value

Simply calling `.insert()` again with the same key replaces the old value.

```rust
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25); // Now "Blue" is 25
```

### B. Adding Only If Key is Missing

The `.entry()` API is used to check for existence before inserting.

```rust
// Only inserts "Yellow": 50 if "Yellow" doesn't exist
scores.entry(String::from("Yellow")).or_insert(50);
```

### C. Updating Based on Old Value

Common for counters. `or_insert` returns a **mutable reference** to the value.

```rust
let text = "hello world wonderful world";
let mut map = HashMap::new();

for word in text.split_whitespace() {
    let count = map.entry(word).or_insert(0);
    *count += 1; // Dereference to increment the value in place
}
```

---

## 5. Hashing Function & Security

- **Default Hasher:** Rust uses **SipHash** by default.
- **Security:** Designed to resist Denial-of-Service (DoS) attacks involving hash collisions.
- **Performance:** While secure, it is not the fastest possible algorithm. You can replace it with a custom "hasher" (a type implementing `BuildHasher`) if profiling shows hashing is a bottleneck.
