---
title: "Meta-Harness: End-to-End Optimization of Model Harnesses"
authors: Yoonho Lee, Roshen Nair, Qizheng Zhang, Kangwook Lee, Omar Khattab, Chelsea Finn
date: 2026-03-30
source: arXiv
url: https://arxiv.org/abs/2603.28052
tags: [meta-harness, harness optimization, terminal bench]
summary: Stanford 提出 Meta-Harness，通过端到端优化自动搜索最优 Agent Harness 配置，在 Terminal Bench 2.0 上超越 Claude Code 和人工设计的 harness。
---

# Meta-Harness: End-to-End Optimization of Model Harnesses

**作者：Yoonho Lee, Roshen Nair, Qizheng Zhang, Kangwook Lee, Omar Khattab, Chelsea Finn (Stanford, MIT, KRAFTON)**  
**arXiv:2603.28052 [cs.AI] — 2026年3月30日**

## 核心贡献

尽管 Harness Engineering 很重要，但目前仍主要依赖人工设计。Meta-Harness 提出了**端到端自动优化**方法。

## 方法

1. **Harness Evaluations** - 收集不同 harness 配置的表现数据
2. **Meta-Harness Optimizer** - 基于搜索的优化器自动发现最优配置
3. **Store all Logs to Filesystem** - 持久化推理轨迹

## 实验结果

在 Terminal Bench 2.0 上：
| 方法 | Pass Rate |
|------|-----------|
| Meta-Harness | **37.6%** |
| Goose | 35.5% |
| Terminus-KIRA | 33.7% |
| Mini-SWE-Agent | 29.8% |
| Claude Code | 27.5% |

Meta-Harness 超越了 Claude Code 约 **10.1 个百分点**。

## 关键发现

- 自动化 Harness 优化可以超越人工设计
- 搜索空间涵盖 System Prompt、Tools、Middleware 配置
- 少量评估即可找到优于人工设计的配置

**项目页面**：https://yoonholee.com/meta-harness/  
**代码仓库**：https://github.com/stanford-iris-lab/meta-harness-tbench2-artifact
