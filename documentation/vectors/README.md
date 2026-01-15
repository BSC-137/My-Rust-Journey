# Rust Collections: Vectors (Vec<T>)

Vectors allow you to store more than one value in a single data structure that puts all values **next to each other in memory**.

## 1. Key Characteristics

- **Contiguous Memory:** All elements are stored next to each other on the heap.
- **Homogeneous:** A vector can only store values of the **same type**.
- **Dynamic Size:** Unlike arrays, vectors can grow or shrink at runtime.

---

## 2. Creating a New Vector

There are two primary ways to initialize a vector:

- **Empty Vector:** Use `Vec::new()`. Note that you usually need a type annotation if you aren't adding data immediately.

    ```rust
    let v: Vec<i32> = Vec::new();
    ```

- **Macro Initialization:** Use the `vec!` macro to create a vector with initial values. Rust will infer the type.

    ```rust
    let v = vec![1, 2, 3]; // Inferred as Vec<i32>
    ```

---

## 3. Updating a Vector

To add elements, use the `.push()` method. The variable **must** be declared with `mut`.

```rust
let mut v = Vec::new();

v.push(5);
v.push(6);
v.push(7);
```

---

## 4. Reading Elements

There are two ways to reference a value in a vector. Choosing between them depends on how you want the program to handle out-of-bounds errors.

| **Method** | **Syntax** | **Behavior on Out-of-Bounds** | **Use Case** |
| --- | --- | --- | --- |
| **Indexing** | `&v[index]` | **Panics** (Crashes program) | When you are sure the index is valid. |
| **Get Method** | `v.get(index)` | Returns `None` | When an index might be out of range (user input). |

### Example: Using `.get()`

```rust
let v = vec![1, 2, 3, 4, 5];

match v.get(2) {
    Some(third) => println!("The third element is {third}"),
    None => println!("There is no third element."),
}
```

---

## 5. Memory and Ownership Safety

### The Reallocation Rule

Rust ensures memory safety through the borrow checker. A common pitfall is trying to add an element while holding a reference to an existing one:

```rust
let mut v = vec![1, 2, 3];
let first = &v[0]; // Immutable borrow occurs here

v.push(4); // ❌ ERROR: Mutable borrow occurs here

println!("{first}"); // Immutable borrow used here
```

> **Why does this fail?**
> Adding a new element might require the vector to find a new, larger spot in memory. If it moves, your original reference (`first`) would point to deallocated (garbage) memory. Rust prevents this "dangling pointer" at compile time.

---

## 6. Iterating Over Values

### Immutable Iteration

```rust
let v = vec![100, 32, 57];
for i in &v {
    println!("{i}");
}
```

### Mutable Iteration

To change values in place, use the `*` **dereference operator** to access the value behind the reference.

```rust
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50; // Use * to modify the underlying value
}
```

---

## 7. Storing Multiple Types with Enums

If you need to store different types in a single vector, wrap them in an **Enum**. Since enum variants are considered the same type (the type of the enum), this bypasses the "same type" restriction.

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell::Float(10.12),
];
```

---

## 8. Dropping a Vector

Just like a `struct`, a vector and all its contents are dropped (freed) when the vector goes out of scope.

```rust
{
    let v = vec![1, 2, 3];
    // do stuff
} // <- v and its elements are dropped here
```
