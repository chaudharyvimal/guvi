# Embeddings & Semantic Search — session plan

A minute-by-minute teaching plan for a 45-minute online session on **embeddings and semantic
search**, aimed at college students, freshers, and engineers with 1–2 years of experience.

## Three files, two of them audience-facing

| File | Share on screen? | Used |
| --- | --- | --- |
| 📐 [session-plan.html](session-plan.html) — the session plan | **No** — it's the script | throughout |
| 🎞 [slides.html](slides.html) — the deck | Yes | 0:00–0:24, 0:38–0:45 |
| 📓 [embeddings_demo.ipynb](embeddings_demo.ipynb) — the demo | Yes | 0:24–0:38 |

The plan carries stage directions, a cut list, and prepared Q&A, so it stays on a second screen.
Both HTML files are single self-contained pages with no external dependencies — they work offline.
In the deck: arrow keys to move, `t` to flip theme, `f` for fullscreen, swipe on a phone.

> Embeddings turn meaning into geometry. Once meaning is geometry, search becomes arithmetic.

## What's in the plan

| Time | Segment |
| --- | --- |
| 0:00 | Hook — a help search that answers "I can't sign in" with *"How do I delete my account permanently?"* |
| 0:04 | Why synonym lists and WordNet don't scale |
| 0:07 | Meaning as coordinates — built up from RGB, not from jargon |
| 0:15 | Cosine similarity worked by hand on 2-D vectors |
| 0:21 | Why averaging word vectors fails (*dog bites man* = *man bites dog*) |
| 0:24 | Four live demo runs, ending on a deliberate failure case |
| 0:38 | How this is the retrieval engine inside RAG |
| 0:41 | Takeaway assignment and Q&A |

It also carries the words to say for each segment, a pre-flight checklist, seven prepared Q&A
answers, and a ranked cut-list for when the theory runs long.

## Running the demos

CPU only — no GPU anywhere in this notebook.

```bash
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install torch --index-url https://download.pytorch.org/whl/cpu
.\.venv\Scripts\python.exe -m pip install sentence-transformers pandas
```

The CPU-only torch index pulls ~122 MB instead of ~2.5 GB for the CUDA build.

Then cache the model once, before presenting, so nothing downloads live:

```python
from sentence_transformers import SentenceTransformer
SentenceTransformer("all-MiniLM-L6-v2")   # ~80 MB
```

## Verified results

Every score in the plan and notebook was executed on `all-MiniLM-L6-v2` (384 dims), Python
3.12.10, torch 2.13.0+cpu, Windows 11. Two findings changed the session:

**Semantic search beats keyword search, and keyword fails in the most useful way.** For the query
*"I can't log in to my account"*, keyword matching hits only on `account` and returns
*"How do I delete my account permanently?"* — the user asked for help getting in, and it offered to
destroy their account. Semantic search ranks password-reset first at `0.502`, without the word
"password" ever appearing in the query.

**But retrieval is not solved, and the plan now shows it.** Rephrase to *"I'm locked out of my
account"* and the wrong article wins outright:

```
0.636   How do I delete my account permanently?
0.539   How do I reset a forgotten password?
```

That measured failure is in the session as Run 3b, as the motivation for rerankers.

**Embeddings miss negation.** Two opposites score `0.800` — a hair under a genuinely similar pair
at `0.829`:

```
0.800   'The flight was on time'  vs  'The flight was delayed'
0.829   'I love this product'     vs  'This product is wonderful'
```

Scores from this model are deterministic, so re-running reproduces them exactly. A different model
or a `sentence-transformers` major-version bump will shift them.
