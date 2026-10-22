# Index

## Sources

- [[ch04-understanding-ownership]] — TRPL ch4: ownership, borrowing and slices.
- [[2018-ch04-02-references-and-borrowing]] — older (2018) TRPL ch4 references and borrowing section; lexical borrow scopes, superseded.
- [[ch10-generics-traits-lifetimes]] — TRPL ch10: generics, traits and trait bounds, lifetimes.
- [[ch15-smart-pointers]] — TRPL ch15: Box, Deref, Drop, Rc, RefCell, reference cycles and Weak.
- [[ch16-fearless-concurrency]] — TRPL ch16: threads, channels, Mutex, Arc, Send and Sync.

## Concepts

- [[ownership]] — the three ownership rules, scope, drop and moving ownership.
- [[stack-and-heap]] — how the two memory regions differ and why ownership targets the heap.
- [[move-copy-clone]] — what assignment does: move, `Copy` for stack-only types, explicit `clone`.
- [[borrowing]] — references, `&` vs `&mut` rules, data races, dangling references, old vs current scope rule.
- [[dangling-references]] — references to freed or out-of-scope data; the compiler rejects them (E0106, E0515, E0597).
- [[data-races]] — three conditions for a data race and how borrowing, `Mutex`, `Send`/`Sync` rule them out.
- [[slices]] — `&str` and `&[T]`, references to contiguous sequences.
- [[generics]] — type parameters on functions, structs, enums and methods.
- [[monomorphization]] — how generics compile to concrete code with no runtime cost.
- [[traits]] — shared behavior, impls, default methods, orphan rule, blanket impls.
- [[trait-bounds]] — `impl Trait`, `T: A + B`, `where`, conditional methods.
- [[lifetimes]] — reference validity, annotations, structs, `'static`, borrow checker.
- [[lifetime-elision]] — the three rules that let you omit lifetime annotations.
- [[smart-pointers]] — pointers with extra capabilities that usually own data; Box vs Rc vs RefCell comparison.
- [[box]] — heap allocation, recursive types, large moves, trait objects.
- [[deref-and-deref-coercion]] — the `Deref` trait, `*` operator, automatic `&T` to `&U` conversion.
- [[drop]] — cleanup code at end of scope, reverse order, `std::mem::drop` for early cleanup.
- [[rc]] — reference counting for multiple owners (single-threaded), `Rc::clone`, strong count.
- [[refcell-and-interior-mutability]] — borrow rules checked at runtime, `borrow`/`borrow_mut`, `Rc<RefCell<T>>`.
- [[reference-cycles-and-weak]] — leaks from `Rc` cycles, `Weak<T>`, `upgrade`, parent/child trees.
- [[threads]] — `thread::spawn`, `join`, `move` closures, 1:1 model.
- [[channels]] — `mpsc` message passing, `send`/`recv`, multiple producers.
- [[mutex]] — `Mutex<T>`, `lock`, `MutexGuard`, deadlocks, `Arc<Mutex<T>>`.
- [[arc]] — atomic reference counting, the thread-safe `Rc`.
- [[send-and-sync]] — marker traits that make types thread-safe, auto-derived.

## Answers
