---
type: concept
sources: [raw/ch16-fearless-concurrency.md]
updated: 2026-10-22
---
# Channels (message passing)

A channel sends data from one thread to another. It has a transmitter (`tx`) and a receiver (`rx`), and is closed if either half is dropped. The idea: "share memory by communicating" (src: [[ch16-fearless-concurrency]]).

## Rules

- `mpsc::channel()` returns `(tx, rx)`. `mpsc` = multiple producer, single consumer: many senders, one receiver. Type inferred from the first use.
- `tx.send(val)` returns `Result`; it errors if the receiver was dropped. Move `tx` into the spawned thread with `move`.
- `send` takes ownership of the value, so using it afterwards is a compile error (E0382). This is how ownership prevents the receiver's thread from racing with the sender. (src: [[ch16-fearless-concurrency]])
- `rx.recv()` blocks until a value arrives; returns an error once the transmitter closes. `rx.try_recv()` never blocks (`Err` if nothing yet), good when the thread has other work.
- `rx` can be used as an iterator (`for received in rx`); iteration ends when the channel closes.
- More producers: `tx.clone()` per extra sending thread. Arrival order is nondeterministic.
- Channels resemble single ownership; the alternative is shared state, see [[mutex]].

## Example

```rust
use std::{sync::mpsc, thread};
let (tx, rx) = mpsc::channel();
thread::spawn(move || {
    tx.send(String::from("hi")).unwrap();
});
println!("Got: {}", rx.recv().unwrap());
```

## Common mistakes

- Using a value after sending it.
- Calling `unwrap` on `send` in real code instead of handling the error.
- Expecting a fixed order with multiple producers.

## Relations

[[threads]], [[ownership]], [[move-copy-clone]], [[mutex]], [[send-and-sync]] (sent values must be `Send`).
