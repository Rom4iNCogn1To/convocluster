# Sprint 4 — [Your name]

## What I did this sprint

- Built the router + operation infrastructure (`week4_demo.ipynb`)
- Implemented three operations: `change_k`, `merge`, `split`
- Ran the full conversational loop end-to-end on the topic target
- Diagnosed and fixed a router design failure across two iterations:
  - **v0.1** router: no K guardrails, no history visibility. ARI 0.245 → 0.199 (over-fragmentation, K exploded from 5 to 22)
  - **v0.2** router: K-discipline in prompt, past-decisions visible to router, ops capped at 2/turn. ARI 0.245 → 0.269
- Generated comparison figure (`data/week4_demo_v01_vs_v02.png`)
- Updated study plan to v0.4: identified semantic split as the next bottleneck
- Updated the proposal deck with two new slides (preliminary results + demo)

## Key findings this sprint

- The loop closes end-to-end (mechanism validated)
- v0.1 → v0.2 documented a real coordination failure between two LLM components
- The next architectural bottleneck is `split`: it partitions by k-means geometry, not by the semantic criterion. When the router asks "split by methodology", the partition is geometric and often doesn't match the requested criterion.

## What blocked me

- The v0.1 → v0.2 iteration cost time that should have gone to the multi-target experiment. But the failure was diagnostic — it documented a real design tension between two LLM agents in a multi-turn loop. Worth the time.
- [FILL: anything else]

## What I'm doing next

- Final report and presentation
- Future work (not in scope for this submission): semantic split via LLM-driven partitioning, multi-target experiment with statistics, validation against real human users
