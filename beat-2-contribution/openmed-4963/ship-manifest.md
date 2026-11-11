# Ship manifest: maziyarpanahi/openmed

## Must pass before a ship

| Check | Command | Source |
|---|---|---|
| Tests for the changed module | uv run --frozen --extra dev pytest tests/unit/eval/test_biomedical_ner_suite.py tests/unit/test_model_registry.py -q -p no:cacheprovider | Issue #4963 acceptance criteria (the suite file); the only two test files that import biomedical_ner |
| Lint | uv run --frozen --extra dev ruff check . && uv run --frozen --extra dev python scripts/security/check_effect_paths.py --check | CONTRIBUTING.md `make lint` (the two commands in that Makefile target; no `make` on Windows) |
| Format check | uv run --frozen --extra dev ruff format --check . | CONTRIBUTING.md `make format-check` (Makefile target) |
| Type check | uv run --frozen --extra dev mypy | CONTRIBUTING.md `make type-check` (Makefile target) |

## Excluded, with a dated reason

| Check | Command | Date | Why it doesn't count against this PR |
|---|---|---|---|
| Full unit suite | uv run --frozen --extra dev pytest tests/ -q -m "not slow and not fuzz" -n auto -p no:cacheprovider | 2026-11-10 | CONTRIBUTING.md asks for it, but on my Windows machine 65 tests in 31 files fail on master too (65 failed, same files, run 2026-11-10): ~55 WinError 1314 (symlinks need admin/Developer Mode), CRLF golden-byte comparisons, a bash-snippet check, gRPC stub drift and skill-pack linking. None import biomedical_ner. On my branch the full run was 65 failed, 29217 passed; the PR's Linux CI runs the full suite. |
