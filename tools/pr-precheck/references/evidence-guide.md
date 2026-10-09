# Evidence guide: where evidence lives in a PR package

Checks that read each family:

- plan fidelity: `diff-matches-plan`, `description-matches-diff`
- test evidence: `repro-before-after`, `repo-checks-run`
- diff quality: `no-debris`
- standards and comms: `template-and-repo-asks`, `ai-disclosure`, `thread-direction-engaged`, `reviewer-can-follow`

## Plan fidelity (harness category: silent-drift)

Where it lives: Eval mode: the Plan context block (the "Plan:" paragraph's
Scope, "Not in scope", Files list, and any "Known limit" or deferral
note) read against the Candidate PR's Diff (the `--- a/` and `+++ b/`
file headers and each `@@` hunk), and the Candidate PR's Title and
Description read against that same Diff. Live mode: the In scope, Not
in scope, Files, and Approach sections of `plan.md`, plus its
`## Deviations` section, read against `git diff main...HEAD` (three
dots, committed changes only) and `git diff main...HEAD --stat` for the
file list. The title and description are the first line and body of
`pr_draft.md`.

What good looks like: Every file in the diff appears in the plan's Files
list or a deviation note, every hunk does something the plan names, and
every change the plan promises is in the diff or is named as deferred.
calib-01's plan names one file (`themes.gitconfig`) and one deletion,
and the diff deletes exactly that line. Its description says "rendering
is unchanged", which is true of the diff. Drift in the "more" direction
is a hunk the plan never named (calib-03's silent rewrite of
`print/property.js`). Drift in the "less" direction is a promised change
missing with no note (pkg-17 claims docs that the diff never touches).
An honest shortfall (pkg-13's `%` escaping, deferred in the plan and
restated in the description) is not drift.

## Test evidence (harness category: not-tested)

Where it lives: Eval mode: the Candidate PR's Test evidence section,
read against the Plan context's "Test plan:" sentence and its "Repro
evidence" lines, and against any test command the Repo facts block
names (for example "pass the test suite (yarn test)"). Live mode:
`test_evidence.md` (the repro script re-run on `main` and on the branch,
and the output of `make test-unit`, `make test-integration`,
`make lint`, `make typecheck`), read against the Test plan section of
`plan.md` and the Testing section of `pr_draft.md`.

What good looks like: The same failing input the repro used is run
before and after, and the output shows the result the test plan said
would change. calib-01 runs the strict `configparser` parse: before is
`DuplicateOptionError`, after is `parsed OK`. Each failure case the test
plan names has its own before and after. The repo's checks appear with
the command and the outcome, such as "`cargo test -p ignore` passes (214
tests)". A check that failed or could not run is shown as it ran, with
the reason (for example Path Review's `make test-integration` stopping
with "no tests ran"). Not good: "tested locally", "works now", a run on
the control case that never failed, or a ticked "tests pass" box with no
output behind it.

## Diff quality (harness category: unreviewable)

Where it lives: Eval mode: every `+` and `-` line of the Candidate PR's
Diff, and the Commits list (messages such as "wip", "fix", or "misc
cleanups while debugging" point at debris). Live mode: the full
`git diff main...HEAD` and `git log --oneline main..HEAD`.

What good looks like: Every changed line belongs to the fix or its
tests. calib-01's diff is one removed line. Debris tells: a print,
`eprintln`, `console.log`, or debug log line left behind; commented-out
code or an earlier attempt; a function nothing calls (often behind
`allow(dead_code)` or named `_unused`); a new TODO or FIXME; re-indented
or reflowed lines with no change in meaning; reordered imports; and
unchanged lines removed and added again. A correct fix with any of
these riding along still fails `no-debris`.

## Standards and comms (harness category: standards-wall)

Where it lives: Eval mode: the Repo facts block's "pull requests:" line
(template sections, checklist items, issue-link form, changelog or
whatsnew entry, title convention) and its "contribution policy:" line
(any AI policy), read against the Candidate PR's Title, Description,
and Diff (for required file entries such as `CHANGELOG.md` or
`doc/source/whatsnew/`). The Thread highlights hold maintainer direction
and open PRs. Live mode: `.github/PULL_REQUEST_TEMPLATE.md` and
`docs/CONTRIBUTING.md` in the scoped Path Review repo, read against
`pr_draft.md`, and the live issue thread for maintainer comments and
other open PRs on the same issue. In live mode the course requires an
AI-use disclosure under Notes for Reviewers, even though Path Review
states no AI policy.

What good looks like: Every section the template asks for is present
and has real content, or "N/A" with a reason. The issue is linked in the
form the repo asks for ("Closes #61" in Path Review, "fixes #123" in
minikube). Checklist boxes are ticked only where the evidence backs
them. Any required file entry (CHANGELOG, whatsnew) is in the diff. When
the policy requires AI disclosure, the description names the tool and
what it did, in the author's words. pkg-11 does this. pkg-20 leaves it
out under a strict policy, and pkg-01 skips pandas' required checklist,
`closes #xxxx` line, and whatsnew entry. calib-01 has no template, and
its description meets the one stated ask by referencing #2211.
