# Rust Match

## 1. The `match` Construct: A Logic "Coin Sorter"

Think of a `match` expression like a coin-sorting machine. The value "slides" down the track and falls into the first hole (pattern) it fits into.

- **Structure:** It consists of a **target expression** followed by one or more **match arms**.
- **Match Arms:** Each arm has a **pattern** and the **code to execute** (separated by `=>`).
- **Flexibility:** Unlike `if`, which only checks Booleans, `match` can check any type.

```rust
enum Coin { Penny, Nickel, Dime, Quarter }

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

---

## 2. Binding to Values (Extracting Data)

One of the most useful features of `match` is the ability to "bind" to data stored inside an enum variant. This is often more concise than using a struct.

### Example: The Quarter with a State

```rust
enum Coin {
    Penny,
    Quarter(UsState), // This variant holds data
}

match coin {
    Coin::Penny => 1,
    Coin::Quarter(state) => {
        println!("State quarter from {:?}!", state);
        25
    },
}
```

In this example, the variable `state` binds to whatever `UsState` is inside the `Quarter`.

---

## 3. Matching with `Option<T>`

Since Rust doesn't have `null`, you use `match` to safely handle the presence (`Some`) or absence (`None`) of a value.

```rust
fn plus_one(x: Option<i32>) -> Option<i32> {
    match x {
        None => None,
        Some(i) => Some(i + 1), // 'i' binds to the value inside Some
    }
}
```

---

## 4. The Golden Rule: Matches are Exhaustive

In Rust, the compiler **forces** you to handle every single possibility. If you define an enum with four variants, your `match` must cover all four.

- **Why?** This prevents bugs where you forget to handle a specific case (like forgetting the `None` case in an `Option`).
- **Compiler Protection:** If you leave a case out, your code simply will not compile.

---

## 5. Catch-All Patterns and `_`

Sometimes you only care about a few specific values and want to treat everything else the same way.

### The `other` Catch-all

If you want to **use** the value for all other cases, give it a name:

```rust
match dice_roll {
    3 => add_hat(),
    7 => remove_hat(),
    other => move_player(other), // 'other' covers 1, 2, 4, 5, 6, 8...
}
```

### The `_` Placeholder

If you **don't** need the value for the other cases, use the underscore (`_`):

```rust
match dice_roll {
    3 => add_hat(),
    _ => reroll(), // Matches anything else but doesn't bind the value
}
```

### Doing Nothing

To match everything else and execute no code, use the unit tuple `()`:

```rust
match dice_roll {
    3 => add_hat(),
    _ => (), // Do nothing for all other rolls
}
```

---

### Summary Checklist

| **Feature** | **Description** |
| --- | --- |
| **Exhaustive** | You MUST handle every variant or use a catch-all. |
| **Binding** | Patterns can "grab" internal data (like the `i` in `Some(i)`). |
| **Order Matters** | Arms are checked top-to-bottom. Catch-alls must go last. |
| **Expressions** | `match` is an expression; it returns the value of the matching arm. |

