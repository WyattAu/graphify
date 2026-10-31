## Task 2: Create `graphify/dedup.py`: entropy gate + MinHash/LSH + Jaro-Winkler
    # All three are the same concept: only one survives
    # "GraphExtractor" vs "Graph Extractor": Jaro-Winkler >= 0.92
    # "AI" and "ML" are low-entropy: entropy gate skips them
    # This is a structural test: use labels that would score ~0.80 without boost
- [ ] **Step 2: Run tests; verify they all fail**
                pass  # duplicate key in LSH; already inserted
- [ ] **Step 4: Run tests; verify they pass**
- [ ] **Step 5: Run full suite; no regressions**
- [ ] **Step 2: Run; verify it fails**
Expected: FAIL; two separate nodes exist (no dedup wired yet).
Also update the safety-check block (around lines 235-242). When `dedup=True` or `prune_sources` is set, the graph can legitimately shrink; skip the shrink guard:
## Task 4: Incremental updates: semantic cache + manifest in `__main__.py`
    # before writing the manifest: and that the path would be correct.
    # Should fail with no API key: but NOT with a path error
- [ ] **Step 2: Run; verify tests fail or pass trivially**
    # Should not crash even with dedup_llm_backend set: just nothing to resolve
- [ ] **Step 2: Run; verify it fails**
Expected: FAIL; `deduplicate_entities` does not accept `dedup_llm_backend` kwarg.
- Feat: `graphify extract` now runs incrementally; auto-detects prior `manifest.json` and re-extracts only changed/new files; semantic results cached by content hash so unchanged docs cost zero LLM tokens on repeat runs (#698)
- Feat: Entity deduplication pipeline runs on every build; entropy gate + MinHash/LSH blocking + Jaro-Winkler verification + same-community boost collapses near-duplicate entities (typos, spacing, plurals) before clustering
- Feat: `--dedup-llm` flag for `graphify extract`; optional LLM tiebreaker for ambiguous entity pairs (~$0.01 for 10k-node graphs), off by default