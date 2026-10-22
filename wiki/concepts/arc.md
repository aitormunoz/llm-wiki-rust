---
type: concept
sources: [raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Arc<T>

`Arc<T>` is atomic reference counting: like [[rc]], it gives multiple owners of one value, but its count changes use atomic operations, so it is safe across threads (src: [[ch16-fearless-concurrency]]).

## Rules

- Same API as `Rc<T>`: `Arc::new`, `Arc::clone(&a)`. Import from `std::sync::Arc`.
- `Rc<T>` fails in `thread::spawn` because it is not `Send` (E0277): unsynchronised count updates could give wrong counts, leaks or early drops. See [[send-and-sync]].
- Why not make everything atomic? Thread safety costs performance you only want to pay when needed. This is why `Rc` stays the single-threaded default.
- `Arc<T>` alone gives only shared immutable access; pair it with [[mutex]] (`Arc<Mutex<T>>`) to mutate.

## Example

```rust
use std::sync::Arc;
let a = Arc::new(5);
let b = Arc::clone(&a); // send b to another thread
```

## Common mistakes

- Using `Rc` with threads.
- Expecting `Arc` to allow mutation without a `Mutex`.

## Relations

[[rc]], [[mutex]], [[smart-pointers]], [[ownership]] (multiple ownership across threads), [[threads]].
