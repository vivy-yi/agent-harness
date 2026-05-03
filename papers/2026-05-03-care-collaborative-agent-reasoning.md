# Collaborative Agent Reasoning Engineering (CARE)

**arXiv ID:** 2604.28043
**Published:** 2026-04-30
**Authors:** Rahul Ramachandran, Nidhi Jha, Muthukumaran Ramasubramanian
**Categories:** cs.AI

## Summary

We present Collaborative Agent Reasoning Engineering (CARE), a disciplined methodology for engineering Large Language Model (LLM) agents in scientific domains. Unlike ad-hoc trial-and-error approaches, CARE specifies behavior, grounding, tool orchestration, and verification through reusable artifacts.

CARE introduces a three-party design involving:
1. **Subject Matter Experts (SMEs):** Domain specialists who define correct behavior
2. **Developers:** Engineers who implement the agent infrastructure
3. **Helper Agents:** AI systems that assist in the development process

## Key Contributions

1. **Three-Party Methodology:** Structured collaboration between SMEs, developers, and AI agents
2. **Reusable Artifacts:** Formal specifications for agent behavior, grounding rules, tool definitions, and verification criteria
3. **Scientific Domain Focus:** Designed for domains requiring high accuracy (medicine, science, engineering)
4. **Iterative Refinement:** Systematic improvement cycle driven by verification results

## Architecture

```
SME Knowledge → Behavior Specification → Developer Implementation → Helper Agent Testing → Verification → Iteration
```

## Relevance to Agent Harness

- Formalizes the agent development process beyond "prompt engineering"
- CARE's artifact-based approach maps directly to skill definitions in agent harness systems
- The three-party model (SME + Developer + Agent) is a practical pattern for building reliable agents
- Verification-centric design aligns with agent harness principles of engineering over prompting