---
type: concept
sources: [raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Mutex<T>

`Mutex<T>` (mutual exclusion) allows only one thread at a time to access the data it guards. A thread must acquire the lock before using the data and release it afterwards; Rust's types enforce both steps (src: [[ch16-fearless-concurrency]]).

## Rules

- `Mutex::new(5)`; `m.lock()` blocks until the lock is available and returns a `LockResult<MutexGuard<T>>`. `lock` fails if another holder panicked, hence `.unwrap()`.
- The type is `Mutex<i32>`, not `i32`, so you cannot reach the data without locking.
- `MutexGuard` implements `Deref` (acts like `&mut` to the inner data, see [[deref-and-deref-coercion]]) and `Drop`, which releases the lock at end of scope; you cannot forget to unlock. See [[drop]]. (src: [[ch16-fearless-concurrency]])
- Sharing across threads needs multiple owners: moving a `Mutex` into a loop of spawns fails (E0382), `Rc<Mutex<T>>` fails (not `Send`), `Arc<Mutex<T>>` works. See [[arc]].
- `Mutex<T>` provides interior mutability: mutate through an immutable `Arc`, the thread-safe analogue of `Rc<RefCell<T>>` ([[refcell-and-interior-mutability]]).
- Rust does not prevent logic errors: a deadlock occurs when two threads each hold one of two locks and wait for the other. This parallels `Rc` reference cycles ([[reference-cycles-and-weak]]).
- For simple numeric state, `std::sync::atomic` types are simpler than a `Mutex`.

## Example

```rust
use std::sync::{Arc, Mutex};
use std::thread;
let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];
for _ in 0..10 {
    let counter = Arc::clone(&counter);
    handles.push(thread::spawn(move || {
        *counter.lock().unwrap() += 1;
    }));
}
for h in handles { h.join().unwrap(); }
// *counter.lock().unwrap() == 10
```

## Common mistakes

- Moving the mutex into each thread instead of sharing it with `Arc`.
- Holding a guard longer than needed, or locking two mutexes in inconsistent order (deadlock).

## Relations

[[arc]], [[send-and-sync]] (`Mutex<T>` is `Sync`), [[threads]], [[channels]], [[smart-pointers]]. Comparison: [[rc-vs-arc-and-refcell-vs-mutex]].
