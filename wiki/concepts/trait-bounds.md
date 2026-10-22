---
type: concept
sources: [raw/ch10-generics-traits-lifetimes.md]
updated: 2026-10-22
---
# Trait bounds

A trait bound restricts a generic type to those implementing certain [[traits]], so the body may use that behavior. The compiler then verifies every concrete type, moving errors that dynamic languages hit at runtime to compile time (src: [[ch10-generics-traits-lifetimes]]).

## Forms

- `impl Trait` in parameter position: `fn notify(item: &impl Summary)`. Sugar for the bound form; each `impl Trait` parameter may be a different type.
- Bound syntax: `fn notify<T: Summary>(item: &T)`. Needed to force two parameters to be the same type: `(a: &T, b: &T)`.
- Multiple bounds with `+`: `T: Summary + Display` or `&(impl Summary + Display)`.
- `where` clauses keep signatures readable: `fn f<T, U>(t: &T, u: &U) -> i32 where T: Display + Clone, U: Clone + Debug`.
- Return position: `-> impl Summary` hides the concrete type (useful for closures and iterators), but the function must return one single type. Returning either of two types needs trait objects (ch18).
- Conditional methods: `impl<T: Display + PartialOrd> Pair<T> { fn cmp_display(&self) {...} }` exists only when `T` meets the bounds, while `new` lives in a plain `impl<T> Pair<T>`.
- Blanket implementations are a trait impl conditioned on a bound (see [[traits]]).

(src for all: [[ch10-generics-traits-lifetimes]])

## Example

```rust
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];
    for item in list { if item > largest { largest = item; } }
    largest
}
```

## Common mistakes

- Using `>` on an unbounded `T` (E0369); the compiler suggests `T: PartialOrd`.
- Using `impl Trait` return with `if`/`else` returning different types.

## Relations

[[generics]], [[traits]], [[monomorphization]], [[lifetimes]] (declared in the same `<>` list, e.g. `<'a, T>`).
