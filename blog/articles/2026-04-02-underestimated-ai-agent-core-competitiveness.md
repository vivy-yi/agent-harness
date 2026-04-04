---
title: "一个被低估的AI Agent核心竞争力"
source: "https://mp.weixin.qq.com/s/vEdDLMXsMsXsOgFIhO0sqQ"
author: "Himanshu, Viv, Tony Kipkemboi (综合分析)"
date: "2026-04-02"
summary: "本文深入分析AI Agent的harness概念，通过三位开发者的不同视角，展示harness比模型更重要的事实，并提供了详细的设计原则和公司实践案例。"
---

# 一个被低估的AI Agent核心竞争力

三位开发者——Himanshu、Viv 和 Tony Kipkemboi——分别从不同角度深入分析了 agent harness 这个概念。

在深入讨论之前，我们需要先搞清楚 harness 这个概念。Tony Kipkemboi 曾在 CrewAI（一个 agent framework）工作，他把这个概念解释得很清楚。他把 agent 开发比作一个光谱：最左边是原始代码，最右边是 agent harness（代理脚手架），这是最有观点的方案。

Viv 的文章从第一性原理出发，推导出我们为什么需要 harness。模型本身能做什么？它们接收文本、图像、音频、视频等数据，输出文本。就这样。开箱即用，它们无法维持跨交互的持久状态，无法执行代码，无法访问实时知识，无法设置环境和安装包来完成工作。这些都是 harness 层面的功能。

## Harness 必须包含的几个核心组件

1. **文件系统** - 最基础的 harness 原语，提供持久存储
2. **Bash 和代码执行** - 通用工具，让 agent 自主解决问题
3. **压缩** - 当上下文窗口接近填满时的策略

## 数据证明 harness 的重要性

- Vercel 删除 agent 80% 的工具后，agent 从失败任务变成了完成任务
- Token 从 145463 降到 67483
- 步骤从 100 降到 19
- 延迟从 724 秒降到 141 秒

## 公司实践案例

- **Claude Code**：采用"模型控制循环"理念，简单 `while(tool_call)` 循环
- **Manus**：最大性能提升来自删除东西
- **Stripe Minions**：完全自主的 coding agent，每周 merge 超过 1300 个 PR
- **OpenAI Codex**：3个工程师、5个月、百万行代码、零手写

## Harness 设计中的关键模式

- **Progressive disclosure（渐进式披露）** - 整个 harness 设计中最被低估的模式
- **Ralph Loop** - 用于继续工作的 harness 模式
- **GAN 式 Harness** - Anthropic 把 generator-discriminator 分离思路搬到 agent harness
