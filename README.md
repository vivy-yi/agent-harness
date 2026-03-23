# Agent Harness

> Agent Harness 技术研究集合，包含深度分析、职业发展和可视化总结。

---

## 项目概述

**Agent Harness** 是2025-2026年 AI 工程领域最具影响力的概念之一，由 Mitchell Hashimoto（Terraform 创始人）正式提出并命名。它代表了从"提示工程"向"系统工程"转变的重要里程碑。

### 核心定义

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
| **Anthropic** | "管理上下文生命周期的完整架构系统：从意图捕获、规范、编译、执行、验证到持久化" |
| **Phil Schmid** | "模型是 CPU，上下文窗口是 RAM，Agent Harness 是操作系统" |

---

## 核心内容

### 1. 技术深度分析

详细内容见 [agent-harness-deep-analysis-report.md](./agent-harness-deep-analysis-report.md)

- **定义与原理**: Agent Harness 核心概念、技术架构
- **三大范式演进**: Prompt Engineering → Context Engineering → Harness Engineering
- **技术架构**: 工具层、内存管理、上下文工程、验证与护栏
- **架构模式**: 单 Agent 监督者、初始化器-执行器分离、多 Agent 协调
- **行业案例**: OpenAI、Stripe、Anthropic 实践

### 2. 职业分析

详细内容见 [harness-engineer-career-analysis.md](./harness-engineer-career-analysis.md)

- **角色定义**: Harness Engineer 核心职责与技能要求
- **薪资基准** (2026):
  - Junior: $120K-160K
  - Mid: $160K-220K
  - Senior: $220K-300K+
- **6个月学习路线图**: 从入门到生产级系统
- **转型路径**: 后端/平台工程师、DevOps、Prompt Engineer 如何转型

### 3. 可视化总结

详细内容见 [visual-summary.md](./visual-summary.md)

- 概念层次结构图
- 核心组件架构
- 工作流程图解
- 关键概念速查

---

## 关键洞察

| 维度 | 传统方法 | Agent Harness 方法 |
|------|---------|-------------------|
| **焦点** | 优化单次 LLM 调用 | 构建完整运行时环境 |
| **失败处理** | 重试提示词 | 工程化修复系统问题 |
| **状态管理** | 无状态/单次会话 | 跨会话持久化 |
| **模型依赖** | 紧密耦合 | 可插拔架构 |

### OpenAI 实证数据

> 3名工程师使用 Harness Engineering，在5个月内产出约1500个PR，平均每人每天3.5个PR，代码库规模达100万行，零人工编写代码。

---

## 为什么需要 Agent Harness?

LLM 的核心限制：

1. **无状态性**: 每次新会话都是"盲人"
2. **上下文窗口有限**: 即使100万token也会填满
3. **工具调用幻觉**: 可能调用不存在的API
4. **状态丢失**: 网络中断或重启后进度归零
5. **Context Rot**: 长上下文中重要信息被淹没

---

## 三层防御架构

```
┌─────────────────────────────────────────────────────────────┐
│  LAYER 1: 架构约束 (Architecture Constraints)                │
│  - 严格的领域分层                                            │
│  - 依赖方向限制                                              │
│  - 通过 linter 和结构测试强制执行                            │
└─────────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────────┐
│  LAYER 2: 知识系统 (Knowledge System)                        │
│  - AGENTS.md 项目级指令                                      │
│  - 渐进式上下文披露                                          │
│  - 失败模式库                                                │
└─────────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────────┐
│  LAYER 3: 工具验证 (Tool Verification)                       │
│  - 机械验证工具                                              │
│  - 人机回环中断                                              │
│  - 自动测试执行                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 参考资源

### 开创性文献

1. [OpenAI] - Harness Engineering: Leveraging Codex in an Agent-First World
2. [Anthropic] - Building Effective Agents
3. Mitchell Hashimoto - Harness Engineering 概念提出

### 相关 Awesome Lists

- [awesome-agent-harness](https://github.com/AutoJunjie/awesome-agent-harness)
- [awesome-cli-coding-agents](https://github.com/bradAGI/awesome-cli-coding-agents)
- [awesome-context-engineering](https://github.com/yzfly/awesome-context-engineering)

---

*最后更新: 2026年3月*
