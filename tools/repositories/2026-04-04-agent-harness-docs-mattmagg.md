---
title: agent-harness (MattMagg)
owner: MattMagg
url: https://github.com/MattMagg/agent-harness
date: 2026-04-04
tags: [harness, docs, principles, checklists, invariants]
category: framework
summary: OpenAI harness 工程指南的实践版，principles/checklists/invariants 开源实现
---

# agent-harness (MattMagg)

## 概述

将 OpenAI 的 harness 工程指南蒸馏为可复用仓库 artifacts：principles、checklists、prompts、invariants。

## 核心结构

```
├── principles.md              # 核心原则
├── autonomy-levels.md         # 自主级别定义
├── repository-knowledge.md    # 知识库管理
├── legibility-and-feedback-loops.md
├── invariants-and-guardrails.md
├── merge-and-throughput.md
├── entropy-and-gc.md
├── checklists/
│   ├── change-preflight.md    # 变更前检查
│   ├── pr-review.md           # PR 审查
│   └── doc-gardening.md       # 文档维护
├── prompts/
│   ├── doc-gardener.md
│   └── harness-enforcer.md
└── openclaw/                  # OpenClaw 操作治理
```

## 亮点

- docs-first 设计
- 渐进式披露 (Progressive Disclosure) 实践
- Tool Subtraction 模式
- 可直接集成到 OpenClaw 工作流
