---
type: concept
sources: [raw/ch04-understanding-ownership.md, raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Move, Copy and Clone

Three ways a value can be duplicated or transferred on assignment or function call. See [[ownership]] for the rules they implement (src: [[ch04-understanding-ownership]]).

## Move

A `String` is a pointer, length and capacity on the stack plus bytes on the heap. `let s2 = s1;` copies only the stack part and invalidates `s1`, so only `s2` frees the heap memory. This avoids a double free. It is like a shallow copy plus invalidation. (src: [[ch04-understanding-ownership]])

Design consequence: Rust never automatically deep-copies, so automatic copying is cheap. (src: [[ch04-understanding-ownership]])

## Clone

`s1.clone()` explicitly copies heap data too. A `clone` call signals possibly expensive arbitrary code. (src: [[ch04-understanding-ownership]]) Exception by convention: `Rc::clone(&a)` only increments a reference count and copies no data, so it is cheap; it is written `Rc::clone` rather than `a.clone()` to keep it visually distinct from deep copies. See [[rc]]; `Arc::clone` is the same, and `tx.clone()` on a channel transmitter makes another producer ([[arc]], [[channels]]). (src: [[ch15-smart-pointers]], [[ch16-fearless-concurrency]])

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
- Using a value after moving it into a thread closure or sending it down a channel ([[threads]], [[channels]]).

## Relations

[[ownership]], [[stack-and-heap]], [[borrowing]], [[rc]], [[drop]].
