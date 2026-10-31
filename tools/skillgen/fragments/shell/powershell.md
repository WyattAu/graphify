# Detect Python with graphify: uv/pipx-aware (fixes #831)
    # 1. uv tool install: 'uv tool dir' is authoritative, respects UV_TOOL_DIR automatically
    # 2. pipx install: 'pipx environment' respects PIPX_HOME automatically
# Not found: install then re-detect
# Save interpreter path: all subsequent steps read this.
**In every subsequent block, run Python through the saved interpreter; `& (Get-Content graphify-out\.graphify_python)` in place of a bare `python3`; so every step uses the interpreter that actually has graphify.**