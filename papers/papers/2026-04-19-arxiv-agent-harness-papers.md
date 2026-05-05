# ArXiv Agent Harness Papers — 2026-04-19

> ArXiv (cs.AI, cs.CL) 最新 agent harness 相关论文抓取
> 数据来源: ArXiv API | 检索词: ti:agent AND ti:harness

---

## 📄 Paper 1: SemaClaw

**arXiv**: [2604.11548](https://arxiv.org/abs/2604.11548)  
**标题**: SemaClaw: A Step Towards General-Purpose Personal AI Agents through Harness Engineering  
**标签:** harness-engineering, multi-agent, personal-ai, dag-orchestration, openclaws  
**相关度:** ⭐⭐⭐⭐

### 摘要

The rise of OpenClaw in early 2026 marks the moment when millions of users began deploying personal AI agents into their daily lives. This scale of adoption signals two parallel arcs have reached an inflection point:

1. **Paradigm shift in AI engineering**: from prompt/context engineering → **harness engineering** — designing complete infrastructure to transform unconstrained agents into controllable, auditable, production-reliable systems
2. **Human-agent interaction evolution**: from discrete tasks → persistent, contextually aware collaborative relationships

### 核心贡献 (SemaClaw 框架)

| 贡献 | 描述 |
|------|------|
| **DAG-based two-phase hybrid agent team orchestration** | 复杂任务的多智能体协调方法 |
| **PermissionBridge behavioral safety system** | 行为安全系统 |
| **Three-tier context management architecture** | 三层上下文管理 |
| **Agentic wiki skill** | 自动化个人知识库构建 |

> **关键洞察**: 随着模型能力趋同，**harness layer 正在成为架构分化的主要场所**

---

## 📄 Paper 2: ClawVM

**arXiv**: [2604.10352](https://arxiv.org/abs/2604.10352)  
**标题**: ClawVM: Harness-Managed Virtual Memory for Stateful Tool-Using LLM Agents  
**标签:** virtual-memory, stateful-agents, harness, context-management, tool-use  
**相关度:** ⭐⭐⭐⭐

### 摘要

Stateful tool-using LLM agents treat the context window as working memory, yet today's agent harnesses manage residency and durability as best-effort, causing recurring failures:
- Lost state after compaction
- Bypassed flushes on reset
- Destructive writeback

### ClawVM 解决方案

**Core idea**: 在 harness 层实现虚拟内存管理，将状态作为 typed pages 管理：

| 特性 | 说明 |
|------|------|
| **Minimum-fidelity invariants** | 保证基本不变量 |
| **Multi-resolution representations** | Token budget 下的多分辨率表示 |
| **Validated writeback** | 生命周期边界处的验证回写 |
| **<50 微秒/turn** | Policy engine 开销（median） |

> **关键洞察**: Harness 是 natural enforcement point，因为它已经负责 assemble prompts, mediate tools, observe lifecycle events

---

## 📄 Paper 3: Externalization in LLM Agents

**arXiv**: [2604.08224](https://arxiv.org/abs/2604.08224)  
**标题**: Externalization in LLM Agents: A Unified Review of Memory, Skills, Protocols and Harness Engineering  
**标签:** externalization, memory, skills, protocols, harness, cognitive-artifacts  
**相关度:** ⭐⭐⭐⭐

### 核心论点

> "Agent infrastructure matters not merely because it adds auxiliary components, but because it **transforms hard cognitive burdens into forms that the model can solve more reliably**."

### 四种外部化形式

| 形式 | 作用 | 类比 |
|------|------|------|
| **Memory** | 跨时间状态外部化 | 工作记忆 |
| **Skills** | 程序性专业知识外部化 | 技能库 |
| **Protocols** | 交互结构外部化 | 协作流程 |
| **Harness Engineering** | 协调以上模块 → governed execution | 元认知 |

### 历史演进
```
Weights → Context → Harness
```

### 新兴方向
- Self-evolving harnesses
- Shared agent infrastructure
- Evaluation & governance of harness

---

## 📊 本次抓取汇总

| # | 论文 | 相关度 | 重点 |
|---|------|--------|------|
| 1 | SemaClaw | ⭐⭐⭐⭐ | Harness engineering 框架 + OpenClaw 生产实践 |
| 2 | ClawVM | ⭐⭐⭐⭐ | Harness 层虚拟内存，stateful agent 可靠性 |
| 3 | Externalization Review | ⭐⭐⭐⭐ | Memory/Skills/Protocols/Harness 统一框架 |

---
*由 Agent Harness 每日抓取 cron 自动生成 (2026-04-19)*
