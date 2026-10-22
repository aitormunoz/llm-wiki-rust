---
type: answer
sources: [raw/ch15-smart-pointers.md, raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Rc vs Arc, and RefCell vs Mutex

**Question:** When should I use `Rc<T>` and when `Arc<T>`, and why not `Arc` everywhere? Same for `RefCell<T>` vs `Mutex<T>`.

## Rc vs Arc

- Both give multiple owners of one value, with the same API (`::new`, `::clone(&a)`). The only difference is how the count is updated: [[rc]] is non-atomic, [[arc]] is atomic. (src: [[ch15-smart-pointers]], [[ch16-fearless-concurrency]])
- Use `Rc` when everything stays on one thread and you cannot know at compile time which owner finishes last. Use `Arc` when a clone must move into another thread (`thread::spawn`).
- `Rc` is not `Send`, so the compiler rejects it across threads (E0277); see [[send-and-sync]]. That is the safety net: pick `Rc` by mistake in threaded code and it does not compile.
- Why not `Arc` everywhere? Thread safety costs performance, which you should pay only when needed. That is why `Rc` is the single-threaded default. (src: [[ch16-fearless-concurrency]])
- Both only give shared immutable access; neither fixes reference cycles ([[reference-cycles-and-weak]]).

## RefCell vs Mutex

- Both provide interior mutability: mutation through a shared reference, with the borrow rules enforced at runtime ([[borrowing]]). They have the same role at different scales. (src: [[ch15-smart-pointers]], [[ch16-fearless-concurrency]])
- `RefCell` panics if you break the rules (two `borrow_mut`); it is not `Sync`, so it is single-threaded only. Use it with `Rc` as `Rc<RefCell<T>>`. See [[refcell-and-interior-mutability]].
- `Mutex` makes other threads wait for a lock; it is `Sync`, so it works across threads. Use it with `Arc` as `Arc<Mutex<T>>`. Its cost is blocking and deadlock risk, which Rust does not prevent. See [[mutex]].
- Why not `Mutex` everywhere? Same reason as `Arc`: locking is for when threads are involved. In single-threaded code `RefCell` is the matching, cheaper tool, and a plain `&mut` is better still when the compiler accepts it.

## Pairings

| Single-threaded | Multi-threaded |
|---|---|
| `Rc<T>` (shared, read only) | `Arc<T>` |
| `Rc<RefCell<T>>` (shared, mutable) | `Arc<Mutex<T>>` |

Mixing them fails: `Rc<Mutex<T>>` is not `Send`, and `Arc<RefCell<T>>` is not `Sync`, so neither can be shared across threads. For a simple counter, `std::sync::atomic` is lighter than a `Mutex`. (src: [[ch16-fearless-concurrency]])

Related: [[smart-pointers]].
