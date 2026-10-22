---
type: concept
sources: [raw/ch04-understanding-ownership.md, raw/ch10-generics-traits-lifetimes.md, raw/2018-ch04-02-references-and-borrowing.md]
updated: 2026-10-22
---
# Dangling references

A dangling pointer references memory that may have been given to someone else, typically because the memory was freed while the pointer was kept. Rust's compiler guarantees references are never dangling: the data cannot go out of scope before the reference to it does (src: [[ch04-understanding-ownership]]). Preventing them is the main aim of [[lifetimes]] (src: [[ch10-generics-traits-lifetimes]]).

## Rules

- A reference must not outlive its referent. The [[lifetimes]] borrow checker compares scopes and rejects code where the referent's lifetime is shorter (E0597 "does not live long enough"). (src: [[ch10-generics-traits-lifetimes]])
- Returning a reference to a local fails: `fn dangle() -> &String { let s = String::from("hello"); &s }` gives E0106 (missing lifetime specifier) and E0515 (cannot return reference to local variable). The fix is to return the owned `String`, moving ownership out. (src: [[ch04-understanding-ownership]])
- The E0106 output suggests a `'static` lifetime, but the chapter calls that uncommon unless returning a borrowed value from a `const` or `static`, and points to returning an owned value. (src: [[ch04-understanding-ownership]]) The 2018 snapshot gives the same suggestion (src: [[2018-ch04-02-references-and-borrowing]]), and ch10 says such suggestions come from trying to create a dangling reference, so fix that rather than add `'static`. (src: [[ch10-generics-traits-lifetimes]])

## Example

```rust
fn main() {
    let r;
    {
        let x = 5;
        r = &x; // error[E0597]: `x` does not live long enough
    }
    println!("r: {r}");
}
```

## Common mistakes

- Returning `&local` instead of the owned value.
- Silencing the error with `'static`.

## Relations

[[borrowing]], [[lifetimes]], [[lifetime-elision]], [[ownership]] (the owner's scope bounds every borrow).
