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

**Unit 6: Installed the voice guard on Windows and wrote a repo memory file for context_chat_backend**

I added four Upstream rules to my voice guide (claims, questions, pings) and installed the guard hook, but on Windows the wiring test printed help text instead of stopping: pyenv-win's `python3` shim dropped the hook's stdin and Claude Code ran the command through its PowerShell tool, which the `Bash`-only matcher never saw, so I pointed the hook at the real `python.exe`, widened the matcher to `Bash|PowerShell` and let `check_voice.py` accept that tool name. Once it fired, the guard stopped my claim twice, first for asking two questions and then for asking without saying what I had checked, and checking the config to fix that showed the setting is `n_batch`, not the `batch_size` my draft named. For the repo memory file I ran one scoped orientation over the embedder config path and distilled it into 32 lines; it could not confirm the test command, so I read the CI workflow for it, and it surfaced the repo's AGENTS.md rule that agents must not write issue or PR text, which I added to the file and now follow.

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

Windows 11, Python 3.13.0 (pyenv-win) in a fresh venv, llama-cpp-python 0.3.36 (the repo pins >=0.3.36) from the prebuilt CPU wheel index, and my fork's clone of nextcloud/context_chat_backend at master 91eabb8 (2026-10-23). The model is multilingual-e5-large-instruct-q6_k.gguf at Hugging Face revision 8738f8d, the exact file the backend downloads in context_chat_backend/controller.py (467,958,912 bytes). Standing up a full Nextcloud instance with the context_chat app and pgvector would have taken days, so I reproduced the embedding step directly: main_em.py passes the config's `embedding.llama` block (including `n_batch`) into llama-cpp's ModelSettings, and the server's /v1/embeddings endpoint calls `create_embedding`, so my script makes that same call with the same model and settings. Challenges: on Windows, `pip install llama-cpp-python` would normally need a C++ build, which the CPU wheel index avoided; and PowerShell 5 saved my output file as UTF-16, so I re-saved it as UTF-8.

**Steps to Reproduce**

1. `python -m venv .venv359` and `.venv359\Scripts\python -m pip install "llama-cpp-python>=0.3.36" --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/cpu`
2. Download the model as `e5.gguf`: https://huggingface.co/Ralriki/multilingual-e5-large-instruct-GGUF/resolve/8738f8d3d8f311808479ecd5756607e24c6ca811/multilingual-e5-large-instruct-q6_k.gguf
3. Save the script below as `repro_359.py` and run `.venv359\Scripts\python repro_359.py <path to clone> e5.gguf`. It reads the e5 embedder's `n_batch` from the clone's config.cpu.yaml (16), embeds the issue's two texts (same first sentence, different last line) with `create_embedding`, then repeats with `n_batch=512`.
4. Observed result: at the default `n_batch: 16` the two vectors are identical; at 512 they differ.

<details><summary>repro_359.py</summary>

```python
import math, re, sys
from importlib.metadata import version
from pathlib import Path
from llama_cpp import Llama

BASE = ("Air India is the flag carrier of India, headquartered in Gurugram, Haryana. "
        "The airline is owned by Air India Limited, which is owned by the Tata Group (74.9%) "
        "and Singapore Airlines (25.1%).\n")
FIRST, SECOND = BASE + "My name is Koala.", BASE + "My mom is a scientist in sanpanpisko."

def cosine(a, b):
    return sum(x * y for x, y in zip(a, b)) / (math.sqrt(sum(x * x for x in a)) * math.sqrt(sum(y * y for y in b)))

def run(model, n_batch):
    llm = Llama(model_path=model, embedding=True, n_batch=n_batch, n_ctx=8192, verbose=False)
    out = llm.create_embedding([FIRST, SECOND])
    a, b = (d["embedding"] for d in out["data"])
    print(f"n_batch={n_batch}: tokens={len(llm.tokenize(SECOND.encode()))} dims={len(a)}")
    print(f"  text 1: {[round(x, 7) for x in a[:6]]}")
    print(f"  text 2: {[round(x, 7) for x in b[:6]]}")
    print(f"  identical={a == b} cosine={cosine(a, b):.6f}")

clone, model = sys.argv[1], sys.argv[2]
cfg = Path(clone, "config.cpu.yaml").read_text(encoding="utf-8")
nb = int(re.search(r"e5-large-instruct.*?n_batch:\s*(\d+)", cfg, re.S).group(1))
print(f"python {sys.version.split()[0]}, llama-cpp-python {version('llama_cpp_python')}, config n_batch={nb}")
run(model, nb)
run(model, 512)
```

</details>

**Reproduction Evidence**

- **Commit showing reproduction:** none (no code change yet)
- **Screenshots/logs:** output of the script, run 2026-10-25:

```
python 3.13.0, llama-cpp-python 0.3.36, config n_batch=16
n_batch=16: tokens=63 dims=1024
  text 1: [0.6221009, 0.533022, -0.590059, -1.2744323, 0.2300023, -0.5570188]
  text 2: [0.6221009, 0.533022, -0.590059, -1.2744323, 0.2300023, -0.5570188]
  identical=True cosine=1.000000
n_batch=512: tokens=63 dims=1024
  text 1: [0.4018736, 0.5523281, -1.0396839, -0.8474972, -0.1033665, -0.5725923]
  text 2: [0.5335093, 0.3626443, -1.1045098, -0.9617444, 0.1893189, -0.4989873]
  identical=False cosine=0.972992
```

- **My findings:** Reproduced. With the default config, two 63-token texts that share only their first sentence get exactly the same vector (cosine 1.000000), which is consistent with each chunk being cut to its first 16 tokens (llama-cpp-python's `embed()` defaults to `truncate=True`, as the issue links). Raising `n_batch` to 512 makes them differ (cosine 0.973). My values are not byte-identical to the issue's (0.6221 vs 0.6259 in the first value), likely from a different llama.cpp build or CPU, but the identical-pair result matches. llama-cpp also warned `n_ctx_seq (8192) > n_ctx_train (512)`: the model was trained on 512-token inputs, which matters for choosing a fixed default. I did not test inside a running Nextcloud instance or check the pgvector rows. The setting is `n_batch` under `embedding.llama`, not the `embedding.batch_size` key, which is commented out and only applies to a remote embedding service.

**Toolkit**

- Tool: repro-check with the wild scope, grading my claim and repro report together, plus the voice guard on the claim.
- What it produced: two rejects and then an accept. All six checks passed on the third run (environment, followable steps, expected vs actual, honest outcome, conventions, claim quality).
- Difficulties: the first run failed steps-followable because my report said "run repro_359.py" without including it; the second graded a half-fixed file with an empty script block (I had run the fix from the wrong folder) and caught that the model URL was missing; it also flagged "everything after 16 tokens is ignored" as stating a cause I had not observed. Starting it as `claude --add-dir <dir> "<prompt>"` silently swallowed the prompt as a folder.
- What changed: I inlined the script, pinned the model URL, reworded the finding to "consistent with truncation", and now put the prompt before `--add-dir`.

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
