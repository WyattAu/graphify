### Step 0: Constrained query expansion (REQUIRED before traversal)
graphify's `query` CLI matches nodes via case-folded substring + IDF; there is **no stemming, no synonyms, no cross-language match** inside the binary, and the inline fallback below matches the same way. If the user's question uses different language or different domain vocabulary than the graph's labels (user says "обработчик" / graph says "handler"; user says "authentication" / graph says "Guardian"), the literal matcher returns 0 hits and the answer collapses to noise.
   - If a query concept has no plausible token in the vocab, skip it; do not substitute a near-synonym from training memory.
If the list is empty, say so plainly and stop; do not proceed to traversal.
### Step 1: Traversal
Build the **expanded query string** by joining the selected tokens with spaces. Use this string as `QUESTION` below; NOT the original user question. (The original question is preserved only for `save-result` at the end.)
**Work memory (self-improving loop).** Add an `--outcome` so future sessions learn from this one; append `--outcome useful|dead_end|corrected` to the `save-result` command (and `--correction "the right answer"` when correcting):
- `useful`; the cited nodes answered the question well (they become *preferred sources*).
- `dead_end`; the question/path led nowhere; don't re-derive it next time.
- `corrected`; the saved answer was wrong; `--correction` records what was right.