---
type: concept
sources: [raw/ch10-generics-traits-lifetimes.md]
updated: 2026-10-22
---
# Traits

A trait defines functionality a type has and can share with other types: a group of method signatures describing a behavior. Similar to interfaces in other languages, with differences (src: [[ch10-generics-traits-lifetimes]]).

## Rules

- Define with `trait`, method signatures ending in `;`. Mark `pub` to export. The compiler enforces that implementors provide exactly those signatures.
- Implement with `impl TraitName for Type { ... }`. To call trait methods, the trait must be in scope (`use aggregator::Summary`).
- **Orphan rule (coherence):** you can implement a trait on a type only if the trait or the type is local to your crate. `Summary` on `Vec<T>` is fine; `Display` on `Vec<T>` is not. It stops two crates implementing the same trait for the same type. (src: [[ch10-generics-traits-lifetimes]])
- **Default implementations:** give a method a body in the trait; implementors keep it with an empty `impl Summary for X {}` or override it with the same syntax. Defaults may call other methods of the trait, even required ones. An overriding implementation cannot call the default.
- **Blanket implementations:** implement a trait for every type meeting a bound, e.g. `impl<T: Display> ToString for T`, which is why `3.to_string()` works.
- Traits can appear as parameters/returns with `impl Trait`; see [[trait-bounds]].

## Example

```rust
pub trait Summary {
    fn summarize_author(&self) -> String;
    fn summarize(&self) -> String {
        format!("(Read more from {}...)", self.summarize_author())
    }
}
```

## Common mistakes

- Implementing an external trait on an external type (orphan rule).
- Forgetting to import the trait before calling its method.
- Passing a type that does not implement the trait (`String`, `i32` to a `&impl Summary`).

## Relations

[[trait-bounds]], [[generics]]. Trait objects (dynamic dispatch) come in chapter 18.
