1. `graphify extract` rebuilds the full graph from scratch every run; re-sends all files to the LLM regardless of what changed. For a 1000-file Markdown corpus updated daily this is expensive.
2. LLM extraction is chunk-by-chunk; the same real-world concept can get different labels across chunks (`AuthManager`, `AuthenticationManager`, `auth_mgr`). No semantic dedup exists beyond exact string normalization.
- `detect_incremental(target)` instead of `detect(target)`; returns `new_files`, `unchanged_files`, `deleted_files`
- `build_merge(new_chunks, prune_sources=deleted_files)` instead of `build_from_json`; merges into existing graph, prunes nodes from deleted files
- After LLM call: `save_semantic_cache(fresh_results)`; keyed by content hash
- `graphify/__main__.py`; `elif cmd == "extract":` block only (~5 targeted changes)
**Step 1; Exact normalization**
**Step 2; Entropy gate**
**Step 3; MinHash + LSH blocking** (`datasketch`)
**Step 4; Jaro-Winkler verification** (`rapidfuzz`)
**Step 5; Same-community boost**
Pairs where both nodes share a Leiden community ID get +0.05 score bonus. Graphify-specific advantage; community structure is a strong signal that GraphRAG/LightRAG don't exploit.
**Step 6; Union-find merge**
**Step 7; Optional LLM tiebreaker** (`--dedup-llm` flag)
- `datasketch`; always required (added to `[project.dependencies]`)
- `rapidfuzz`; always required (added to `[project.dependencies]`)
- `graphify/dedup.py`; new module, full pipeline
- `graphify/build.py`; call `deduplicate_entities` after graph construction; wire dormant `deduplicate_by_label`
- `graphify/__main__.py`; add `--dedup-llm` flag parsing in extract block
- `pyproject.toml`; add `datasketch`, `rapidfuzz` to base dependencies
- `--dedup embed` (MiniLM cosine); explicitly excluded, no PyTorch dependency
- Incremental support for `graphify update` (AST-only); already handled by existing AST cache
- Dedup across different graph.json files (merge two graphs); separate feature