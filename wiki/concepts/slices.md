---
type: concept
sources: [raw/ch04-understanding-ownership.md, raw/ch10-generics-traits-lifetimes.md]
updated: 2026-10-22
---
# Slices

A slice is a reference to a contiguous sequence of elements in a collection. It does not own the data, and internally stores a start pointer and a length (src: [[ch04-understanding-ownership]]). It is a kind of [[borrowing]].

## Rules and forms

- String slice `&str`: `&s[0..5]`; `..` can omit start (`&s[..2]`), end (`&s[3..]`) or both (`&s[..]`). Range end is exclusive. (src: [[ch04-understanding-ownership]])
- Indices must fall on UTF-8 character boundaries or the program panics. (src: [[ch04-understanding-ownership]])
- String literals are `&str`, slices into the binary, which is why they are immutable.
- Other slices: `&a[1..3]` of an array has type `&[i32]`.
- Prefer `&str` over `&String` for parameters: it accepts `&String`, `&str` and literals via deref coercion (explained in a later chapter). (src: [[ch04-understanding-ownership]])

## Why slices

A `first_word` returning a `usize` index can go stale after `s.clear()` with no error. Returning `&str` ties the result to the string, so `clear()` (needs `&mut`) while the slice is alive is a compile error (E0502). (src: [[ch04-understanding-ownership]])

```rust
fn first_word(s: &str) -> &str {
    for (i, &b) in s.as_bytes().iter().enumerate() {
        if b == b' ' { return &s[0..i]; }
    }
    &s[..]
}
```

`first_word` needs no lifetime annotations because of [[lifetime-elision]]: its single input lifetime is assigned to the returned `&str`. (src: [[ch10-generics-traits-lifetimes]])

## Common mistakes

- Clearing/mutating the owner while a slice is still used.
- Slicing in the middle of a multibyte character.

## Relations

[[borrowing]], [[ownership]], [[lifetime-elision]].
