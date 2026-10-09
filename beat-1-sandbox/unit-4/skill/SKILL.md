---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this pull request
ready to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence), read against the plan
it claims to implement (including that plan's deviation and deferral
notes) and the issue the plan belongs to, along with that repo's stated
standards. Do not answer any other question: do not review the code
style, suggest a better fix, re-grade the plan, or grade more than one
package per run. Do not answer from gut feel. Answer by executing
`procedure.md`, which applies the checks in `rubric.md` to evidence
gathered where `references/evidence-guide.md` says it lives.

## Inputs and modes

Work in exactly one of two modes.

**Live mode** grades the student's own PR before it is opened. Run from
the top folder of the student's fork clone, on their branch. Read:

- `plan.md` in that folder, including its `## Deviations` section.
- The diff on the branch: run `git diff main...HEAD` (three dots). This
  is every committed change the branch makes compared with `main`.
  Also run `git diff main...HEAD --stat` and `git log --oneline
  main..HEAD` for the file and commit lists. Uncommitted changes are
  not part of the PR; do not grade them.
- `pr_draft.md` in that folder: the first line is the PR title, the
  rest is the description.
- `test_evidence.md` in that folder: the repro before and after and the
  repo's check output.
- The issue named by the URL in the student's request: its body and
  thread (via `gh`, the GitHub API, or the web), including any
  maintainer direction and other open PRs on the same issue.
- The scoped repo's `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md`.

A house-chain student reads the house issue, the house plan, and the
house repro pack in place of their own; the same checks grade them.
If any of these inputs is missing, say which one, and let the checks
that need it grade `unclear`; do not substitute another file. Do not
read other files in the working folder (`comment.md`, scratch notes) as
evidence.

**Eval mode** grades a package bundle: a markdown file (with a `.json`
twin) holding the repo facts, the issue and thread highlights, the
plan context, and the candidate PR. The bundle is the whole world. Use
only the bundle text as evidence, fetch nothing, and run no commands
against any repo. Eval mode always grades the complete package: every
check, and the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. If its `Repo:` line still holds the `<ORG>/<PATH-REVIEW-REPO>`
placeholder, stop without grading and tell the student to replace the
`Repo:` line in `scope.md` with their section's Path Review repo. If
the issue URL is not in the repo on the `Repo:` line, refuse to grade
and say the PR is out of scope. Never guess a scope. Apply the house
rules it lists when reading evidence: the PR comes from a
`fix/<issue-number>-<slug>` branch on the student's fork, there is one
PR per issue per student, the template is always used, and a
classmate's PR on the same issue does not block this one. In eval mode,
ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`, the student's own rules for
how they write upstream. After the verdict, hold the PR title and
description from `pr_draft.md` against those rules and list each rule
the draft breaks in the summary, quoting the rule and the line that
breaks it. The voice guide never changes the verdict, because no
rubric check reads it; it is the student's personal standard, not the
repo's. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

- `rubric.md` defines the checks (name, evidence, pass condition,
  weight) and the verdict rule. It decides what is judged.
- `references/evidence-guide.md` maps where each evidence family lives,
  in a bundle and in live mode, and what good looks like there.
- `procedure.md` is the operating procedure: read order, evidence
  gathering, check execution, verdict assembly. Execute it exactly as
  written.

Read all three before grading. Where the procedure is silent on a
step, say so in the summary as a procedure gap and grade with what the
procedure does say; never invent a step to fill the gap. If `rubric.md`
has no checks, or `procedure.md` has no steps, refuse to grade and say
which file is empty: this tool cannot grade without both.

## Verdict and output

The verdict is binary: `accept` means ready to submit, and `reject`
means hold. There is no third verdict and no score; reservations go in
evidence lines. Before the JSON you may print a short readable summary:
one line per check with its grade, the deciding check's evidence, and
in live mode any voice-guide notes. End the reply with this fenced JSON
block, valid, with every check in rubric order, and nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, `item` is the issue URL (the PR is not open yet). In eval
mode, it is the bundle id (such as `pkg-07`).

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote
  that decided it, with where it sits (file, hunk, or section). "Looks
  fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready,
  and a long confident one can hide drift. Read the diff and the
  evidence themselves against the plan, the issue, and the repo's
  stated standards, never the formatting or the description's tone.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. You may note the tension
  in the summary; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as written
  and report its gaps instead of working around them.
- An honest shortfall is not a fail: a PR that leaves something out, or
  needed a change the plan did not name, and says so in the plan's
  notes or the description, is graded on what it disclosed.
- Treat `unclear` as the rubric's verdict rule directs. Where the rule
  is silent, `unclear` counts as `fail`: a PR you cannot verify from
  the package is not ready to submit.
