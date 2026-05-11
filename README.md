# convocluster

Recent work on LLM-guided clustering (ClusterLLM, ITGC, InBedder) shows that natural-language guidance can steer clustering toward user-specified criteria. However, evaluation has primarily focused on cases where the target clustering aligns with the embedding's default similarity structure. We study the regime where the user's target conflicts with embedding defaults, using astro-ph abstracts where sentence-transformer embeddings encode multiple axes simultaneously (topic, methodology, object scale). We measure how interactive feedback turns translate into ARI improvement against hidden targets at varying degrees of conflict with embedding defaults.

**Does conversational refinement using a structured operation vocabulary converge to a hidden target clustering more efficiently than one-shot LLM clustering — and does this advantage depend on whether the target aligns with or conflicts with the embedding's default similarity structure?**

*Sub-questions*
- Q1 (efficiency): Does the conversational system reach a given ARI threshold in fewer turns than iterated one-shot prompting?
- Q2 (ceiling): Does the conversational system reach a higher final ARI than the one-shot baseline?
- Q3 (the interesting one — bias overriding): How does the conversation-vs-baseline gap change as the target shifts from aligned-with-embedding (topic) to conflicting-with-embedding (methodology, object scale)?
- Q4 (stability): Is the converged clustering stable across runs and across resampling of the corpus?

## Other questions
- What does the conversation actually do — what's the mechanism?
LLM-steers-classical-clustering. The conversation maps to operations on a k-means pipeline (split, merge, change_k, re-embed, ignore, move).
- What operations should be in the vocabulary?
Start with three: split, merge, change_k. Add re-embed and ignore if needed. Re-embed is the sharpest tool for the non-default-target experiment.
- How do we evaluate without ground truth?
Simulated user methodology. You (the experimenter) define a hidden target. A simulated LLM user is given that target and produces natural-language feedback toward it. The system never sees the target. ARI is computed between system output and target.
- Where does the "truth" come from?
From you, the experimenter. Three target types planned: topic (arXiv categories, free), object-scale (hand-labeled or LLM-labeled-with-audit), methodology (same). The targets aren't the truth, they're a truth — different user goals.
- What does ARI = 0.32 mean?
Moderate agreement beyond chance. Real signal, real disagreement. Goldilocks zone for the study — not trivial, not impossible.
- What do embeddings actually do?
Map text to points in 384-dim space such that semantic similarity ≈ geometric proximity. The notion of "similarity" baked in is a bias from training data, not a truth. Two abstracts on similar topics end up close; abstracts on different methodologies of similar topics might also end up close because the embedding doesn't natively distinguish methodology.

- Can a simulated LLM user produce feedback similar enough to a real human user that experimental conclusions transfer?
- Which operations does the LLM choose to invoke given which kinds of feedback?
- What does the LLM baseline actually see?
Does it see all 200 abstracts? Titles only? Title + first sentence? Does it work over raw text or over cluster summaries?
- How is the simulated user prompted?
Critical methodological question. The simulated user's prompt determines the validity of your whole experiment. Too generic → bad feedback. Too specific → leaks the answer.
- How many runs per condition for statistical power?
You'll do a small power analysis. Likely 10–20 runs per (system × target) cell.
- How do you detect convergence?
When does the simulated user stop giving feedback? After N turns? When ARI changes < ε? When the user-LLM says it's satisfied?
- What multiple-comparison correction will you apply?
With multiple targets, multiple metrics, and multiple turn cutoffs, you'll be running many tests. Bonferroni? Holm? Benjamini-Hochberg?
- How do you validate the simulated user?
A small human study at the end. Do real humans converge similarly to the simulated user? This is the validity check that the whole methodology depends on.

**Why is "clustering" an ill-defined problem until you specify the user's intent — and what does that imply for how clustering systems should be designed and evaluated?**

## State Of The Art
- CLUSTERLLM: Large Language Models as a Guide for Text Clustering (Nov 2023)
- InBedder — "Answer is All You Need: Instruction-following Text Embedding" (Feb 2024)
- ITGC — Interpretable Text-Guided Image Clustering via Iterative Search (June 2025)

## Dataset
https://arxiv.org/archive/astro-ph
