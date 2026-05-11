# convocluster

## State Of The Art
CLUSTERLLM: Large Language Models as a Guide for Text Clustering (November 2023)
InBedder — "Answer is All You Need: Instruction-following Text Embedding" (Feb 2024)
ITGC — Interpretable Text-Guided Image Clustering via Iterative Search (June 2025)

# Description
Recent work on LLM-guided clustering (ClusterLLM, ITGC, InBedder) shows that natural-language guidance can steer clustering toward user-specified criteria. However, evaluation has primarily focused on cases where the target clustering aligns with the embedding's default similarity structure. We study the regime where the user's target conflicts with embedding defaults, using astro-ph abstracts where sentence-transformer embeddings encode multiple axes simultaneously (topic, methodology, object scale). We measure how interactive feedback turns translate into ARI improvement against hidden targets at varying degrees of conflict with embedding defaults.

## dataset
https://arxiv.org/archive/astro-ph
