# Log

## [2026-10-22] ingest | Chapter 4: Understanding Ownership
- Created: [[ch04-understanding-ownership]], [[ownership]], [[stack-and-heap]], [[move-copy-clone]], [[borrowing]], [[slices]]
- Updated: [[index]], [[log]]

## [2026-10-22] ingest | Chapter 10: Generic Types, Traits, and Lifetimes
- Created: [[ch10-generics-traits-lifetimes]], [[generics]], [[monomorphization]], [[traits]], [[trait-bounds]], [[lifetimes]], [[lifetime-elision]]
- Updated: [[borrowing]], [[slices]], [[index]], [[log]]

## [2026-10-22] ingest | Chapter 15: Smart Pointers
- Created: [[ch15-smart-pointers]], [[smart-pointers]], [[box]], [[deref-and-deref-coercion]], [[drop]], [[rc]], [[refcell-and-interior-mutability]], [[reference-cycles-and-weak]]
- Updated: [[ownership]] (conflict callout: single owner vs `Rc`), [[borrowing]] (conflict callout: compile-time vs `RefCell` runtime checks), [[move-copy-clone]], [[stack-and-heap]], [[slices]], [[traits]], [[index]], [[log]]

## [2026-10-22] ingest | Chapter 16: Fearless Concurrency
- Created: [[ch16-fearless-concurrency]], [[threads]], [[channels]], [[mutex]], [[arc]], [[send-and-sync]]
- Updated: [[rc]], [[refcell-and-interior-mutability]], [[ownership]], [[borrowing]], [[drop]], [[smart-pointers]], [[move-copy-clone]], [[traits]], [[reference-cycles-and-weak]], [[index]], [[log]]

## [2026-10-22] ingest | References and Borrowing (2018 edition snapshot)
- Created: [[2018-ch04-02-references-and-borrowing]]
- Updated: [[borrowing]] (conflict callout: lexical vs last-use scope), [[lifetimes]], [[ch04-understanding-ownership]], [[index]], [[log]]

## [2026-10-22] lint | 6 issues
- High: 1 stale claim (`'static` suggestion attributed only to older compiler output). Medium: 3 (truncated sentence in [[drop]], missing raw source in [[lifetimes]] frontmatter, missing concept pages). Low: 2 (uncited claims, ungrounded inference in [[borrowing]]).
- No broken wikilinks, no orphan pages. Report only, no pages changed.

## [2026-10-22] lint | fix: 4 issues resolved
- High: corrected stale `'static` claim in [[borrowing]] (current ch4 also suggests it, with a caveat); aligned [[lifetimes]] and [[2018-ch04-02-references-and-borrowing]].
- Medium: restored truncated sentence in [[drop]]; added missing raw sources to [[lifetimes]] frontmatter; created [[dangling-references]] and [[data-races]] (explained by the sources). Closures, trait objects and deadlocks not created: sources only forward-reference or mention them.
- Updated: [[borrowing]], [[lifetimes]], [[drop]], [[ownership]], [[2018-ch04-02-references-and-borrowing]], [[index]], [[log]]
- Skipped: low-severity citation sweep.
