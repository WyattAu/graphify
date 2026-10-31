A small document pipeline; parser, validator, processor, storage, API; with architecture notes and research notes. Seven files, two languages, clear call relationships between modules.
├── parser.py       ; reads files, detects format, kicks off the pipeline
├── validator.py    ; schema checks, calls processor for text normalization
├── processor.py    ; keyword extraction, cross-reference detection
├── storage.py      ; persists everything, maintains the index
├── api.py          ; HTTP handlers that orchestrate the above four modules
├── architecture.md ; design decisions and module responsibilities
└── notes.md        ; open questions and tradeoffs
No PDF or image extraction; runs entirely on AST and markdown with no token cost for semantic extraction.