# Daily Crawl — 2026-04-18

**抓取时间**: 2026-04-18 08:30 UTC
**抓取范围**: ArXiv (cs.AI, cs.CL) · GitHub Trending · 技术社区

---

## 📄 Papers (ArXiv)

### 新发现 5 篇相关论文

| 编号 | 标题 | 分类 | 相关度 |
|------|------|------|--------|
| 2604.15309 | **MM-WebAgent: A Hierarchical Multimodal Web Agent for Webpage Generation** | Web Agent / Multimodal | ⭐⭐⭐ |
| 2604.15236 | **Agentic Microphysics: A Manifesto for Generative AI Safety** | Agent Safety | ⭐⭐⭐⭐ |
| 2604.15267 | **CoopEval: Benchmarking Cooperation-Sustaining Mechanisms and LLM Agents in Social Dilemmas** | Multi-Agent | ⭐⭐⭐ |
| 2604.15244 | **SpecGuard: Verification-Aware Speculative Decoding for Multi-Step Reasoning** | Reasoning / Harness | ⭐⭐⭐ |
| 2604.15233 | **Blue Data Intelligence Layer: Streaming Data and Agents for Multi-source Multi-modal Data-Centric Applications** | Data Agent | ⭐⭐⭐ |

### 重点论文摘要

**Agentic Microphysics (2604.15236)** — ⚠️ 高度重要
> 论文 advancing a methodological proposal for safety research in **agentic AI**. 核心观点：
> - 随着系统获得 planning, memory, tool use, persistent identity，系统安全分析不能再停留在 isolated model 层面
> - **Population-level risks** 来自 agent 间 structured interaction（communication, observation, mutual influence）
> - 提出 **Agentic microphysics** 概念：local interaction dynamics where one agent's output becomes another's input
> - **Generative safety** 方法论：从 micro-level conditions 生长 phenomena，识别 thresholds，设计 interventions
> - 方法论 gap：single agent / aggregate outcome 方法无法识别 collective risk 的 interaction-level mechanisms
>
> 这篇论文直接关联 **agent harness 工程** — harness 本质是构建 agent interaction 的 controlled environment

**SpecGuard (2604.15244)**
> 解决 speculative decoding 在 multi-step reasoning 中的错误传播问题。核心：step-level verification using model-internal signals。
> - 提出 attention-based grounding score + log-probability-based score
> - 在 reasoning benchmarks 上 accuracy +3.6%，latency -11%
> - 对 harness 验证框架有参考价值：step-level verification 思路可迁移到 agent harness

**MM-WebAgent (2604.15309)**
> 层次化多模态网页生成 agent，协调 AIGC 工具做 coherent webpage generation。
> - hierarchical planning + iterative self-reflection
> - 对比 code-generation 和 agent-based baselines
> - 多级评估协议 (multi-level evaluation protocol)

---

## 🛠️ Tools / Repositories

- 近期新增: `agent-tools-2026` (2026-04-12)
- 趋势监测: OpenClaw, DeerFlow 2, ComfyUI Workflow Skills

---

## 📝 Blog / Articles

- 近期新增 11 篇博客文章 (2026-04-02 至 2026-04-05)
- 覆盖: langchain harness engineering, agentic workflow patterns, prompt engineering vs harness engineering

---

## 📊 统计

- **Papers**: +5 篇 ArXiv 论文（含 1 篇高相关度安全论文）
- **Tools**: 无新增（最近 2026-04-12）
- **Blog**: 无新增（最近 2026-04-05）
- **总计新增**: 5 项

---

## 🔮 后续建议

1. **优先**: 将 Agentic Microphysics (2604.15236) 加入论文分析队列，这篇论文的方法论对 harness 框架设计有直接指导意义
2. **关注**: SpecGuard 的 step-level verification 机制，可作为 agent harness 验证层的参考架构
3. **跟踪**: MM-WebAgent 的 hierarchical evaluation protocol 实现

---
*由 Agent Harness 每日抓取 cron 自动生成 (crawl-2026-04-18 分支)*
