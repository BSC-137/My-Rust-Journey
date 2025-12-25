# Structs

Structs (**structures**) are custom data types that let you package together and name multiple related values that make up a meaningful group. They are the building blocks for creating complex data structures in Rust.

---

## 1. Structs vs. Tuples

While both hold multiple types, **structs** are more flexible because you name each piece of data (**fields**).

- **Tuples:** Rely on the order of data to access values.
- **Structs:** Rely on names, so the order doesn't matter when instantiating or accessing.

---

## 2. Defining and Instantiating

To define a struct, use the `struct` keyword. To use it, create an **instance** by providing concrete values.

```rust
// 1. Definition
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {
    // 2. Instantiation
    let user1 = User {
        active: true,
        username: String::from("creative_polymath"),
        email: String::from("user@sydney.edu.au"),
        sign_in_count: 1,
    };

    // 3. Accessing Data (Dot Notation)
    println!("Email: {}", user1.email);
}
```

### 🔑 Key Rule: Mutability

Rust does **not** allow marking only specific fields as mutable. If you want to change a value, the **entire instance** must be mutable.

```rust
let mut user1 = User { /* ... */ };
user1.email = String::from("newemail@example.com");
```

---

## 3. Developer Shortcuts

### Field Init Shorthand

If your function parameter names match your struct field names exactly, you can use the shorthand to avoid repetition.

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username, // Shorthand for username: username
        email,    // Shorthand for email: email
        sign_in_count: 1,
    }
}
```

### Struct Update Syntax

Use the `..` syntax to create a new instance using values from an existing instance.

```rust
let user2 = User {
    email: String::from("another@example.com"),
    ..user1 // Use all other fields from user1
};
```

> [!WARNING]
> **Ownership Move:** Struct update syntax uses `=` which moves data. In the example above, `user1` is no longer valid because the `String` in `username` was moved to `user2`. If only `Copy` types (like `bool` or `u64`) were used, `user1` would remain valid.

---

## 4. Specialized Struct Types

| **Type** | **Syntax** | **Best Use Case** |
| :--- | :--- | :--- |
| **Tuple Structs** | `struct Color(i32, i32, i32);` | When the struct name provides meaning, but field names are redundant (e.g., coordinates). |
| **Unit-Like Structs** | `struct AlwaysEqual;` | When you need to implement a trait on a type but don't need to store any actual data. |

---

## 5. Ownership of Struct Data

For now, we use the owned `String` type instead of the `&str` (string slice) in structs.

- **Why?** We want the struct instance to **own** its data so the data stays valid as long as the struct exists.
- **References in Structs:** Storing references (like `&str`) requires **Lifetimes** (covered in Chapter 10), which tell the compiler how long the referenced data lives. Without them, the code won't compile.

