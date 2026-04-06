---
title: "A Systematic Security Evaluation of OpenClaw and Its Variants"
authors: [ArXiv 2604.03131]
date: 2026-04-06
source: arXiv
url: https://arxiv.org/abs/2604.03131
tags: [agent, security, OpenClaw, framework, vulnerability, evaluation]
summary: 系统性安全评估 OpenClaw 系列 Agent 框架，发现所有评估的 agent 均存在重大安全漏洞，Agent 系统比独立模型风险显著更高，侦察和发现行为是最常见弱点。
---

# A Systematic Security Evaluation of OpenClaw and Its Variants

## 核心发现

**所有评估的 Agent 均存在重大安全漏洞**

研究评估了 6 个代表性 OpenClaw 系列 agent 框架：OpenClaw, AutoClaw, QClaw, KimiClaw, MaxClaw, ArkClaw

**关键数据：**
- 构建了 **205 个测试用例**，覆盖 agent 完整执行生命周期的代表性攻击行为
- **所有评估的 agent 均表现出重大安全漏洞**
- Agent 系统比底层独立模型的风险**显著更高**

## 主要弱点分类

| 攻击类型 | 描述 | 风险等级 |
|----------|------|----------|
| **侦察和发现** | 最常见的弱点 | 🔴 高 |
| 凭证泄露 | Credential leakage | 🔴 高 |
| 横向移动 | Lateral movement | 🔴 高 |
| 权限提升 | Privilege escalation | 🔴 高 |
| 资源开发 | Resource development | 🟡 中 |

## 安全发现

1. **Agent 系统的安全不仅取决于骨干模型的安全属性**
   - 还取决于：模型能力 + 工具使用 + 多步规划 + 运行时编排之间的耦合

2. **早期阶段的弱点可被放大为系统级故障**
   - 一旦 agent 被授予执行能力和持久运行时上下文，早期阶段的弱点会被放大

3. **需要从 prompt 级防护转向全生命周期安全治理**

## 对 Agent Harness 的启示

这篇论文直接针对 OpenClaw（也就是你正在使用的框架）进行了系统性安全评估：

- OpenClaw 本身存在安全漏洞
- 不同框架暴露不同的高风险配置
- 安全应该是 agent harness 设计的前置考虑

## 原文链接

https://arxiv.org/abs/2604.03131
