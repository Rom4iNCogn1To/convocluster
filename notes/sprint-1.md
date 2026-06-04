# Sprint 1 — [Your name]

## What I did this sprint

- Defined project scope: conversational clustering, solo, astro-ph corpus
- Pulled 200 astro-ph abstracts via arXiv API
- Embedded with `all-MiniLM-L6-v2`, ran k-means at K=3..10
- Manually inspected clusters at K=5 — identified that embeddings encode multiple axes (topic, methodology, object scale), not just topic
- Initial study plan v0.1 committed (`docs/study_plan.md`)
- Initial related-work scan v0.1 committed (`docs/related_work.md`)

## Key finding this sprint

ARI of k-means clustering vs arXiv primary categories landed at 0.32. Not 0, not 1 — the "goldilocks" regime where the task has signal but is not trivially solved. This empirically validates the dataset choice and motivates the rest of the project.

## What blocked me

- [FILL: anything that actually blocked you. Examples: API rate limits, embedding model download time, deciding between datasets, ...]

## What I'm doing next

- LLM baselines on Claude
- Compare against local model (Ollama) as a control
- Detailed-axis baselines for topic / object_scale / methodology
