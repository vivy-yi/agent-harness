---
title: "Building AI Coding Agents for the Terminal: Scaffolding, Harness, Context Engineering"
authors: N.D.Q. Bui et al.
date: 2026-03-05
source: arXiv
url: https://arxiv.org/abs/2603.05344
tags: [terminal agent, scaffolding, context engineering, long-running agent]
summary: 提出 terminal agent 架构的两阶段方法：Scaffolding（组装 Agent）和 Harness（运行时编排），解决了上下文窗口管理、安全执行、能力扩展三大挑战。
---

# Building AI Coding Agents for the Terminal: Scaffolding, Harness, Context Engineering

**作者：N.D.Q. Bui et al.**  
**arXiv:2603.05344 [cs.AI] — 2026年3月5日（v3修订）**

## 三大工程挑战

长时间运行的 terminal agent 必须解决：
1. **上下文窗口管理** - 超过模型 token 预算的长会话
2. **安全执行** - 防止 Agent 执行任意 shell 命令造成破坏
3. **能力扩展** - 不超出 Agent 的 prompt 预算

## 两阶段架构

### Scaffolding（脚手架）
在首次 prompt 前组装 Agent：
- System Prompt
- Tool Definitions
- Subagent Registry

### Harness（运行时刻）
运行时编排：
- Tool Dispatch（工具分发）
- Context Management（上下文管理）
- Safety Enforcement（安全执行）

## 行动模式 Agent 的五层功能架构

1. **Core Identity** - Agent 角色和不可协商的约束
2. **Tool Definitions** - 工具使用和代码质量指导
3. **Safety & Rules** - 条件加载策略（git 规范、任务跟踪指令）
4. **Provider-Specific Guidance** - LLM 提供商特定行为提示
5. **Dynamic Context** - 会话特定元数据

**链接**：
- 论文：https://arxiv.org/abs/2603.05344
- HTML：https://arxiv.org/html/2603.05344v3
