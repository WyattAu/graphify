**Pass 1; Code structure (free, no API calls)**
**Pass 2; Video and audio (local, no API calls)**
Video and audio files are transcribed with faster-whisper. To focus the transcript on your domain, the transcription prompt is seeded with your top god nodes (the most-connected concepts in your code graph so far). Transcripts are cached; re-runs skip already-processed files.
**Pass 3; Docs, papers, images (Claude subagents, costs tokens)**
Communities are found using the [Leiden algorithm](https://www.nature.com/articles/s41598-019-41695-z); a graph-clustering method that groups nodes by edge density. Nodes with many connections between them end up in the same community.
**No embeddings needed.** The semantic similarity edges that Claude extracts (`semantically_similar_to`) are already in the graph, so they influence community shape directly. The graph structure is the similarity signal; there's no separate embedding step or vector database.
| `AMBIGUOUS` | Uncertain; flagged in the report for manual review |
- **0.95**; near-certain (explicit cross-file reference, one plausible target)
- **0.85**; strong evidence (naming + context align)
- **0.75**; reasonable (contextual but not explicit)
- **0.65**; weak (naming similarity only)
- **0.55**; speculative
The first run extracts and builds the graph; this costs tokens. Every subsequent query reads the compact graph instead of raw files. That's where the savings compound.
Token reduction scales with corpus size. Six files already fits in a context window; the graph value there is structural clarity, not compression. At 52 files the savings compound quickly.
Code files are extracted in parallel using `ProcessPoolExecutor`; bypasses Python's GIL for genuine multiprocessing. Doc/paper/image batches are dispatched as parallel Claude subagents. On a corpus of 84 code files, parallel AST extraction runs in about 1.66x less time than sequential.
Every extracted file is fingerprinted by content hash. Re-runs skip unchanged files entirely; only new or modified files go through extraction again. The cache lives in `graphify-out/cache/`.
- `id`; stable identifier
- `label`; human-readable name
- `file_type`; `code`, `document`, `paper`, `image`, `rationale`
- `source_file`; where it came from
- `source`, `target`; node IDs
- `relation`; verb phrase (e.g. `calls`, `imports`, `implements`, `semantically_similar_to`)
- `confidence`; `EXTRACTED`, `INFERRED`, or `AMBIGUOUS`
- `confidence_score`; float (INFERRED only)
- `source_file`; where the relationship was found