---
type: concept
sources: [raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# RefCell<T> and interior mutability

Interior mutability is a pattern that lets you mutate data even when there are immutable references to it. `RefCell<T>` implements it by checking the [[borrowing]] rules at runtime instead of compile time; the `unsafe` code is wrapped in a safe API (src: [[ch15-smart-pointers]]).

## Rules

- `RefCell<T>` has a single owner (like [[box]], unlike [[rc]]). Violating the rules causes a `panic!` (`already borrowed: BorrowMutError`), not a compile error.
- `borrow()` returns `Ref<T>`, `borrow_mut()` returns `RefMut<T>`; both implement `Deref` ([[deref-and-deref-coercion]]). `RefCell` counts active ones: many `Ref` or one `RefMut`, like the compile-time rules.
- Why: static analysis is conservative and may reject a correct program; `RefCell` is for when you know the code is correct but the compiler cannot prove it.
- Costs: bugs found at runtime (maybe in production) and a small performance penalty.
- Single-threaded only: `RefCell<T>` is not `Sync` because its runtime borrow tracking is not thread-safe. `Mutex<T>` is the thread-safe equivalent, and `Arc<Mutex<T>>` mirrors `Rc<RefCell<T>>`. (src: [[ch16-fearless-concurrency]], see [[mutex]], [[send-and-sync]])
- `Rc<RefCell<T>>` gives multiple owners of mutable data: mutate once, all owners see it. (src: [[ch15-smart-pointers]])
- Typical use: a mock object in tests whose trait method takes `&self` but must record calls.

## Example

```rust
use std::cell::RefCell;
struct MockMessenger { sent_messages: RefCell<Vec<String>> }
impl Messenger for MockMessenger {
    fn send(&self, message: &str) {
        self.sent_messages.borrow_mut().push(String::from(message));
    }
}
// assert_eq!(mock.sent_messages.borrow().len(), 1);
```

## Common mistakes

- Holding two `borrow_mut()` results at once: compiles, panics at runtime.
- Using `RefCell` where a plain `&mut` would work; compile-time checking is the better default.
- Using it across threads.

## Relations

[[borrowing]], [[rc]], [[smart-pointers]], [[reference-cycles-and-weak]], [[mutex]] (thread-safe analogue), [[traits]] (the `Messenger` trait is why `&mut self` could not be used).
