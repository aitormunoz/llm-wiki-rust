---
type: concept
sources: [raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Threads

A thread is an independent part of a program that runs simultaneously with others; a program runs in a process and the OS manages many. Threads can improve performance but add complexity, because the order in which threads run is not guaranteed (src: [[ch16-fearless-concurrency]]).

## Rules

- `thread::spawn(closure)` starts a new thread. The standard library uses a 1:1 model: one OS thread per language thread. Other models exist in crates, and async is another approach.
- When the main thread finishes, all spawned threads are shut down, finished or not.
- `spawn` returns `JoinHandle<T>`, an owned value; `handle.join().unwrap()` blocks the current thread until that thread ends. Where `join` is called decides whether output interleaves. (src: [[ch16-fearless-concurrency]])
- `thread::sleep` only encourages the OS to switch threads; interleaving is never guaranteed.
- Risks: race conditions, deadlocks, bugs that appear only in some situations.
- A closure that uses outer data captures it by reference when it can, but Rust cannot know how long the thread lives, so borrowing fails (E0373: closure may outlive the current function). Use `move` to transfer ownership into the thread.
- `move` does not bypass ownership: after moving `v` into the closure, calling `drop(v)` in the main thread is E0382. It guarantees to Rust that the main thread no longer uses the value. (src: [[ch16-fearless-concurrency]])

## Example

```rust
use std::thread;
let v = vec![1, 2, 3];
let handle = thread::spawn(move || {
    println!("Here's a vector: {v:?}");
});
handle.join().unwrap();
```

## Common mistakes

- Forgetting `join` and losing the spawned thread's work when `main` exits.
- Omitting `move` when the closure uses outer variables (E0373).
- Using a value in the parent after moving it into the thread (E0382).

## Relations

[[ownership]] and [[borrowing]] (why `move` is needed), [[move-copy-clone]], [[drop]], [[channels]], [[mutex]], [[send-and-sync]] (the bounds `spawn` requires).
