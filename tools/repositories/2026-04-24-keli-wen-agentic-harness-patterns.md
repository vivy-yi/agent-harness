---
title: "agentic-harness-patterns-skill"
description: "Agent skill for harness engineering — memory, permissions, context engineering, multi-agent coordination. Distilled from Claude Code, with Codex CLI and Gemini CLI on the roadmap. EN/ZH. Install via npx skills add."
source: github
url: https://github.com/keli-wen/agentic-harness-patterns-skill
stars: 241
language: TypeScript/JSON
date: 2026-04-01
tags: [agent, harness, skill, memory, permissions, context-engineering, multi-agent, claude-code]
summary: Agent Harness 工程技能包，萃取了 Claude Code 的精华，含 memory/permissions/multi-agent 协调
---

# agentic-harness-patterns-skill

## 核心功能

### 1. Memory（记忆系统）
- **短期记忆**：会话内上下文保持
- **长期记忆**：跨会话知识积累
- **向量存储**：高效语义检索

### 2. Permissions（权限管理）
- **最小权限原则**：Agent 只获取必要的权限
- **权限层级**：支持不同安全级别的操作
- **动态权限**：根据任务需求动态调整

### 3. Context Engineering（上下文工程）
- **上下文压缩**：管理长上下文
- **上下文优先级**：关键信息优先
- **上下文注入**：按需插入背景知识

### 4. Multi-Agent Coordination（多 Agent 协调）
- **Agent 通信协议**：标准化的消息格式
- **任务分解**：复杂任务拆分为子任务
- **结果聚合**：多 Agent 输出整合

## 安装

```bash
npx skills add agentic-harness-patterns-skill
```

## 路线图

- ✅ Claude Code 支持
- 🔄 Codex CLI 支持
- 🔄 Gemini CLI 支持

## Stars 分布

241 stars，正在快速增长中。
