# Effective Harnesses for Long-Running Agents

> Source: Anthropic Engineering | Date: November 2025

---

As AI agents become more capable, developers are increasingly asking them to take on complex tasks requiring work that spans hours, or even days. However, getting agents to make consistent progress across multiple context windows remains an open problem.

## The Long-Running Agent Problem

The core challenge of long-running agents is that they must work in discrete sessions, and each new session begins with no memory of what came before.

Imagine a software project staffed by engineers working in shifts, where each new engineer arrives with no memory of what happened on the previous shift. Because context windows are limited, and because most complex projects cannot be completed within a single window, agents need a way to bridge the gap between coding sessions.

We developed a two-fold solution: an initializer agent that sets up the environment on the first run, and a coding agent that is tasked with making incremental progress in every session, while leaving clear artifacts for the next session.

## The Solution

Claude's failures manifested in two patterns:

1. **The agent tried to do too much at once** — attempting to one-shot the app. Often, this led to the model running out of context in the middle of its implementation, leaving the next session to start with a feature half-implemented and undocumented.

2. **Later agents would declare the job done** — after some features had already been built, a later agent instance would look around, see that progress had been made, and declare the job done.

### Initializer Agent

The very first agent session uses a specialized prompt that asks the model to set up the initial environment:
- An init.sh script
- A claude-progress.txt file that keeps a log of what agents have done
- An initial git commit

### Coding Agent

Every subsequent session asks the model to make incremental progress, then leave structured updates.

## Environment Management

### Feature List

To address the problem of the agent one-shotting an app or prematurely considering the project complete, we prompted the initializer agent to write a comprehensive file of feature requirements expanding on the user's initial prompt.

We prompt coding agents to edit this file only by changing the status of a passes field, and we use strongly-worded instructions like "It is unacceptable to remove or edit tests because this could lead to missing or buggy functionality."

### Incremental Progress

The next iteration of the coding agent was then asked to work on only one feature at a time. This incremental approach turned out to be critical.

Once working incrementally, it's essential that the model leaves the environment in a clean state after making a code change. We ask the model to commit its progress to git with descriptive commit messages and to write summaries of its progress in a progress file.

### Testing

One final major failure mode we observed was Claude's tendency to mark a feature as complete without proper testing.

Providing Claude with browser automation tools and doing all testing as a human user would dramatically improved performance.

## Getting Up to Speed

Every coding agent is prompted to run through a series of steps to get its bearings:
- Run pwd to see the directory you're working in
- Read the git logs and progress files to get up to speed on what was recently worked on
- Read the features list file and choose the highest-priority feature that's not yet done to work on

This approach saves Claude some tokens in every session since it doesn't have to figure out how to test the code.
