# gemini-cli

**URL:** https://github.com/google-gemini/gemini-cli
**Stars:** 102,991 | **Forks:** 13,468 | **License:** Apache License 2.0
**Language:** TypeScript
**Created:** 2025-04-17 | **Pushed:** 2026-05-02

## Summary

An open-source AI agent that brings the power of Gemini directly into your terminal.

gemini-cli is Google's official CLI tool for interacting with Gemini models as a coding agent. It provides a terminal-native interface for AI-assisted development, enabling developers to invoke Gemini-powered agent capabilities directly from the command line.

## Architecture

- TypeScript/Node.js CLI application
- Connects to Google's Gemini models (Gemini 2.5 series)
- Tool-calling support for file system, shell, search
- Streaming output for real-time feedback
- Session-based context management

## Key Features

- **Official Google Tool:** First-party Gemini agent implementation
- **Terminal-Native:** Built for developer workflows in the CLI
- **Tool Calling:** File system access, shell command execution, web search
- **Streaming Responses:** Real-time token output
- **Context Management:** Maintains session state across commands

## Relevance to Agent Harness

- Official Google reference implementation of a terminal-based agent
- Demonstrates first-party tool-calling patterns for Gemini models
- Large star count (103K) indicates widespread adoption
- Useful as a baseline for comparing other agent harness implementations