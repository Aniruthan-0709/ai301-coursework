# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61

**Verdict output**

```
Mode: live. Candidate is in the scoped repo (codepath/pathreview-ai301-fa26-s3), so it's a valid candidate. Path Review house rule noted — no claim comments exist here anyway.

Issue #61 — "Health check DB probe passes a raw SQL string, which fails under SQLAlchemy 2.x" (open, opened 2026-09-10 by Aburke225, COLLABORATOR; labels: bug, good first issue, api, tier-1; 0 comments)

┌───────────────────┬───────┬────────────────────────────────────────────────────────────────┐
│       Check       │ Grade │                            Evidence                            │
├───────────────────┼───────┼────────────────────────────────────────────────────────────────┤
│ maintainer-active │ pass  │ Aburke225 (COLLABORATOR) commented on issue #52 on 2026-09-16  │
│                   │       │ — 3 days ago                                                   │
├───────────────────┼───────┼────────────────────────────────────────────────────────────────┤
│ repo-shipping     │ pass  │ No releases; all 5 latest main commits (2026-09-16 ×3,         │
│                   │       │ 2026-08-24 ×2) landed within 30 days                           │
├───────────────────┼───────┼────────────────────────────────────────────────────────────────┤
│                   │       │ One decided fix in one file: wrap "SELECT 1" in                │
│ bounded-scope     │ pass  │ sqlalchemy.text() in api/routes/health.py, with repro steps    │
│                   │       │ and exact error                                                │
├───────────────────┼───────┼────────────────────────────────────────────────────────────────┤
│ unclaimed         │ pass  │ No assignees, no linked PRs, 0 comments; the repo has 0 pull   │
│                   │       │ requests total                                                 │
├───────────────────┼───────┼────────────────────────────────────────────────────────────────┤
│ policy-compatible │ pass  │ docs/CONTRIBUTING.md (189 lines) has no AI/LLM/generated-code  │
│                   │       │ mention — silence passes                                       │
└───────────────────┴───────┴────────────────────────────────────────────────────────────────┘

All required checks pass → accept. Fit note: it's a small, single-file Python bug fix touching a SQLAlchemy call — squarely the "small, well-scoped bug fix in Python" profile, not a refactor or docs-only change.

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61",
  "checks": [
    {"name": "maintainer-active", "grade": "pass",
     "evidence": "Collaborator Aburke225 commented on issue #52 on 2026-09-16, 3 days before grading (well within 60 days)"},
    {"name": "repo-shipping", "grade": "pass",
     "evidence": "No release history; all 5 most recent main commits (2026-09-16 x3, 2026-08-24 x2) landed within the last 30 days"},
    {"name": "bounded-scope", "grade": "pass",
     "evidence": "Single decided fix: wrap the literal \"SELECT 1\" in sqlalchemy.text() in api/routes/health.py; repro steps given, no debate, no abandoned PRs"},
    {"name": "unclaimed", "grade": "pass",
     "evidence": "assignees: []; no linked PRs and 0 comments on the issue; the repo contains 0 pull requests of any state"},
    {"name": "policy-compatible", "grade": "pass",
     "evidence": "docs/CONTRIBUTING.md contains no mention of AI/LLM/generated or assisted contributions; no AI_POLICY.md exists"}
  ],
  "verdict": "accept"
}
```

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

- Smoke test (--limit 3): 3/3 agree (diagnostic only, no bar)
- Full run 1: 15/20 agreement — below bar; category floor unmet (policy 0/1)
- Full run 2 (after revising bounded-scope, unclaimed, repo-shipping, policy-compatible for the 5 disagreeing issues): 17/20 agreement — below bar; three new bounded-scope failures (issue-01, issue-09, issue-19) introduced by an over-broad clause added to catch issue-20
- Full run 3 (after narrowing bounded-scope to require the reporter's OWN wording to admit unresolved uncertainty, rather than penalizing any issue with multiple named parts): 20/20 agreement — PASS, all categories cleared
- Full run 4 (after live-mode testing exposed that repo-shipping's "merged PRs" fallback clause fails on repos where staff push directly to main with zero PRs ever, even with active recent commits; reworded to count any recent default-branch commit, not just merged-PR ones): 19/20 agreement — PASS, all categories cleared. This is the run committed as eval-run.txt.

**Issue analysis**

issue-20 (excalidraw/excalidraw#11811). Gold: reject. My rubric: reject (agree). The issue was opened by cursor[bot] — an AI agent, not a human — requesting a brand-new toolbar feature (a company-logo shape) whose own description says the logo asset is "TBD" and describes possible app-level wiring as "if needed," meaning the implementation surface is genuinely undecided by the reporter's own words, not just briefly written. My bounded-scope check specifically looks for that kind of self-admitted uncertainty (as distinct from an issue that simply lists several already-decided sub-steps, which should still pass) — so it failed this issue on bounded-scope, which matches the gold reject.

**Check rationale**

policy-compatible: "Fails when the policy explicitly states it does not accept, prohibits, disallows, or otherwise forbids AI-generated or AI-assisted contributions - regardless of whether it uses the literal word 'ban.' Conditions (disclosure, personal understanding/testing requirements, human review) are terms to follow, not a fail. No stated policy passes."

This wording exists because an earlier version of the check keyed on the literal word "ban," and it missed bookwyrm-social/bookwyrm's CONTRIBUTING.md, which states "We do not accept AI-generated code or documentation" without ever using the word "ban." The check needed broader trigger language (accept/prohibit/disallow/forbid) to catch an outright refusal phrased in ordinary language, while still explicitly distinguishing a ban from a mere condition (disclosure, human review), since the evidence guide is clear those are terms to follow, not reasons to reject.

**Trade-offs**

Before this wording, the check missed issue-12 (bookwyrm) entirely because it required the literal word "ban" - a full false accept on a policy the repo stated in plain language. Broadening the trigger words fixed that specific case (confirmed by re-running --only on the affected issues). What this check still gives up: it explicitly treats "no stated policy" as a pass, so a repo with an unwritten but real anti-AI norm - one that isn't documented anywhere the evidence guide tells this check to look - will still be accepted. I accept that miss deliberately, since guessing at unstated norms would make this check unreliable in the opposite direction (rejecting on nothing but vibes).

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit: #61 is a tier-1, single-file Python bug (SQLAlchemy 2.x requires text() around raw SQL) with the root cause and fix already diagnosed. It's a small, bounded first PR that matches my stated preference for bug fixes over new features, and it's close to my SQL/database background even though I haven't done backend web work before.

2. What the verdict got right vs. what I weighed beyond it: the rubric correctly confirmed the mechanical facts - unclaimed, repo actively developed (recent direct-push commits), no restrictive AI policy, and a genuinely bounded fix. What I weighed that the rubric can't see: this is the smallest and lowest-risk of the three accepted issues, which matters to me as a first-ever open-source PR - I'd rather build confidence on a one-line fix than take on #6's ML-scoring logic or #10's from-scratch feature build first.

3. Anticipated difficulty: the code change itself is trivial (wrap the literal SQL string in sqlalchemy.text()). The real friction I expect is environment setup - getting the FastAPI + database stack running locally to verify the fix via GET /health or a unit test - rather than the fix's logic.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.