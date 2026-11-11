# Pull Request

## Description
`run_biomedical_ner_benchmark` ran the model over every fixture twice: once in the overall `run_benchmark` call, and again in `_per_corpus_metrics`, which called `run_benchmark` per corpus with the same runner. This PR records each fixture's predictions during the overall pass and replays them for the per-corpus slices, so each fixture is inferred once and the slices are scored from the same predictions as the overall score. This is the replay approach the issue suggests. `harness.py` and the report format are unchanged.

Fixes #4963

## Type of Change
- [x] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update
- [ ] Code refactoring
- [ ] Performance improvement
- [x] Test addition/improvement

## Changes Made
- `openmed/eval/datasets/biomedical_ner.py`: resolve the runner once (`runner` or `_shared_default_model_runner()`, the same fallback `run_benchmark` uses), pass `run_benchmark` a recording runner keyed by `fixture_id`, and pass `_per_corpus_metrics` a replay runner. `_per_corpus_metrics` now always gets a runner, so its type is `ModelRunner` instead of `ModelRunner | None`.
- `tests/unit/eval/test_biomedical_ner_suite.py`: two tests from the acceptance criteria. One is a counting runner that must be called exactly once per fixture. The other is a runner that returns gold spans on the first call and nothing after, and overall and every corpus slice must still score 1.0.
- `CHANGELOG.md`: one line under Unreleased / Fixed.

## Testing
- [x] I have added tests that prove my fix is effective or that my feature works
- [x] New and existing unit tests pass locally with my changes
- [ ] I have tested this change with different models/inputs: N/A. I tested with fake runners and synthetic fixtures only, as the issue's acceptance criteria describe. I did not run a real NER model.

Before, on master 35fe34e22 with only the new tests applied (offline repro with five synthetic one-document corpora):

```
1) counting runner: 5 fixtures, 10 calls   (2 per fixture)
2) runner that changes its answer on a second call:
   overall exact F1: 1.00
   bc5cdr / ncbi_disease / jnlpba / species_800 / bc2gm exact F1: 0.00
uv run --frozen --extra dev pytest tests/unit/eval/test_biomedical_ner_suite.py -q
2 failed, 8 passed
```

After, on this branch:

```
1) counting runner: 5 fixtures, 5 calls    (1 per fixture)
2) overall exact F1: 1.00, every corpus exact F1: 1.00
uv run --frozen --extra dev pytest tests/unit/eval/test_biomedical_ner_suite.py -q
10 passed
uv run --frozen --extra dev pytest tests/unit/eval/test_biomedical_ner_suite.py tests/unit/test_model_registry.py -q
23 passed, 4 subtests passed
```

Full selection: `uv run --frozen --extra dev pytest tests/ -q -m "not slow and not fuzz" -n auto` gives `65 failed, 29217 passed` on my Windows 11 machine. The same 31 test files give the same `65 failed` on master. They're Windows environment failures: symlink creation needs admin rights (WinError 1314), golden files expect LF, and a few similar checks. None of them import `biomedical_ner`, so I'm relying on CI for the full run on Linux.

## Documentation
- [ ] I have updated the documentation accordingly: N/A. The public behavior and report format don't change.
- [ ] I have added docstrings to new functions/classes: N/A. The two runners are small local closures with no new public API, and there's a comment above where they're used.
- [x] I have updated the CHANGELOG.md

## Code Quality
- [x] I ran `make format`, `make lint`, and `make format-check`. Windows has no `make`, so I ran the commands behind those targets with uv. `ruff check .`: All checks passed. `check_effect_paths.py --check`: effect_paths_verified. `ruff format --check .`: 3331 files already formatted. `mypy`: no issues found.
- [ ] For Swift/OpenMedKit changes, I ran `make format-swift` and `make lint-swift`: N/A, no Swift changes.
- [x] I have performed a self-review of my own code
- [x] I have commented my code, particularly in hard-to-understand areas
- [x] My changes generate no new warnings: the tests for the changed module run with no warnings (`23 passed, 4 subtests passed`), and `ruff check` and `mypy` are clean.

## Dependencies
- [x] I have not added any new dependencies
- [ ] OR I have added new dependencies and they are justified because: N/A

## Checklist
- [x] I have read the [contributing guide](https://github.com/maziyarpanahi/openmed/blob/master/CONTRIBUTING.md), [Code of Conduct](https://github.com/maziyarpanahi/openmed/blob/master/CODE_OF_CONDUCT.md), and [maintainer guide](https://github.com/maziyarpanahi/openmed/blob/master/MAINTAINERS.md)
- [x] My commits have clear, descriptive messages
- [x] I have squashed/organized my commits appropriately: two commits, the tests first and then the fix.

## Related Issues
Closes #4963
Related to #416 (linked from the issue)

## Screenshots/Examples
N/A. The before and after output is under Testing.

## Notes for Reviewers
AI use: I used an AI coding assistant for this PR. It traced the double inference, wrote the fix, the two tests, the offline repro script and a draft of this description. I ran every command and test shown here on my Windows 11 machine, and I'll handle the review feedback.
