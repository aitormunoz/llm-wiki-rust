---
type: source
sources: [raw/ch15-smart-pointers.md]
updated: 2026-10-22
---
# Chapter 15: Smart Pointers

Chapter 15 of The Rust Programming Language. Smart pointers are data structures that act like pointers but carry extra metadata and capabilities, and often own their data, unlike plain references. They are usually structs implementing `Deref` and `Drop` (src: [[ch15-smart-pointers]]). See [[smart-pointers]].

## Structure

1. **`Box<T>`.** Heap allocation with no overhead; used for types of unknown size (recursive types such as the cons list), moving large data without copying, and trait objects. See [[box]].
2. **The `Deref` trait.** Customizes `*`; `MyBox<T>` example; deref coercion (`&MyBox<String>` to `&str`) and the three mutability cases with `DerefMut`. See [[deref-and-deref-coercion]].
3. **The `Drop` trait.** Cleanup code run at end of scope, reverse order of declaration, early drop with `std::mem::drop`. See [[drop]].
4. **`Rc<T>`.** Reference counting for multiple owners in single-threaded code; `Rc::clone` only bumps the count. See [[rc]].
5. **`RefCell<T>` and interior mutability.** Borrowing rules checked at runtime, mock object example, `Rc<RefCell<T>>` for shared mutable data. See [[refcell-and-interior-mutability]].
6. **Reference cycles and `Weak<T>`.** `Rc` + `RefCell` can leak memory; `Weak<T>` and `upgrade` break cycles in a parent/child tree. See [[reference-cycles-and-weak]].

## Key takeaways

- `Box<T>`: known size, heap data, single owner, borrow rules checked at compile time. `Rc<T>`: multiple owners, immutable access, compile-time checks. `RefCell<T>`: single owner, mutable borrows checked at runtime.
- Memory leaks are memory safe in Rust; the language does not guarantee against them.
- `Rc<T>` and `RefCell<T>` are single-threaded only; ch16 covers the thread-safe versions (`Mutex<T>` is the counterpart of `RefCell<T>`).
- Forward references: trait objects (ch18), associated types and unsafe (ch20), concurrency (ch16).

## Concept pages

New: [[smart-pointers]], [[box]], [[deref-and-deref-coercion]], [[drop]], [[rc]], [[refcell-and-interior-mutability]], [[reference-cycles-and-weak]]. Updated: [[ownership]], [[borrowing]], [[move-copy-clone]], [[stack-and-heap]], [[slices]], [[traits]].
