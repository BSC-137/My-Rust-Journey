# If let and let else in Rust

## 1. Concise Control Flow: `if let`

The `if let` syntax is "syntactic sugar" for a `match` that only cares about **one specific pattern** and ignores everything else.

- **Problem:** Using `match` for a single variant requires a "catch-all" arm (`_ => ()`), which adds boilerplate.
- **Solution:** `if let` lets you write the "happy path" concisely.

### Comparison: Match vs. If Let

| **Feature** | **match** | **if let** |
| --- | --- | --- |
| **Exhaustiveness** | **Mandatory.** Compiler ensures all cases are handled. | **Optional.** Ignores unhandled cases. |
| **Verbosity** | High (requires `_ => ()`). | Low (cleaner for single checks). |
| **Scope** | Variables bound in arms stay in arms. | Variables bound in pattern stay in the `if` block. |

```rust
// Verbose match
match config_max {
    Some(max) => println!("The max is {max}"),
    _ => (),
}

// Concise if let
if let Some(max) = config_max {
    println!("The max is {max}");
}
```

---

## 2. Using `if let` with `else`

You can add an `else` block to `if let`. This is exactly equivalent to the `_` arm in a `match` expression.

```rust
let mut count = 0;
if let Coin::Quarter(state) = coin {
    println!("State quarter from {:?}!", state);
} else {
    count += 1; // This runs for Penny, Nickel, and Dime
}
```

---

## 3. The "Happy Path" with `let...else`

Introduced to handle "guard" logic, `let...else` is used when you want to extract a value and continue the function, but **diverge** (return, break, or panic) if the pattern doesn't match.

### Why use it?

It prevents "arrow code" (deep nesting). Instead of putting your main logic inside an `if let` block, you handle the error case early and keep the main logic at the top level of the function.

### The Rules:

1. **Scope:** Variables bound in the pattern are available in the **outer scope** (after the statement).
2. **Divergence:** The `else` block **must** diverge (e.g., `return`, `break`, `continue`, or `panic!`). It cannot just fall through.

### Example: Refactoring for Clarity

```rust
// Using let...else (Clean "Happy Path")
fn describe_state_quarter(coin: Coin) -> Option<String> {
    // If it's not a quarter, return None immediately
    let Coin::Quarter(state) = coin else {
        return None;
    };

    // 'state' is now available in the rest of the function!
    if state.existed_in(1900) {
        Some(format!("{state:?} is old!"))
    } else {
        Some(format!("{state:?} is new."))
    }
}
```

---

## 4. Summary Table: Which one to use?

| **Scenario** | **Recommended Tool** |
| --- | --- |
| Handling **all** variants of an enum. | `match` |
| Performing an action for **only one** variant. | `if let` |
| Extracting data to use in the **rest of the function**. | `let...else` |
| Providing a default action for "everything else". | `if let ... else` or `match` |

