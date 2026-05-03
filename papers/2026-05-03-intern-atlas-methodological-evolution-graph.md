# Intern-Atlas: A Methodological Evolution Graph as Research Infrastructure for AI Scientists

**arXiv ID:** 2604.28158
**Published:** 2026-04-30
**Authors:** Yujun Wu, Dongxu Zhang, Xinchen Li (+ 11 more)
**Categories:** cs.AI

## Summary

Existing research infrastructure is fundamentally document-centric — providing citation links between papers but lacking explicit representations of methodological evolution. It does not capture the structured relationships explaining how and why research methods emerge, adapt, and build upon one another.

With the rise of AI-driven research agents as consumers of scientific knowledge, this limitation becomes consequential: agents cannot reliably reconstruct method evolution topologies from unstructured text.

Intern-Atlas addresses this by building a methodological evolution graph from 1,030,314 papers, comprising 9,410,201 semantically typed edges grounded in source evidence.

## Key Contributions

1. **Methodological Evolution Graph:** Causal network of how research methods evolved over time
2. **Large-Scale Construction:** Built from 1M+ papers across AI venues, producing 9.4M method edges
3. **Self-Guided Temporal Tree Search:** Algorithm for constructing evolution chains tracing method progression
4. **AI Scientist Applications:** Enables idea evaluation and automated idea generation for AI research agents
5. **Grounded Evidence:** Every edge is grounded in verbatim source text

## Architecture

```
Papers (1M+) → Method Entity Extraction → Lineage Relationship Inference → Evolution Graph
                                                                          ↓
                                          Self-Guided Temporal Tree Search → Evolution Chains
                                                                          ↓
                                      Idea Evaluation + Automated Idea Generation
```

## Relevance to Agent Harness

- Directly relevant to AI research agents that need to understand the evolution of techniques
- The graph-based approach is a model for organizing agent knowledge about methodology
- Could serve as a knowledge base for agent harness systems researching agent techniques
- Provides a framework for AI agents to reason about the provenance of methods in their domain