# What Makes a Good Terminal-Agent Benchmark Task

**arXiv ID:** 2604.28093
**Published:** 2026-04-30
**Authors:** Ivan Bercovich
**Categories:** cs.AI

## Summary

Terminal-agent benchmarks have become a primary signal for measuring the coding and system-administration capabilities of large language models. As the market for evaluation environments grows, so does the pressure to ship tasks quickly, often without thorough adversarial review of the verification logic.

This paper provides guidelines for designing terminal-agent benchmark tasks that are adversarial (resistant to gaming), difficult (meaningful), and legible (transparent and auditable).

## Key Contributions

1. **Taxonomy of Benchmark Failure Modes:** Categorizes common ways agents exploit benchmark flaws — shortcut solutions, data leakage, verification bypass
2. **Design Principles:** Seven guidelines for creating robust benchmark tasks:
   - Adversarial review of verification logic
   - Diversity of task types
   - Clear scoring criteria
   - Separation of task难度 fromirrelevancies
   - Reproducibility requirements
   - Anti-gaming measures
   - Human-legible task descriptions
3. **Evaluation Framework:** A structured approach for assessing whether a benchmark genuinely tests agent capabilities

## Relevance to Agent Harness

- Directly addresses the challenge of evaluating agent systems fairly
- Guidelines help prevent agents from "faking" capability through benchmark gaming
- Critical reading for anyone building agent evaluation infrastructure
- Can be applied to improve any agent harness evaluation methodology