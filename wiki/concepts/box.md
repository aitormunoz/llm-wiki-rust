---
type: concept
sources: [raw/ch15-smart-pointers.md]
updated: 2026-10-22
---
# Box<T>

`Box<T>` is the simplest [[smart-pointers]]: it stores its data on the heap, and only a pointer stays on the stack. It has no overhead beyond the heap allocation and no other capabilities (src: [[ch15-smart-pointers]]). See [[stack-and-heap]].

## Rules

- Use it when (1) a type's size is unknown at compile time but an exact size is needed, (2) you want to move a large amount of data without copying it (only the pointer is copied), (3) you want to own a value known only by the trait it implements (trait object, ch18). (src: [[ch15-smart-pointers]])
- Dropping the box frees both the box and the heap data, via [[drop]]. It can be used like a reference thanks to [[deref-and-deref-coercion]].
- A `Box` has a fixed pointer size, so it breaks infinite size in recursive types: `enum List { Cons(i32, List) }` fails with E0072 "recursive type has infinite size"; `Cons(i32, Box<List>)` compiles. (src: [[ch15-smart-pointers]])
- A single value on the heap (`Box::new(5)`) is rarely useful.
- A box owns its content, so two `Box` lists cannot share a tail: the value is moved (E0382). Use [[rc]] for sharing.

## Example

```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
use crate::List::{Cons, Nil};
let list = Cons(1, Box::new(Cons(2, Box::new(Cons(3, Box::new(Nil))))));
```

## Common mistakes

- Defining a recursive type without indirection (E0072).
- Using a box for small stack values where it adds nothing.

## Relations

[[smart-pointers]], [[ownership]], [[stack-and-heap]], [[rc]], [[traits]] (trait objects, ch18). The size of a non-recursive enum is that of its largest variant (src: [[ch15-smart-pointers]]).
