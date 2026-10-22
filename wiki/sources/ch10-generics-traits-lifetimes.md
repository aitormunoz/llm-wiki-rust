---
type: source
sources: [raw/ch10-generics-traits-lifetimes.md]
updated: 2026-10-22
---
# Chapter 10: Generic Types, Traits, and Lifetimes

Chapter 10 of The Rust Programming Language. It shows how to remove duplication with generics, constrain generics with traits, and keep references valid with lifetimes, all checked at compile time with no runtime cost (src: [[ch10-generics-traits-lifetimes]]).

## Structure

1. **Removing duplication by extracting a function.** Identify duplicate code, extract it into a function, call it. Generics apply the same idea to types. See [[generics]].
2. **Generic data types.** In functions (`fn largest<T>`), structs (`Point<T, U>`), enums (`Option<T>`, `Result<T, E>`) and methods (`impl<T> Point<T>`, `impl Point<f32>`). Cost: none, thanks to [[monomorphization]]. See [[generics]].
3. **Traits.** Defining, implementing, default methods, orphan rule, `impl Trait` parameters and returns, blanket implementations. See [[traits]], [[trait-bounds]].
4. **Lifetimes.** Dangling references, the borrow checker, annotations in functions, structs and methods, elision rules, `'static`. See [[lifetimes]], [[lifetime-elision]].
5. **All together.** `longest_with_an_announcement<'a, T>` mixes a lifetime, a generic and a `where` bound.

## Key takeaways

- Generics replace concrete types with placeholders; trait bounds say what those placeholders must be able to do.
- Rust compiles generics into concrete code per type used, so they cost nothing at runtime.
- You can implement a trait on a type only if the trait or the type is local to your crate (orphan rule).
- Lifetimes are a kind of generic: annotations describe how reference lifetimes relate, they never change how long anything lives.
- Elision rules let most signatures omit lifetimes; when they cannot decide, you get E0106.
- Forward references: trait objects (ch18), closures and iterators (ch13), tests (next chapter).

## Concept pages

[[generics]], [[monomorphization]], [[traits]], [[trait-bounds]], [[lifetimes]], [[lifetime-elision]]. Updated: [[borrowing]], [[slices]].
