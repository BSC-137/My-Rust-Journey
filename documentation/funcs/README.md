# Rust Functions

### **1. Function Anatomy & Style**

- **Keyword:** `fn`
- **Naming Convention:** `snake_case` (all lowercase, underscores for spaces).
- **Hoisting:** You can call functions before or after they are defined. Rust scans the scope for the definition regardless of line order.

### **2. Strict Signatures**

In Rust, **type annotations are mandatory** for all parameters.

> Why? It prevents the compiler from having to guess, leads to faster compilation, and provides crystal-clear error messages.

```rust
fn print_measurement(value: i32, unit: char) { 
    // Types must be explicit here!
    println!("{value}{unit}"); 
}
```

### **3. The Expression vs. Statement Split**

This is the most common "gotcha" for new Rustaceans.

| **Feature** | **Statements** | **Expressions** |
| --- | --- | --- |
| **Definition** | Instructions that perform an action. | Instructions that result in a value. |
| **Returns Value?** | **No.** | **Yes.** |
| **Semicolon?** | **Ends with `;`** | **NO semicolon.** |
| **Example** | `let y = 6;` | `5 + 6` |

**The Golden Rule:** If you add a semicolon to an expression, you turn it into a statement, and it **returns nothing** (the "unit type" `()`).

---

### **4. Returning Values**

Rust functions return the value of the **final expression** in the block implicitly.

- **The Arrow:** Use `>` to define the return type.
- **No `return` required:** You don't need the `return` keyword if the value is on the last line without a semicolon.
- **Early Returns:** Use the `return` keyword only if you need to exit the function early (e.g., inside an `if` block).

**Correct Example:**

```rust
fn plus_one(x: i32) -> i32 {
    x + 1  // No semicolon = this is the return value
}
```

**Common Error (The Semicolon Trap):**

```rust
fn plus_one(x: i32) -> i32 {
    x + 1; // ERROR: The semicolon makes this a statement.
           // It now returns (), not i32.
}
```

---

### **5. Scope Blocks as Expressions**

You can use curly brackets `{}` to create a new scope that evaluates to a value. This is incredibly powerful for initializing variables with logic.

```rust
let y = {
    let x = 3;
    x + 1 // This block evaluates to 4
}; 
// y is now 4
```

---

## **Pro-Tip for Learning**

When you see a compiler error that says `expected i32, found ()`, **90% of the time** you accidentally put a semicolon at the end of a function or a block where you meant to return a value.
