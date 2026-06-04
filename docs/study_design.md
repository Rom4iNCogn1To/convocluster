# Study Design

How the experiment is set up. This document captures the operationalization of the constructs and the evaluation strategy.

---

## Construct operationalization

The project's central construct is *"the user's desired clustering"*. This is fundamentally ill-defined — same data admits multiple valid clusterings. We operationalize it as a **hidden target labeling** that the simulated user holds and that the system does not see.

### Three hidden targets, of varying conflict with embedding defaults

| Target | Categories | Source | Labeling cost |
|--------|------------|--------|---------------|
| **Topic** (easy) | 6 categories: galactic-extragalactic, solar-stellar, cosmology, planetary, high-energy, instrumentation | arXiv primary category tags | Free (already in metadata) |
| **Object scale** (hard) | 4 categories: planetary / stellar / galactic / cosmological | Hand-labeled or LLM-labeled with audit | ~2 hours hand-label OR LLM-label + 30-sample audit |
| **Methodology** (hardest) | 4 categories: observational / theoretical / simulation / instrumental | Hand-labeled or LLM-labeled with audit | ~3 hours hand-label OR LLM-label + 30-sample audit |

**Justification for choosing these three targets:** Week 1 eyeballing of k-means clusters on sentence-transformer embeddings revealed all three axes are encoded in the representation. We can verify this:

- K-means with K=5 achieves ARI=0.32 vs arXiv categories — substantial but not perfect topical recovery
- The same K-means clustering visually contains a "methodology" cluster (Cluster 0 in Week 1 inspection: methodology-themed papers across multiple subfields)
- This empirically confirms multiple-axis encoding

### Validity of the hidden targets

- **Topic** is unimpeachable: real human-curated labels from paper authors.
- **Object scale and methodology** are *operationalizations*, not gold standards. The same paper could legitimately be classified into multiple methodology buckets (e.g., a paper presenting observations + a simulation comparison). Validity depends on documented labeling rules.

**Decision on labeling source:** For the main experiment, methodology and object_scale targets will be LLM-labeled with a 30-sample human audit. If audit agreement >85%, LLM labels are adopted. If <85%, full hand-labeling is required. The audit threshold is documented before launching the experiment.

---

## Conditions

### Three systems compared

| System | What the simulated user delivers | Turns | Information budget |
|--------|----------------------------------|-------|---------------------|
| **A. One-shot generic** | "Cluster these into K groups" | 1 | Minimal |
| **B. One-shot detailed** | Full axis description upfront | 1 | High (matches multi-turn aggregate) |
| **C. Multi-turn (this project's system)** | Brief initial instruction + reactive feedback per turn | 6 | High (matches B) |

**Why three systems, not two.** Without **B**, the comparison conflates "feedback helps" with "more information helps." **B** controls for information content delivered upfront; **C** delivers the same total information but reactively. The contribution measured is the *structure of interaction*, not the *amount of information*.

### Cross-product of conditions

3 systems × 3 targets = 9 cells. Each cell run ~15 times for statistics.

---

## Anti-cheating measures

The simulated user **must not reveal the target directly to the system**, or the whole experiment becomes meaningless.

| Measure | Implementation |
|---------|----------------|
| User sees axis description only | Not the per-paper mapping |
| User cannot see paper IDs | Clustering view shows labels + sample titles, no IDs |
| User cannot reference paper IDs in feedback | Explicit prompt constraint; programmatic check after each turn |
| System never sees the target | One-way information barrier; verified by design |

Verification of these measures was performed in Week 3 dry-run. No paper IDs detected in any of 4 sample turns. Methodology keywords appeared in every turn when target was methodology; scale keywords appeared in every turn when target was object_scale.

---

## Evaluation metrics

### Primary

**ARI vs hidden target**, computed after each turn. Produces a *trajectory* per run rather than a single value.

### Secondary

- **Turns-to-threshold**: number of turns required for ARI to reach a fixed threshold (e.g., 0.30, 0.50)
- **Final ARI**: ARI at the end of the 6-turn budget
- **ARI variance across runs**: stability metric (RQ4)
- **Operation mix**: how many `split` / `merge` / `change_k` per condition (descriptive, not a primary claim)

---

## Data provenance & ethics

- **Source**: arXiv.org via official API. Public, open-access papers.
- **No PII**: abstracts are scientific text. No personal data.
- **No copyrighted text reproduction**: full abstracts are stored locally for analysis but not reproduced beyond fair use (short excerpts in the report at most).
- **No deception of human subjects**: the "user" is a simulated LLM, not a human participant.
- **Compute cost**: ~$3 total API spend across all phases. Local response cache prevents redundant calls.

---

## Limitations of the design

Acknowledged upfront, not hidden:

- **Single corpus** (astro-ph). Generalization to other domains unverified.
- **Single embedding model** (`all-MiniLM-L6-v2`). Different embeddings would have different defaults; the bias-override question is partly model-dependent.
- **Single LLM family** (Claude Sonnet 4.6) for both the router and the simulated user. A different LLM as the simulated user could behave differently.
- **Simulated user is not a real user.** Final phase plans a small human spot-check on ~5 runs to verify the simulated user produces comparable feedback to a human astronomer.
- **Geometric split limitation.** The `split` operation partitions by k-means in embedding space; it cannot enforce a semantic criterion. Identified in Week 4 demo as the next bottleneck to address.
