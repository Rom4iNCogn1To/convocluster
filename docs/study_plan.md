# Study Plan

The versioned record of what this project is trying to measure and how that has evolved.

---

## Changelog

| Version | Date | What changed | Why |
|---------|------|------|------|
| v0.1 | Sprint 1 | Initial RQs framed as "does conversation help cluster?" | Course brief, before any data exploration |
| v0.2 | Sprint 2 | Reframed around **bias override**: does multi-turn help recover non-default targets? | Week 1 eyeballing of astro-ph k-means clusters surfaced multiple axes in embeddings (topic / methodology / object scale). Initial "does conversation help" was too vague to be a hypothesis. |
| v0.3 | Sprint 3 | Added 3-baseline comparison (one-shot generic / one-shot detailed / multi-turn). Dropped "easy baseline" as the only comparison. | Week 2 cloud results showed one-shot detailed already reaches ARI=0.36 on topic. Comparison vs *generic* one-shot would conflate "feedback helps" with "more information helps". Need to control for information budget. |
| v0.4 | Sprint 4 (current) | Confirmed: methodology is the hard target. Demo run validated the loop closes on topic; semantic split identified as the next bottleneck. | Empirical findings: spread topic→methodology = 0.29 ARI is the gap to study. Demo trajectory v0.1→v0.2 documents router design choices. |

---

## Current research questions (v0.4)

### Headline question

*Does multi-turn natural-language feedback recover hidden target clusterings more efficiently than one-shot prompting — and does this advantage depend on whether the target aligns with or conflicts with the embedding's default similarity structure?*

### Decomposed sub-questions

- **RQ1 (efficiency):** Does the multi-turn system reach a target ARI threshold in fewer turns than iterated one-shot prompting?
- **RQ2 (ceiling):** Does the multi-turn system reach a higher final ARI than the one-shot baselines?
- **RQ3 (bias override — the interesting one):** How does the conversation-vs-baseline gap change as targets shift from aligned with embedding defaults (topic) to conflicting (methodology, object scale)?
- **RQ4 (stability):** Is the converged clustering stable across runs and across resampling of the corpus?

### Methodological sub-questions

- Can a simulated LLM user produce feedback similar enough to a real human user that experimental conclusions transfer? (To be validated by a small human spot-check at the end.)
- Which operations does the LLM router invoke given which kinds of feedback? (Descriptive analysis from logs — useful for the writeup, not a primary claim.)

---

## Status of each question

| RQ | Status | Evidence so far |
|----|--------|------|
| RQ1 | Open (Week 5+) | Demo on topic showed ARI rises with feedback (0.245 → 0.269). Single run, no statistic. |
| RQ2 | Open (Week 5+) | Multi-turn (0.269 demo) didn't beat one-shot detailed (0.361) on topic. As expected for easy target. |
| RQ3 | Open (Week 5+) | The *gap* (0.29 ARI between topic and methodology one-shot baselines) is the empirical hook. Conversation evaluation on methodology is the key experiment, not run yet. |
| RQ4 | Open (Week 5+) | Not measured yet. |

---

## Scope decisions

- **Solo project.** Tight scope: one corpus (astro-ph), one model class (Sonnet 4.6), three targets, three systems.
- **Dataset:** ~200 arXiv astro-ph abstracts, recent. Public API.
- **Targets, in increasing difficulty:**
  - **Easy:** topic (arXiv primary category labels — free)
  - **Hard:** object scale (planetary / stellar / galactic / cosmological)
  - **Hardest:** methodology (observational / theoretical / simulation / instrumental)
- **Systems:**
  - **A.** One-shot generic (no axis specified)
  - **B.** One-shot detailed (axis described upfront)
  - **C.** Multi-turn (this project's system)
- **Evaluation metric:** ARI vs hidden target.
---

## Honest framing of contribution

The system architecture is **not novel** relative to ClusterLLM, ITGC, InBedder. The contribution is **empirical**: a measurement of how interaction structure (single-shot vs multi-turn) affects clustering quality when the target conflicts with embedding defaults.
