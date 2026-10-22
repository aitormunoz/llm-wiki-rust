# llm-wiki-rust

A small example of Andrej Karpathy's [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
pattern: an Obsidian vault where an LLM agent (Claude Code) compiles sources into an interlinked wiki and keeps it
maintained.

- `raw/`: immutable sources, four chapters of [The Rust Programming Language](https://github.com/rust-lang/book)
  plus the 2018 version of "References and Borrowing" (MIT/Apache-2.0, see `raw/README.md`).
- `wiki/`: the compiled wiki, written entirely by the LLM. Start at `wiki/index.md`; `wiki/log.md` has the history.
- `CLAUDE.md`: the schema with the `ingest`, `query` and `lint` operations.

Every operation is its own commit, so `git log -p` shows exactly what each ingest, lint and query changed.

## Try it

```bash
git clone https://github.com/aitormunoz/llm-wiki-rust.git
cd llm-wiki-rust
claude
```

Open the same folder as a vault in Obsidian, then ask Claude Code things like:

```text
ingest raw/ch04-understanding-ownership.md
query: When should I use Rc<T> and when Arc<T>?
lint
```
