# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: Eval mode: the candidate plan's Diagnosis (or "Cause")
line, read against the Repro evidence block's steps, control runs, and
expected/actual lines, and against the Thread highlights for any
maintainer cause explanation. Live mode: the diagnosis section of
`plan.md`, read against the student's posted repro comment on the
issue (as quoted in the plan) and the issue thread.

What good looks like: The stated cause explains the failing step AND
every control run. In calib-01, the cause (the branch-commits view is
not refreshed after push) fits step 4, where re-entering the view
fixes the color. A bad diagnosis blames something a control already
ruled out, like blaming a tokenizer when the same items parse fine
without the flag.

## Scope

Where it lives: Eval mode: the candidate plan's Scope or "Change"
paragraph (In / Not in scope lines), its Files list, and any numbered
"Proposed changes". Live mode: the scope section and files list in
`plan.md`.

What good looks like: One change that fixes this issue, plus tests for
it, with a not-in-scope line that names what is left out. calib-01
changes one callback in `sync_controller.go` and puts push-status
computation and other views out of scope. A drive-by rewrite adds
migrations, new options, refactors, or other issues' fixes next to the
real fix.

## Executability

Where it lives: Eval mode: the candidate plan's Files list and
Approach steps. Live mode: the files and approach sections of
`plan.md`.

What good looks like: The plan names where the change goes (a file,
function, or specific code area) and one chosen way of doing it, so a
stranger could open the file and start. calib-01 names
`pkg/gui/controllers/sync_controller.go` and the exact change (add the
commits context to the post-push refresh scope). A vague plan says
"investigate", "somewhere", or "whichever is easier".

## Test plan

Where it lives: Eval mode: the candidate plan's Test plan (or "Test")
line, read against the Repro evidence block's steps and output. Live
mode: the test plan section of `plan.md`, read against the repro
comment's commands and output.

What good looks like: The test re-runs the repro steps and names the
exact result that should now be different. calib-01: "at step 3 the
color must flip without leaving the view." A vague test plan says
"should feel fast", "nothing breaks", or only "run the test suite".

## Honesty

Where it lives: Eval mode: the candidate plan's Risk / unknowns line
and any sentence that states something as certain, in the plan or the
plan comment. Live mode: the risks and unknowns section of `plan.md`,
and its `## Deviations` section after the build.

What good looks like: Anything not checked yet is called an unknown
("I have not verified which layer clamps the viewport"), not stated as
fact. A short plan with no risks line is fine if it claims nothing the
evidence does not show. A deviation found during the build is written
under Deviations with what changed and why.

## Comms

Where it lives: Eval mode: the Candidate plan comment, read against
the Thread highlights (OWNER / MEMBER / COLLABORATOR comments and open
PRs) and the Repo facts block's contribution policy line. Live mode:
`comment.md`, read against the live issue thread and the repo's
`docs/CONTRIBUTING.md` and PR template.

What good looks like: If a maintainer named a culprit, gave a
direction, or posted a patch, the comment follows it or says why not;
if a PR is already open, the comment says how this plan relates to it.
If the policy requires AI disclosure in comments or all contributions,
the comment says AI was used and how. calib-01 has no comments in the
thread and no disclosure rule, and its comment still respects the
CONTRIBUTING note by keeping the change minimal. Boilerplate ignores
the thread and could be pasted on any issue.
