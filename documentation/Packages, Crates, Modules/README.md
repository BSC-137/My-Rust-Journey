# Packages and Crates

## 1. Crates: The Unit of Compilation

A **crate** is the smallest amount of code that the Rust compiler (`rustc`) considers at a time. Even a single file is considered a crate by the compiler.

### Two Forms of Crates

| **Crate Type** | **Key Characteristic** | **Purpose** |
| :--- | :--- | :--- |
| **Binary Crate** | Must have a `main` function. | Compiles to an executable (e.g., a CLI tool, a server). |
| **Library Crate** | No `main` function. | Defines functionality to be shared with other projects (often just called a "crate"). |

- **Crate Root:** The source file that the compiler starts from. It forms the "root module" of your crate.

---

## 2. Packages: The Bundle

A **package** is a bundle of one or more crates that provides a set of functionality. It is managed by **Cargo**.

### Rules of a Package

- Must contain a `Cargo.toml` file (this describes how to build the crates).
- Can contain **at most one** library crate.
- Can contain **as many binary crates** as you like.
- Must contain at least one crate (either library or binary).

---

## 3. Cargo Conventions (Crate Roots)

Cargo follows specific conventions to determine which files are crate roots without you having to explicitly list them in `Cargo.toml`.

- **`src/main.rs`**: Cargo assumes this is the root of a **binary crate** with the same name as the package.
- **`src/lib.rs`**: Cargo assumes this is the root of a **library crate** with the same name as the package.
- **`src/bin/`**: Any file placed in this directory is treated as a **separate binary crate**.

---

## 4. Visualizing the Hierarchy

Understanding the relationship between these terms is crucial for larger projects:

1. **Package**: The container (the folder with `Cargo.toml`).
2. **Crate**: The compilation unit (Library or Binary).
3. **Modules**: Defined *inside* crates to organize code within that unit.

### Summary Table

| **Concept** | **Scope** | **Defined By** |
| :--- | :--- | :--- |
| **Package** | Project level | `Cargo.toml` |
| **Crate** | Compilation level | `src/main.rs` or `src/lib.rs` |
| **Binary** | Executable level | `fn main()` |
| **Library** | Shared logic level | `src/lib.rs` |

> **Pro-Tip for your current studies:**
> Since you are working on the **VisionForge** CPU path tracer, you are likely creating a **Binary Crate** (if it's a CLI tool) or potentially a **Package** that contains both a **Library Crate** (the rendering logic) and a **Binary Crate** (the CLI interface).

