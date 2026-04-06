# Agent Harness 每日抓取报告 - 2026-04-06

## 执行状态

✅ 抓取完成，内容已写入，准备推送。

---

## 📊 抓取统计

| 分类 | 今日新增 | 状态 |
|------|---------|------|
| 博客文章 (blog/articles/) | +0 | — |
| 工具仓库 (tools/repositories/) | +2 | ✅ 已写入 |
| 论文 (papers/papers/) | +2 | ✅ 已写入 |
| **合计** | **4** | |

---

## 📄 论文 (papers/papers/)

### 新增

1. **2026-04-06-code-review-agents-empirical-study.md**
   - 来源: ArXiv 2604.03196
   - 标题: From Industry Claims to Empirical Reality: An Empirical Study of Code Review Agents in Pull Requests
   - 核心发现:
     - CRA-only PR 合并率 45.20%，比 human-only 低 23.17 个百分点
     - 60.2% 的 CRA-only PR 落在 0-30% 信号范围
     - 13 个 CRAs 中 12 个平均信号比低于 60%
     - **结论：CRA 应辅助而非替代人类审核者**
   - 链接: https://arxiv.org/abs/2604.03196

2. **2026-04-06-openclaw-security-evaluation.md**
   - 来源: ArXiv 2604.03131
   - 标题: A Systematic Security Evaluation of OpenClaw and Its Variants
   - 核心发现:
     - 评估了 OpenClaw/AutoClaw/QClaw/KimiClaw/MaxClaw/ArkClaw 6 个框架
     - **所有评估的 agent 均存在重大安全漏洞**
     - Agent 系统比独立模型风险显著更高
     - 侦察和发现行为是最常见的弱点
     - 凭证泄露、横向移动、权限提升等高风险
   - 链接: https://arxiv.org/abs/2604.03131

---

## 🛠️ 工具仓库 (tools/repositories/)

### 新增

1. **2026-04-06-desloppify-agent-harness.md**
   - 来源: GitHub - peteromallet/desloppify
   - 名称: Desloppify
   - ⭐ 2,691 stars | 184 forks
   - 定位: "Agent harness to make your slop code well-engineered and beautiful"
   - 最新更新: 2026-04-05
   - 亮点: 聚焦代码质量提升的垂直场景 Agent，2691 stars 说明市场对代码质量 agent 有强烈需求

2. **2026-04-06-comfyui-workflow-skill.md**
   - 来源: GitHub - twwch/comfyui-workflow-skill
   - 名称: ComfyUI Workflow Skill
   - ⭐ 72 stars
   - 创建时间: 2026-04-04
   - 亮点:
     - Natural language → ComfyUI workflow JSON
     - 34 内置模板，360+ 节点定义
     - 支持 SD1.5/SDXL/SD3/FLUX/Wan2.2/HunyuanVideo 等
     - 可作为 Claude Code、Cursor 等 AI coding agents 的 skill

---

## 🔍 今日趋势观察

### OpenClaw 安全评估的警示
ArXiv:2604.03131 直接对 OpenClaw 系列框架进行了系统性安全评估，发现所有框架均存在重大安全漏洞。这对 Agent Harness 的设计有重要启示：

1. **安全应该是前置考虑**，而非事后补救
2. **侦察和发现行为是最常见弱点** — agent 的信息收集能力被滥用的风险
3. **需要全生命周期安全治理** — 而非仅 prompt 级防护

### Code Review Agent 的实证洞见
ArXiv:2604.03196 用数据证明了一个反直觉的结论：行业宣称的"80% PR 可由 CRA 管理"与现实差距巨大。这提示 Agent Harness 设计者：

1. **信号质量比覆盖范围更重要**
2. **Human-in-the-loop 仍然是必要条件**
3. **Agent 应该辅助人类，而非替代人类**

---

## 📅 推送目标

推送到 GitHub 仓库: `https://github.com/vivy-yi/agent-harness`
