---
title: Why Your AI Agent Keeps Failing: A Deep Dive into Harness Engineering
author: gitconnected
date: 2026-04-04
source: Medium / gitconnected
url: https://medium.com/gitconnected/why-your-ai-agent-keeps-failing-a-deep-dive-into-harness-engineering-f579e7609e48
tags: [harness, engineering, agent, evaluation]
summary: 深入分析 Harness Engineering 为何是 AI Agent 可靠性的关键，附 OpenAI Codex 1500 PR 实践案例
---

# Why Your AI Agent Keeps Failing: A Deep Dive into Harness Engineering

## 核心观点

AI Agent 失败的根本原因往往不是模型能力不足，而是 **Harness（牵引系统）** 设计不当。Harness 是包围 Agent 的基础设施，负责定义任务边界、验证输出、处理异常。

## OpenAI Codex 实践

2026年2月，OpenAI 发表博客描述工程团队如何用 Codex 编写 1500 个 pull request 的全部代码。关键成功因素：
- **渐进式披露 (Progressive Disclosure)**: Agent 只看到当前步骤需要的信息
- **Tool Subtraction**: 逐步减少可用工具，避免 Agent 混乱
- **明确的不变量 (Invariants)**: 强制执行不可违反的约束

## Harness Engineering 最佳实践

1. **Validation Loops**: 每次工具调用后验证结果，不信任直接输出
2. **Change Preflight**: 变更前检查，防止 Agent 做出不可逆操作
3. **Entropy & GC**: 控制 Agent 产生的上下文噪音，定期清理状态
4. **Autonomy Levels**: 明确 Agent 的自主级别 (1-5)，高自主需更多 guardrails
