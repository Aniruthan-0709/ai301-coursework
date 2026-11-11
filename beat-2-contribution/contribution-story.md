# Contribution: Biomedical NER benchmark runs the model twice per fixture

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Aniruthan Swaminathan Arulmurugan
**GitHub Username:** Aniruthan-0709
**Issue:** https://github.com/maziyarpanahi/openmed/issues/4963
**Pull Request:** https://github.com/maziyarpanahi/openmed/pull/5541

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Turned my issue-select skill into issue-scout and patched its rubric after a bad accept**

I rewrote my fit profile around three priorities (AI engineering, then MLOps, then data engineering bugs in Python) and wrote one `gh search issues` query per priority, plus eight ranking rules that drop claimed, non-bug, and front-end results, cap each repo at two, and order by priority. My first queries returned nothing because I wrapped the OR keywords in quotes, which made `gh` search them as one exact phrase, and my first scan accepted AdeiaSaif/TicketFlow #9, a personal repo with no outside users, even though my profile asks for projects other people actually use. I added a required `repo-adopted` check (100+ stars or 5+ outside contributors) and dropped `Waiting for reply`/`security` labels before grading; on my next run that check rejected agentic-operator-core #86 (51 stars, one outside contributor). What still falls short: nothing in the rubric checks that the fix is a Python bug, so rafter-cli #31, a TypeScript feature, still passed.

**Unit 6: Installed the voice guard on Windows and wrote a repo memory file for context_chat_backend**

I added four Upstream rules to my voice guide (claims, questions, pings) and installed the guard hook, but on Windows the wiring test printed help text instead of stopping: pyenv-win's `python3` shim dropped the hook's stdin and Claude Code ran the command through its PowerShell tool, which the `Bash`-only matcher never saw, so I pointed the hook at the real `python.exe`, widened the matcher to `Bash|PowerShell` and let `check_voice.py` accept that tool name. Once it fired, the guard stopped my claim twice, first for asking two questions and then for asking without saying what I had checked, and checking the config to fix that showed the setting is `n_batch`, not the `batch_size` my draft named. For the repo memory file I ran one scoped orientation over the embedder config path and distilled it into 32 lines; it could not confirm the test command, so I read the CI workflow for it, and it surfaced the repo's AGENTS.md rule that agents must not write issue or PR text, which I added to the file and now follow.

**Unit 7: Installed the ship-gate on Windows, and it stopped my PR three times before letting it through**

My gate manifest for openmed lists four must-pass checks (the two test files that import `biomedical_ner`, the commands behind `make lint`, `make format-check` and `make type-check`) and excludes the full suite with a dated reason, because 65 tests fail on my Windows machine on master too. The gate needed the same Windows patch as my voice guard (real `python.exe`, a `Bash|PowerShell` matcher, `claude` found with `shutil.which`). On my real branch all four checks passed every time, but my pr-precheck stopped the PR three times: first because the description file didn't exist yet, then because the draft ended in a "Generated with" footer that openmed's AGENTS.md bans and ticked "no new warnings" without evidence. The third stop came from my voice guard: my rule said to name the AI tool, but AGENTS.md forbids tool names in PR text. So I fixed the rule instead of breaking the repo's: when a repo bans vendor names, I write "an AI coding assistant" and still say what it did and what I checked. After that the gate passed and I opened the PR through Claude Code.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My first issue was nextcloud/context_chat_backend #359, which my issue-scout accepted on 2026-10-19. I claimed it on 2026-10-25 (https://github.com/nextcloud/context_chat_backend/issues/359#issuecomment-6400518147) and reproduced it, but the maintainer then said they would rather fix it themselves, so I had to pick again with a week left. I scanned again on 2026-11-11 with Claude Code, using my scout's fit profile (AI engineering, then MLOps, then data engineering bugs in Python) and its checks for no competing PR, an adopted repo, and repo conventions. I limited the scan to issues opened since October 1 and to repos with 100+ stars. Most strong results were already taken: pycord #3437, repowise #5422, smolagents #3143, openinference #4269 and all three openmed good-first-issues had open PRs. pycord also failed the conventions check, because its CONTRIBUTING says AI use by new contributors "will likely result in the direct closure" of the PR, and the course requires me to disclose AI use. linagora/openrag #1384, a RAG chunking bug, passed the other checks, but the core team had put it on their next milestone without a help-wanted label. After #359, I didn't want another issue a maintainer planned to fix themselves.

maziyarpanahi/openmed #4963 passed everything. It's labeled bug and help wanted, with no assignee, and none of the repo's ~230 open PRs mention it. The repo has 5.8k stars, and outside contributors' PRs get merged within days. It fits my first priority, because it's in the evaluation harness for clinical NER models: the biomedical NER benchmark runs the model over every fixture twice, once for the overall score and once more for each corpus slice. That doubles inference cost. Worse, a model that doesn't answer the same way twice gets per-corpus numbers and an overall number in one report that come from different predictions. I want to learn how evaluation harnesses keep their numbers consistent. The issue is small and well specified (size S, with acceptance criteria), which matters with two units squeezed into one week. openmed has no AI policy. I drafted my claim with Claude Code, checked it against my voice guide, and posted it myself after my voice guard stopped it once for a missing report-back date. My claim comment is on the issue thread: https://github.com/maziyarpanahi/openmed/issues/4963

What the scout got right: the open-PR check saved me from five dead ends in one pass. What fell short: it can't tell when a maintainer has already planned an issue themselves, which is what sank #359. #4963 has a maintainer milestone too, but the help-wanted label says outside help is welcome. That's the signal I'm now adding to how I read issues.

**Toolkit**

- Tool: issue-scout's fit profile and checks, run as a fresh scan with Claude Code on 2026-11-11 after #359 fell through.
- What it produced: an accept for openmed #4963 (help wanted, no competing PR, adopted repo, no AI restrictions) and rejects for eight others: seven with open PRs, and pycord for its AI policy. openrag #1384 was set aside because the core team had planned it.
- Difficulties: about 300 recent issues came back, and most good-first-issues already had an open PR. The rubric also has no signal for "the core team already planned this", which ended #359. GitHub's search rate limit slowed the scan.
- What changed: I now read a maintainer-set milestone without a help-wanted label as "the team plans to do this", and I treat a help-wanted label as an invitation. I'd add that as a scout check.

---

## Understanding the Issue

**Problem Description**

openmed's biomedical NER benchmark (`run_biomedical_ner_benchmark`) scores a model on five public corpora (BC5CDR, NCBI-disease, JNLPBA, Species-800, BC2GM) and reports an overall score plus one score per corpus. To get the per-corpus scores, it runs the model over every fixture a second time instead of reusing the predictions from the overall pass.

**Expected Behavior**

Each fixture goes through the model once, and the overall and per-corpus metrics come from those same predictions. For a deterministic model the report doesn't change.

**Current Behavior**

Each fixture goes through the model twice, so a benchmark run costs twice the inference. When no runner is passed, each per-corpus pass also builds its own default runner and model loader. A runner that doesn't return the same spans every time can produce per-corpus slices that disagree with the overall score in the same report. In my repro, the overall F1 was 1.00 and every corpus scored 0.00.

**Affected Components**

`openmed/eval/datasets/biomedical_ner.py`: `run_biomedical_ner_benchmark` and `_per_corpus_metrics`. The fix also relies on `run_benchmark` and `_shared_default_model_runner` in `openmed/eval/harness.py` but doesn't change them. Tests are in `tests/unit/eval/test_biomedical_ner_suite.py`.

**Toolkit**

- Tool: Claude Code reading the source at master 35fe34e22, starting from the line numbers in the issue's "Problem and existing coverage" section.
- What it produced: a trace of the two `run_benchmark` calls, plus a detail the issue doesn't mention: with no runner, every per-corpus call builds a fresh `_shared_default_model_runner()`, so the model loader is rebuilt for each corpus.
- Difficulties: `harness.py` is about 3,600 lines, so I had to work out which helpers the suite actually reaches (`run_benchmark` and its default-runner fallback) before I trusted the trace.
- What changed: I put the default-runner detail in my claim and my plan, so the fix resolves the runner once in the suite function instead of only caching predictions.

---

## Reproduction Process

**Environment Setup**

Windows 11 and Python 3.13.0 (pyenv-win). The clone is my fork of maziyarpanahi/openmed at master 35fe34e22 (2026-11-08), cloned with `gh repo fork maziyarpanahi/openmed --clone -- --filter=blob:none -c core.autocrlf=false` and set up with `uv sync --frozen --extra dev`, as CONTRIBUTING.md says. No model download is needed. The repro uses fake runners and five synthetic one-document corpora written to a temp folder, the same pattern the repo's own tests use, so it runs offline in seconds. Challenges: the repo is about 3,300 Python files, so I used a blobless clone. I turned off `core.autocrlf` so ruff and the patch files see LF line endings. Windows has no `make`, so I ran the commands behind the Makefile targets directly. My first attempt failed because I hadn't saved the script and patch files where the commands expected them.

**Steps to Reproduce**

1. `gh repo fork maziyarpanahi/openmed --clone -- --filter=blob:none -c core.autocrlf=false`, then `cd openmed` and `uv sync --frozen --extra dev`
2. Save the script below as `repro_4963.py` next to the clone (I used `..\unit7\`).
3. On master, run `uv run --frozen --extra dev python ..\unit7\repro_4963.py`. It counts runner calls per fixture, then uses a runner that returns gold spans on a fixture's first call and nothing after, and prints the overall and per-corpus exact F1.
4. Observed result: 10 runner calls for 5 fixtures (2 each). With the drifting runner, overall exact F1 is 1.00 and every corpus is 0.00.

<details><summary>repro_4963.py</summary>

```python
"""Reproduce maziyarpanahi/openmed#4963: the biomedical NER suite runs the
model over every fixture twice (once overall, once more per corpus).

Run from the top folder of the openmed clone:
    uv run --frozen --extra dev python repro_4963.py
Uses five synthetic one-document corpora and fake runners, so no model is
downloaded and nothing touches the network.
"""

import json
import subprocess
import tempfile
from collections import Counter
from pathlib import Path

from openmed.eval.datasets import (
    BC2GM,
    BC5CDR,
    BIOMEDICAL_NER_CORPORA,
    JNLPBA,
    NCBI_DISEASE,
    SPECIES_800,
    load_biomedical_ner_fixtures,
    run_biomedical_ner_benchmark,
)


def bigbio(record_id, text, entities):
    return {
        "id": record_id,
        "passages": [{"offsets": [[0, len(text)]], "text": text}],
        "entities": [
            {"id": f"T{s}-{e}", "offsets": [[s, e]], "text": [text[s:e]], "type": t}
            for t, s, e in entities
        ],
    }


def write_suite(root: Path) -> None:
    docs = {
        BC5CDR: bigbio(
            "bc5-1",
            "Aspirin treats melanoma.",
            [("Chemical", 0, 7), ("Disease", 15, 23)],
        ),
        NCBI_DISEASE: bigbio(
            "ncbi-1", "Breast cancer progresses.", [("Disease", 0, 13)]
        ),
        JNLPBA: bigbio(
            "jnlpba-1", "BRCA1 protein activates T cells.", [("protein", 0, 13)]
        ),
    }
    for corpus, doc in docs.items():
        (root / f"{corpus}.json").write_text(
            json.dumps({"documents": [doc]}), encoding="utf-8"
        )
    (root / f"{SPECIES_800}.conll").write_text(
        "Escherichia B-Species\ncoli I-Species\ngrows O\n", encoding="utf-8"
    )
    (root / f"{BC2GM}.conll").write_text(
        "TP53 B-GENE\nregulates O\nEGFR B-GENE\n", encoding="utf-8"
    )


def main() -> None:
    commit = subprocess.run(
        ["git", "log", "-1", "--format=%h %cd", "--date=short"],
        capture_output=True,
        text=True,
    ).stdout.strip()
    branch = subprocess.run(
        ["git", "branch", "--show-current"], capture_output=True, text=True
    ).stdout.strip()
    print(f"openmed {branch} at {commit}")

    with tempfile.TemporaryDirectory() as tmp:
        write_suite(Path(tmp))
        fixtures = load_biomedical_ner_fixtures(tmp)

    calls = Counter()

    def counting_runner(fixture, model_name, device):
        calls[fixture.fixture_id] += 1
        return list(fixture.gold_spans)

    run_biomedical_ner_benchmark(fixtures, model_name="fake", runner=counting_runner)
    print(f"1) counting runner: {len(fixtures)} fixtures, {sum(calls.values())} calls")
    for fixture_id, count in sorted(calls.items()):
        print(f"   {fixture_id}: {count}")

    seen = Counter()

    def drifting_runner(fixture, model_name, device):
        # Gold spans on a fixture's first call, nothing on later calls.
        seen[fixture.fixture_id] += 1
        return list(fixture.gold_spans) if seen[fixture.fixture_id] == 1 else []

    report = run_biomedical_ner_benchmark(
        fixtures, model_name="fake", runner=drifting_runner
    ).to_dict()["metrics"]
    print("2) runner that changes its answer on a second call:")
    print(f"   overall exact F1: {report['exact_span_f1']['f1']:.2f}")
    for corpus in BIOMEDICAL_NER_CORPORA:
        f1 = report["per_corpus"][corpus]["exact_span_f1"]["f1"]
        print(f"   {corpus} exact F1: {f1:.2f}")


if __name__ == "__main__":
    main()
```

</details>

**Reproduction Evidence**

- **Commit showing reproduction:** none. The repro is a standalone script run against master 35fe34e22. My regression tests (commit cfa912599 on my branch) reproduce it inside the repo's own suite: both fail on master.
- **Screenshots/logs:** output on master, run 2026-11-10:

```
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
```

- **My findings:** Reproduced. Every fixture is passed to the runner twice, once by the overall `run_benchmark` call and once more by `_per_corpus_metrics`. When the runner's answer changes between calls, the per-corpus F1 comes from different predictions than the overall F1, so the same report says 1.00 overall and 0.00 for every corpus. I did not run a real NER model. The fake runners show the call count and the mismatch directly, and the issue's own acceptance criteria use the same kind of counting runner.

**Toolkit**

- Tool: a standalone repro script with fake runners, written with Claude Code and run by me on my machine. My voice guard checked the claim before it posted.
- What it produced: the call count (10 for 5 fixtures) and the F1 mismatch (1.00 overall, 0.00 per corpus) on master. These are the same numbers my plan and tests use.
- Difficulties: my voice guard stopped my claim because it had no report-back date, which my own unit 6 rule requires. My first run also failed with "can't open file" because I hadn't saved the files yet.
- What changed: I added "by Thursday, November 12" to the claim, and from then on I wrote the files from PowerShell instead of downloading them.

---
## Solution Approach

**Analysis**

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

**Proposed Solution**

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

**Implementation Plan**

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

**Toolkit**

- Tool: plan-check, grading `plan.md` and `plan-comment.md` for #4963 in an interactive `claude` session, with the skill folder added through `--add-dir`.
- What it produced: accept on the first run. All six required checks passed (cause-fits-evidence, fix-targets-cause, one-bounded-change, stranger-can-start, decisive-test, thread-and-policy), and so did both preferred checks. It confirmed my line references at 35fe34e22 and that `run_benchmark` rejects duplicate fixture IDs, which my replay dict relies on.
- Difficulties: its procedure still expects the classroom repo. It looks for a `Repo:` line and a separately posted repro comment, and it doesn't read the voice guide in `~/.claude/voice-guard/`. It also said my comment repeats the issue's trace, and that it doesn't name the file it changes.
- What changed: I put the 65 Windows-only test failures in the plan as a dated deviation, so the PR says it up front. I still need to update plan-check's procedure for real repos: no `Repo:` line, a repro that can live in the plan comment, and a pointer to my real voice guide.

---

## Testing Strategy

Three layers, each run before and after the fix, on Windows 11 with Python 3.13.0:

1. **The reproduction, re-run.** `repro_4963.py` on master gives 10 runner calls for 5 fixtures, plus overall F1 1.00 against 0.00 for every corpus. On my branch it gives 5 calls (1 each) and 1.00 everywhere.
2. **Regression tests that fail without the fix.** `test_biomedical_ner_runs_the_model_once_per_fixture` (a counting runner must be called exactly once per fixture) and `test_biomedical_ner_per_corpus_slices_use_the_overall_predictions` (a drifting runner must still score 1.00 on every slice) come straight from the issue's acceptance criteria. With only the tests applied on master: `2 failed, 8 passed`. With the fix: `10 passed`. The existing per-corpus F1 test and the CLI test still pass, which shows the report is unchanged for a deterministic runner.
3. **The repo's own checks** from CONTRIBUTING.md, run with uv because Windows has no `make`: `pytest tests/unit/eval/test_biomedical_ner_suite.py tests/unit/test_model_registry.py` (the only two test files that import the module) gives `23 passed`; `ruff check .` prints `All checks passed!`; the effect-path check prints `effect_paths_verified`; `ruff format --check .` reports `3331 files already formatted`; and `mypy` prints `Success: no issues found`. The full PR selection (`pytest tests/ -m "not slow and not fuzz" -n auto`) gave `65 failed, 29217 passed`. I re-ran those 31 failing files on master and got the same `65 failed`. They're Windows environment failures: symlinks need admin rights (WinError 1314), golden files expect LF, and a few similar checks. None of them touch `biomedical_ner`, so I excluded the full suite in my ship manifest with that reason and will rely on the PR's Linux CI.

**Toolkit**

- Tool: the repo's pytest, ruff and mypy through `uv run`, my repro script, and the ship-gate manifest that lists which checks must pass.
- What it produced: before and after outputs for all three layers (saved in `test_evidence.md`) and a manifest with four checks that must pass and one dated exclusion.
- Difficulties: the full suite took 11 minutes and failed 65 tests, all on Windows and none in my code. Proving that took a second run of just those files on master.
- What changed: I narrowed the manifest's test row to the files that exercise my change, and moved the full suite to the excluded table with the master result as evidence, instead of deleting it.

---

## Implementation Notes

**2026-11-10**

My #359 claim fell through after the maintainer said they'd fix it themselves, so I scanned again with my scout's checks and picked openmed #4963. Reading `run_biomedical_ner_benchmark` confirmed the double `run_benchmark` call. It also showed that with no runner passed, each per-corpus call builds a fresh default runner, so the model loader is rebuilt for every corpus. That's why the fix resolves the runner once in the suite function, instead of only caching predictions.

**2026-11-10**

I considered adding a predictions parameter to `run_benchmark` in `harness.py`. I dropped it because every other suite uses that function, and the issue says a replay runner is enough. The fix stays inside `biomedical_ner.py`: a recording runner for the overall pass and a replay runner keyed by `fixture_id` for the per-corpus pass. That's safe because `run_benchmark` already rejects duplicate IDs. The per-corpus calls still time the replay, but `_per_corpus_metrics` keeps only F1 and counts, so no reported latency changes. The repo's pinned ruff (0.15.22) asked for one line to be wrapped.

**2026-11-10**

Posted my claim after the voice guard made me add a report-back date. Forked and cloned with LF line endings, then ran the repro on master: 10 calls for 5 fixtures, and 0.00 per-corpus F1 with the drifting runner. I applied only the tests first and confirmed both fail (`2 failed, 8 passed`), then committed them (cfa912599). I applied the fix and re-ran: 5 calls, 1.00 everywhere, `10 passed`, ruff clean. Committed the fix (11991c6c4).

**2026-11-10**

The full test suite failed 65 tests, which worried me until I saw they were all symlink-privilege and line-ending failures. Re-running the same 31 files on master gave the same 65, so I recorded the full suite as excluded in my manifest instead of chasing failures my change didn't cause. I installed the ship-gate and patched it for Windows the same way as my unit 6 voice guard: the real python.exe instead of the pyenv shim, the matcher widened to `Bash|PowerShell`, and `claude` resolved with `shutil.which`. The hook test stopped `gh pr create --help` with `ship-gate: STOP.`, and plan-check accepted my plan on the first run.

**Toolkit**

- Tool: Claude Code for drafting the fix, the tests and the PowerShell blocks. I ran every command on my machine and pasted the output back.
- What it produced: two commits (tests, then fix), the patch files, and the Windows patch for the ship-gate.
- Difficulties: the cloud session that drafted the fix wasn't allowed to run openmed's code, so nothing was verified until I ran it. Copying long blocks out of the app sometimes adds `\_`, so every block that writes a file strips that.
- What changed: I treat every number as unconfirmed until my own run prints it. The plan's predicted numbers were marked "confirm" until my output matched them.

---

## Pull Request

**PR Link:** https://github.com/maziyarpanahi/openmed/pull/5541 (opened 2026-11-10)

**PR Description:** The PR follows openmed's template. It explains that `run_biomedical_ner_benchmark` ran the model over every fixture twice, and that the fix records each fixture's predictions in the overall pass and replays them for the per-corpus slices, the replay approach the issue suggests. It lists the three changed files, shows the before and after numbers (10 calls to 5, per-corpus F1 0.00 to 1.00, `2 failed` to `10 passed`), states the 65 Windows-only full-suite failures and that the same 65 fail on master, and marks every template box I couldn't tick as N/A with a reason. It says `Fixes #4963` and `Closes #4963`. Under Notes for Reviewers it discloses that I used an AI coding assistant to trace the bug and draft the fix, tests, repro and description, and that I ran every command myself. It doesn't name the tool, because AGENTS.md forbids that.

**Maintainer Feedback:**

- 2026-11-10: none yet. I opened the PR the same evening.

**Status:** Awaiting review

**Toolkit**

- Tool: my ship-gate (a PreToolUse hook that runs my manifest's checks and my pr-precheck before `gh pr create`), with my voice guard behind it. I ran `check_gate.py --branch --base master` by hand until it passed, then opened the PR by asking Claude Code to run `gh pr create --body-file pr_draft.md` unchanged.
- What it produced: three stops, then `ship-gate: PASS` with every repo check and every rubric check passing, and PR #5541.
- Difficulties: the first stop was mine, because I ran the gate before writing the description file. The second stop caught a generation footer that openmed's AGENTS.md bans, a rule I'd missed when I wrote "no AI policy" in my notes. The third came from my voice guard: its rule to name the AI tool conflicted with the same AGENTS.md rule. The gate only lets an unchanged PR through for 30 minutes, so every fix meant running it again.
- What changed: I removed the footer and the tool name, cited evidence for the warnings box, and added a repo exception to my voice-guide disclosure rule. I now read a repo's AGENTS.md as well as CONTRIBUTING before calling it policy-free.

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
