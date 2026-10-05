# Procedure: how this skill grades a plan package

## Read order

1. Live mode only: read `scope.md` first. Confirm the issue URL is in
   the repo on the `Repo:` line. If it is not, or the line still has
   the placeholder, stop and say so without grading. Note the house
   rules (a classmate's plan does not block this one; no piggybacking).
2. Read `rubric.md` and `references/evidence-guide.md`. Write down the
   list of check names, which ones are required, and the verdict rule.
3. Read the issue (title, body, any steps or root cause the reporter
   gave). Note in one line what the bug is and what the reporter
   expected.
4. Read the repro evidence before the plan. Note: the failing step and
   its exact output, every control run and what it changed, and the
   expected vs actual lines. The controls matter most, because they
   are what `cause-fits-evidence` tests the plan's cause against.
   Reading the plan first makes it easy to accept its story and miss a
   control that contradicts it.
5. Read the thread highlights. Note every comment from an OWNER,
   MEMBER, or COLLABORATOR that names a cause, gives a direction,
   posts a patch or test build, or rejects an approach, and any open
   PR that targets the same fix.
6. Read the repo-facts block. Note the exact wording of the
   contribution policy, especially anything about AI use and where
   disclosure is required (comments, all contributions, or only PRs).
7. Only now read the candidate plan, then the candidate plan comment.

## Evidence gathering

For each check, pull the evidence from these places and write down the
quote or fact before grading:

1. `cause-fits-evidence`: quote the plan's cause sentence. Then list
   each control run from the repro evidence and, next to it, whether
   that control's result is what you would expect if the plan's cause
   were true. Also note any maintainer cause explanation from step 5
   of the read order.
2. `fix-targets-cause`: quote the plan's change or approach steps.
   Note which code or behavior each step changes, and whether it is
   the thing the repro evidence shows failing, a workaround (docs, a
   different command form), or a wrapper that hides the failure.
3. `one-bounded-change`: list every numbered change, file, and area
   the plan names. Mark each one as "needed for this issue",
   "regression test for this issue", or "extra" (migration, upgrade,
   new option, refactor, restructure, other issue). Also quote the
   plan's not-in-scope line if it has one.
4. `stranger-can-start`: quote the files, functions, or code areas the
   plan names and the approach it commits to. Note any phrase that
   leaves a decision open ("somewhere", "or", "whichever", "not sure",
   "investigate", "look into").
5. `decisive-test`: quote the test plan. Next to it, write the failing
   output from the repro evidence that the test plan says will change,
   and what it says the new result will be.
6. `thread-and-policy`: from your read-order notes, list the
   maintainer direction and open PRs, then quote the part of the plan
   comment that responds to each (or note that none does). Quote the
   AI policy and quote any disclosure sentence in the comment.
7. `unknowns-named` and `comment-matches-plan`: quote the plan's risks
   or unknowns line (or note there is none), and put the comment's
   one-line description of the change next to the plan's.

In live mode, the issue body and thread come from the live GitHub
issue, the repro evidence is the student's posted repro comment as
quoted in the drafts, the repo facts come from the repo's
`docs/CONTRIBUTING.md` and PR template, and the plan and comment are
`plan.md` and `comment.md`. In eval mode, use only the bundle text.

## Check execution

1. Run the checks in table order: `cause-fits-evidence`,
   `fix-targets-cause`, `one-bounded-change`, `stranger-can-start`,
   `decisive-test`, `thread-and-policy`, then the preferred checks.
   Grade every check even after a required check fails, so the student
   sees all the problems in one run.
2. For each check, compare the gathered evidence with the pass
   condition in `rubric.md`, word for word, and grade `pass` or
   `fail`. Judge what the plan will do, not how long or polished it
   is: a short plan can pass every check.
3. Grade `unclear` only when the evidence the check needs is truly
   missing from the package (for example, the plan has no test plan at
   all). Missing evidence is not the same as a fact you did not look
   for, so re-read the part named in the rubric before writing
   `unclear`.
4. A check that only needs one part of the package (for example,
   `decisive-test` needs the test plan and the repro output) may be
   graded from your gathered notes without re-reading the whole
   package.
5. For each grade, write one line of evidence: the quote or fact that
   decided it.

## Verdict assembly

1. Collect the grades of the required checks only.
2. Treat every `unclear` on a required check as `fail`.
3. If every required check is `pass`, the verdict is `accept`.
   Otherwise the verdict is `reject`.
4. Preferred checks never change the verdict; list them in the summary
   as notes.
5. In the summary, name each failing required check and quote the line
   from the package that decided it, so the student knows exactly what
   to fix.
6. Live mode: after the verdict, hold `comment.md` against
   `voice-guide.md` and list any rule it breaks, quoting the rule.
   This never changes the verdict.
7. End with the JSON block from `SKILL.md`, with every check in it, and
   nothing after it.
