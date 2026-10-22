---
type: source
sources: [raw/2018-ch04-02-references-and-borrowing.md]
updated: 2026-10-22
---
# References and Borrowing (2018 edition snapshot)

An older (2018-edition-era) version of the "References and Borrowing" section of TRPL chapter 4. It covers the same ground as the matching part of [[ch04-understanding-ownership]] but predates the "scope ends at last use" rule (src: [[2018-ch04-02-references-and-borrowing]]).

## Contents

- `&` references let a function use a value without taking ownership, so `calculate_length(&s1)` does not need to return the `String`. Passing a reference as a parameter is called *borrowing*. See [[borrowing]].
- References are immutable by default (E0596 when mutating through `&String`); `&mut` plus a `mut` variable fixes it.
- Only one `&mut` per piece of data in a scope (E0499), prevents data races; an inner `{}` block can end a borrow early.
- Cannot mix `&mut` with `&` (E0502); many `&` are fine.
- Dangling references are prevented at compile time (E0106 missing lifetime specifier); fix by returning the owned value. See [[lifetimes]].
- Rules recap: either one `&mut` or any number of `&`; references must always be valid.
- Dereference operator `*` mentioned as the opposite of `&`, with forward references to later chapters. See [[deref-and-deref-coercion]].

## Differences from the current chapter

- Error output shows "borrow ends here" at the closing brace: borrows last to the end of the enclosing block (lexical scope), not to the last use. This is outdated; see the conflict callout in [[borrowing]].
- The dangling-reference error suggests a `'static` lifetime (the current ch4 still suggests it, with a caveat; see [[dangling-references]]).
- Text says "a particular scope" for the one-`&mut` rule.

## Concept pages

[[borrowing]], [[lifetimes]], [[ownership]]
