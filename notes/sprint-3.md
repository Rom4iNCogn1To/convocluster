# Sprint 3 — [Your name]

## What I did this sprint

- Designed and implemented the simulated user (`week3_simulated_user.ipynb`)
- Locked five design choices: (A) target visibility = axis description only, (B) user sees labels + sizes + 5 sample titles per cluster, (C) persona = professional astronomer, (D) fixed 6-turn budget, (E) user sees its own past messages
- Validated anti-cheating: programmatic regex check on 4 dry-run turns, no paper IDs found in any
- Validated target alignment: target swap (methodology → object_scale) produces visibly different feedback
- Locked prompt at v0.1 in `data/week3_simulated_user_prompts_v01.json`
- Updated study plan to v0.3: introduced the three-system comparison (generic / detailed / multi-turn)

## Key findings this sprint

The simulated user works as designed. Across 4 turns:
- 0 paper IDs leaked
- Every turn engaged methodology concepts (4/4 turns mentioned observation, theoretical, simulation, instrument)
- Cost: ~$0.022 per 4-turn run → projected ~$15-20 for the full experiment (800+ user calls)

## What blocked me

- The temperature choice (0.4) is a guess. Lower would be more deterministic, higher more variable. I locked it for now; revisit if Week 4+ reveals issues.
- [FILL: anything else]

## What I'm doing next

- Build the conversational system (router + operations)
- Run a demo on the topic target for the presentation
