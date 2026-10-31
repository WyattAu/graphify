> Call `Task` once per chunk; ALL in the same response so they run in parallel.
CHUNK_PATH must be an **absolute** path; derive it before dispatching:
PROJECT_ROOT=$(pwd)  # cwd; where Part C globs graphify-out/ (NOT .graphify_root/scan dir, #1392)