# Test evidence: maziyarpanahi/openmed#4963

Branch `fix/issue-4963-single-ner-inference` (cfa912599 tests, 11991c6c4 fix) on master 35fe34e22. Windows 11, Python 3.13.0, `uv sync --frozen --extra dev`.

## Before the fix (master code, new tests applied)

```
> uv run --frozen --extra dev python ..\unit7\repro_4963.py
openmed fix/issue-4963-single-ner-inference at 35fe34e22 2026-11-08
1) counting runner: 5 fixtures, 10 calls
   bc2gm:bc2gm-1: 2
   bc5cdr:bc5-1: 2
   jnlpba:jnlpba-1: 2
   ncbi_disease:ncbi-1: 2
   species_800:species_800-1: 2
2) runner that changes its answer on a second call:
   overall exact F1: 1.00
   bc5cdr exact F1: 0.00
   ncbi_disease exact F1: 0.00
   jnlpba exact F1: 0.00
   species_800 exact F1: 0.00
   bc2gm exact F1: 0.00

> git apply --recount ..\unit7\1-test.diff   (new tests only, no fix)
> uv run --frozen --extra dev pytest tests/unit/eval/test_biomedical_ner_suite.py -q
....FF....
FAILED tests/unit/eval/test_biomedical_ner_suite.py::test_biomedical_ner_runs_the_model_once_per_fixture - AssertionError: assert {'bc5cdr:bc5-...00-1': 2, ...} == {'bc5cdr:bc5-...00-1': 1, ...}
FAILED tests/unit/eval/test_biomedical_ner_suite.py::test_biomedical_ner_per_corpus_slices_use_the_overall_predictions - assert 0.0 == 1.0
2 failed, 8 passed in 7.85s
```

## After the fix

```
> uv run --frozen --extra dev python ..\unit7\repro_4963.py
openmed fix/issue-4963-single-ner-inference at cfa912599 2026-11-10
1) counting runner: 5 fixtures, 5 calls
   bc2gm:bc2gm-1: 1
   bc5cdr:bc5-1: 1
   jnlpba:jnlpba-1: 1
   ncbi_disease:ncbi-1: 1
   species_800:species_800-1: 1
2) runner that changes its answer on a second call:
   overall exact F1: 1.00
   bc5cdr exact F1: 1.00
   ncbi_disease exact F1: 1.00
   jnlpba exact F1: 1.00
   species_800 exact F1: 1.00
   bc2gm exact F1: 1.00

> uv run --frozen --extra dev pytest tests/unit/eval/test_biomedical_ner_suite.py -q
..........
10 passed in 5.77s
> uv run --frozen --extra dev ruff check .
All checks passed!
> uv run --frozen --extra dev ruff format --check .
3331 files already formatted

# Full PR-CI suite on branch (2026-11-10, Windows):
> uv run --frozen --extra dev pytest tests/ -q -m "not slow and not fuzz" -n auto -p no:cacheprovider
65 failed, 29217 passed, 506 skipped, 81 warnings, 11243 subtests passed in 643.84s (0:10:43)
# All 65 failures are WinError 1314 symlink privilege, CRLF golden bytes, bash snippet check, gRPC stub drift, skill-pack linking.
# Same 31 files on master 35fe34e22:
65 failed, 713 passed, 31 skipped, 11 warnings, 513 subtests passed in 49.51s
# Tests for the changed module on branch 11991c6c4:
> uv run --frozen --extra dev pytest tests/unit/eval/test_biomedical_ner_suite.py tests/unit/test_model_registry.py -q -p no:cacheprovider
23 passed, 4 subtests passed in 5.33s
# Commits: cfa912599 test: ... ; 11991c6c4 fix: run the biomedical NER model once per fixture (#4963)

# Lint (effect paths) and type check on branch, 2026-11-10 ~10:02 PM:
> uv run --frozen --extra dev python scripts/security/check_effect_paths.py --check
effect_paths_verified
> uv run --frozen --extra dev mypy
Success: no issues found in 4 source files
```
