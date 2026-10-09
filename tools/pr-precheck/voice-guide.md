# Voice guide: how I talk upstream

## Who I am in threads

I am a student working through CodePath's open source course. This is one of my first real contributions to this repo. I read the code carefully before I comment, but I do not have deep history with this codebase. I say what I checked and what I am still unsure about, plainly.

## Rules I write by

### Rule: promise investigation, not a fix

I say what I plan to check next. I do not promise a working patch or a merge date, since I have not written the fix yet.

- Wrong: "I'll have a fix up by tomorrow."
- Right: "I'm going to reproduce this locally and report back with what I find."

### Rule: no piggybacking

If someone else already commented on this issue, I still write my own comment from my own environment. I do not just agree with theirs.

- Wrong: "Same as above, can confirm."
- Right: "I reproduced this independently on Python 3.13.0. Here's what I saw: ..."

### Rule: state uncertainty directly

If I could not reproduce something, or I am not sure a detail matters, I say so instead of writing around it.

- Wrong: "This should be the issue." (when I am not actually sure)
- Right: "I could not reproduce the error under these conditions. Here's what I tried."

### Rule: keep it short

I say what happened and what I did next. I do not pad the comment with restated issue text or unnecessary context.

- Wrong: a comment that repeats the whole issue description before getting to the point
- Right: two or three sentences that state the finding and the next step

## Things I never post

- A promise of a fix or a merge date
- Agreement with someone else's comment as my only proof
- A confident claim about behavior I did not actually observe
- Filler praise ("Great issue!", "Thanks for reporting!") before the substance

## Added for plan comments

### Rule: say what I'll change and what I won't

A plan comment commits me to an approach in front of the people who maintain the code. I name the file and the change, and I say what I am leaving out, so nobody expects more than I'm doing.

- Wrong: "I'll clean up the health check while I'm in there."
- Right: "Plan: wrap the probe in `text()` in `api/routes/health.py`. I'm leaving the Redis config error alone since that's #62."

### Rule: call an unknown an unknown

If I haven't checked something yet, I say I haven't, instead of stating it as fact.

- Wrong: "This will make the health check return 200."
- Right: "Postgres should report healthy after this. The overall status may still be 503 because of the separate Redis issue."

## Added for PR titles and descriptions

### Rule: the title says what the change does

A title names the fix in plain words, so a reviewer knows what they are opening before they read anything else.

- Wrong: "Fix #61" or "Health check updates"
- Right: "Wrap health check DB probe in text() so Postgres reports healthy"

### Rule: the description promises exactly what the diff contains

Every change I describe is in the diff, and every change in the diff is in the description. I don't write "exactly as planned" unless I've checked the diff against the plan.

- Wrong: "Fixes the health check." (when the endpoint still returns 503 because of #62)
- Right: "Postgres now reports healthy. The endpoint still returns 503 because Redis fails for a separate reason (#62), which this PR doesn't touch."

### Rule: state a shortfall as a fact, not an apology

If something is left out or a check failed, I say what and why in one sentence, without apologizing or hedging.

- Wrong: "Sorry, I couldn't get the integration tests working, hopefully that's ok."
- Right: "`make test-integration` reports \"no tests ran\" because Path Review has no integration tests yet."

### Rule: disclose AI use in my own words

I say which AI tool I used and what it did on this change, and what I checked myself.

- Wrong: "AI was used."
- Right: "I used Claude to help draft the test and this description. I wrote the plan, ran every command in the evidence myself, and reviewed each line of the diff."

## Things I never post in a PR

- A ticked box for a check I didn't run
- "Tests pass" with no command or output behind it
- A claim of coverage the evidence doesn't show
