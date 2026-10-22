---
type: concept
sources: [raw/ch15-smart-pointers.md]
updated: 2026-10-22
---
# Drop

The `Drop` trait lets you run custom code when a value goes out of scope, such as releasing files, sockets, locks or heap memory. The compiler inserts the call automatically, so you cannot forget cleanup (src: [[ch15-smart-pointers]]). It is the mechanism behind "dropped when the owner goes out of scope" in [[ownership]].

## Rules

- Implement one method: `fn drop(&mut self)`. `Drop` is in the prelude.
- Values drop in reverse order of creation. (src: [[ch15-smart-pointers]])
- You cannot call `x.drop()` yourself (E0040): it would be called again at scope end, a double free. To drop early, call `std::mem::drop(x)` (in the prelude), which takes ownership. Useful for releasing a lock early.
- Ownership guarantees `drop` runs exactly once and never on a value still in use.
- `Rc<T>` decrements its count in its `Drop` impl ([[rc]]); `Box<T>` frees its heap memory in its.

## Example

```rust
struct CustomSmartPointer { data: String }
impl Drop for CustomSmartPointer {
    fn drop(&mut self) { println!("Dropping `{}`!", self.data); }
}
let c = CustomSmartPointer { data: String::from("some data") };
drop(c); // early cleanup
```

## Common mistakes

- Calling `.drop()` directly.
- Confusing the `Drop::drop` method with the `std::mem::drop` function.

## Relations

[[ownership]], [[smart-pointers]], [[deref-and-deref-coercion]], [[move-copy-clone]] (a `Drop` type cannot be `Copy`), [[traits]].
