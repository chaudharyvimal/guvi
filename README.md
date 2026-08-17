# Embeddings & Semantic Search — session plan

A minute-by-minute teaching plan for a 45-minute online session on **embeddings and semantic
search**, aimed at college students, freshers, and engineers with 1–2 years of experience.

📐 **[meaning-as-geometry.html](meaning-as-geometry.html)** — open it in any browser. It is a single
self-contained file with no external dependencies, so it works offline.

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
| 0:37 | How this is the retrieval engine inside RAG |
| 0:41 | Takeaway assignment and Q&A |

It also carries the words to say for each segment, a pre-flight checklist, seven prepared Q&A
answers, and a ranked cut-list for when the theory runs long.

## Running the demos

The hands-on portion needs CPU only — no GPU:

```bash
pip install sentence-transformers pandas
```

Then cache the model once, before presenting, so nothing downloads live:

```python
from sentence_transformers import SentenceTransformer
SentenceTransformer("all-MiniLM-L6-v2")   # ~80 MB
```

The four demo runs are listed in full inside the plan.

> [!NOTE]
> The demo code in the plan has not been executed yet, so the exact similarity rankings in Run 3
> are the intended result rather than a measured one. Run it once before presenting and tune the
> query or corpus until the keyword-vs-semantic contrast is unmistakable.
