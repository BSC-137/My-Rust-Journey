# Rust Methods

## 1. Methods vs. Functions

While methods and functions both use the `fn` keyword and hold logic, they differ in context and execution.

- **Functions:** Standalone blocks of code.
- **Methods:** Defined within the context of a `struct`, `enum`, or `trait`.
- **The `self` Parameter:** The first parameter of a method is always `self`, representing the instance the method is called on.

---

## 2. Method Syntax & The `impl` Block

To define methods, you must use an **`impl` (implementation) block**.

### Example: Rectangle Area

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // This is a method because it takes &self
    fn area(&self) -> u32 {
        self.width * self.height
    }
}
```

- **Calling the method:** Use "dot notation" (e.g., `rect1.area()`).
- **Organization:** `impl` blocks keep all capabilities of a type in one place, making the code easier to navigate.

---

## 3. Understanding the `self` Parameter

The signature `&self` is shorthand for `self: &Self`. The type `Self` is an alias for the type the `impl` block is defining.

| **Shorthand** | **Meaning** | **Use Case** |
| --- | --- | --- |
| `&self` | Immutable Borrow | **Most common.** Reading data without changing the instance. |
| `&mut self` | Mutable Borrow | Changing the instance's data. |
| `self` | Taking Ownership | **Rare.** Used when transforming the instance into something else (prevents further use of the original). |

---

## 4. Automatic Referencing and Dereferencing

Coming from languages like C or C++, you might expect to use `->` for pointers. Rust simplifies this:

- **The Feature:** Rust automatically adds `&`, `&mut`, or `*` so the object matches the method’s signature.
- **The Logic:** Because methods have a clear receiver (`self`), Rust knows exactly what the method requires (reading, mutating, or consuming).
- **Equivalency:** `p1.distance(&p2)` is functionally identical to `(&p1).distance(&p2)`.

---

## 5. Associated Functions

Associated functions are functions defined inside an `impl` block that **do not** take `self` as a parameter.

- **Purpose:** Often used as **constructors** (e.g., `String::from`).
- **Syntax:** Called using the "turbofish" or double colon operator `::`.
- **Example (Constructor):**

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self { width: size, height: size }
    }
}
// Called as: let sq = Rectangle::square(10);
```

---

## 6. Key Nuances

- **Getters:** Rust doesn't generate "getters" (methods to return a private field) automatically. You can name a method the same as a field; Rust distinguishes them by the presence of parentheses:
    - `rect.width` -> Refers to the **field**.
    - `rect.width()` -> Refers to the **method**.
- **Multiple `impl` Blocks:** A single struct can have multiple `impl` blocks scattered across a file. This is useful for organizing code when dealing with generic types or traits (discussed in later chapters).

> **Note on Organization:** Methods are preferred over functions for specific type logic to improve discoverability for other developers using your library.

