# Paragents

**URL:** https://github.com/FrankHui/paragents
**Stars:** 25 | **Forks:** 0 | **License:** None
**Language:** Python
**Created:** 2026-04-29 | **Pushed:** 2026-04-30
**Topics:** agent-runtime, ai-agent, asyncio, llm, multi-agent, parallel-agents, python, tui

## Summary

Parallel AI agent sessions in one panel, with permission-aware tools, preflight conflict checks. TUI-first workflow, extensible tools, and explicit policy gates.

## Key Features

- **Parallel Sessions:** Multiple agent sessions running simultaneously in one panel
- **Permission-Aware Tools:** Agents ask before risky actions
- **Preflight Conflict Checks:** Detects conflicts before execution
- **Context Memory:** Remembers context across turns
- **TUI-First:** Terminal user interface design
- **Async/Await:** Python asyncio-based architecture

## Architecture Inspiration

Inspired by:
- Anthropic Claude Code
- mercury-agent (cosmicstack-labs)
- hermes-agent (NousResearch)
- nanobot (HKUDS)

## Relevance to Agent Harness

- Multi-agent parallel execution pattern — directly relevant to agent harness design
- Permission/gate mechanism is a key safety pattern for agent runtimes
- TUI approach for multi-agent coordination
- Conflict detection shows emerging patterns for multi-agent safety