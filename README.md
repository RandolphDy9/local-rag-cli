# local-rag-cli

A small retrieval-augmented generation (RAG) command-line app that runs entirely on your machine. It indexes the Markdown files in `docs/`, finds the passages most relevant to your question, and has a local model answer using only those passages.

The repo includes a sample apartment rental handbook in `docs/handbook.md` to try it on.

## Requirements

- [uv](https://docs.astral.sh/uv/)
- Python 3.14 or newer (uv can install this for you)
- [Ollama](https://ollama.com/), running on its default port (`localhost:11434`)

Pull the two models the app uses:

```sh
ollama pull nomic-embed-text
ollama pull qwen3:4b-instruct
```

## Usage

Run this from the repo root, because the app reads `docs/` relative to the current directory:

```sh
uv run rag
```

Then ask questions at the prompt:

```text
Indexed 21 chunks.

Ask: When is rent due?
```

Press Ctrl-C to quit.

## How it works

1. Each `docs/*.md` file is split into chunks on blank lines.
2. Every chunk is embedded with `nomic-embed-text`.
3. Your question is embedded, and the 3 chunks with the highest cosine similarity are selected.
4. `qwen3:4b-instruct` answers from those chunks, and is told to say "I don't know" if the answer isn't there.

To use your own content, add Markdown files to `docs/`. The index is rebuilt in memory each time the app starts.

## License

[MIT](LICENSE)
