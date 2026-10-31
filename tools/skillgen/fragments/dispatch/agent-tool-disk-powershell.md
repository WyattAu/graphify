CHUNK_PATH must be an **absolute** path; derive it before dispatching:
$PROJECT_ROOT = (Get-Location).Path  # cwd; where Part C globs graphify-out\ (NOT .graphify_root/scan dir, #1392)