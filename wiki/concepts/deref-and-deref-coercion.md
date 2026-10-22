---
type: concept
sources: [raw/ch15-smart-pointers.md]
updated: 2026-10-22
---
# Deref and deref coercion

The `Deref` trait customizes the dereference operator `*`, so a type (typically a [[smart-pointers]]) can be treated like a reference. Deref coercion is the compiler converting `&T` into `&U` automatically when `T: Deref<Target=U>` (src: [[ch15-smart-pointers]]).

## Rules

- Without `Deref` the compiler can only dereference `&` references. `Deref` requires one method, `deref(&self) -> &Self::Target`, plus `type Target = T;` (an associated type; ch20).
- `*y` is replaced by `*(y.deref())`, once, not recursively. `deref` returns a reference so the inner value is not moved out of `self` (see [[ownership]]).
- Deref coercion applies to arguments of functions and methods, chaining `deref` calls as needed: `&MyBox<String>` to `&String` to `&str`. It is resolved at compile time, so it has no runtime cost. (src: [[ch15-smart-pointers]])
- Three coercion cases: `&T` to `&U` if `T: Deref<Target=U>`; `&mut T` to `&mut U` if `T: DerefMut<Target=U>`; `&mut T` to `&U` if `T: Deref<Target=U>`.
- `&` to `&mut` is never possible: the borrowing rules do not guarantee an immutable reference is the only one (see [[borrowing]]).

## Example

```rust
use std::ops::Deref;
struct MyBox<T>(T);
impl<T> Deref for MyBox<T> {
    type Target = T;
    fn deref(&self) -> &T { &self.0 }
}
fn hello(name: &str) { println!("Hello, {name}!"); }
let m = MyBox(String::from("Rust"));
hello(&m); // &MyBox<String> -> &String -> &str
```

## Common mistakes

- `*y` on a type without `Deref` (E0614).
- `assert_eq!(5, y)` with `y: &i32`: number vs reference are different types (E0277); write `*y`.
- Expecting coercion from `&` to `&mut`.

## Relations

[[traits]], [[box]], [[slices]] (why `&str` parameters accept `&String`), [[refcell-and-interior-mutability]] (`Ref`/`RefMut` implement `Deref`), [[drop]].
