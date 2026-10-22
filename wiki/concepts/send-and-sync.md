---
type: concept
sources: [raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Send and Sync

`Send` and `Sync` are marker traits (in `std::marker`, no methods) that tell the compiler which types are safe to use across threads. They are among the few concurrency concepts built into the language rather than the standard library (src: [[ch16-fearless-concurrency]]).

## Rules

- `Send`: ownership of values of the type can be transferred between threads. Almost all types are `Send`; `Rc<T>` is not (two threads might update the count at once). `thread::spawn` requires its closure to be `Send`.
- `Sync`: the type can be referenced from multiple threads. `T` is `Sync` if `&T` is `Send`.
- Not `Sync`: `Rc<T>`, `RefCell<T>` and the `Cell<T>` family (runtime borrow checking is not thread-safe). `Mutex<T>` is `Sync`. (src: [[ch16-fearless-concurrency]])
- Auto traits: a type made entirely of `Send` (or `Sync`) types is `Send` (or `Sync`). Primitives are both, except raw pointers (ch20).
- Implementing them by hand requires `unsafe` code and care; see the Rustonomicon.

## Example

The error from using `Rc` in a thread:

```text
error[E0277]: `Rc<Mutex<i32>>` cannot be sent between threads safely
the trait `Send` is not implemented for `Rc<Mutex<i32>>`
```

## Common mistakes

- Using `Rc`/`RefCell` in multithreaded code; switch to [[arc]]/[[mutex]].
- Implementing `Send`/`Sync` manually without upholding the invariants.

## Relations

[[traits]] (marker traits, bounds on `spawn`), [[trait-bounds]], [[rc]], [[refcell-and-interior-mutability]], [[threads]], [[mutex]].
