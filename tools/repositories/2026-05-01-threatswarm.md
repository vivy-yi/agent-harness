# ThreatSwarm

**URL:** https://github.com/mukul975/ThreatSwarm
**Stars:** 19 | **Forks:** 1 | **License:** MIT License
**Language:** Python
**Created:** 2026-04-29 | **Pushed:** 2026-04-29
**Homepage:** https://github.com/mukul975/Anthropic-Cybersecurity-Skills
**Topics:** ai-agents, ai-security, anthropic, autonomous-agents, blue-team, bug-bounty, claude-code, claude-code-plugin, cybersecurity, dfir

## Summary

27 scope-enforced AI agents that run the full pentest kill-chain (recon → exploit → post-ex → DFIR → report) as a one-command Claude Code plugin. Backed by 754 MITRE-mapped skills.

## Key Features

- **Full Kill Chain:** Recon, exploitation, post-exploitation, DFIR, and CVSS-scored report in a single session
- **Scope Enforcement:** Every network command is scope-gated by `scope_check.py` — violations blocked at OS level
- **Claude Code Plugin:** One-command install (`claude --plugin-dir ./threatswarm-plugin`), no Docker/Postgres/cloud required
- **MITRE ATT&CK Mapped:** Methodology loaded from Anthropic-Cybersecurity-Skills library
- **754 MITRE-Mapped Skills:** Covers ATT&CK, CSF 2.0, ATLAS, D3FEND, and AI RMF

## Agent Types (27 agents)

Each agent handles a specific phase of the penetration testing kill chain, with scope enforcement on every tool invocation.

## Relevance to Agent Harness

- Demonstrates multi-agent orchestration for specialized security workflows
- Scope enforcement is a critical safety pattern for autonomous agents
- Maps to MITRE frameworks — shows how agent skills can be taxonomically organized
- One-command Claude Code plugin shows agent extensibility model