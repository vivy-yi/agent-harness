# Daily Crawl — 2026-04-19

**抓取时间**: 2026-04-19 01:04 UTC  
**抓取范围**: ArXiv (cs.AI, cs.CL) · GitHub Trending

---

## 📄 Papers (ArXiv)

### 新发现 3 篇相关论文

| 编号 | 标题 | 分类 | 相关度 |
|------|------|------|--------|
| 2604.11548 | **SemaClaw: General-Purpose Personal AI Agents through Harness Engineering** | Harness Framework | ⭐⭐⭐⭐ |
| 2604.10352 | **ClawVM: Harness-Managed Virtual Memory for Stateful Tool-Using LLM Agents** | Memory/Harness | ⭐⭐⭐⭐ |
| 2604.08224 | **Externalization in LLM Agents: Memory, Skills, Protocols and Harness Engineering** | Survey | ⭐⭐⭐⭐ |

### 重点论文摘要

**SemaClaw (2604.11548)** — 🔥 直接关联 OpenClaw
> OpenClaw 在 2026 年初已达到数百万用户规模，标志着 personal AI agent 时代到来。论文提出：
> - **Harness engineering** 作为新范式：不再只是调 prompt，而是构建完整的 agent 控制基础设施
> - **DAG-based two-phase hybrid agent team orchestration** — 多智能体协调
> - **PermissionBridge behavioral safety system** — 行为安全
> - **Three-tier context management** — 三层上下文
> - **Agentic wiki skill** — 自动化个人知识库
>
> 直接关联当前 agent harness 工程实践，特别是 OpenClaw 的生产部署经验

**ClawVM (2604.10352)** — 基础设施层面重要
> Stateful tool-using agents 将 context window 当作工作记忆，但现有 harness 对 residency 和 durability 的管理是 best-effort，导致：
> - Compaction 后状态丢失
> - Reset 时 flush 被绕过
> - Writeback 被破坏
>
> **ClawVM** 解决方案：在 harness 层实现虚拟内存，用 typed pages + minimum-fidelity invariants + validated writeback，median overhead <50 微秒/turn

**Externalization Review (2604.08224)** — 方法论框架
> 用 cognitive artifacts 视角解释为什么 agent 基础设施重要：
> - Memory → 跨时间状态外部化
> - Skills → 程序性专业知识外部化
> - Protocols → 交互结构外部化
> - **Harness Engineering → 统一协调层**
>
> 历史演进：**Weights → Context → Harness**

---

## 🛠️ Tools / Repositories

### 新增 2 个 GitHub Trending 仓库

| 仓库 | ⭐ | 描述 | 分类 |
|------|-----|------|------|
| langflow-ai/langflow | 147,087 | Visual AI workflow builder (LangChain-based) | Agent Framework |
| langgenius/dify | 138,261 | Production-ready agentic workflow platform | Low-code Platform |

> 上次抓取 (2026-04-12) 已有 obra/superpowers, firecrawl, gemini-cli, browser-use 等

---

## 📊 统计

| 类别 | 新增数量 | 文件路径 |
|------|----------|----------|
| **Papers** | +3 篇 ArXiv 论文 | `papers/papers/2026-04-19-arxiv-agent-harness-papers.md` |
| **Tools** | +2 个仓库 | `tools/repositories/2026-04-19-agent-tools-trending.md` |
| **Blog** | 0（无新来源） | — |
| **总计新增** | **5 项** | — |

---

## 🔮 后续建议

1. **优先**: SemaClaw 和 ClawVM 代表了 harness 工程的前沿方向，建议加入论文精读队列
2. **关注**: Externalization review 提供了 Memory/Skills/Protocols/Harness 的统一框架，可作为知识库方法论基础
3. **对比**: LangFlow vs Dify — 两个主流低代码 agent 开发平台的对比值得持续跟踪

---
*由 Agent Harness 每日抓取 cron 自动生成 (crawl-2026-04-19 分支)*
