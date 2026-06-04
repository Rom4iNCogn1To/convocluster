# Related Work

Survey of prior work on LLM-guided clustering, interactive clustering, and clustering evaluation without ground truth. Versioned as the project's understanding of the literature evolves.

---

## Changelog

| Version | Date | What changed | Why |
|---------|------|------|------|
| v0.1 | Sprint 1 | Initial anchor set: ClusterLLM, Dial-In LLM | Course brief mentioned LLM-guided clustering as the methodological neighborhood |
| v0.2 | Sprint 2 | Added ITGC, InBedder, Viswanathan et al., TnT-LLM | Targeted search after framing shifted to "interaction structure × bias-override" |
| v0.3 | Sprint 3 | Added classical interactive-clustering thread (constrained clustering, Scholastic). Added evaluation-without-ground-truth note. | Realized comparison must include pre-LLM interactive clustering methods. Simulated-user methodology needed grounding. |

---

## Three threads

This project sits at the intersection of three research areas.

### Thread 1 — LLM-augmented clustering

The dominant recent thread. Several systems use LLMs at different stages of a clustering pipeline:

- **ClusterLLM** (Zhang et al., EMNLP 2023). User-specified "perspective" + LLM-judged triplet/pairwise tasks refine cluster boundaries. The closest prior art to this project. Differs from our setup in that it uses *structured* triplet supervision, not free-form natural-language feedback, and evaluation is on benchmarks where the target aligns with embedding defaults.

- **ITGC — Iterative Text-Guided Clustering** (2025). Iterative refinement via natural-language instructions on *images*. Methodologically the closest cousin to this project's multi-turn loop, but different modality and the iteration is system-driven internal refinement toward a single upfront instruction, not user-driven across turns.

- **InBedder** (Peng et al., 2024). Instruction-following embeddings: changes what gets encoded based on user instructions, producing personalized clusterings. Analogous to this project's `re-embed` operation (stretch goal). Single-instruction, not multi-turn.

- **Dial-In LLM** (Liu et al., 2024). LLM-in-the-loop for dialogue intent clustering with iterative cluster-level refinement. Domain-specific (intent), not free-form text.

- **TnT-LLM** (Microsoft, 2024). LLM generates a topic taxonomy, then trains a downstream classifier. Scale-focused.

- **HERCULES** (2025). Hierarchical LLM-augmented clustering with interactive interpretation.

- **k-LLMmeans**. Replaces classical k-means centroids with LLM-generated cluster summaries.

- **Viswanathan et al. (2024).** Few-shot LLM-guided clustering at three pipeline stages — features, constraints, post-correction.

**Consensus across this thread:** LLM augmentation reliably outperforms purely classical clustering on standard benchmarks. **What's missing:** evaluation on targets that conflict with the embedding's default similarity structure. Most benchmarks have ground-truth labels that *happen to* align with what sentence embeddings naturally cluster on.

### Thread 2 — Classical interactive clustering

Predates LLMs by decades. Relevant because it frames "what kind of user input helps clustering":

- **Constrained clustering** (Wagstaff et al., 2001 and successors). Must-link / cannot-link pairwise constraints from the user. Algorithm-level supervision.

- **Active learning for clustering.** Algorithm selects which pairs to query the user about. Reduces user effort to maximize information per query.

- **Scholastic** (Hong et al., 2022). Machine-in-the-loop sensemaking for qualitative research with hierarchical clusters constrained by user-applied codes. The closest pre-LLM analog to what this project does.

**What this thread teaches us:** the *structure* of user input matters. Must-link constraints are precise but high-cost. This project asks: how does free-form natural language compare?

### Thread 3 — Evaluation without ground truth

Standard clustering benchmarks assume a single ground-truth labeling, but real users often want clusterings that disagree with canonical labels:

- **Internal validity indices** (silhouette, Davies-Bouldin). Measure geometric properties of the clustering, not alignment with user intent. Useful as sanity checks but they don't measure "did the system find what the user wanted."

- **Stability-based methods** (Lange et al., 2004). Measure reproducibility under resampling. Used in this project as RQ4.

- **Simulated user methodology.** Common in dialogue systems for evaluating task-oriented agents. Less common in clustering. This project adopts it: by giving the simulated user a hidden target labeling, "no ground truth" becomes "ground truth by construction."

---

## Where this work sits

System architecture is **not novel** relative to ClusterLLM, ITGC, InBedder, or Dial-In LLM.

The interaction format — free-form natural language translated into a defined operation vocabulary — differs from the structured triplet/constraint feedback of most prior systems but is not the headline contribution.

**The contribution is empirical:** most prior work evaluates on benchmarks where the desired clustering aligns with the embedding's default similarity structure (i.e., canonical topic labels). Our preliminary analysis of arXiv astro-ph abstracts (Week 1) shows that sentence-transformer embeddings encode multiple axes simultaneously — topic, methodology, object scale. A single user-desired clustering can align with one and conflict with the others.

We measure how much interactive refinement helps under both regimes — when the target aligns with embedding defaults (canonical case) and when it actively conflicts with them (the harder case that, to our knowledge, has not been cleanly studied in the LLM-clustering literature).

---

## Gaps we had to fill ourselves

Per the presentation rubric: where prior work didn't give us an answer, what did we have to come up with?

- **Simulated-user prompt design.** The dialogue-systems user-simulation literature doesn't transfer cleanly because clustering feedback is fundamentally different in shape from task-oriented dialogue turns. We designed our own anti-cheating constraints (no paper IDs, axis-description visibility only).

- **Operation vocabulary for cluster-level edits.** Existing systems either work at the algorithm level (k-means hyperparameters) or at the assignment level (move papers). Cluster-level operations (split/merge/change_k) with semantic criteria are a project-specific design.

- **Bias-override evaluation framework.** Most clustering papers report a single ARI per method per benchmark. We report ARI across three targets of varying conflict-with-embedding-defaults — explicitly to expose the bias-override effect.

---

## Citations to verify before final submission

The drafts above use paraphrases of paper claims. For the final report:

- [ ] Verify Zhang et al. EMNLP 2023 exact title and method description for ClusterLLM
- [ ] Find correct citation for ITGC (2025) — exact venue and authors
- [ ] Verify InBedder authors (Peng et al., 2024) — exact title
- [ ] Wagstaff et al. 2001 — must-link/cannot-link canonical paper
- [ ] Search "alternative clustering" and "multi-view clustering" — terms not yet surveyed; may overlap with bias-override framing
