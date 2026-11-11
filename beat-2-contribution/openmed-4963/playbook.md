# Playbook: Aniruthan Swaminathan Arulmurugan

## Response rules

- My reply's first sentence answers the reviewer's point or says what I changed, and names the commit when I pushed one. No "thanks for the review" opener.
- I push every requested change before I reply, and answer each comment in one or two sentences. If I disagree, I say why with evidence from a run, once, and then go with the maintainer's call.
- I never name an AI tool or add a generation footer in PR text, because openmed's AGENTS.md bans it. I never claim a check I didn't run.

## Staleness thresholds

- CONTRIBUTING.md and MAINTAINERS.md state no review wait. openmed has one maintainer, who merges outside PRs in batches.
- Five recent merged outside PRs (checked 2026-11-10), days from opening to merge, with no review comments before it: #5499 0, #4949 1, #4249 17, #4229 22, #4129 28. Waits are either about a day or about 3 to 4 weeks, until the next batch.
- So 30 quiet days with no maintainer comment or review is normal here. I nudge only after 30 quiet days. After a review, I nudge only after 14 quiet days following my reply.
- Failing CI is never a reason to wait: if CI fails because of my change, I fix it before anything else.

## Nudge etiquette

- One nudge, posted as a comment on the PR. It says in one sentence what the PR fixes and that CI passes, and asks one thing: whether anything is needed from me to get it reviewed.
- It tags only @maziyarpanahi, the only maintainer, and never says "just following up", "gentle reminder" or anything about my deadline.
- I don't send a second nudge. If the first gets no answer, I go to the walk-away rules.

## Walk-away criteria

- I close my PR if the maintainer says no or asks for a different approach I won't build, if #4963 gets fixed by another PR, or if 60 days pass with no maintainer response after my nudge.
- My close note is two sentences: why I'm closing (naming the other PR if there is one), and that the branch stays on my fork if anyone wants the tests. No apology and no complaint.
