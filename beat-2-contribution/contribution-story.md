# Contribution: Internal embedder with the default config only looks at the first 16 tokens of a chunk

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Aniruthan Swaminathan Arulmurugan
**GitHub Username:** Aniruthan-0709
**Issue:** https://github.com/nextcloud/context_chat_backend/issues/359
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched with my issue-scout: three `gh search issues` queries, one per priority in my fit profile (AI engineering, then MLOps, then data engineering bugs in Python), plus ranking rules that drop claimed, non-bug and front-end results. My first scan on 2026-10-19 accepted nextcloud/context_chat_backend #359. The Issue Selection site had no AI tags, so I graded three picks from there and my scan, but this week every one of them had fallen through: inspect_ai #5712 was fixed by a merged PR, rafter-cli #31 was claimed by another contributor the day before, and agentic-operator-core #86 was closed. I went back to my scan and graded #359 and xarray #11527. The scout accepted #359 on all six checks with an "ambitious" difficulty note, because the fix design is still open: the issue proposes an admin setup check, and those usually live in the PHP app, not this backend. It rejected xarray #11527 because an open PR (#11582) already fixes it, which I had missed when I read the comments. Before claiming, I read CONTRIBUTING.md and Nextcloud's AI_POLICY.md: AI help is allowed with disclosure, but issue and PR text has to be in my own words. My claim comment: https://github.com/nextcloud/context_chat_backend/issues/359#issuecomment-6400518147

#359 fits my first priority: it's an embeddings bug in a RAG backend, and the repo is all Python. The default config sets `n_batch: 16` for the multilingual-e5-large-instruct embedder, and llama-cpp-python silently truncates each chunk to that many tokens, so documents with the same first 16 tokens get identical vectors. I want to learn how to reproduce and test a silent quality bug like this one, where nothing errors and the output always looks valid, and how to work through an open design question with maintainers. My honest read of the scout: it held up better than I did. It caught the open xarray PR that I missed, and its repo-adopted check passed #359 on its outside contributors despite only 29 stars. It fell short in two ways. It still has no Python-only or bug-only check, so I checked the repo's languages by hand. And its verdicts go stale fast: two of my three picks went from open to taken or closed within a week, so I now regrade right before I claim.

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I regraded last week's three picks after this week's scope patch, then graded two new picks from my 2026-10-19 scan.
- What it produced: an accept for #359 (all six checks pass, difficulty "ambitious": the admin setup check may belong in the PHP app), and rejects for the other four (merged fix, claimed, closed, open PR #11582).
- Difficulties: my unit 1 `bounded-scope` check rejected issues whose design was still under open maintainer debate. #359 has exactly that (a member questioned the proposed setup check), so the old check would likely have rejected the issue I chose. The scout also has no Python-only check, and it ran into a permissions problem: `claude -p` can't approve reads of the skill folder, so I had to run it interactively.
- What changed: I replaced `bounded-scope` with the `scope-feasible` check, which only rejects umbrella or checklist issues and turns open edges into a difficulty note. I also logged the change in scan-notes.md. I'd still add a check that the fix is in Python.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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
