---
type: concept
sources: [raw/ch04-understanding-ownership.md]
updated: 2026-10-22
---
# Borrowing and references

A reference (`&T`) is an address that lets you use a value owned by someone else without taking ownership. Creating one is called borrowing. Unlike a raw pointer, a reference is guaranteed to point to a valid value for its whole life (src: [[ch04-understanding-ownership]]).

## Rules

- At any time: either one mutable reference (`&mut T`) or any number of immutable references (`&T`).
- References must always be valid. (src: [[ch04-understanding-ownership]])

Details:
- References are immutable by default; mutation needs `&mut` and a `mut` variable (E0596 otherwise).
- A reference's scope runs from creation to its last use, not to the end of the block, so a `&mut` is fine after the last use of earlier `&` references. (src: [[ch04-understanding-ownership]])
- A new inner block can end a `&mut` early to allow another.
- The one-`&mut` rule prevents data races at compile time: two or more pointers to the same data, at least one writing, with no synchronization. (src: [[ch04-understanding-ownership]])
- A dropped reference never drops the referent, since it does not own it.

## Dangling references

The compiler guarantees data does not go out of scope before references to it. Returning `&s` for a local `s` fails (E0106 missing lifetime specifier, E0515). Fix: return the owned `String`. The message mentions lifetimes, which the chapter defers to ch10. (src: [[ch04-understanding-ownership]])

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

[[ownership]], [[slices]] (a slice is a kind of reference), [[move-copy-clone]]. Dereference operator `*` is the inverse of `&` (details in later chapters).
