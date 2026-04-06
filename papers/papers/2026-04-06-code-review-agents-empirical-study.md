---
title: "From Industry Claims to Empirical Reality: An Empirical Study of Code Review Agents in Pull Requests"
authors: [ArXiv 2604.03196]
date: 2026-04-06
source: arXiv
url: https://arxiv.org/abs/2604.03196
tags: [agent, code-review, empirical-study, CRA, PR, autonomous-coding]
summary: 实证研究 Code Review Agents 在 Pull Requests 中的表现，发现 CRA-only PR 的合并率比 human-only 低 23.17 个百分点，信号噪声比分析揭示 60.2% 的 CRA-only PR 落在 0-30% 信号范围。
---

# From Industry Claims to Empirical Reality: An Empirical Study of Code Review Agents in Pull Requests

## 核心发现

**行业宣称 vs 实证结果：巨大落差**

行业宣称 Code Review Agents (CRAs) 可以管理 80% 的 PR 无需人工介入，但实证数据揭示：

| 指标 | CRA-only | Human-only | 差距 |
|------|----------|------------|------|
| 合并率 | 45.20% | 68.37% | -23.17pp |
| 信号比 0-30% | 60.2% | — | — |
| 平均信号比 <60% | 12/13 CRAs | — | — |

**关键洞察：**
1. **CRA 生成的低信号反馈与 PR 放弃高度相关** — 13 个 CRAs 中 12 个平均信号比低于 60%
2. **CRA 应辅助而非替代人类审核者** — 人类参与对有效、可操作的代码审查仍然关键
3. **OpenAI Codex 在 2 个月内创建了超过 400,000 个 PR** — Agent 生成代码规模前所未有

## 方法论

- 数据集：AIDev 19,450 个 PRs，3,109 个独特 PR 处于评论审查状态
- 对比组：CRA-only PRs vs Human-only PRs
- 信号噪声分析：评估 CRA 生成评论的信号质量

## 对 Agent Harness 的启示

这篇论文直接回答了一个关键问题：**什么样的 agent harness 能让 CRA 真正有效？**

研究结果表明，当前 CRAs 失败的根本原因是：
- **反馈信号质量不足** — 缺乏针对性、可操作的建议
- **噪声过高** — 生成过多无关或浅层的评论
- **缺乏上下文理解** — 无法真正把握代码意图和业务逻辑

这为 agent harness 设计提供了明确的改进方向。

## 原文链接

https://arxiv.org/abs/2604.03196
