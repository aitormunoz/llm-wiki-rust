---
type: concept
sources: [raw/ch04-understanding-ownership.md, raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Data races

A data race is similar to a race condition and happens when three behaviors occur together: two or more pointers access the same data at the same time, at least one of them writes, and nothing synchronizes access. Data races cause undefined behavior and are hard to diagnose at runtime; Rust refuses to compile code that has them (src: [[ch04-understanding-ownership]]).

## Rules

- The compile-time defense is the [[borrowing]] rule: one `&mut` or many `&`, never both. That removes the "write while others access" combination. (src: [[ch04-understanding-ownership]])
- `Rc<T>` only hands out immutable references because multiple mutable references to one place could cause data races and inconsistencies. (src: [[ch15-smart-pointers]])
- [[refcell-and-interior-mutability]] moves the same rules to runtime, and the chapter says those runtime checks protect against data races, but `RefCell<T>` does not work across threads. (src: [[ch15-smart-pointers]])
- Across threads, ch16 states that Rust code using [[mutex]], [[arc]] and [[send-and-sync]] won't end up with data races or invalid references (src: [[ch16-fearless-concurrency]]). Deadlocks are not prevented (src: [[ch16-fearless-concurrency]]).

## Common mistakes

- Confusing data races (prevented at compile time) with deadlocks or general race conditions, which the type system does not rule out.

## Relations

[[borrowing]], [[threads]], [[mutex]], [[send-and-sync]], [[refcell-and-interior-mutability]], [[rc]].
