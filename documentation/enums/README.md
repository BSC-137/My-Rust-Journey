# Rust Enums

## 1. What are Enums?

While **Structs** group related fields (AND logic), **Enums** allow you to say a value is one of a possible set of variants (OR logic).

- **Definition:** Enums enumerate all possible variants of a type.
- **Property:** An enum instance can only be one of its variants at any given time.
- **Namespacing:** Variants are namespaced under the enum name, accessed using the double colon `::` (e.g., `IpAddrKind::V4`).

---

## 2. Attaching Data to Enums

One of Rust's most powerful features is the ability to store data directly inside an enum variant. This is often more concise than using a struct.

### Comparison: Struct vs. Enum

| **Approach** | **Syntax** | **Benefit** |
| --- | --- | --- |
| **Struct** | `struct IpAddr { kind: Kind, addr: String }` | Traditional, separate fields. |
| **Enum** | `enum IpAddr { V4(String), V6(String) }` | Concise; data is tied to the variant. |

### Advanced Usage: Heterogeneous Data

Each variant can have different types and amounts of data:

```rust
enum Message {
    Quit,                       // No data (unit-like)
    Move { x: i32, y: i32 },    // Named fields (struct-like)
    Write(String),              // Single value (tuple-like)
    ChangeColor(i32, i32, i32), // Three values (tuple-like)
}
```

> Pro-Tip: Just like structs, you can use impl blocks to define methods on enums.

---

## 3. The `Option` Enum: Rust's Solution to Null

Rust does not have a `null` value. Instead, it uses the `Option<T>` enum to represent the presence or absence of a value.

### The Problem with Null

In other languages, `null` often causes runtime crashes (the "Billion Dollar Mistake") when you try to use a null value as if it were valid.

### The Rust Implementation

```rust
enum Option<T> {
    None,
    Some(T),
}
```

- **`Some(T)`**: Represents a value of type `T`.
- **`None`**: Represents the absence of a value (equivalent to null).

---

## 4. Why `Option<T>` is Better than Null

The primary advantage is **Type Safety**. Rust treats `T` and `Option<T>` as entirely different types.

- **Compilation Error:** You cannot perform operations on an `Option<T>` as if it were the inner type. For example, you cannot add an `i8` to an `Option<i8>`.
- **Explicit Handling:** You are forced by the compiler to "unwrap" or handle the `None` case before you can access the data inside `Some`.
- **Safety Guarantee:** If a variable is a type like `String` (not `Option<String>`), you are **guaranteed** it is never null.

---

## 5. Using Option Values

To get the data out of a `Some` variant, you typically use:

1. **`match` expressions:** A control flow construct that ensures you handle both the `Some` and `None` cases.
2. **Standard Library Methods:** The `Option` type has many built-in methods (like `unwrap`, `expect`, or `map`) to safely handle data.

---

### Summary Table

| **Concept** | **Description** |
| --- | --- |
| **Variant** | A specific state of an Enum. |
| **Constructor** | Each variant acts as a function to create an instance. |
| **Generic `<T>`** | Allows `Option` to hold any data type. |
| **Match** | The safest way to extract data from an Enum. |

