---
title: "shadowclaw - Agent Harness in C"
description: "Agent Harness running in C"
source: github
url: https://github.com/webxos/shadowclaw
stars: 2
language: C
date: 2026-04-23
tags: [agent, harness, c-language, lightweight, embedded]
summary: 用 C 语言实现的轻量级 Agent Harness，适合嵌入式和资源受限环境
---

# shadowclaw

## 核心功能

- **C 语言实现**：极低的内存占用和启动开销
- **轻量级设计**：适合嵌入式系统和资源受限环境
- **核心 Agent 能力**：支持 planning、tool calling、memory

## 技术亮点

选择 C 语言实现 Agent Harness 代表了一个有趣的方向：
- 极致性能：原生代码执行，无运行时开销
- 嵌入式友好：可在边缘设备和嵌入式系统运行
- FFI 友好：可与其他语言生态集成

## 相关项目

- 对比 Python 实现：OpenClaw（Python-based）
- 对比 Rust 实现：likely 类似的轻量 harness

## Stars 分布

目前 2 stars，社区早期阶段。
