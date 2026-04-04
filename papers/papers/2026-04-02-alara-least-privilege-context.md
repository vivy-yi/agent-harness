---
title: "ALARA for Agents: Least-Privilege Context Engineering"
authors: (待补充)
date: 2026-03-??
source: arXiv
url: https://arxiv.org/html/2603.20380v1
tags: [context engineering, least-privilege, agent security]
summary: 提出 ALARA 原则，将最小权限原则引入 Agent Context Engineering，通过上下文文件作用域共享资源、NPC 文件定义 Agent 权限、Jinxes 指定工具执行模板。
---

# ALARA for Agents: Least-Privilege Context Engineering

**arXiv:2603.20380 [cs.AI] — 2026年3月**

## 核心贡献

将最小权限原则（Least-Privilege）引入 Agent Context Engineering。

## 三层 Harness 文件系统

1. **Context Files（上下文文件）**
   - 作用域共享资源
   - 指定编排器（Orchestrators）

2. **NPC Files（NPC 文件）**
   - 定义单个 Agent
   - 模型配置
   - 工具权限

3. **Jinxes（Jinja Execution Templates）**
   - YAML 格式的工具定义
   - 可执行的模板

## 实验覆盖

- 22 个本地模型（0.6B - 35B 参数）
- 115 个实际任务
- 约 2500 次总执行

任务类型涵盖：文件操作、网页搜索、多步脚本、工具链、多 Agent 委托

**链接**：https://arxiv.org/html/2603.20380v1
