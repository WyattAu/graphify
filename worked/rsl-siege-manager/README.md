A graphify dry-run against [`glitchwerks/rsl-siege-manager`](https://github.com/glitchwerks/rsl-siege-manager); a real-world web app with a Python (FastAPI) backend, a TypeScript (React + Vite) frontend, and a Python Discord bot, all in one repo. Captured here as a worked example of running graphify on a mixed-language full-stack codebase that includes a substantial test suite.
If "not recognized" on Windows, open a new shell; `uv tool` updates PATH but the current session won't see it.
| `README.md` | This file; corpus description + reproduction steps |
- **Three services in one repo**; Python backend, TypeScript frontend, Python bot; exercises cross-language inference.
- **Substantial test suite**; both Python (`backend/tests/`) and TypeScript (`frontend/src/**/__tests__/`); surfaces how degree-centrality behaves on covered codebases.
- **17 Alembic migrations** with revision docstrings; exercises how docstring-shaped content is or is not treated as graph entities.
- **Clean three-tier architecture**; a developer can describe the structure in one sentence, giving a clear ground truth to evaluate community detection against.