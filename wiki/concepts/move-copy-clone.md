---
type: concept
sources: [raw/ch04-understanding-ownership.md]
updated: 2026-10-22
---
# Move, Copy and Clone

Three ways a value can be duplicated or transferred on assignment or function call. See [[ownership]] for the rules they implement (src: [[ch04-understanding-ownership]]).

## Move

A `String` is a pointer, length and capacity on the stack plus bytes on the heap. `let s2 = s1;` copies only the stack part and invalidates `s1`, so only `s2` frees the heap memory. This avoids a double free. It is like a shallow copy plus invalidation. (src: [[ch04-understanding-ownership]])

Design consequence: Rust never automatically deep-copies, so automatic copying is cheap. (src: [[ch04-understanding-ownership]])

## Clone

`s1.clone()` explicitly copies heap data too. A `clone` call signals possibly expensive arbitrary code. (src: [[ch04-understanding-ownership]])

## Copy

Types stored entirely on the stack can implement the `Copy` trait; assignment then copies and the original stays valid. A type cannot be `Copy` if it, or any part, implements `Drop`. Copy types: integers, `bool`, floats, `char`, and tuples of only `Copy` types (`(i32, i32)` yes, `(i32, String)` no). (src: [[ch04-understanding-ownership]])

```rust
let s1 = String::from("hello");
let s2 = s1.clone();   // both valid
let x = 5;
let y = x;             // Copy: both valid
```

## Common mistakes

- Using `s1` after `let s2 = s1;` (E0382). Fix with `clone()` if the cost is acceptable, or borrow ([[borrowing]]).
- Passing a `String` to a function and then using it again.

## Relations

[[ownership]], [[stack-and-heap]], [[borrowing]].
