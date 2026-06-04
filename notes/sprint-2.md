# Sprint 2 — Romain NOBLET

## What I did this sprint

- Built the Claude one-shot baseline infrastructure (notebook `week2_cloud_baselines.ipynb`)
- Implemented two-layer caching: prompt cache server-side (90% input cost reduction) + local response cache (free re-runs)
- Ran generic baseline + three detailed baselines (topic, object_scale, methodology)
- Local-model control run (`llama3.2:3b` via Ollama) — confirmed 3B-class models cannot do this task
- Updated study plan to v0.2: reframed around "bias override" rather than vague "does conversation help"
- Updated related work to v0.2: added ITGC, InBedder, Viswanathan et al.

## Key findings this sprint

ARI vs arXiv primary category:
- Generic (no axis): 0.245
- Topic (axis-aligned): **0.361**
- Object scale: 0.264
- Methodology: **0.071**

**Spread topic → methodology = 0.29 ARI.** This is the empirical magnitude of the bias-override effect I'm studying.

Local llama3.2:3b: ARI ≈ 0.03. Justifies the model choice.

## What blocked me

- Initially over-spent on prompt design (qwen2.5:3b, llama3.2:3b followed JSON but didn't produce coherent clusters). Lost ~half a day before realizing the local-model phase was the wrong path.

## What I'm doing next

- Build the simulated user (Week 3)
- Validate it doesn't leak the target through paper IDs or other channels
