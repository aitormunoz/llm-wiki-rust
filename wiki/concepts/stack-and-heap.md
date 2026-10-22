---
type: concept
sources: [raw/ch04-understanding-ownership.md, raw/ch15-smart-pointers.md]
updated: 2026-10-22
---
# Stack and heap

Both are memory available at runtime, structured differently. The stack is last-in-first-out and holds only fixed-size, known-at-compile-time data; the heap holds data of unknown or changing size, reached through a pointer (src: [[ch04-understanding-ownership]]).

## Key points

- Pushing to the stack is faster than heap allocation, since no search for free space is needed; heap access is slower because you follow a pointer and lose locality. (src: [[ch04-understanding-ownership]])
- A pointer has a fixed size, so it can live on the stack while its data lives on the heap.
- `Box<T>` is the explicit way to put a value on the heap: the pointer stays on the stack, the data on the heap. Moving a large value then copies only the pointer. See [[box]]. (src: [[ch15-smart-pointers]])
- Function arguments and locals are pushed on the stack and popped when the function returns.
- [[ownership]] tracks who uses heap data, limits duplication and cleans it up. Knowing this explains why the rules are shaped as they are.

## Example

```rust
let x = 5;                        // stack only
let s = String::from("hello");    // pointer, len, capacity on stack; bytes on heap
```

## Relations

[[ownership]], [[move-copy-clone]] (what is copied: stack part vs heap part), [[box]].
