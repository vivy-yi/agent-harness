# Harness Engineering: Leveraging Codex in an Agent-First World

**来源**: [OpenAI Blog](https://openai.com/index/harness-engineering/)

**日期**: 2026年3月

**核心内容**:

OpenAI 工程团队分享用 Codex 写了 1500 个 PR 代码的经验：

1. **从空 git 仓库开始** — 仓库知识作为系统记录
2. **Agent 可读性是目标** — 可观测性优先
3. **轻量级临时计划用于小改动，复杂工作使用执行计划**
4. **Ralph Wiggum Loop** — agent 自我审查 → 反馈 → 迭代直到所有 reviewer 满意

**Harness 关键组件**:
- Ephemeral lightweight plans（小型变更）
- Execution plans with progress and decision logs（复杂工作）
- Agent self-review loop（质量保障）
- Repository knowledge as system of record

**标签**: `#openai` `#codex` `#harness-engineering` `#engineering`
