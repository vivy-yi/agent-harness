# Agent Frameworks, Runtimes, and Harnesses — oh my!

> Source: LangChain Blog | Date: 2026

---

There are few different open source packages we maintain: LangChain and LangGraph being the biggest ones, but DeepAgents being an increasingly popular one. I've started using different terms to describe them: LangChain is an agent framework, LangGraph is an agent runtime, DeepAgents is an agent harness.

## Agent Frameworks (LangChain)

Most packages out there that help build with LLMs I would classify as agent frameworks. The main value add they provide is abstractions. These abstractions represent a mental model of the world.

We think of LangChain as an agent framework. As part of the 1.0 we spent a lot of time thinking about the abstractions - for structured content blocks, for the agent loop, for middleware (which we think adds flexibility to the standard agent loop).

Other examples of what I would consider agent frameworks are Vercel's AI SDK, CrewAI, OpenAI Agents SDK, Google ADK, LlamaIndex, and lot more.

## Agent Runtimes (LangGraph)

When you need to run agents in production, you will want some sort of runtime for agents. This runtime should provide more infrastructure level considerations.

The main one that comes to mind is durable execution, but I would also put considerations like support for streaming, human-in-the-loop support, thread level persistence and cross-thread persistence here.

When we build LangGraph, we wanted to build in a production ready agent runtime from scratch.

Agent runtimes are generally lower level than agent frameworks and can power agent frameworks. For example, LangChain 1.0 is built on top of LangGraph to take advantage of the agent runtime it provides.

## Agent Harnesses (DeepAgents)

DeepAgents is the newest project we're working on. It is higher level than agent frameworks - it builds on top of LangChain. It adds in default prompts, opinionated handling for tool calls, tools for planning, has access to a filesystem, and more. It's more than a framework - it comes with batteries included.

Another way that we've used to describe DeepAgents is as a "general purpose version of Claude Code".

To be fair, Claude Code is also trying to be an agent harness - they've released things like Claude Agent SDK as a step in that direction. Besides Claude Agent SDK, I don't think there are many other general purpose agent harnesses out there today.

## When to use each one

The lines are blurry. LangGraph is probably best described as both a runtime and a framework. "Agent Harness" is a term I'm just starting to see be used more.

Part of the fun of developing in an early space is coming up with the mental models for how to talk about things.

We know LangChain is different from LangGraph, and DeepAgents is different from both of them. We think describing them as a framework, runtime, and harness respectively is a helpful distinction.
