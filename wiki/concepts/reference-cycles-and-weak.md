---
type: concept
sources: [raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Reference cycles and Weak<T>

Rust makes memory leaks hard but not impossible: preventing them is not one of its guarantees, so leaks are memory safe. Items that refer to each other through [[rc]] form a cycle whose strong counts never reach 0, so they are never dropped (src: [[ch15-smart-pointers]]).

## Rules

- Cycles need `Rc` plus interior mutability, e.g. `Cons(i32, RefCell<Rc<List>>)` whose tail is repointed ([[refcell-and-interior-mutability]]). Rust will not catch them; treat as a logic bug (tests, reviews). Printing a cyclic structure overflows the stack.
- Fix 1: restructure so some references express ownership and others do not; only ownership affects dropping.
- Fix 2: `Weak<T>`. `Rc::downgrade(&rc)` gives a `Weak<T>` and increments `weak_count`, not `strong_count`. Cleanup needs only `strong_count == 0`. (src: [[ch15-smart-pointers]])
- A `Weak<T>` may point to dropped data, so `weak.upgrade()` returns `Option<Rc<T>>`: `Some` if alive, `None` if dropped. The compiler forces handling both cases. `Weak::new()` makes an empty one.
- Pattern: parent owns children (`children: RefCell<Vec<Rc<Node>>>`), child refers to parent weakly (`parent: RefCell<Weak<Node>>`). Dropping the parent drops children; dropping a child leaves the parent alive.

## Example

```rust
struct Node {
    value: i32,
    parent: RefCell<Weak<Node>>,
    children: RefCell<Vec<Rc<Node>>>,
}
*leaf.parent.borrow_mut() = Rc::downgrade(&branch);
```

## Common mistakes

- Making a child's `parent` field an `Rc` (cycle, leak).
- Using a `Weak` without calling `upgrade` and handling `None`.

## Related hazards

Mutexes have a similar logic-error risk that Rust cannot prevent: deadlocks. See [[mutex]]. (src: [[ch16-fearless-concurrency]])

## Relations

[[rc]], [[refcell-and-interior-mutability]], [[drop]], [[ownership]], [[smart-pointers]].
