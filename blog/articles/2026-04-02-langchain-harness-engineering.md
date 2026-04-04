---
title: Improving Deep Agents with Harness Engineering
author: LangChain Blog
date: 2026-04-02
source: LangChain Blog
url: https://blog.langchain.com/improving-deep-agents-with-harness-engineering/
tags: [harness engineering, langchain, terminal bench, deep agents]
summary: LangChain 通过 Harness Engineering 在 Terminal Bench 2.0 上将 deepagents-cli 得分从 52.8 提升到 66.5，聚焦 System Prompt、Tools 和 Middleware 三个维度。
---

# Improving Deep Agents with Harness Engineering

**作者：LangChain Blog**  
**发布日期：2026年2月**

## 核心方法

LangChain 使用了一套简洁的配方迭代改进 deepagents-cli（编码 Agent），在 Terminal Bench 2.0 上从 **52.8 提升到 66.5**（+13.7 分）。

## 三大优化维度

1. **System Prompt** - Agent 的核心身份和不可协商的约束
2. **Tools** - 工具定义和代码质量指导
3. **Middleware** - 模型调用和工具调用周围的钩子（LangChain 术语）

## Middleware 的关键作用

类比 Ralph Wiggum Loop：当 Agent 遇到 exit 时，一个钩子强制 Agent 继续执行，用于验证环节。

## 多模型 Harness

在多模型 Harness 中平衡推理预算：
- 用大模型做规划
- 将任务交接（handoff）给小模型执行

## 外部 Loop 优化

Agent 改进的外部循环正在探索 **RLMs**（Reasoning Model Leaderboards）来更高效地挖掘 traces。

**代码仓库**：https://github.com/langchain-ai/deepagents  
**Benchmark**：https://www.tbench.ai/
