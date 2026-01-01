# Rust: Control Scope & Privacy (Modules)

## Modules Cheat Sheet

*Quick reference for organizing code and managing visibility.*

| **Action** | **Keyword / Rule** |
| --- | --- |
| **Start from Crate Root** | The compiler looks at `src/lib.rs` (library) or `src/main.rs` (binary) first. |
| **Declare a Module** | Use `mod garden;` in the root file. The compiler looks in:<br>1. Inline `{}`<br>2. `src/garden.rs`<br>3. `src/garden/mod.rs` |
| **Declare Submodules** | Use `mod vegetables;` inside `src/garden.rs`. Compiler looks in:<br>1. Inline `{}`<br>2. `src/garden/vegetables.rs`<br>3. `src/garden/vegetables/mod.rs` |
| **Privacy Rules** | Items are **private by default**. Use `pub` to make modules or items accessible to parent modules/external code. |
| **The `use` Keyword** | Creates a shortcut to a long path (e.g., `use crate::garden::veg::Asparagus;`). |

---

## The Module Tree

The module system is structured like a filesystem. The root of this tree is the implicit module named `crate`.

### Example Structure: Restaurant Crate

```text
crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment
```

---

## Key Concepts

### 1. Grouping Related Code

Modules allow us to organize code into logical groups. For example, a restaurant library can be split into `front_of_house` and `back_of_house`.

- **Readability:** Users can navigate by groups rather than scrolling through a single file.
- **Maintainability:** New code has a designated "home" based on its function.

### 2. Privacy & Encapsulation

- **Private (Default):** Code within a module is hidden from its parents. This allows you to change internal implementation details without breaking external code.
- **Public (`pub`):** Exposes the item for use by external code or parent modules.

### 3. Paths to Code

To call a function or use a struct, you must use a **path**.

- **Absolute Path:** Starts from the crate root (e.g., `crate::front_of_house::hosting::add_to_waitlist`).
- **Relative Path:** Starts from the current module using `self`, `super`, or an identifier in the current module.

---

## File Organization Example

If you have a binary crate named `backyard`, your filesystem might look like this:

```text
backyard
├── Cargo.toml
└── src
    ├── main.rs          // Crate root
    ├── garden.rs        // "garden" module definition
    └── garden
        └── vegetables.rs // "vegetables" submodule
```

**`src/main.rs`**

```rust
use crate::garden::vegetables::Asparagus;
pub mod garden; // Declares the module

fn main() {
    let plant = Asparagus {};
    println!("Growing {:?}!", plant);
}
```

**`src/garden.rs`**

```rust
pub mod vegetables; // Declares submodule as public
```

**`src/garden/vegetables.rs`**

```rust
#[derive(Debug)]
pub struct Asparagus {} // Public struct
```

---

> [!TIP]
>
> **Pro-Tip:** Think of modules as folders and items (functions/structs) as files. If you want someone outside the folder to see a file, you have to "unlock" the folder (`pub mod`) and the file itself (`pub fn`).

