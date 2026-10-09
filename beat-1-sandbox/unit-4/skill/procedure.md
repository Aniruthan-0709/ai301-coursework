# Procedure: how this tool grades a PR package

## Read order

1. Live mode only: read `scope.md` first. Confirm the issue URL belongs
   to the repo on the `Repo:` line. If it does not, or the line still
   holds the `<ORG>/<PATH-REVIEW-REPO>` placeholder, stop without
   grading and tell the student to fill the `Repo:` line. Note the
   house rules (PR from a `fix/<issue-number>-<slug>` branch on a fork,
   one PR per issue per student, the template is always used, a
   classmate's PR does not block this one).
2. Read `rubric.md` and `references/evidence-guide.md`. Write down the
   check names in table order, which are required, and the verdict
   rule.
3. Read the issue (title, body, repro). Note in one line what the bug
   is.
4. Read the plan before anything the PR says. From the Plan context
   (live: `plan.md`), list: the files it names, what is in scope, what
   is not in scope, every promised change, any deferral or known limit,
   and any deviation note. Then copy the test plan: the failing case(s)
   it names and the expected-after for each. The plan comes first
   because every side-by-side check measures the PR against it; reading
   the description first makes it easy to believe "implements the plan
   exactly" and miss a hunk that says otherwise.
5. Read the repo facts (live: `.github/PULL_REQUEST_TEMPLATE.md` and
   `docs/CONTRIBUTING.md`). List each pull request ask (template
   sections, checklist items, issue-link form, required file entries
   such as CHANGELOG or whatsnew, title rules), any test or check
   command the repo names, and the exact AI-policy wording.
6. Read the thread highlights (live: the issue thread). Note any
   maintainer direction, constraint, or rejected approach, and any open
   PR on the same fix.
7. Read the diff and the commit list (live: `git diff main...HEAD` and
   `git log --oneline main..HEAD`). For each file, list its hunks and
   what each one does in a few words.
8. Read the test evidence (live: `test_evidence.md`).
9. Read the title and description last (live: `pr_draft.md`). List
   every claim it makes about what changed, what was tested, and what
   was left out.

## Evidence gathering

Write down the quote or fact for each check before grading it.

1. `diff-matches-plan`: put the diff's file and hunk list (read order
   step 7) next to the plan's list (step 4). Mark each hunk as
   "planned", "regression test", "deviation note", or "extra". Then mark
   each promised change as "in diff", "deferred with a note", or
   "missing".
2. `description-matches-diff`: next to each description claim (step
   9), write the hunk that backs it, or "none". Flag fidelity claims
   ("exactly as planned", "no other changes") and check them against
   the "extra" marks from item 1.
3. `repro-before-after`: next to each failing case in the plan's test
   plan, write the before output and the after output from the test
   evidence, or "not shown". Check that the input is the failing case,
   not a control that never failed.
4. `repo-checks-run`: next to each check command from read order step
   5 and the plan, write the result shown in the evidence (passed,
   failed with reason, or not shown). Note any ticked template box with
   no matching output.
5. `no-debris`: go through every `+` line and every hunk. List any
   debug output, commented-out code, dead function, new TODO or FIXME,
   formatting or re-indent churn, import reshuffle, or reprinted
   unchanged line. Note debris-style commit messages ("wip", "misc
   cleanups").
6. `template-and-repo-asks`: next to each ask from read order step 5,
   write where the PR meets it (section, line, or file in the diff) or
   "missing".
7. `ai-disclosure`: quote the AI policy, then quote the disclosure
   sentence from the description, or write "none". In live mode, look
   under Notes for Reviewers.
8. `thread-direction-engaged` and `reviewer-can-follow`: list each
   thread item from step 6 with the description line that answers it;
   then write in one line what the title and summary tell a stranger.

In eval mode every fact comes from the bundle text and nothing else. In
live mode use the files and commands named above, and the live issue
thread and repo files in the scoped repo.

## Check execution

1. Run the checks in rubric order: `diff-matches-plan`,
   `description-matches-diff`, `repro-before-after`, `repo-checks-run`,
   `no-debris`, `template-and-repo-asks`, `ai-disclosure`, then the
   preferred checks. Grade every check even after one fails, so the
   student sees every problem in one run.
2. For each check, compare the gathered notes with the rubric's pass
   condition, word for word, and grade `pass` or `fail`. Judge what the
   diff and evidence show, not how long or polished the description is:
   a terse PR can pass every check.
3. A disclosed shortfall is not a fail. Before failing
   `diff-matches-plan` or `description-matches-diff` for something
   missing, check whether the plan's deferral or deviation notes, or
   the description, say it is left out.
4. Grade `unclear` only when the evidence a check needs is truly absent
   from the package (for example, no test evidence section at all).
   Re-read the part the rubric names before writing `unclear`.
5. A check may be graded from the gathered notes without re-reading the
   whole package, as long as the notes cover every part its Evidence
   column names.
6. For each grade, write one line of evidence: the quote or fact that
   decided it, naming the file, hunk, or section.

## Verdict assembly

1. Collect the grades of the required checks only. Count every
   `unclear` on a required check as `fail`.
2. If every required check is `pass`, the verdict is `accept`.
   Otherwise it is `reject`.
3. Preferred checks never change the verdict. List them in the summary
   as notes.
4. The deciding check is the first failing required check in rubric
   order. Quote its evidence line first in the summary, then list any
   other failing required checks with theirs.
5. Live mode only: after the verdict, hold the title and description in
   `pr_draft.md` against `voice-guide.md` and list each rule the draft
   breaks, quoting the rule. This never changes the verdict.
6. End with the JSON block from `SKILL.md`, listing every check in
   rubric order with its grade and evidence line, and nothing after it.
