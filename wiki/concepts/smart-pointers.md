---
type: concept
sources: [raw/ch15-smart-pointers.md]
updated: 2026-10-22
---
# Smart pointers

A pointer is a variable holding a memory address. A reference (`&`) is the most common kind: it only borrows and has no overhead. A smart pointer is a data structure that acts like a pointer but also has metadata and extra capabilities, and in many cases owns the data it points to (src: [[ch15-smart-pointers]]).

## Rules

- Usually implemented as a struct that implements [[deref-and-deref-coercion]] (behave like a reference) and [[drop]] (custom cleanup at end of scope). (src: [[ch15-smart-pointers]])
- Standard library examples: [[box]] (heap allocation), [[rc]] (multiple owners), `Ref<T>`/`RefMut<T>` returned by `RefCell<T>` ([[refcell-and-interior-mutability]]), `Weak<T>` ([[reference-cycles-and-weak]]). `String` also implements `Deref` (to `&str`).
- Libraries can define their own.

## Choosing one

| | `Box<T>` | `Rc<T>` | `RefCell<T>` |
|---|---|---|---|
| Owners | one | many | one |
| Borrows checked | compile time (mut or immut) | compile time (immut only) | runtime (mut or immut) |

(src: [[ch15-smart-pointers]])

## Relations

[[ownership]] (smart pointers own data, references do not), [[borrowing]], [[traits]], [[stack-and-heap]].
