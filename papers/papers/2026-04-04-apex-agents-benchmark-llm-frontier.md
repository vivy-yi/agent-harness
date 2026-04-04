---
title: APEX–Agents Benchmark: Evaluating Frontier LLM Agents
authors: arXiv
date: 2026-01-14
source: arXiv:2601.14242
url: https://arxiv.org/abs/2601.14242
tags: [benchmark, frontier-models, evaluation, agent]
subjects: [cs.CL]
summary: 评估前沿 Agent 模型（GPT-5/Claude Opus 4.5/Gemini 3 Pro）在专业场景的表现
---

# APEX–Agents Benchmark

## 核心贡献

评估前沿 LLM Agent 在专业场景（IB analyst / Consultant / Lawyer）的综合表现。

## Benchmark 数据

| Model | Pass@1 | Pass@8 | Mean Score |
|-------|--------|--------|------------|
| GPT-5 | 18.3% | 31.0% | 15.3% |
| Claude Opus 4.5 | 18.4% | 34.0% | 20.2% |
| Gemini 3 Pro | 18.4% | 37.3% | 23.9% |
| GPT-5.2 | 23.0% | 40.0% | 18.9% |
| Kimi K2 Thinking | 4.0% | 14.4% | 8.0% |

## 关键发现

- 即使最强配置也仅 68% 任务完成率
- 工具编排错误 (23%)、多资产推理降级 (14.9%)、跨设备泛化 (42.7%)

## 引用

```
arXiv:2601.14242 [cs.CL]
```
