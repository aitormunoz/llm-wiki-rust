---
type: concept
sources: [raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Rc<T>

`Rc<T>` is a reference-counting [[smart-pointers]] that enables multiple owners of the same heap data. It tracks how many references exist and cleans the data up when the count reaches zero (src: [[ch15-smart-pointers]]).

## Rules

- Use it when several parts of the program must read data and you cannot know at compile time which finishes last. If you knew, that part would simply own it. Example: graph nodes owned by several edges.
- Not in the prelude: `use std::rc::Rc;`.
- `Rc::clone(&a)` increments the strong count and does **not** deep copy; `a.clone()` works too, but `Rc::clone` is the convention so deep-copy clones stand out when hunting performance problems. (src: [[ch15-smart-pointers]])
- `Rc::strong_count(&a)` reads the count. It decrements automatically when an `Rc` is dropped, through [[drop]].
- Only gives immutable access (shared mutable access would break the borrowing rules); combine with `RefCell<T>` for mutation: `Rc<RefCell<T>>`, see [[refcell-and-interior-mutability]].
- Single-threaded only: `Rc<T>` is not `Send`, so using it in `thread::spawn` is a compile error (E0277), because its count updates are not atomic. The thread-safe version is [[arc]]. (src: [[ch16-fearless-concurrency]], see [[send-and-sync]])
- Cycles of `Rc` leak memory; see [[reference-cycles-and-weak]].

## Example

```rust
use std::rc::Rc;
enum List { Cons(i32, Rc<List>), Nil }
use crate::List::{Cons, Nil};
let a = Rc::new(Cons(5, Rc::new(Cons(10, Rc::new(Nil)))));
let b = Cons(3, Rc::clone(&a)); // count = 2
let c = Cons(4, Rc::clone(&a)); // count = 3
```

## Common mistakes

- Sending an `Rc` to another thread; use `Arc`.
- Using `Box` and expecting two lists to share a tail (value moved, E0382); see [[box]].
- Assuming `Rc::clone` is expensive, or `.clone()` on the inner data is cheap.
- Trying to mutate through an `Rc` without `RefCell`.

## Relations

[[ownership]] (relaxes the single-owner rule), [[move-copy-clone]], [[borrowing]], [[smart-pointers]], [[arc]] (atomic counterpart), [[send-and-sync]]. Comparison: [[rc-vs-arc-and-refcell-vs-mutex]].
