# Fixing duplicate inference in OpenMed's biomedical NER benchmark

## Overview

OpenMed is an open-source project that includes tools for working with biomedical AI models. Its biomedical Named Entity Recognition (NER) benchmark evaluates how well models identify biomedical entities in text, such as diseases, chemicals, genes, and species, and reports performance across the complete benchmark and separately for individual datasets. The benchmark was running model inference twice for every fixture. This doubled the inference work, and if a model returned different predictions on its second run, the overall and per-corpus scores could disagree in the same report. I changed `run_biomedical_ner_benchmark` to record each fixture's predictions during the overall benchmark run and reuse those predictions when calculating per-corpus metrics, so each fixture goes through the actual model once. I opened [PR #5541](https://github.com/maziyarpanahi/openmed/pull/5541), which is currently open. The maintainer acknowledged my reproduction and assigned me to issue #4963. The PR has no reviews listed as of November 14, 2026.

## What happened

### My first issue

I initially chose issue [#359](https://github.com/nextcloud/context_chat_backend/issues/359) in Nextcloud's Context Chat Backend. It was a bug in the default embedding configuration. The internal embedding model used an `n_batch` value of 16, and the embedding process silently truncated each text chunk after 16 tokens. That meant the model was generating embeddings using only a small portion of the actual content. I volunteered to investigate it and proposed reproducing the issue using two documents with identical first 16 tokens but different content afterward.

The maintainer agreed that my general approach made sense, but explained that fixing the issue also affected existing installations. Their vector databases would need to be cleared and documents reindexed. Because of the sensitive nature of that change, they preferred to handle it themselves and encouraged me to choose another issue:

> hello, thanks for your enthusiasm, yes the general approach is good, we also need to fix the existing installs with a wipe of the vector db + a reindex.
> given the sensitive nature of this issue, we would like to do it ourselves.
> but feel free to pick from one of the other existing issues.
>
> (maintainer on nextcloud/context_chat_backend #359, October 26, 2026)

I was initially disappointed because I had already started investigating it. However, I understood the maintainer's reasoning. I decided to move on rather than get stuck on an issue the maintainers wanted to handle themselves.

### Choosing #4963

I then found [#4963](https://github.com/maziyarpanahi/openmed/issues/4963) in OpenMed, which gave me an opportunity to independently reproduce a bug, write regression tests, and implement a focused fix. I liked that it was a clearly defined problem with two measurable consequences: unnecessary model inference and inconsistent benchmark scores. I could trace the execution, reproduce the problem with a controlled runner, and verify the fix using two specific acceptance criteria. It also gave me a chance to work with an actual evaluation pipeline rather than just changing a configuration value.

### Finding the double run

I started by tracing `run_biomedical_ner_benchmark` in `openmed/eval/datasets/biomedical_ner.py`. I found that it first called `run_benchmark` across all fixtures to calculate the overall metrics. Then it called `_per_corpus_metrics`, which grouped the fixtures by their dataset and called `run_benchmark` again for each corpus.

What surprised me was that the second pass wasn't simply recalculating metrics from predictions already collected. It was actually calling the model runner again. I also noticed that when no runner was passed, each `run_benchmark` call could create its own default model runner. So the problem wasn't limited to duplicate inference; it could also cause repeated model-runner initialization.

### Proving the bug with tests

I wrote two tests based on the issue's acceptance criteria. The first test, `test_biomedical_ner_runs_the_model_once_per_fixture`, uses a counting runner. It checks that every fixture triggers exactly one model call. The second test, `test_biomedical_ner_per_corpus_slices_use_the_overall_predictions`, uses a runner that returns the correct gold spans on its first call but returns no predictions on subsequent calls. It checks that the overall score and every individual corpus score still equal 1.00:

```python
def runner(fixture, model_name, device):
    # Gold spans on the first call, nothing after: a second pass over the
    # same fixture would score differently from the first.
    calls[fixture.fixture_id] += 1
    return list(fixture.gold_spans) if calls[fixture.fixture_id] == 1 else []
```

I wrote the tests before implementing the fix so I could verify that they actually detected the original problem. When I applied the tests to the original code on master, both failed. The counting test detected duplicate calls, and the second test detected the mismatch between overall and per-corpus scores. That gave me confidence that the tests were validating the actual bug.

My reproduction and the tests on master 35fe34e22, November 10, 2026:

```
> python repro_4963.py
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

> pytest tests/unit/eval/test_biomedical_ner_suite.py -q
FAILED ...::test_biomedical_ner_runs_the_model_once_per_fixture - AssertionError: assert {'bc5cdr:bc5-...00-1': 2, ...} == {'bc5cdr:bc5-...00-1': 1, ...}
FAILED ...::test_biomedical_ner_per_corpus_slices_use_the_overall_predictions - assert 0.0 == 1.0
2 failed, 8 passed in 7.85s
```

Someone reading the report could see an overall F1 score of 1.00 and conclude that the model performed perfectly. But when they looked at the individual corpora, every one of them showed an F1 score of 0.00. That makes the report misleading. A researcher or engineer might conclude that the model performs differently across datasets, when the real problem is that the benchmark ran inference again and received different predictions. Both sets of metrics should be based on the same predictions.

### The fix

I first resolve the model runner once, using the provided runner or the same default runner fallback used by `run_benchmark`. For the overall benchmark pass, I pass a recording runner. It calls the actual model runner and stores each fixture's raw predictions in a dictionary, using `fixture_id` as the key. For the per-corpus calculations, I pass a replay runner instead. It looks up the predictions already stored for each fixture and returns them without calling the actual model again. This allows the existing benchmark logic to calculate the per-corpus metrics using the same predictions as the overall score.

```diff
# openmed/eval/datasets/biomedical_ner.py (excerpt)
+    model_runner = runner or _shared_default_model_runner()
+    predictions: dict[str, list[Any]] = {}
+
+    def recording_runner(
+        fixture: BenchmarkFixture, model_name: str, device: str
+    ) -> list[Any]:
+        predictions[fixture.fixture_id] = list(
+            model_runner(fixture, model_name, device)
+        )
+        return list(predictions[fixture.fixture_id])
+
+    def replay_runner(
+        fixture: BenchmarkFixture, model_name: str, device: str
+    ) -> list[Any]:
+        return list(predictions[fixture.fixture_id])
+
     report = run_benchmark(
         ...
-        runner=runner,
+        runner=recording_runner,
     ...
     metrics["per_corpus"] = _per_corpus_metrics(
         ...
-        runner=runner,
+        runner=replay_runner,
```

I kept the change inside `biomedical_ner.py` because the issue was in how this specific benchmark orchestrated its two passes. I didn't need to change the shared `harness.py` implementation, which could affect other benchmark suites. Keeping the fix local reduced the scope of the change and avoided unnecessary changes to shared code.

### The after run

The after run showed that the fix addressed both problems. Before the fix, five fixtures resulted in ten model calls. After the fix, the same five fixtures resulted in five calls, one per fixture. The overall exact F1 remained 1.00, and every corpus score changed from 0.00 to 1.00 in the controlled reproduction.

```
> python repro_4963.py        (branch, commits cfa912599 and 11991c6c4)
1) counting runner: 5 fixtures, 5 calls
2) runner that changes its answer on a second call:
   overall exact F1: 1.00
   bc5cdr exact F1: 1.00
   ncbi_disease exact F1: 1.00
   jnlpba exact F1: 1.00
   species_800 exact F1: 1.00
   bc2gm exact F1: 1.00

> pytest tests/unit/eval/test_biomedical_ner_suite.py -q
10 passed in 5.77s
```

The focused biomedical NER test file passed all 10 tests. Running it together with the model registry tests resulted in 23 passing tests and 4 passing subtests. Ruff, formatting, effect-path checks, and mypy also passed.

### When the full suite failed

The full test selection reported 65 failures on my Windows 11 machine. Instead of immediately assuming they were caused by my changes, I checked whether the same failures happened on the original master branch.

```
# my branch, November 10, 2026
65 failed, 29217 passed, 506 skipped ... in 643.84s
# the same 31 test files on master 35fe34e22
65 failed, 713 passed, 31 skipped ... in 49.51s
```

I found the same 65 failures across the same 31 test files on master. The failures involved environment-specific issues, including symlink permissions (`WinError 1314`), line-ending differences in golden files, a bash snippet check, gRPC stub drift, and skill-pack linking. None of the failing files imported the biomedical NER module I had changed.

I updated my implementation plan and ship manifest to document the full-suite failures as an environment limitation. I kept the focused tests, lint, formatting, and type checks as required checks, and documented that the full suite should be validated through Linux CI. This is the row I added to the manifest's excluded checks:

| Check | Date | Why it doesn't count against this PR |
|---|---|---|
| Full unit suite | 2026-11-10 | 65 tests in 31 files fail on master too on Windows (WinError 1314 symlinks, CRLF golden files, bash snippet check, gRPC stub drift, skill-pack linking). None import biomedical_ner. The PR's Linux CI runs the full suite. |

I thought this was the right decision because I had evidence that the failures were already present on the base branch. I didn't want to misrepresent the results by claiming that the full suite passed, or spend time changing unrelated code to address failures outside the scope of my contribution.

### The maintainer's reply

After I posted my reproduction results on issue #4963, the maintainer thanked me for checking the call count and testing the runner that changed its answer on a second call. They specifically said those were the two regression cases requested by the issue and that they had assigned the issue to me:

> Thanks for checking the call count and following up with the changing-answer runner, @Aniruthan-0709! I've assigned this to you. Those are the two regression cases the issue asks for, and I see you've opened #5541 with the replay approach. Thanks for getting the implementation and focused tests together.
>
> (maintainer on maziyarpanahi/openmed #4963, November 11, 2026)

That was meaningful because it showed that my investigation had addressed the actual acceptance criteria. I wasn't just proposing a solution based on reading the code; I had demonstrated the problem with a reproducible test and shown that the fix addressed both cases. It also gave me confidence to move forward with the pull request.

### Keeping the PR mergeable

After I opened the PR, the owner changed the CHANGELOG in a new release, which caused a merge conflict with my branch, so I fixed that on November 14, 2026. GitHub's merge status for the PR before and after:

```
November 13, 2026:  head 11991c6, base 35fe34e, mergeable: false (conflict in CHANGELOG.md)
November 14, 2026:  head 99c600e, base 174fd48, mergeable: true
```

## Limitations and what's next

I validated the fix using fake model runners and synthetic fixtures. I did not run an actual biomedical NER model, so I haven't yet demonstrated the behavior with a real model backend or a full real-world benchmark run. The fix also doesn't implement caching across separate benchmark executions. It only reuses predictions within a single benchmark run.

Next, I would validate the change with a real biomedical NER model and a representative benchmark run. I would compare the overall and per-corpus predictions and metrics, verify that each fixture is still inferred only once, and confirm that the existing report format remains unchanged. I would also rely on the project's Linux CI to validate the full test suite in an environment that avoids the Windows-specific failures.

If I started over, I would establish the reproduction and regression tests as early as possible, before changing the implementation. That helped me understand exactly what was broken and gave me a clear way to measure whether the fix worked. I would also check the project's CI environment and test requirements earlier. That would help me distinguish environment-specific failures from actual regressions sooner. Most importantly, I would continue using evidence to guide decisions rather than assuming that a failing test automatically means my changes introduced a problem.
