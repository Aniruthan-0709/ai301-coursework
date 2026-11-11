# Plan: maziyarpanahi/openmed#4963

Issue: https://github.com/maziyarpanahi/openmed/issues/4963
Base: master at 35fe34e22 (2026-11-08). Branch: `fix/issue-4963-single-ner-inference`
(the repo's `<type>/issue-<number>-<slug>` naming).

## Analysis

`run_biomedical_ner_benchmark` in `openmed/eval/datasets/biomedical_ner.py` scores the
suite in two passes. First it calls `run_benchmark` (`openmed/eval/harness.py`) over all
fixtures, and `run_benchmark` calls the model runner once per fixture. Then it calls
`_per_corpus_metrics`, which groups the fixtures by their `dataset` metadata and calls
`run_benchmark` again for each corpus with the same `runner`. So every fixture goes through
the model twice. When `runner` is None it is worse: each `run_benchmark` call builds its own
`_shared_default_model_runner()`, so the model loader is created again for every corpus.

Two consequences, both in the issue: a reproduction run costs twice the inference, and a
runner that does not return the same spans every time (sampling, GPU nondeterminism, a
remote model) can produce per-corpus slices that disagree with the overall score in the
same report. My offline repro (`repro_4963.py`, fake runners, five synthetic one-document
corpora) shows both on master (run 2026-11-10): 10 runner calls for 5 fixtures, and a runner that answers
differently on a second call gives overall exact F1 1.00 but 0.00 for every corpus.

## Proposed Solution

Run the model once and score the slices from the same predictions, the "replay runner over
the in-memory results" the issue suggests:

- In `run_biomedical_ner_benchmark`, resolve the runner once (`runner` or
  `_shared_default_model_runner()`, the same fallback `run_benchmark` uses).
- Pass `run_benchmark` a recording runner that calls the real runner and keeps each
  fixture's raw predictions in a dict keyed by `fixture_id` (`run_benchmark` already
  rejects duplicate ids).
- Pass `_per_corpus_metrics` a replay runner that returns the recorded predictions, so the
  per-corpus `run_benchmark` calls do no inference. Their F1 math stays exactly as it is.

Not changing: `run_benchmark` and the rest of `harness.py`, `_per_corpus_metrics`'s
grouping and output keys, the report format, the CLI, and the other suites. For a
deterministic runner the report is unchanged. The per-corpus calls still time the replay,
but `_per_corpus_metrics` only keeps F1, fixture and span counts, so no latency number
changes. Caching across runs (`cache_dir`) is out of scope.

## Implementation Plan

1. `tests/unit/eval/test_biomedical_ner_suite.py`: add
   `test_biomedical_ner_runs_the_model_once_per_fixture` (a counting runner must be called
   exactly once per fixture) and
   `test_biomedical_ner_per_corpus_slices_use_the_overall_predictions` (a runner that
   returns gold spans on the first call and nothing after; overall and every corpus slice
   must both score 1.0). Run them on master first and confirm both fail.
2. `openmed/eval/datasets/biomedical_ner.py`: add the recording and replay runners in
   `run_biomedical_ner_benchmark` as above, import `_shared_default_model_runner` from the
   harness, and tighten `_per_corpus_metrics`'s `runner` type from `ModelRunner | None` to
   `ModelRunner`, since it now always gets one.
3. `CHANGELOG.md`: one line under Unreleased / Fixed, as CONTRIBUTING asks when behavior
   changes.
4. Re-run `repro_4963.py` on the branch (expect 5 calls and F1 1.00 everywhere), run
   `uv run --frozen --extra dev pytest tests/unit/eval/test_biomedical_ner_suite.py -q`
   (the issue's acceptance command) and the full suite, then the commands behind
   `make lint`, `make format-check` and `make type-check` (`uv run ... ruff check .`,
   the effect-path check, `ruff format --check .`, `mypy`), since Windows has no `make`.

Deviation (2026-11-10): the full suite gives 65 failures on my Windows machine, and the
same 65 fail on master (symlink privilege WinError 1314, CRLF golden files, and similar
environment checks), none in files that import `biomedical_ner`. I list it as excluded in
my ship manifest and rely on the PR's Linux CI for it.

## Test plan

- Failure case 1 (double inference): counting runner, calls per fixture before (2) and
  after (1).
- Failure case 2 (slices disagree): drifting runner, per-corpus exact F1 before (0.00) and
  after (1.00), overall stays 1.00.
- Unchanged output: the existing
  `test_biomedical_ner_report_contains_per_corpus_exact_and_relaxed_f1` and the CLI test
  still pass.
