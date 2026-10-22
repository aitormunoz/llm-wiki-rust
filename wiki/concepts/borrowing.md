---
type: concept
sources: [raw/ch04-understanding-ownership.md, raw/ch10-generics-traits-lifetimes.md, raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Borrowing and references

A reference (`&T`) is an address that lets you use a value owned by someone else without taking ownership. Creating one is called borrowing. Unlike a raw pointer, a reference is guaranteed to point to a valid value for its whole life (src: [[ch04-understanding-ownership]]).

## Rules

- At any time: either one mutable reference (`&mut T`) or any number of immutable references (`&T`).
- References must always be valid. (src: [[ch04-understanding-ownership]])

> [!warning] Conflict
> ch4 presents these rules as errors caught by the compiler. ch15 shows that `RefCell<T>` enforces the same rules at runtime and panics on a violation (src: [[ch15-smart-pointers]]). Resolved: compile-time checking is Rust's default and still holds for `&`/`&mut`; the runtime variant is an opt-in for cases the conservative compiler rejects, see [[refcell-and-interior-mutability]]. The rules themselves do not change.

Details:
- References are immutable by default; mutation needs `&mut` and a `mut` variable (E0596 otherwise).
- A reference's scope runs from creation to its last use, not to the end of the block, so a `&mut` is fine after the last use of earlier `&` references. (src: [[ch04-understanding-ownership]])
- A new inner block can end a `&mut` early to allow another.
- The one-`&mut` rule prevents data races at compile time: two or more pointers to the same data, at least one writing, with no synchronization. (src: [[ch04-understanding-ownership]]) ch16 builds on this: a spawned thread may outlive the borrowed data, so closures passed to `thread::spawn` must `move` what they use (E0373 otherwise), see [[threads]]. (src: [[ch16-fearless-concurrency]])
- A dropped reference never drops the referent, since it does not own it.

## Dangling references

The compiler guarantees data does not go out of scope before references to it. Returning `&s` for a local `s` fails (E0106 missing lifetime specifier, E0515). Fix: return the owned `String`. The message mentions lifetimes, which ch10 explains: the borrow checker rejects a reference whose referent has a shorter lifetime (E0597 "does not live long enough"), and a function returning a reference must tie it to a parameter via annotations; see [[lifetimes]] and [[lifetime-elision]]. (src: [[ch04-understanding-ownership]], [[ch10-generics-traits-lifetimes]])

## Example

```rust
fn calculate_length(s: &String) -> usize { s.len() }
let mut s = String::from("hello");
let len = calculate_length(&s);
change(&mut s);
fn change(t: &mut String) { t.push_str(", world"); }
```

## Common mistakes

- Taking `&mut` while an `&` is still used later (E0502), or two `&mut` at once (E0499).
- Mutating through `&T`.

## Relations

[[ownership]], [[lifetimes]], [[slices]] (a slice is a kind of reference), [[move-copy-clone]]. The dereference operator `*` follows a reference to its value; `assert_eq!(5, y)` with `y: &i32` fails (E0277) while `*y` works. Custom types can support `*` via `Deref`, see [[deref-and-deref-coercion]]. Cross-thread sharing is done with [[mutex]] and [[arc]] rather than plain references. Interior mutability ([[refcell-and-interior-mutability]]) mutates behind an immutable value with runtime-checked borrows. (src: [[ch15-smart-pointers]])
