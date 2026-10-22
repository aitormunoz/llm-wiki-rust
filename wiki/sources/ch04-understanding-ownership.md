---
type: source
sources: [raw/ch04-understanding-ownership.md]
updated: 2026-10-22
---
# Chapter 4: Understanding Ownership

Chapter 4 of The Rust Programming Language. It explains how Rust guarantees memory safety without a garbage collector, using compiler-checked ownership rules (src: [[ch04-understanding-ownership]]).

## Structure

1. **What is ownership?** Stack vs heap, the three ownership rules, scope, `String`, `drop`, move, clone, `Copy`, ownership through functions and return values. See [[stack-and-heap]], [[ownership]], [[move-copy-clone]].
2. **References and borrowing.** `&` and `&mut`, the rules of references, dangling references. See [[borrowing]].
3. **The slice type.** `&str`, `&[T]`, why slices beat raw indices. See [[slices]].

## Key takeaways

- Ownership manages heap data: one owner, dropped when the owner leaves scope.
- Assignment and function calls move non-`Copy` values; `clone` is the explicit deep copy.
- References borrow without owning; at any time either one `&mut` or many `&`, and always valid.
- Slices are references into a contiguous sequence, so the compiler keeps them in sync with the data.
- Forward references in the chapter: lifetimes (ch10), traits (ch10), deref coercion (ch15), `String` details (ch8).

## Concept pages

[[ownership]], [[stack-and-heap]], [[move-copy-clone]], [[borrowing]], [[slices]]

An older snapshot of section 2 exists as [[2018-ch04-02-references-and-borrowing]] (lexical borrow scopes, outdated).
