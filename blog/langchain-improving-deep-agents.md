# Improving Deep Agents with Harness Engineering

![Improvement Results](./langchain-images/improving-01.png)
> Source: LangChain Blog | Date: 2026

---

**TLDR:** Our coding agent went from Top 30 to Top 5 on Terminal Bench 2.0. We only changed the harness. Here's our approach to harness engineering (teaser: self-verification & tracing help a lot).

---

## The Goal of Harness Engineering

The goal of a harness is to mold the inherently spiky intelligence of a model for tasks we care about. Harness Engineering is about systems, you're building tooling around the model to optimize goals like task performance, token efficiency, latency, etc.

At LangChain, we use Traces to understand agent failure modes at scale. Models today are largely black-boxes, their inner mechanisms are hard to interpret. But we can see their inputs and outputs in text space which we then use in our improvement loops.

We used a simple recipe to iteratively improve our coding agent 13.7 points from 52.8 to 66.5 on Terminal Bench 2.0. We only tweaked the harness and kept the model fixed.

## The Knobs we can Turn

An agent harness has a lot of knobs: system prompts, tools, hooks/middleware, skills, sub-agent delegation, memory systems, and more.

We deliberately compress the optimization space and focus on three: System Prompt, Tools, and Middleware.

We start with a default prompt and standard tools+middleware. This scores 52.8% with GPT-5.2-Codex. A solid score, just outside the Top 30 of the leaderboard today, but room to grow.

## What Actually Improved Agent Performance

![Self Verification](./langchain-images/improving-05.png)

### Build & Self-Verify

Today's models are exceptional self-improvement machines.

Self-verification allows agents to self-improve via feedback within a run. However, they don't have a natural tendency to enter this build-verify loop.

The most common failure pattern was that the agent wrote a solution, re-read its own code, confirmed it looks ok, and stopped. Testing is a key part of autonomous agentic coding.

We added guidance to the system prompt on how to approach problem solving:

- **Planning & Discovery**: Read the task, scan the codebase, and build an initial plan
- **Build**: Implement the plan with verification in mind. Build tests.
- **Verify**: Run tests, read the full output, compare against what was asked
- **Fix**: Analyze any errors, revisit the original spec, and fix issues

### Giving Agents Context about their Environment

Part of harness engineering is building a good delivery mechanism for context engineering.

- **Directory Context & Tooling**: A LocalContextMiddleware runs on agent start to map the cwd and other directories.
- **Teaching Agents to Write Testable Code**: We add prompting saying their work will be measured against programmatic tests.
- **Time Budgeting**: We inject time budget warnings to nudge the agent to finish work and shift to verification.

### Encouraging Agents to Step Back & Reconsider Plans

Agents can be myopic once they've decided on a plan which results in "doom loops" that make small variations to the same broken approach.

We use a LoopDetectionMiddleware that tracks per-file edit counts and adds context like "…consider reconsidering your approach" after N edits to the same file.

### Choosing How Much Compute to Spend on Reasoning

Reasoning models can run autonomously for hours so we have to decide how much compute to spend on every subtask.

gpt-5.2-codex has 4 reasoning modes: low, medium, high, and xhigh.

We found that reasoning helps with planning to fully understand the problem, and later stage verification also benefits from more reasoning.

## Practical Takeaways for Building Agent Harnesses

- **Context Engineering on Behalf of Agents**: Onboarding models with context like directory structures, available tools, coding best practices, and problem solving strategies helps reduce the error surface.
- **Help agents self-verify their work**: Prompt them aggressively to verify their work by running tests and refining solutions.
- **Tracing as a feedback signal**: Traces allow agents to self-evaluate and debug themselves.
- **Detect and fix bad patterns in the short term**: The job of the harness designer is to design around today's shortcomings while planning for smarter models in the future.
- **Tailor Harnesses to Models**: Different models may need different harness configurations.
