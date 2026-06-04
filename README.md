# Conversational Clustering

Solo capstone for the KDD course. We measure whether multi-turn natural-language feedback recovers hidden target clusterings better than one-shot prompting — and whether the advantage depends on whether the target conflicts with the embedding's default similarity structure.

**Status:** weeks 1–4 complete (baselines + demo). See `docs/study_plan.md` for the current version of the research questions.

---

## Headline result so far

On arXiv astro-ph abstracts (n=200), Claude one-shot baselines achieve:

| Target | ARI vs reference labels | Notes |
|--------|------|-------|
| Topic (arXiv categories) | **0.361** | Aligned with embedding defaults |
| Object scale | 0.264 | Partial alignment |
| Methodology | **0.071** | Conflicts with embedding defaults |

Spread topic→methodology = **0.29 ARI**: the gap that multi-turn feedback should close.

A demo run on the topic target shows the conversational loop closes end-to-end (v0.2 router: ARI 0.245 → 0.269 across 6 turns). The interesting case — methodology target — is the next experiment.

---

## Reproducing the experiments

### Setup

```bash
git clone <repo-url>
cd conversational-clustering
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

API key for Claude:

```bash
echo "ANTHROPIC_API_KEY=sk-ant-..." > .env
```

A local Ollama install is optional and only needed to reproduce the Week 2 local experiment.

### Run

Notebooks are numbered in order of execution. Run them sequentially:

```
notebooks/week1_pull_and_embed.ipynb          # pull arXiv abstracts, embed, k-means
notebooks/week2_cloud_baselines.ipynb         # Claude one-shot baselines (generic + 3 detailed axes)
notebooks/week2_local_ollama.ipynb            # OPTIONAL: local model validation (showed 3B insufficient)
notebooks/week3_simulated_user.ipynb          # build + validate the simulated user
notebooks/week4_demo.ipynb                    # closes the loop, single demo run on topic target
```

Each notebook caches its outputs to `data/` and `cache_claude/`. Re-running a notebook after the first successful run is free (uses local response cache).

### What gets produced

- `data/astro_ph_abstracts.json` — 200 abstracts pulled from arXiv (cached so we don't re-pull)
- `data/claude_baseline_*.json` — one-shot baseline assignments + labels + cost reports
- `data/week3_simulated_user_dryrun.json` — dry-run conversation
- `data/week4_demo_run.json` — full trajectory of the demo loop (assignments, ops, ARI per turn)
- `data/week4_demo_v01_vs_v02.png` — comparison plot of the two router iterations
- `data/week3_simulated_user_prompts_v01.json` — versioned prompt artifact

### Headline figure

`data/week4_demo_v01_vs_v02.png` is the figure shown in the presentation (slide 8 of the deck). It can be regenerated from `data/week4_demo_run.json` by running the plot cell at the end of `notebooks/week4_demo.ipynb`.

---

## Cost

Total API spend across all four notebooks is under **$3** on Claude Sonnet 4.6, thanks to two-layer caching (prompt cache server-side + response cache local). Local response cache means re-running cells costs $0 after the first successful run.

---

## Documents

- `docs/study_plan.md` — research questions, current version of the hypotheses (versioned)
- `docs/related_work.md` — survey of LLM-guided clustering literature (versioned)
- `docs/study_design.md` — experimental design, operationalization of targets, evaluation strategy
- `docs/report.md` — technical report (8-15 pages, the main writeup)
- `notes/` — per-sprint notes (week-by-week)

---

## Repository layout

```
.
├── README.md                          
├── requirements.txt                   
├── docs/
│   ├── study_plan.md                  
│   ├── related_work.md                
│   ├── study_design.md
│   └── report.md                      
├── notebooks/
│   ├── week1_pull_and_embed.ipynb
│   ├── week2_cloud_baselines.ipynb
│   ├── week2_local_ollama.ipynb
│   ├── week3_simulated_user.ipynb
│   └── week4_demo.ipynb
├── data/
│   ├── astro_ph_abstracts.json
│   ├── claude_baseline_*.json
│   ├── week3_*.json
│   ├── week4_demo_run.json
│   └── week4_demo_v01_vs_v02.png
├── cache_claude/                     
├── cache_ollama/                      
└── notes/
    ├── sprint-1.md
    ├── sprint-2.md
    └── ...
```

---

## Honest limitations (more in `docs/report.md`)

- **Single demo run on the easy target.** No statistics, no confidence intervals. The full experiment (3 targets × 3 systems × ~15 runs) is the obvious next step.
- **The `split` operation is geometric, not semantic.** When the router asks "split by methodology", k-means partitions by embedding geometry — which often doesn't correspond to the requested criterion. This is documented in the report's limitations section.
- **No human-labeled ground truth.** Methodology and object_scale targets use Claude's own detailed-baseline assignments as proxy targets. Spot-checking against hand-labeling is future work.

---

## Contact

Romain NOBLET, Università degli Studi di Trento
