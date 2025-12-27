# Rust Style Guide Summary

This document summarizes key rules and best practices from the official Rust Style Guide and community conventions.

## 1. Formatting (Use `rustfmt`)
- **Indentation:** 4 spaces (never tabs)
- **Line Length:** Maximum 100 characters
- **Braces:** Opening brace on same line
- **rustfmt:** Always run `cargo fmt` before committing

```rust
fn main() {
    let x = 5;
    if x > 0 {
        println!("Positive");
    } else {
        println!("Non-positive");
    }
}
```

## 2. Naming Conventions
- **snake_case:** Functions, variables, modules, macros, fields
- **PascalCase:** Types (structs, enums, traits), type parameters
- **SCREAMING_SNAKE_CASE:** Constants and statics
- **Lifetime Parameters:** Short, lowercase (e.g., `'a`, `'b`)

```rust
// Constants
const MAX_POINTS: u32 = 100_000;
static LANGUAGE: &str = "Rust";

// Struct (PascalCase)
struct UserAccount {
    user_name: String,    // Field (snake_case)
    email_address: String,
}

// Function (snake_case)
fn calculate_total(items: &[Item]) -> f64 {
    items.iter().map(|item| item.price).sum()
}

// Trait (PascalCase)
trait Drawable {
    fn draw(&self);
}

// Generic type parameter (PascalCase)
fn process_items<T: Display>(items: Vec<T>) {
    // ...
}
```

## 3. Modules and Crates
- **File Structure:** One module per file, `mod.rs` for module directories
- **Visibility:** Use `pub` explicitly, default is private
- **Re-exports:** Use `pub use` to simplify public API
- **Crate Structure:** Organize with `lib.rs` or `main.rs` as root

```rust
// src/lib.rs or src/main.rs
pub mod models;
pub mod services;
mod utils;  // Private module

// Re-export for convenience
pub use models::User;
pub use services::UserService;

// src/models/mod.rs
pub mod user;
pub mod post;

// src/models/user.rs
pub struct User {
    pub id: u32,
    pub name: String,
}
```

## 4. Error Handling
- **Result Type:** Use `Result<T, E>` for fallible operations
- **Option Type:** Use `Option<T>` for values that may not exist
- **? Operator:** Use for error propagation
- **Custom Errors:** Implement `Error` trait for custom error types
- **thiserror/anyhow:** Use for better error handling

```rust
use std::fs::File;
use std::io::{self, Read};

// Result with ? operator
fn read_file(path: &str) -> Result<String, io::Error> {
    let mut file = File::open(path)?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
}

// Option handling
fn find_user(id: u32) -> Option<User> {
    // Returns Some(user) or None
}

// Custom error with thiserror
use thiserror::Error;

#[derive(Error, Debug)]
pub enum UserError {
    #[error("User not found: {0}")]
    NotFound(u32),
    #[error("Invalid email format")]
    InvalidEmail,
    #[error("Database error: {0}")]
    Database(#[from] sqlx::Error),
}
```

## 5. Ownership and Borrowing
- **Move Semantics:** Default for non-Copy types
- **Borrowing:** Use references (`&T`, `&mut T`) to avoid moves
- **Immutable by Default:** Variables are immutable unless `mut`
- **Clone Sparingly:** Only clone when necessary

```rust
// Immutable by default
let x = 5;  // Immutable
let mut y = 10;  // Mutable

// Borrowing
fn calculate_length(s: &String) -> usize {
    s.len()  // Borrow, don't take ownership
}

// Mutable borrowing
fn append_text(s: &mut String, text: &str) {
    s.push_str(text);
}

// Using references
let name = String::from("Alice");
let len = calculate_length(&name);  // name still valid here
println!("{} has length {}", name, len);
```

## 6. Types and Traits
- **Type Inference:** Let compiler infer when possible
- **Explicit Types:** Use for function signatures and struct fields
- **Trait Bounds:** Use for generic constraints
- **Associated Types:** Use in traits when appropriate

```rust
// Type inference
let numbers = vec![1, 2, 3];  // Vec<i32> inferred

// Explicit types in signatures
fn process_data(data: &[u8]) -> Result<Vec<String>, ParseError> {
    // ...
}

// Trait bounds
fn print_all<T: Display>(items: &[T]) {
    for item in items {
        println!("{}", item);
    }
}

// Multiple trait bounds
fn notify<T: Display + Clone>(item: &T) {
    println!("{}", item);
}

// Where clause for complex bounds
fn complex_function<T, U>(t: &T, u: &U) -> i32
where
    T: Display + Clone,
    U: Clone + Debug,
{
    // ...
}
```

## 7. Iterators and Closures
- **Prefer Iterators:** Use iterator methods over manual loops
- **Iterator Chains:** Chain methods for readable transformations
- **Closures:** Use for short, local functions
- **Collect:** Use to gather results into collections

```rust
// Iterator chains
let sum: i32 = numbers
    .iter()
    .filter(|&&x| x > 0)
    .map(|&x| x * 2)
    .sum();

// Closure
let is_positive = |x: &i32| *x > 0;
let positives: Vec<_> = numbers.iter().filter(is_positive).collect();

// for_each instead of for loop when side effects only
users.iter().for_each(|user| println!("{}", user.name));

// Avoid collecting when not needed
let has_admin = users.iter().any(|u| u.role == Role::Admin);
```

## 8. Pattern Matching
- **match:** Use for exhaustive pattern matching
- **if let:** Use for single pattern match
- **while let:** Use for loops with pattern matching
- **Destructuring:** Use in let bindings and function parameters

```rust
// Exhaustive match
match result {
    Ok(value) => println!("Success: {}", value),
    Err(e) => eprintln!("Error: {}", e),
}

// if let for single case
if let Some(user) = find_user(id) {
    println!("Found: {}", user.name);
}

// Destructuring
let Point { x, y } = point;

// Match guards
match value {
    x if x < 0 => println!("Negative"),
    x if x > 0 => println!("Positive"),
    _ => println!("Zero"),
}
```

## 9. Documentation
- **Doc Comments:** Use `///` for public items, `//!` for module/crate level
- **Examples:** Include code examples in doc comments
- **cargo doc:** Generate documentation with examples that are tested

```rust
/// Calculates the area of a rectangle.
///
/// # Arguments
///
/// * `width` - The width of the rectangle
/// * `height` - The height of the rectangle
///
/// # Examples
///
/// ```
/// let area = calculate_area(5, 10);
/// assert_eq!(area, 50);
/// ```
///
/// # Panics
///
/// Panics if either `width` or `height` is negative.
pub fn calculate_area(width: i32, height: i32) -> i32 {
    if width < 0 || height < 0 {
        panic!("Dimensions must be positive");
    }
    width * height
}

//! This module provides user authentication functionality.
//!
//! It includes functions for login, logout, and session management.
```

## 10. Common Patterns
- **Builder Pattern:** For complex object construction
- **RAII:** Resources automatically cleaned up when dropped
- **Type State Pattern:** Use types to enforce state transitions
- **Newtype Pattern:** Wrap types for type safety

```rust
// Builder pattern
let user = User::builder()
    .name("Alice")
    .email("alice@example.com")
    .role(Role::Admin)
    .build();

// RAII - file automatically closed when dropped
{
    let file = File::open("data.txt")?;
    // Use file
}  // File closed here

// Newtype pattern
struct UserId(u32);
struct OrderId(u32);

fn get_user(id: UserId) -> User {
    // Type safety: can't accidentally pass OrderId
}
```

## 11. Testing
- **Unit Tests:** Place in same file with `#[cfg(test)]`
- **Integration Tests:** Place in `tests/` directory
- **Doc Tests:** Include examples in doc comments
- **Test Organization:** Use modules to organize tests

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_calculate_area() {
        assert_eq!(calculate_area(5, 10), 50);
    }

    #[test]
    #[should_panic(expected = "Dimensions must be positive")]
    fn test_negative_dimensions() {
        calculate_area(-5, 10);
    }

    #[test]
    fn test_user_creation() -> Result<(), Box<dyn Error>> {
        let user = User::new("Alice", "alice@example.com")?;
        assert_eq!(user.name, "Alice");
        Ok(())
    }
}
```

## 12. Best Practices
- **clippy:** Use `cargo clippy` for linting
- **Avoid `unwrap`:** Use proper error handling in production code
- **Avoid `clone` When Possible:** Design APIs to minimize cloning
- **Use `&str` for Parameters:** Accept `&str` instead of `&String`
- **Return Owned Types:** Return `String` instead of `&str` when appropriate
- **Derive Common Traits:** Use `#[derive(...)]` for standard traits

```rust
// Good: Accept &str
fn greet(name: &str) -> String {
    format!("Hello, {}!", name)
}

// Derive common traits
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct User {
    pub id: u32,
    pub name: String,
}

// Avoid unwrap in production
// Bad
let value = some_option.unwrap();

// Good
let value = some_option.ok_or(Error::MissingValue)?;
```

**BE CONSISTENT.** Use `rustfmt` and `clippy` to enforce style and catch common mistakes.

*Sources: [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/), [The Rust Programming Language Book](https://doc.rust-lang.org/book/)*
