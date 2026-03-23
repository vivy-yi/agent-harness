# Awesome Agent Harness Ecosystem

> 一个以角色为中心的 AI Agent 工程生态系统资源列表，涵盖从入门到专家的完整学习路径和职业发展指南。

---

## 核心洞察

**为什么需要以角色为中心的视角？**

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      AI Agent 工程角色金字塔                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│   战略层 (AI Leader)                                                    │
│   ├── AI Architect        - 系统架构与基础设施设计                        │
│   ├── AI Product Manager  - 产品规划与需求管理                           │
│   └── Head of AI          - 团队建设与技术战略                           │
│                                                                         │
│   平台层 (Harness Engineering)                                          │
│   ├── Harness Engineer ⭐  - Agent 运行环境构建 (本项目核心)              │
│   ├── Platform Engineer    - AI 平台基础设施                             │
│   └── MLOps Engineer      - ML 流水线与部署                             │
│                                                                         │
│   应用层 (AI Application)                                                │
│   ├── AI Engineer         - AI 应用开发                                   │
│   ├── AI Agent Engineer   - Agent 系统开发                               │
│   └── LLM Engineer       - LLM 集成与优化                               │
│                                                                         │
│   优化层 (Prompt & Context)                                              │
│   ├── Context Engineer    - 上下文工程                                   │
│   └── Prompt Engineer    - 提示工程                                     │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 目录

- [核心概念](#核心概念)
- [角色定义与职责](#角色定义与职责)
- [学习路径](#学习路径)
- [工具与框架生态](#工具与框架生态)
- [最佳实践](#最佳实践)
- [行业案例](#行业案例)
- [职业发展](#职业发展)
- [贡献指南](#贡献指南)

---

## 核心概念

### 什么是 Agent Harness?

**权威定义对比**:

| 来源 | 定义 |
|------|------|
| **Mitchell Hashimoto** | "将 Agent 能力'约束'和'引导'到特定任务的软件基础设施" |
| **Anthropic** | "管理上下文生命周期的完整架构系统：从意图捕获、规范、编译、执行、验证到持久化" |
| **Phil Schmid** | "模型是 CPU，上下文窗口是 RAM，Agent Harness 是操作系统" |

### 三大范式演进

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

---

## 角色定义与职责

### 11 个核心角色详解

本项目定义了 11 个相互关联的角色，每个角色都有其独特的技能要求和发展路径：

#### 1. Harness Engineer (Harness 工程师)

> **核心职责**: 构建使 AI Agent 在生产环境可靠运行的基础设施层

```
Harness = 工具 + 知识 + 观察 + 行动接口 + 权限

主要工作:
├── 实现工具 (文件读写、Shell执行、API调用)
├── 策划知识 (产品文档、架构决策、风格指南)
├── 管理上下文 (子Agent隔离、上下文压缩、持久化)
├── 控制权限 (沙箱访问、破坏性操作审批)
└── 收集过程数据 (感知-推理-行动轨迹)
```

**相关资源**:
- [harness-engineer-career-analysis.md](./harness-engineer-career-analysis.md)
- [roles/for-developers/harness-engineer.md](./roles/for-developers/harness-engineer.md)

#### 2. Context Engineer (上下文工程师)

> **核心职责**: 设计动态系统，在正确时间以正确格式提供正确信息给 LLM

**核心技能**:
- RAG (检索增强生成)
- 内存管理 (短期/长期/工作记忆)
- 上下文压缩与优先排序
- "Lost in the Middle" 研究应用

#### 3. Prompt Engineer (提示工程师)

> **核心职责**: 编写清晰的单次任务指令

**核心技能**:
- 少样本学习 (Few-shot Learning)
- 思维链 (Chain-of-Thought)
- 结构化输出提示
- 角色定义与风格控制

#### 4. AI Engineer (AI 工程师)

> **核心职责**: 构建 AI 应用功能

**核心技能**:
- API 集成
- RAG 系统实现
- 应用架构设计
- LangChain/LlamaIndex 熟练使用

#### 5. AI Agent Engineer (AI Agent 工程师)

> **核心职责**: 构建使用工具、规划 和自我反思的 Agent 系统

**核心技能**:
- Agent 设计模式 (ReAct, Plan-and-Execute, Tool Use)
- 多步骤任务编排
- Agent 评估与测试

#### 6. LLM Engineer (LLM 工程师)

> **核心职责**: LLM 集成、评估与优化

**核心技能**:
- 模型 API 集成
- Prompt 优化
- 输出质量评估
- 成本优化

#### 7. Platform Engineer - AI (AI 平台工程师)

> **核心职责**: 构建和维护 AI 基础设施平台

**核心技能**:
- Kubernetes & Docker
- ML 流水线设计
- API 网关与服务网格
- 成本监控系统

#### 8. MLOps Engineer (MLOps 工程师)

> **核心职责**: 管理 ML 模型生命周期

**核心技能**:
- 模型训练流水线
- 模型监控与再训练
- A/B 测试框架
- 数据质量管理

#### 9. AI Architect (AI 架构师)

> **核心职责**: 设计企业级 AI 系统架构

**核心技能**:
- 系统设计
- 技术选型
- 可扩展性规划
- 安全与合规

#### 10. AI Product Manager (AI 产品经理)

> **核心职责**: 定义 AI 产品需求和路线图

**核心技能**:
- AI 能力理解
- 用户需求分析
- 优先级排序
- 跨团队协调

#### 11. Head of AI (AI 负责人)

> **核心职责**: 制定 AI 技术战略和团队建设

**核心技能**:
- 技术战略制定
- 团队建设
- 投资回报分析
- 行业趋势洞察

---

## 学习路径

### 按角色分类的学习资源

#### For Learners (入门学习者)

```
目标: 理解基础概念，建立全局视图

推荐学习顺序:
1. Prompt Engineering 基础
   └── Prompt Engineering Guide

2. LLM 工作原理
   └── DeepLearning.AI - LangChain

3. Agent 概念
   └── Anthropic - Building Effective Agents

4. Context Engineering
   └── RAG 系统实战

5. Harness Engineering 入门
   └── OpenAI - Harness Engineering
```

#### For Developers (实践开发者)

```
目标: 掌握实际技能，能够构建生产级系统

核心技能栈:
├── LangChain / LangGraph
├── Claude Agent SDK / OpenAI SDK
├── 向量数据库 (Pinecone, Weaviate, Chroma)
├── MCP (Model Context Protocol)
├── Docker & Kubernetes
└── 监控 (LangSmith, Weights & Biases)
```

#### For Architects (架构师)

```
关注重点:
├── Agent 系统架构模式
├── 多 Agent 编排策略
├── 安全性与权限控制
├── 可观测性设计
└── 成本优化策略
```

#### For PMs (产品经理)

```
关注重点:
├── AI 能力边界理解
├── Prompt Engineering 基础
├── Agent 应用场景识别
├── 需求优先级框架
└── 质量评估方法
```

---

## 工具与框架生态

### 按层级分类

#### 工具层 (Tool Layer)

| 工具 | 用途 | 适用角色 |
|------|------|----------|
| Claude Code | 编码 Agent | All |
| Codex CLI | 编码 Agent | All |
| OpenCode | 可扩展编码 Agent | Harness Engineer |
| Gemini CLI | 编码 Agent | All |

#### 框架层 (Framework Layer)

| 框架 | 特点 | 适用角色 |
|------|------|----------|
| LangChain | 全栈框架 | AI Engineer |
| LangGraph | 状态机编排 | AI Agent Engineer |
| CrewAI | 多 Agent 团队 | AI Agent Engineer |
| AutoGen | 多 Agent 对话 | AI Agent Engineer |
| LlamaIndex | 数据索引 | Context Engineer |

#### 基础设施层 (Infrastructure Layer)

| 工具 | 用途 | 适用角色 |
|------|------|----------|
| Docker/K8s | 容器化部署 | Platform Engineer |
| Pinecone | 向量存储 | Context Engineer |
| LangSmith | 监控调试 | Harness Engineer |
| Weights & Biases | 实验跟踪 | MLOps Engineer |

#### 协议与标准

| 标准 | 描述 |
|------|------|
| MCP (Model Context Protocol) | AI 模型与外部工具连接的标准 |
| agents.md | 项目级 Agent 配置标准 |
| AGENTS.md | OpenAI 的仓库级 Agent 指令规范 |

---

## 最佳实践

### Harness Engineering 核心原则

#### 1. 系统性修复而非重试

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
        self.validate_schema(params)  # 机械验证
        result = self.run(params)
        self.verify_output(result)    # 输出验证
        return result
```

#### 2. AGENTS.md 知识积累

```markdown
# AGENTS.md - 项目级 Agent 指令

## 架构规则
- 领域分层：Types → Config → Repo → Service → Runtime → UI
- 依赖方向严格验证

## 已知失败模式 (持续更新)
### F-001: 忘记运行测试
- 修复: 每次提交前必须运行测试
- 工具: pre-commit hook 强制执行
```

#### 3. 机械可验证性

```python
# 构建工具使正确行为可验证
class ScreenshotVerifier:
    """截图验证工具"""
    def capture_and_verify(self, selector):
        screenshot = browser.screenshot(selector)
        analysis = vision_model.analyze(screenshot)
        return VerificationResult(passed=analysis.is_correct)

class APIResponseValidator:
    """API 响应验证器"""
    def validate(self, response, schema):
        # 机械检查必填字段
        # 验证数据类型
        # 检查边界条件
        pass
```

### 三层防御架构

```
┌─────────────────────────────────────────┐
│ LAYER 1: 架构约束                        │
│ - 严格的领域分层                         │
│ - 依赖方向限制                          │
│ - 自定义 linter 强制执行                │
└─────────────────────────────────────────┘
                │
┌─────────────────────────────────────────┐
│ LAYER 2: 知识系统                        │
│ - AGENTS.md 项目级指令                  │
│ - 渐进式上下文披露                      │
│ - 失败模式库                            │
└─────────────────────────────────────────┘
                │
┌─────────────────────────────────────────┐
│ LAYER 3: 工具验证                        │
│ - 机械验证工具                          │
│ - 人机回环中断                          │
│ - 自动测试执行                          │
└─────────────────────────────────────────┘
```

---

## 行业案例

### OpenAI - Harness Engineering 实践

**成果**: 3 名工程师 + 5 个月 = 100 万行 AI 生成代码

**三大支柱**:
1. **上下文工程**: 仓库作为单一真相源
2. **架构约束**: 刚性分层架构 + 自定义 linter
3. **熵管理**: 背景 Agent 持续扫描并重构

### Stripe - Minions 系统

**规模**: 每周 1,300 个 AI 生成的 PR

**架构特点**:
- 窄范围任务: 每个 Minion 处理独立任务
- 并行执行: 数百个 Minion 同时运行
- 人机回环: PR 仍需人工审查
- 质量指标: 合并率、审查周期、测试通过率

### Anthropic - Claude Code

**设计理念**:
- 更少的工具胜过更多的工具
- 渐进式披露优于前期加载
- Harness 必须随模型进化

---

## 职业发展

### 薪资基准 (2026)

| 角色 | Junior | Mid | Senior | Lead/Architect |
|------|--------|-----|--------|-----------------|
| Harness Engineer | $120K-160K | $160K-220K | $220K-300K+ | $280K-400K+ |
| AI Engineer | $100K-150K | $150K-200K | $200K-280K | $250K-350K+ |
| MLOps Engineer | $110K-150K | $150K-200K | $200K-270K | $250K-350K+ |
| Prompt Engineer | $80K-130K | $130K-180K | $180K-250K | N/A |

### 转型路径

```
后端/平台工程师 ──────────────▶ Harness Engineer
(3-6个月)
转移: 系统设计、API架构、可观测性
需学习: LLM行为、Agent架构

DevOps/MLOps ──────────────▶ Harness Engineer
(3-6个月)
转移: CI/CD、监控、基础设施自动化
需学习: Agent特定模式、验证循环

Prompt Engineer ──────────────▶ Harness Engineer
(6-12个月)
转移: LLM行为理解、指令设计
需学习: 软件工程、基础设施技能
```

### 6 个月学习路线图

| 月份 | 主题 | 里程碑 |
|------|------|--------|
| 1 | AI Agent 基础 | 构建使用工具完成多步骤任务的 Agent |
| 2 | Agent 设计模式 | 为任务选择正确模式并解释 |
| 3 | 验证与测试 | 构建自动化评估管道 |
| 4 | 生产基础设施 | 拥有完整 Harness 的生产 Agent |
| 5 | 高级模式 | 多 Agent 系统设计与操作 |
| 6 | 作品集与求职 | 展示技能的作品集 |

---

## 参考资源

### 开创性文献

1. **OpenAI** - [Harness Engineering: Leveraging Codex in an Agent-First World](https://openai.com/index/harness-engineering/)
2. **Anthropic** - [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)
3. **Mitchell Hashimoto** - Harness Engineering 概念提出

### 相关 Awesome Lists

- [awesome-agent-harness](https://github.com/AutoJunjie/awesome-agent-harness) - 工具与框架精选
- [awesome-cli-coding-agents](https://github.com/bradAGI/awesome-cli-coding-agents) - CLI 编码 Agent
- [awesome-context-engineering](https://github.com/yzfly/awesome-context-engineering) - 上下文工程
- [awesome-agentic-engineering](https://github.com/jordimas/awesome-agentic-engineering) - Agentic 工程实践

---

## 贡献指南

欢迎贡献！请遵循以下原则：

1. **按角色分类**: 资源应归类到对应的角色
2. **保持更新**: 定期更新薪资、工具版本等信息
3. **中文优先**: 优先收录中文资源
4. **质量优先**: 只收录经过验证的高质量资源

---

*最后更新: 2026年3月*
*本项目基于公开技术文献和行业实践整理*
