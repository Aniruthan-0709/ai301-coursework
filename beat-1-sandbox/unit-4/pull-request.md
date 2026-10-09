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
2. Revised no-debris in `rubric.md` and `references/evidence-guide.md`, then ran `--only pkg-02,pkg-12,pkg-18` (pkg-12 and pkg-18 as unreviewable canaries): 2/3, still "failed: no-debris, thread-direction-engaged" on pkg-02. My installed copy still had the old no-debris row, because I hadn't pasted the revision in yet.
3. Re-graded `--only pkg-02` with the revised files (run by Claude in a cloud session on the same files, to save my credit): 1/1. The no-debris evidence read "reprinted export_html( / theme=theme lines are inside the edited statement".
4. Full run with the revised files (also run by Claude in that cloud session): 19/20, every category matched (clear-accept 6/7). pkg-05 was the only miss.
5. Put the revised row into my installed copy and ran the confirming full run with `--save-run eval-run.txt`: 19/20, every category matched. This is the run in `eval-run.txt`.

**Package analysis**

pkg-05 (nushell/nushell#18848, clear-accept). My rubric's verdict: reject. Gold label: accept. My rubric failed it on `repro-before-after`, with the evidence "The issue-script case has before and after tables, but the plan's same-key case (expect one row plus a warning) is only asserted ('Same-key redefine prints the one-time warning'), with no input or output shown." My pass condition requires an observable before and after "for every failure case the plan's test plan names". The plan's test plan names two behaviors, the merged keybinding rows and the same-key replace warning, and the evidence only shows output for the first. The gold label treats the warning as a secondary behavior that a one-line statement covers, since the issue's actual repro (the atuin ctrl-r binding being dropped) is shown decisively before and after, along with `cargo test`, fmt, and clippy. I kept my reading because the same rule is what rejects calib-04 (a plan that names two failure modes, with evidence for only one). Loosening it to "the issue's main repro" would risk letting pkg-07 and pkg-14 through, since they also show some before/after, just not for the failing case.

**Check rationale**

> | no-debris | Every hunk in the diff and the commit list | The diff contains only the change and its tests. Fails if any hunk holds debug output (prints, eprintln, console.log, debug logging added for the session), commented-out code or earlier attempts, dead or unused functions, new TODO or FIXME notes, pure formatting or re-indent churn, import reshuffles, or whole blocks of unchanged lines removed and re-added with no change, even when the fix itself is correct. Unchanged lines that the diff shows as removed and re-added inside the statement the fix edits (for example the opening line of a call whose argument changed) are how diffs display that edit, not churn | required |

My first version failed on "reprinted unchanged lines that the fix does not need". That was meant to catch pkg-18's two blocks of `println!` lines that are removed and re-added identically. But it also caught pkg-02, a clear accept: changing `clear=clear` to `clear=False` inside `self.export_html(...)` makes the diff show `html = self.export_html(` and `theme=theme,` as removed and re-added too, even though nothing about them changed. I narrowed the rule to "whole blocks" of reprinted lines and added the last sentence, so an unchanged line inside the edited statement counts as how a diff displays the edit, not as churn. The fix had to stay narrow, because the unreviewable category depends on this check. pkg-12, pkg-15, and pkg-18 all have correct fixes, and the debris is the only reason they're held.

**Trade-offs**

Loosening no-debris could have flipped the unreviewable packages to accept, so I re-ran pkg-12 and pkg-18 as canaries with `--only` alongside pkg-02, and both stayed reject. The full run then kept unreviewable at 3/3. pkg-18 would still reject even if the narrowed churn rule missed its reprinted blocks, because it also carries a commented-out `eprintln`. The trade-off I accept is in `repro-before-after`: requiring before/after output for every failure case the test plan names costs me pkg-05, a clear accept, but it's what catches calib-04 and the "ran the control, not the failing case" packages. A gap I'm leaving: when I ran the skill on my own PR, it reported a procedure gap. `procedure.md` doesn't say how to grade test-plan items about the test itself rather than the repro (my plan's "the new test fails if I revert the `text()` change"). I didn't change the procedure after the confirming run, so the files I uploaded match the fingerprints in `eval-run.txt`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
