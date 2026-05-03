# openai/codex

**URL:** https://github.com/openai/codex
**Stars:** 79,610 | **Forks:** 11,438 | **License:** Apache License 2.0
**Language:** Rust
**Created:** 2025-04-13 | **Pushed:** 2026-05-03

## Summary

Lightweight coding agent that runs in your terminal.

OpenAI's official codex CLI is a Rust-based coding agent designed to be lightweight, fast, and terminal-native. It brings GPT-4o and o3/o4 reasoning models to the command line for developer assistance.

## Architecture

- **Language:** Rust — chosen for speed, memory safety, and minimal dependencies
- **Models:** Supports OpenAI's GPT-4o, o3, and o4 models
- **Tool Interface:** File system, shell, git operations via structured JSON tool calls
- **Streaming:** Real-time output via terminal TTY

## Key Features

- **Rust-Based:** Minimal binary size, fast startup, memory-safe
- **Official OpenAI:** First-party CLI from the creators of the Codex API
- **Terminal-First:** Designed for developers who prefer CLI over GUI
- **Model Flexibility:** Switch between GPT-4o, o3, o4 on the fly
- **Tool System:** Structured tool calls for code editing, shell commands, git ops

## Relevance to Agent Harness

- Reference implementation of a lightweight, terminal-native coding agent
- Rust architecture is interesting for production deployment (security, performance)
- Shows the trend toward "thin" agent runtimes vs. heavyweight frameworks
- Useful benchmark for comparing agent harness performance characteristics