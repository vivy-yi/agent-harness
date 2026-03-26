# The Anatomy of an Agent Harness

> By Vivek Trivedy | Source: LangChain Blog | Date: March 2026

![Harness Architecture](./langchain-images/anatomy-01.png)

---

**TLDR:** Agent = Model + Harness. Harness engineering is how we build systems around models to turn them into work engines. The model contains the intelligence and the harness makes that intelligence useful.

---

## Can Someone Please Define a "Harness"?

**Agent = Model + Harness**

If you're not the model, you're the harness.

A harness is every piece of code, configuration, and execution logic that isn't the model itself. A raw model is not an agent. But it becomes one when a harness gives it things like state, tool execution, feedback loops, and enforceable constraints.

Concretely, a harness includes things like:
- System Prompts
- Tools, Skills, MCPs + and their descriptions
- Bundled Infrastructure (filesystem, sandbox, browser)
- Orchestration Logic (subagent spawning, handoffs, model routing)
- Hooks/Middleware for deterministic execution (compaction, continuation, lint checks)

---

## Why Do We Need Harnesses…From a Model's Perspective

There are things we want an agent to do that a model cannot do out of the box. This is where a harness comes in.

Models (mostly) take in data like text, images, audio, video and they output text. That's it. Out of the box they cannot:
- Maintain durable state across interactions
- Execute code
- Access realtime knowledge
- Setup environments and install packages to complete work

These are all harness level features. The structure of LLMs requires some sort of machinery that wraps them to do useful work.

---

## Working Backwards from Desired Agent Behavior to Harness Engineering

![Harness Components](./langchain-images/anatomy-02.png)

Harness Engineering helps humans inject useful priors to guide agent behavior. And as models have gotten more capable, harnesses have been used to surgically extend and correct models to complete previously impossible tasks.

### Filesystems for Durable Storage and Context Management

We want agents to have durable storage to interface with real data, offload information that doesn't fit in context, and persist work across sessions.

The filesystem is arguably the most foundational harness primitive because of what it unlocks:
- Agents get a workspace to read data, code, and documentation.
- Work can be incrementally added and offloaded instead of holding everything in context.
- The filesystem is a natural collaboration surface. Multiple agents and humans can coordinate through shared files.

Git adds versioning to the filesystem so agents can track work, rollback errors, and branch experiments.

---

## Bash + Code as a General Purpose Tool

We want agents to autonomously solve problems without humans needing to pre-design every tool.

The main agent execution pattern today is a ReAct loop, where a model reasons, takes an action via a tool call, observes the result, and repeats in a while loop. But harnesses can only execute the tools they have logic for.

Harnesses ship with a bash tool so models can solve problems autonomously by writing & executing code.

Bash + code exec is a big step towards giving models a computer and letting them figure out the rest autonomously.

---

## Sandboxes and Tools to Execute & Verify Work

Agents need an environment with the right defaults so they can safely act, observe results, and make progress.

Sandboxes give agents safe operating environments. Instead of executing locally, the harness can connect to a sandbox to run code, inspect files, install dependencies, and complete tasks.

Good environments come with good default tooling. This includes pre-installing language runtimes and packages, CLIs for git and testing, browsers for web interaction and verification.

---

## Memory & Search for Continual Learning

Agents should remember what they've seen and access information that didn't exist when they were trained.

For memory, the filesystem is again a core primitive. Harnesses support memory file standards like AGENTS.md which get injected into context on agent start.

Knowledge cutoffs mean that models can't directly access new data. For up-to-date knowledge, Web Search and MCP tools help agents access information beyond the knowledge cutoff.

---

## Battling Context Rot

Agent performance shouldn't degrade over the course of work.

**Context Rot** describes how models become worse at reasoning and completing tasks as their context window fills up.

Compaction addresses what to do when the context window is close to filling up. It intelligently offloads and summarizes the existing context window so the agent can continue working.

Skills address the issue of too many tools or MCP servers loaded into context on agent start which degrades performance.

---

## Long Horizon Autonomous Execution

We want agents to complete complex work, autonomously, correctly, over long time horizons.

This is where the earlier harness primitives start to compound. Long-horizon work requires durable state, planning, observation, and verification to keep working across multiple context windows.

- **Filesystems and git** for tracking work across sessions
- **Ralph Loops** for continuing work - a harness pattern that intercepts the model's exit attempt via a hook and reinjects the original prompt in a clean context window
- **Planning and self-verification** to stay on track
