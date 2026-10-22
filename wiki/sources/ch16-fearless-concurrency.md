---
type: source
sources: [raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Chapter 16: Fearless Concurrency

Chapter 16 of The Rust Programming Language. Rust found that ownership and the type system help with memory safety and concurrency alike, so many concurrency errors become compile-time errors ("fearless concurrency"). Rust offers several models (threads, message passing, shared state) rather than one dogmatic approach (src: [[ch16-fearless-concurrency]]).

## Structure

1. **Threads.** `thread::spawn` takes a closure; the std library uses a 1:1 OS-thread model; `join` on the `JoinHandle<T>` waits for a thread; `move` closures transfer captured values into the thread. See [[threads]].
2. **Message passing.** `mpsc::channel` gives a transmitter and receiver; `send` takes ownership of the value; `recv`/`try_recv`/iteration; cloning `tx` for multiple producers. See [[channels]].
3. **Shared state.** `Mutex<T>` guards data behind a lock released by its guard's `Drop`; `Rc<T>` cannot cross threads, `Arc<T>` can. See [[mutex]] and [[arc]].
4. **`Send` and `Sync`.** Marker traits, automatically derived, that extend the guarantees to user types. See [[send-and-sync]].

## Key takeaways

- Race conditions, deadlocks and hard-to-reproduce bugs are the risks of threads; Rust turns data races and invalid references into compile errors, but not deadlocks (src: [[ch16-fearless-concurrency]]).
- Channels resemble single ownership; shared memory resembles multiple ownership.
- `Mutex<T>`/`Arc<T>` are to threads what `RefCell<T>`/`Rc<T>` are to one thread.
- Most concurrency features live in the standard library or crates, not the language; only `Send` and `Sync` are built in.
- Forward references: async (ch17), unsafe and raw pointers (ch20), the ch21 project.

## Concept pages

New: [[threads]], [[channels]], [[mutex]], [[arc]], [[send-and-sync]]. Updated: [[rc]], [[refcell-and-interior-mutability]], [[ownership]], [[borrowing]], [[drop]], [[smart-pointers]], [[move-copy-clone]], [[traits]], [[reference-cycles-and-weak]].
