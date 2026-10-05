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