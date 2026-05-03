# browser-use

**URL:** https://github.com/browser-use/browser-use
**Stars:** 91,739 | **Forks:** 10,437 | **License:** MIT License
**Language:** Python
**Created:** 2024-10-31 | **Pushed:** 2026-05-03

## Summary

🌐 Make websites accessible for AI agents. Automate tasks online with ease.

browser-use is a Python framework that enables AI agents to interact with websites through a generic browser automation layer. It provides a unified interface for AI models to navigate, click, type, read, and interact with web pages — making web-based workflows accessible to autonomous agents.

## Architecture

- Generic browser automation layer for any AI model
- Supports major LLMs (OpenAI, Anthropic, local models)
- DOM parsing and element identification
- Multi-step task execution with memory
- Screenshot + structured output approach

## Key Features

- **Universal Web Access:** AI agents can browse any website without site-specific adapters
- **Multi-Model Support:** Works with OpenAI, Anthropic, local Ollama models, etc.
- **Task Memory:** Maintains context across multi-step web workflows
- **Human-in-the-loop:** Optional confirmation for sensitive actions
- **Playwright/Selenium backend:** Reliable browser automation

## Relevance to Agent Harness

- Critical infrastructure for real-world agent tasks (research, shopping, data collection)
- Demonstrates tool abstraction pattern: browser as a tool with structured outputs
- Active development with 91K stars — widely adopted in agent community
- Connected to the agent harness ecosystem via browser-based task automation