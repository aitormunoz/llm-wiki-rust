---
type: concept
sources: [raw/ch04-understanding-ownership.md, raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Ownership

Ownership is the set of rules by which a Rust program manages memory. The compiler checks them, so a violation is a compile error, and there is no runtime cost and no garbage collector (src: [[ch04-understanding-ownership]]). Its main purpose is managing heap data (see [[stack-and-heap]]).

## Rules

- Each value has an owner.
- There is only one owner at a time (by default; see the callout below).
- When the owner goes out of scope, the value is dropped. (src: [[ch04-understanding-ownership]])

> [!warning] Conflict
> ch4 states there is only one owner at a time. ch15 shows `Rc<T>` explicitly enabling multiple owners of one value, freed when the last owner goes (src: [[ch15-smart-pointers]]). Resolved: the single-owner rule is the default for ordinary values and compile-time checked; multiple ownership is opt-in via [[rc]], with the count checked at runtime. Rule 3 still holds in spirit: the value is dropped when the last owner goes out of scope. ch16 extends this across threads with [[arc]] (atomic count) while [[channels]] keep single ownership by moving values between threads (src: [[ch16-fearless-concurrency]]).

A scope runs from a variable's declaration to the end of the enclosing block. At the closing brace Rust automatically calls `drop`, where the type's author frees resources. This is the same idea as RAII in C++. (src: [[ch04-understanding-ownership]])

Assigning a new value to an existing variable drops the old value immediately. (src: [[ch04-understanding-ownership]])

The `Drop` trait is the hook behind that call ([[drop]]). Smart pointers such as [[box]] and [[rc]] own the data they point to ([[smart-pointers]]).

## Transferring ownership

Assignment, passing to a function and returning from a function all move ownership of non-`Copy` values; see [[move-copy-clone]]. Returning a value moves it out to the caller, so it is not dropped. Passing a value in and handing it back is tedious, which is why [[borrowing]] exists. (src: [[ch04-understanding-ownership]])

```rust
fn main() {
    let s = String::from("hello");
    takes_ownership(s);      // s moved; unusable afterwards
}
fn takes_ownership(some_string: String) {
    println!("{some_string}");
} // dropped here
```

## Relations

- [[stack-and-heap]]: why ownership matters.
- [[move-copy-clone]]: what happens on assignment.
- [[borrowing]] and [[slices]]: using data without owning it.
- [[smart-pointers]], [[box]], [[rc]], [[drop]]: owning pointers, shared ownership and custom cleanup (ch15).
- [[threads]], [[channels]], [[arc]], [[send-and-sync]]: ownership across threads (ch16); `move` closures and `send` transfer ownership, and the checker rejects later use.

## Common mistakes

- Using a variable after it was moved (E0382 "borrow of moved value").
- Assuming a reference's lifetime can outlive its owner (see [[dangling-references]]).
