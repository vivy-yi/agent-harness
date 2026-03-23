# Agent Harness

> Agent Harness 技术研究集合，包含深度分析报告、职业发展和可视化总结。

---

## 执行摘要

**Agent Harness** 是2025-2026年 AI 工程领域最具影响力的概念之一，由 Mitchell Hashimoto（Terraform 创始人）正式提出并命名。它代表了从"提示工程"向"系统工程"转变的重要里程碑。

**OpenAI 实证数据**: 3名工程师使用 Harness Engineering，在5个月内产出约1500个PR，平均每人每天3.5个PR，代码库规模达100万行，零人工编写代码。

---

## 什么是 Agent Harness?

Agent Harness 是围绕 LLM 的软件基础设施层，负责管理除模型推理本身之外的一切：

```
┌─────────────────────────────────────────────────────────────┐
│                    AGENT HARNESS                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │ Tool Layer   │  │ Memory &     │  │ Context      │   │
│  │ (工具执行)    │  │ State Mgmt   │  │ Engineering  │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │ Verification │  │ Guardrails   │  │ Orchestration│   │
│  │ (验证层)     │  │ (安全护栏)   │  │ (编排逻辑)   │   │
│  └──────────────┘  └──────────────┘  └──────────────┘   │
└─────────────────────────────────────────────────────────────┘
                           │
                    ┌──────┴──────┐
                    │  LLM Model  │  ← 仅负责推理
                    │  (CPU核心)  │
                    └─────────────┘
```

### 权威定义

| 来源 | 定义 |
|------|------|
| **Mitchell Hashimoto** | "Agent Harness 是将 Agent 能力'约束'和'引导'到特定任务的软件基础设施" |
| **Anthroipc** | "管理上下文生命周期的完整架构系统" |
| **Phil Schmid** | "模型是 CPU，上下文窗口是 RAM，Agent Harness 是操作系统" |

### 为什么需要 Agent Harness?

LLM 的核心限制：

1. **无状态性**: 每次新会话都是"盲人"
2. **上下文窗口有限**: 即使100万token也会填满
3. **工具调用幻觉**: 可能调用不存在的API
4. **状态丢失**: 网络中断或重启后进度归零
5. **Context Rot**: 长上下文中重要信息被淹没

---

## 核心组件

### 1. 工具集成层 (Tool Integration Layer)

- 文件读写、代码执行沙箱、数据库查询、API 调用、Web 访问
- 模型永不直接接触外部系统
- 所有调用都经过验证

### 2. 内存与状态管理 (Memory & State Management)

三层内存架构：
- **长期记忆**: 向量存储、结构化文件( JSON > Markdown)、知识图谱
- **会话状态**: 工具结果日志、已完成子任务、进度笔记
- **工作上下文**: 即时提示词、当前推理步骤

### 3. 上下文工程与压缩 (Context Engineering)

- 压缩旧历史
- RAG 检索：仅拉取相关文档
- 应用 "Lost in the Middle" 研究结论：关键信息放在提示词边界

### 4. 验证与护栏 (Verification & Guardrails)

- 编码 Agent：运行测试套件
- 敏感操作：人机回环

---

## 三大架构模式

### 模式1: 单 Agent 监督者 (Single-Agent Supervisor)

适用场景: 有界任务，如客服 Agent、知识库问答

### 模式2: 初始化器-执行器分离 (Initializer-Executor Split)

Anthropic 的长代码任务方案：
- 初始化器：设置持久项目环境、创建文件夹结构、生成功能列表
- 执行器：增量实现、运行测试、Git 提交

### 模式3: 多 Agent 协调 (Multi-Agent Coordination)

- 协调器 Agent + 研究/写作/审核 Agent
- 上下文过滤、历史清理、状态传递

---

## 三层防御架构

```
LAYER 1: 架构约束
- 严格的领域分层
- 依赖方向限制
- 通过 linter 和结构测试强制执行

LAYER 2: 知识系统
- AGENTS.md 项目级指令
- 渐进式上下文披露
- 失败模式库

LAYER 3: 工具验证
- 机械验证工具
- 人机回环中断
- 自动测试执行
```

---

## 核心原则

### 原则1: 系统性修复而非重试

```python
# ❌ 错误：提示词重试
for attempt in range(3):
    result = llm.generate(prompt)
    if is_valid(result):
        return result
    prompt += "\nPlease try again."

# ✅ 正确：工程化修复
class ValidatedTool:
    def execute(self, params):
        self.validate_schema(params)
        result = self.run(params)
        self.verify_output(result)
        return result
```

### 原则2: AGENTS.md 知识积累

```markdown
# AGENTS.md - 项目级 Agent 指令

## 已知失败模式
### F-001: 忘记运行测试
- 修复: 每次提交前必须运行测试
- 工具: pre-commit hook 强制执行
```

### 原则3: 机械可验证性

构建工具使正确行为可验证：截图验证、API 响应验证

---

## Harness Engineer

### 角色定义

Harness Engineer 是构建使 AI Agent 在生产环境可靠运行的基础设施层工程师。

**核心公式**:
```
Agent = LLM Model + Harness
Harness = 工具 + 知识 + 观察 + 行动接口 + 权限
```

### 核心职责

1. **实现工具**: 文件读写、Shell执行、API调用、浏览器控制
2. **策划知识**: 产品文档、架构决策记录、风格指南
3. **管理上下文**: 子Agent隔离、上下文压缩、任务持久化
4. **控制权限**: 沙箱访问、破坏性操作审批
5. **收集过程数据**: 感知-推理-行动轨迹

### AI 工程角色金字塔

```
层4: 架构/战略层
├── AI Architect (AI架构师)
├── AI Product Manager (AI产品经理)
└── Head of AI (AI负责人)

层3: 系统/平台层
├── Harness Engineer ⭐
├── Platform Engineer (AI)
└── MLOps Engineer

层2: 应用/实现层
├── AI Engineer
├── AI Agent Engineer
└── LLM Engineer

层1: 交互/优化层
├── Context Engineer
└── Prompt Engineer
```

### 薪资基准 (2026 美国)

| 经验水平 | 薪资范围 |
|----------|----------|
| Junior (0-2年) | $120K - $160K |
| Mid-level (2-5年) | $160K - $220K |
| Senior (5+年) | $220K - $300K+ |
| Lead/Architect | $280K - $400K+ |

### 6个月学习路线图

| 月份 | 主题 | 里程碑 |
|------|------|--------|
| 1 | AI Agent 基础 | 构建使用工具完成多步骤任务的 Agent |
| 2 | Agent 设计模式 | 为任务选择正确模式并解释 |
| 3 | 验证与测试 | 构建自动化评估管道 |
| 4 | 生产基础设施 | 拥有完整 Harness 的生产 Agent |
| 5 | 高级模式 | 多 Agent 系统设计与操作 |
| 6 | 作品集与求职 | 展示技能的作品集 |

---

## 行业案例

### OpenAI

**成果**: 3名工程师 + 5个月 = 100万行代码，3.5 PR/工程师/天

**三大支柱**:
1. 上下文工程：仓库作为单一真相源
2. 架构约束：刚性分层架构 + 自定义 linter
3. 熵管理：背景 Agent 持续扫描

### Stripe Minions

**规模**: 每周 1,300 个 AI 生成的 PR

**架构特点**:
- 窄范围任务：每个 Minion 处理独立任务
- 并行执行：数百个 Minion 同时运行
- 人机回环：PR 仍需人工审查

### Anthropic Claude Code

**设计理念**:
- 更少的工具胜过更多的工具
- 渐进式披露优于前期加载
- Harness 必须随模型进化

---

## 评估指标

| 指标 | 描述 | 目标值 |
|------|------|--------|
| Merge Rate | AI PR 被合并的比例 | >80% |
| Review Cycle Time | AI PR 审查时间 vs 人工 PR | <1.5x |
| Test Pass Rate | 首次测试通过率 | >70% |
| Revert Rate | 合并后被回退的比例 | <5% |
| Session Continuity | 跨会话任务成功率 | >90% |

---

## 三代范式演进

```
2022-2024: Prompt Engineering 时代
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
焦点: 精心构造单次指令
问题: "我们告诉它什么?"

2025: Context Engineering 时代
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
焦点: 为每个决策点动态构建上下文
问题: "模型应该知道什么?"

2026+: Harness Engineering 时代
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
焦点: 设计完整的控制系统
问题: "我们如何让错误不可能发生?"
```

### 关系总结

```
PROMPT ENGINEERING
  └── 子集 of CONTEXT ENGINEERING
      └── 子集 of AGENT HARNESS

层次关系：
- Prompt: 说什么
- Context: 给什么信息
- Harness: 在什么环境中运行
```

---

## 相关文档

详细内容请参阅：

- [agent-harness-deep-analysis-report.md](./agent-harness-deep-analysis-report.md) - 深度技术分析
- [harness-engineer-career-analysis.md](./harness-engineer-career-analysis.md) - 职业发展分析
- [visual-summary.md](./visual-summary.md) - 可视化总结

---

## 参考资源

### 开创性文献

1. Mitchell Hashimoto - "Harness Engineering" (2026年2月)
2. OpenAI - "Harness Engineering: Leveraging Codex in an Agent-First World"
3. Anthropic - "Building Effective Agents"

### 相关 Awesome Lists

- [awesome-agent-harness](https://github.com/AutoJunjie/awesome-agent-harness)
- [awesome-cli-coding-agents](https://github.com/bradAGI/awesome-cli-coding-agents)
- [awesome-context-engineering](https://github.com/yzfly/awesome-context-engineering)

---

*最后更新: 2026年3月*
