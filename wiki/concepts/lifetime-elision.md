---
type: concept
sources: [raw/ch10-generics-traits-lifetimes.md]
updated: 2026-10-22
---
# Lifetime elision

The lifetime elision rules are patterns built into the compiler that let you omit [[lifetimes]] in common signatures. They are not rules for programmers, and they are not full inference: if ambiguity remains after applying them, the compiler errors instead of guessing (src: [[ch10-generics-traits-lifetimes]]).

## Rules

Lifetimes on parameters are *input lifetimes*; on return values, *output lifetimes*. Applies to `fn` and `impl` blocks.

1. Each reference parameter gets its own lifetime parameter.
2. If there is exactly one input lifetime, it is assigned to all outputs.
3. If one parameter is `&self` or `&mut self`, its lifetime is assigned to all outputs.

If an output lifetime is still unknown after the three rules, error (E0106). (src: [[ch10-generics-traits-lifetimes]])

## Example

`fn first_word(s: &str) -> &str` becomes `fn first_word<'a>(s: &'a str) -> &'a str` by rules 1 and 2. `fn longest(x: &str, y: &str) -> &str` fails: two inputs, no `self`. In `fn announce_and_return_part(&self, announcement: &str) -> &str` rule 3 gives the output `self`'s lifetime.

## History

Pre-1.0 Rust required explicit lifetimes everywhere; common patterns were folded into the compiler and more may be added later.

## Relations

[[lifetimes]], [[slices]] (`first_word`), [[borrowing]].
