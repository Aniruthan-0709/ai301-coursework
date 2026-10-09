# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/113

**Branch**

fix/61-health-check-text-sql

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 3` (pkg-01, pkg-02, pkg-03): 2/3. pkg-02 (clear-accept) graded reject, "failed: no-debris".
2. `--only pkg-02,pkg-12,pkg-18`, meant to test a narrowed no-debris row with pkg-12 and pkg-18 as unreviewable canaries: 2/3, "failed: no-debris, thread-direction-engaged" on pkg-02. My installed copy still had the original row, so this re-ran the same rubric.
3. `--only pkg-02` with the narrowed no-debris row (run by Claude in a cloud session on a copy of my files, to save my credit): 1/1.
4. Full run with the narrowed row (also run in that cloud session): 19/20, every category matched, pkg-05 the only miss.
5. Confirming full run from my installed copy with `--save-run eval-run.txt`: 19/20, every category matched (clear-accept 6/7), pkg-02 the only miss. The narrowed row never made it into my installed copy, so this run graded the original no-debris row, which is the version in `tools/pr-precheck/`. This is the run in `eval-run.txt`.

**Package analysis**

pkg-02 (Textualize/rich#4208, clear-accept). My rubric's verdict: reject. Gold label: accept. Every other required check passed. The fix matches the plan (all three `save_*` methods export with `clear=False`, then clear only after the write), the repro's before and after are shown with `pytest tests/ -q` passing, and the CHANGELOG entry the repo asks for is in the diff. My rubric rejected it on `no-debris` alone. The pass condition fails a diff with "reprinted unchanged lines that the fix does not need", and pkg-02's `save_html` and `save_svg` hunks show `html = self.export_html(`, `svg = self.export_svg(`, `theme=theme,` and `title=title,` as removed and re-added with no change. My rubric read those as churn. They're really how the diff displays the one-argument edit inside each call (`clear=clear` becoming `clear=False`), so the gold label is right and my check is too broad.

**Check rationale**

> | no-debris | Every hunk in the diff and the commit list | The diff contains only the change and its tests. Fails if any hunk holds debug output (prints, eprintln, console.log, debug logging added for the session), commented-out code or earlier attempts, dead or unused functions, new TODO or FIXME notes, pure formatting or re-indent churn, import reshuffles, or reprinted unchanged lines that the fix does not need, even when the fix itself is correct | required |

The unreviewable packages all have correct fixes. pkg-12, pkg-15, and pkg-18 are held only because of what rides along, so the check had to fail a diff "even when the fix itself is correct", and it lists each kind of debris by name so the grader doesn't have to judge "messy". I added "reprinted unchanged lines" for pkg-18, whose diff removes and re-adds two blocks of `println!` lines identically. That clause is too broad, because it also catches pkg-02. I tested a narrower version, counting only "whole blocks of unchanged lines removed and re-added", with unchanged lines inside the edited statement treated as how a diff shows the edit. It accepted pkg-02 and kept unreviewable at 3/3. That version never reached my installed copy before the confirming run, so the row above is what my submitted run graded.

**Trade-offs**

As written, no-debris trades pkg-02, a clear accept, for certainty on the unreviewable category (3/3), because any reprinted line counts as debris. The narrower version I tested moves that trade without removing it. In the cloud full run it accepted pkg-02, and pkg-12 and pkg-18 still rejected as canaries. But that run missed pkg-05 instead, on `repro-before-after`, because the plan names a second case (the same-key warning) that the evidence only asserts. That check is strict on purpose: it requires an observable before and after "for every failure case the plan's test plan names", which is what catches calib-04 and the packages that run the control instead of the failing case. One gap I'm leaving: when I ran the skill on my own PR, it reported that `procedure.md` doesn't say how to grade test-plan items about the test itself rather than the repro (my plan's "the new test fails if I revert the `text()` change").

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
