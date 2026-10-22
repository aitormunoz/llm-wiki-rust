---
type: concept
sources: [raw/ch10-generics-traits-lifetimes.md]
updated: 2026-10-22
---
# Generics

Generics are abstract stand-ins for concrete types, letting one definition work for many types. They are the type-level equivalent of a function parameter (src: [[ch10-generics-traits-lifetimes]]). `Option<T>`, `Vec<T>`, `HashMap<K, V>` and `Result<T, E>` are all generic.

## Rules

- Declare type parameters in angle brackets before using them: `fn largest<T>(list: &[T]) -> &T`. By convention names are short, UpperCamelCase, often `T`. (src: [[ch10-generics-traits-lifetimes]])
- Structs and enums: `struct Point<T> { x: T, y: T }`. One parameter means both fields share one type; use `Point<T, U>` to allow different types. Many parameters suggest the code needs splitting up.
- Methods: declare the parameter after `impl` (`impl<T> Point<T>`) so Rust knows `Point<T>` is generic. `impl Point<f32>` instead defines methods only for that concrete type.
- A method can add its own parameters (`fn mixup<X2, Y2>`); those declared on `impl` belong to the struct, those on `fn` only to the method.
- Using an operation on `T` (such as `>`) requires a bound, otherwise E0369; see [[trait-bounds]].
- No runtime cost: see [[monomorphization]].

## Example

```rust
struct Point<T> { x: T, y: T }
impl<T> Point<T> { fn x(&self) -> &T { &self.x } }
impl Point<f32> {
    fn distance_from_origin(&self) -> f32 { (self.x.powi(2) + self.y.powi(2)).sqrt() }
}
```

## Common mistakes

- `Point { x: 5, y: 4.0 }` with a single `T`: mismatched types (E0308).
- Comparing or printing a `T` without a trait bound (E0369).

## Relations

[[traits]], [[trait-bounds]], [[monomorphization]], [[lifetimes]] (a lifetime is a kind of generic parameter and shares the same angle-bracket list).
