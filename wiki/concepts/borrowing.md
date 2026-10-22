---
type: concept
sources: [raw/ch04-understanding-ownership.md, raw/2018-ch04-02-references-and-borrowing.md, raw/ch10-generics-traits-lifetimes.md, raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Borrowing and references

A reference (`&T`) is an address that lets you use a value owned by someone else without taking ownership. Creating one is called borrowing; taking references as function parameters means the function need not return the value to give ownership back (src: [[2018-ch04-02-references-and-borrowing]]). Unlike a raw pointer, a reference is guaranteed to point to a valid value for its whole life (src: [[ch04-understanding-ownership]]).

## Rules

- At any time: either one mutable reference (`&mut T`) or any number of immutable references (`&T`).
- References must always be valid. (src: [[ch04-understanding-ownership]])

> [!warning] Conflict
> ch4 presents these rules as errors caught by the compiler. ch15 shows that `RefCell<T>` enforces the same rules at runtime and panics on a violation (src: [[ch15-smart-pointers]]). Resolved: compile-time checking is Rust's default and still holds for `&`/`&mut`; the runtime variant is an opt-in for cases the conservative compiler rejects, see [[refcell-and-interior-mutability]]. The rules themselves do not change.

Details:
- References are immutable by default; mutation needs `&mut` and a `mut` variable (E0596 otherwise). (src: [[2018-ch04-02-references-and-borrowing]])
- A reference's scope runs from creation to its last use, not to the end of the block, so a `&mut` is fine after the last use of earlier `&` references. (src: [[ch04-understanding-ownership]])

> [!warning] Conflict
> The 2018-era snapshot shows the borrows ending at the closing brace ("first borrow ends here" at `}`), so `let r1 = &s; let r2 = &s; let r3 = &mut s;` fails there even if `r1`/`r2` are never used again (src: [[2018-ch04-02-references-and-borrowing]]). The current ch4 says a reference's scope ends at its last use (src: [[ch04-understanding-ownership]]). Resolved: the current chapter wins; the old text describes pre-non-lexical-lifetimes behavior (my inference from the two texts, the 2018 source does not name the change). The one-`&mut`-or-many-`&` rules are the same in both.

- A new inner block can end a `&mut` early to allow another; this was the only way to do it in the older lexical model. (src: [[2018-ch04-02-references-and-borrowing]])
- The one-`&mut` rule prevents [[data-races]] at compile time. (src: [[ch04-understanding-ownership]]) ch16 builds on this: a spawned thread may outlive the borrowed data, so closures passed to `thread::spawn` must `move` what they use (E0373 otherwise), see [[threads]]. (src: [[ch16-fearless-concurrency]])
- A dropped reference never drops the referent, since it does not own it.

## Dangling references

The compiler guarantees data does not go out of scope before references to it. Returning `&s` for a local `s` fails (E0106 missing lifetime specifier, E0515). Fix: return the owned `String`. The message mentions lifetimes, which ch10 explains: the borrow checker rejects a reference whose referent has a shorter lifetime (E0597 "does not live long enough"), and a function returning a reference must tie it to a parameter via annotations; see [[lifetimes]] and [[lifetime-elision]]. (src: [[ch04-understanding-ownership]], [[ch10-generics-traits-lifetimes]]) The compiler output for this error suggests a `'static` lifetime in both the 2018 snapshot and the current chapter; the current text calls it uncommon outside `const`/`static` values and points to returning an owned value instead, which is the fix (src: [[ch04-understanding-ownership]], [[2018-ch04-02-references-and-borrowing]]). See [[dangling-references]] for the full picture.

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
