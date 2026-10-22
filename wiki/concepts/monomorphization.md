---
type: concept
sources: [raw/ch10-generics-traits-lifetimes.md]
updated: 2026-10-22
---
# Monomorphization

Monomorphization is the compile-time process of turning generic code into specific code by filling in the concrete types actually used (src: [[ch10-generics-traits-lifetimes]]). It is why [[generics]] cost nothing at runtime.

## Rules

- The compiler finds every call site of generic code and generates a copy per concrete type. `Some(5)` and `Some(5.0)` yield two specialized enums, conceptually `Option_i32` and `Option_f64`. (src: [[ch10-generics-traits-lifetimes]])
- The result performs as if you had written each version by hand.

## Relations

[[generics]], [[trait-bounds]] (checked at compile time too, so no runtime behavior checks are needed).
