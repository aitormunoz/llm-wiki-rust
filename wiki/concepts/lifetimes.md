---
type: concept
sources: [raw/ch10-generics-traits-lifetimes.md]
updated: 2026-10-22
---
# Lifetimes

Every reference has a lifetime, the scope for which it is valid. Lifetimes are a kind of generic: instead of ensuring a type has some behavior, they ensure references stay valid as long as needed. Usually inferred; annotated when relationships between references are ambiguous (src: [[ch10-generics-traits-lifetimes]]). They exist to prevent dangling references; see [[borrowing]].

## Rules

- The **borrow checker** compares scopes: if the referent's lifetime is shorter than the reference's, the code is rejected. `r = &x` with `x` in a shorter inner scope gives E0597 "does not live long enough". (src: [[ch10-generics-traits-lifetimes]])
- Annotation syntax: apostrophe plus short lowercase name, after the `&`: `&'a i32`, `&'a mut i32`. Declared in `<>` like type parameters.
- Annotations **do not change how long anything lives**; they describe relations so the borrow checker can reject violating callers.
- `fn longest<'a>(x: &'a str, y: &'a str) -> &'a str`: the result lives as long as the **smaller** of the two argument lifetimes. Using the result after the shorter-lived argument is gone is an error, even if the compiler-invisible runtime path would be fine.
- Annotate only what is related: if the function always returns `x`, `y` needs no lifetime.
- A returned reference must derive from a parameter. Returning a reference to a local fails (E0515); return an owned value instead.
- Structs holding references need a lifetime on each: `struct ImportantExcerpt<'a> { part: &'a str }`. An instance cannot outlive the reference it holds.
- In `impl`, declare lifetimes after `impl` and use them after the type name: `impl<'a> ImportantExcerpt<'a>`. Method signatures often need nothing more thanks to [[lifetime-elision]].
- `'static`: the reference *can* live for the whole program; all string literals are `&'static str` (stored in the binary). Error messages suggesting `'static` usually signal a dangling reference or lifetime mismatch; fix that instead.
- Lifetime and type parameters share one list: `fn f<'a, T>(x: &'a str, ann: T) -> &'a str where T: Display`.

## Example

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
```

## Common mistakes

- Missing specifier on a signature with several reference inputs and a reference output (E0106).
- Annotating the return with `'a` while it refers to a local (E0515).
- Reaching for `'static` to silence an error.

> [!note] Scope vs last use
> Chapter 10 explains lifetimes with block scopes (the diagrams `'a`, `'b`), while [[borrowing]] (ch4) notes that a reference's scope ends at its last use. Not a contradiction: the lexical picture is the teaching model, last use is how the borrow checker actually shortens it. Ch10 does not state the last-use refinement itself.

## Relations

[[borrowing]], [[lifetime-elision]], [[generics]], [[trait-bounds]], [[ownership]] (the owner's scope bounds every borrow).
