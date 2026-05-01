# Daily Crawl — 2026-05-01

## Summary

| Category | Count | Notable Finds |
|----------|-------|--------------|
| Tools/Repos | 7 | Compose Performance Skills, Swiss Design Skill, ThreatSwarm, Paragents, lazar, Bug-Bounty-Agents, OpenCode Orchestration Kit |
| Papers | 3 | Claw-Eval-Live (Live Agent Benchmark), Crab (Sandbox Checkpoint/Restore), Synthetic Computers at Scale |
| Blog | 0 | — |
| **Total** | **10** | |

## Tools / Repositories (7 new)

### Agent Skills & Frameworks
- [compose-performance-skills](tools/repositories/2026-05-01-compose-performance-skills.md) — Curated library of Agent Skills for Jetpack Compose performance (stability, recomposition, lazy layouts, baseline profiles). Follows the open `SKILL.md` standard from agentskills.io. 226 stars, Apache-2.0.
- [swiss-design-skill](tools/repositories/2026-05-01-swiss-design-skill.md) — Swiss International Style design system skill for AI agents: IBM Plex Sans typography, stone color palette, 12-column grid, Tailwind CSS. 86 stars, MIT.

### Agent Infrastructure & Runtime
- [opencode-agent-orchestration-kit](tools/repositories/2026-05-01-opencode-agent-orchestration-kit.md) — Starter kit for OpenCode agent orchestration with product-development workflows, Open Design integration, Superpowers, Docker. 21 stars, Apache-2.0.
- [paragents](tools/repositories/2026-05-01-paragents.md) — Parallel AI agent sessions in one panel with permission-aware tools, preflight conflict checks, TUI-first workflow. Python, asyncio-based. 25 stars.

### Security / Red Team
- [ThreatSwarm](tools/repositories/2026-05-01-threatswarm.md) — 27 scope-enforced AI agents running the full pentest kill-chain (recon → exploit → post-ex → DFIR → report) as a one-command Claude Code plugin. 754 MITRE-mapped skills. 19 stars, MIT.
- [Bug-Bounty-Agents](tools/repositories/2026-05-01-bug-bounty-agents.md) — AI-Powered Agents for Bug-Bounty Pentesting and Red-Teaming. 37 stars, MIT.

### Research / Experimental
- [lazar](tools/repositories/2026-05-01-lazar.md) — The smallest self-evolving agent harness in Rust. One tool: `execute(command)`. One protocol: skills as filesystem entries. Everything else is emergent. 15 stars, MIT.

## Papers (3 new)

### Agent Evaluation & Benchmarking
- [Claw-Eval-Live: A Live Agent Benchmark for Evolving Real-World Workflows](papers/2026-05-01-claw-eval-live.md) — Live agent benchmark for evolving real-world workflows. Addresses the gap between static agent benchmarks and dynamic production environments. arXiv:2604.28139

### Agent Infrastructure
- [Crab: A Semantics-Aware Checkpoint/Restore Runtime for Agent Sandboxes](papers/2026-05-01-crab.md) — Checkpoint and restore runtime for agent sandboxes (containers/microVMs) with semantics awareness. arXiv:2604.28138

### Synthetic Data & Simulation
- [Synthetic Computers at Scale for Long-Horizon Productivity Simulation](papers/2026-05-01-synthetic-computers.md) — Scalable methodology for creating realistic computer environments (folder hierarchies, content-rich artifacts) to generate synthetic data for long-horizon agent productivity simulations. arXiv:2604.28181

## Known Updated Repos (already in DB, noted for completeness)
- everything-claude-code: 171,089 stars (last crawled 2026-04-29)
- superpowers: 174,845 stars (last crawled 2026-04-29)

## Notes
- GitHub API rate limit allowed discovery of 7 new repos created 2026-04-29~04-30
- ArXiv API working: 3 agentically-relevant papers from 2026-04-30
- "everything-claude-code" repo is directly relevant — it's a major agent harness performance optimization system
- ThreatSwarm and Bug-Bounty-Agents both target pentesting use cases (different approaches: full kill-chain vs. focused agents)
- lazar's minimalistic "skills as filesystem" approach is architecturally interesting for skill-based agent design
- Swiss Design Skill shows growing ecosystem of design-system skills for AI agents
- Compose Performance Skills demonstrates the Skills standard expanding beyond Android general dev into specific framework domains