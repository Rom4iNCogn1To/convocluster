# Related Work

Survey of prior work on LLM-guided clustering, interactive clustering, and clustering evaluation without ground truth. Versioned as the project's understanding of the literature evolves.

---

## Changelog

| Version | Date | What changed | Why |
|---------|------|------|------|
| v0.1 | Sprint 1 | Initial anchor set: ClusterLLM, Dial-In LLM | Course brief mentioned LLM-guided clustering as the methodological neighborhood |
| v0.2 | Sprint 2 | Added ITGC, InBedder, Viswanathan et al., TnT-LLM | Targeted search after framing shifted to "interaction structure × bias-override" |

---

Several systems use LLMs at different stages of a clustering pipeline:

- **ClusterLLM** (Zhang et al., EMNLP 2023). User-specified "perspective" + LLM-judged triplet/pairwise tasks refine cluster boundaries. The closest prior art to this project. Differs from our setup in that it uses *structured* triplet supervision, not free-form natural-language feedback, and evaluation is on benchmarks where the target aligns with embedding defaults.

- **ITGC — Iterative Text-Guided Clustering** (2025). Iterative refinement via natural-language instructions on *images*. Methodologically the closest cousin to this project's multi-turn loop, but different modality and the iteration is system-driven internal refinement toward a single upfront instruction, not user-driven across turns.

- **InBedder** (Peng et al., 2024). Instruction-following embeddings: changes what gets encoded based on user instructions, producing personalized clusterings. Analogous to this project's `re-embed` operation (stretch goal). Single-instruction, not multi-turn.

- **Dial-In LLM** (Liu et al., 2024). LLM-in-the-loop for dialogue intent clustering with iterative cluster-level refinement. Domain-specific (intent), not free-form text.


- **Viswanathan et al. (2024).** Few-shot LLM-guided clustering at three pipeline stages — features, constraints, post-correction.

**Consensus** LLM augmentation reliably outperforms purely classical clustering on standard benchmarks. **What's missing:** evaluation on targets that conflict with the embedding's default similarity structure. Most benchmarks have ground-truth labels that *happen to* align with what sentence embeddings naturally cluster on.

---

## Where this work sits

System architecture is **not novel** relative to ClusterLLM, ITGC, InBedder, or Dial-In LLM.

The interaction format — free-form natural language translated into a defined operation vocabulary — differs from the structured triplet/constraint feedback of most prior systems but is not the headline contribution.

**The contribution is empirical:** most prior work evaluates on benchmarks where the desired clustering aligns with the embedding's default similarity structure (i.e., canonical topic labels). Our preliminary analysis of arXiv astro-ph abstracts (Week 1) shows that sentence-transformer embeddings encode multiple axes simultaneously — topic, methodology, object scale. A single user-desired clustering can align with one and conflict with the others.

We measure how much interactive refinement helps under both regimes — when the target aligns with embedding defaults (canonical case) and when it actively conflicts with them (the harder case that, to our knowledge, has not been cleanly studied in the LLM-clustering literature).
