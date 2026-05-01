# Compose Performance Skills

**URL:** https://github.com/skydoves/compose-performance-skills
**Stars:** 226 | **Forks:** 7 | **License:** Apache License 2.0
**Language:** Shell
**Created:** 2026-04-29 | **Pushed:** 2026-04-30
**Topics:** agent-skills, android, claude, claude-code, compose, compose-performance, jetpack-compose, kotlin, llm-tools, recomposition

## Summary

A curated library of Agent Skills focused on Jetpack Compose performance. Every skill is grounded in primary sources: Android Developers documentation, the Compose compiler, posts by Ben Trengrove, Chris Banes, Manuel Vivo, and blog posts. Every API reference is pinned to a version.

The skills follow the open Agent Skills standard published at agentskills.io, the same `SKILL.md` format used by Anthropic's Skills API, Android Studio Agent mode, and Gemini. The library was iterated against Claude Code.

## What is a Skill

A Skill is a single Markdown file (`SKILL.md`) plus optional `references/` material that teaches an agent how to perform one focused task. It declares trigger vocabulary in YAML frontmatter and a numbered workflow in the body. The agent reads the frontmatter, decides whether the skill applies to the current task, then follows the workflow step by step.

## Directory Layout

```
compose-performance-skills/
├── README.md                 # this file
├── INDEX.md                  # symptom to skill lookup table
└── <category>/<slug>/
    ├── SKILL.md              # the skill (required)
    └── references/           # optional, one level deep
```

## Skills Coverage

The library covers stability, recomposition, lazy layouts, custom modifiers, side effects, baseline profiles, R8, hot reload with Compose HotSwan, and the measurement loop.

## Relevance to Agent Harness

- Demonstrates the Agent Skills standard expanding into specific framework domains (Jetpack Compose)
- Directly useful for Claude Code agents working on Android/Compose projects
- Grounded in primary sources — citation trail included for developer review
- Shows the skills library pattern being adopted by Android ecosystem