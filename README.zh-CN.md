# Agent Harness 全面技术分析报告

> **研究日期**: 2026年3月
> **核心主题**: Agent Harness 技术架构、实现模式及其与 Prompt Engineering、Context Engineering 的关系

---

## 执行摘要

**Agent Harness** 是2025-2026年 AI 工程领域最具影响力的概念之一，由 Mitchell Hashimoto（Terraform 创始人）正式提出并命名。它代表了从"提示工程"向"系统工程"转变的重要里程碑。

### 关键洞察

| 维度 | 传统方法 | Agent Harness 方法 |
|------|---------|-------------------|
| **焦点** | 优化单次 LLM 调用 | 构建完整运行时环境 |
| **失败处理** | 重试提示词 | 工程化修复系统问题 |
| **状态管理** | 无状态/单次会话 | 跨会话持久化 |
| **模型依赖** | 紧密耦合 | 可插拔架构 |

**OpenAI 实证数据**: 3名工程师使用 Harness Engineering，在5个月内产出约1500个PR，平均每人每天3.5个PR，代码库规模达100万行，零人工编写代码。

---

## 第一部分: Agent Harness 核心定义

### 1.1 什么是 Agent Harness?

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

**权威定义对比**:

| 来源 | 定义 |
|------|------|
| **Mitchell Hashimoto** | "将 Agent 能力'约束'和'引导'到特定任务的软件基础设施" |
| **Anthony Alcaraz** (O'Reilly) | "管理上下文生命周期的完整架构系统：从意图捕获、规范、编译、执行、验证到持久化" |
| **Anthropic** | "一个强大的通用 Agent Harness，擅长编码和其他需要模型使用工具收集上下文、规划和执行的任务" |
| **Phil Schmid** | "模型是 CPU，上下文窗口是 RAM，Agent Harness 是操作系统" |

### 1.2 为什么需要 Agent Harness?

LLM 的核心限制：

1. **无状态性**: 每次新会话都是"盲人"
2. **上下文窗口有限**: 即使100万token也会填满
3. **工具调用幻觉**: 可能调用不存在的API
4. **状态丢失**: 网络中断或重启后进度归零
5. **Context Rot**: 长上下文中重要信息被淹没

---

## 第二部分: Agent Harness 技术架构

### 2.1 核心组件详解

#### 2.1.1 工具集成层

```python
class ToolIntegrationLayer:
    """
    定义 Agent 在世界中能做什么：
    - 文件读写
    - 代码执行沙箱
    - 数据库查询
    - API 调用
    - Web 访问
    """

    def execute_tool(self, tool_call: ToolCall) -> ToolResult:
        # 1. 验证调用参数
        self.validate_parameters(tool_call)

        # 2. 在沙箱中执行
        with Sandbox() as sandbox:
            result = sandbox.run(tool_call)

        # 3. 清理和格式化输出
        cleaned = self.sanitize_output(result)

        # 4. 注入回上下文
        return ToolResult(cleaned)
```

**关键设计原则**:
- 模型永不直接接触外部系统
- 所有调用都经过验证
- 输出被清理和结构化
- 错误可被重试

#### 2.1.2 内存与状态管理

三层内存架构：

```
┌─────────────────────────────────────────────────────────┐
│  LONG-TERM MEMORY (长期记忆)                              │
│  - 向量存储                                               │
│  - 结构化文件 (JSON > Markdown)                          │
│  - 知识图谱                                               │
│  - 跨任务持久化                                           │
└─────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────┐
│  SESSION STATE (会话状态)                                 │
│  - 工具结果日志                                           │
│  - 已完成子任务                                           │
│  - 进度笔记                                               │
│  - 当前任务持续期间                                       │
└─────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────┐
│  WORKING CONTEXT (工作上下文)                             │
│  - 即时提示词                                             │
│  - 当前推理步骤                                           │
│  - 易失性                                                 │
└─────────────────────────────────────────────────────────┘
```

**Anthropic 发现**: JSON 比 Markdown 更适合功能跟踪文件，因为模型不太可能意外覆盖或重新格式化 JSON。

#### 2.1.3 上下文工程与压缩

```python
class ContextManager:
    """
    在每次模型调用时决定：包含什么、压缩什么
    """

    def prepare_context(self, task: Task, history: History) -> Context:
        # 1. 压缩旧历史
        if history.token_count > THRESHOLD:
            history = self.compact(history)

        # 2. RAG检索：仅拉取相关文档
        relevant_docs = self.rag_retrieve(task.query)

        # 3. 应用 "Lost in the Middle" 研究结论
        context = Context(
            system_prompt=self.get_system_prompt(),  # 开头
            retrieved_docs=relevant_docs,            # 中间（可压缩）
            user_query=task.query,                   # 结尾（最近）
        )

        return context
```

**Stanford "Lost in the Middle" 研究发现**:
- 关键内容 buried 在长提示词中间时，性能显著下降
- 模型对开头和结尾内容注意力最强
- 应用到 Harness 设计：静态决策上下文放开头，动态操作上下文放结尾

#### 2.1.4 验证与护栏

```python
class VerificationLayer:
    """
    生产级 Harness 在将工作标记为完成前验证输出
    """

    def verify_completion(self, work: Work) -> VerificationResult:
        # 编码 Agent：运行测试套件
        if work.type == "code":
            test_results = run_test_suite(work)
            if not test_results.passed:
                return VerificationResult(
                    passed=False,
                    feedback=test_results.errors
                )

        # 敏感操作：人机回环
        if work.risk_level > HIGH:
            return self.request_human_approval(work)

        return VerificationResult(passed=True)
```

### 2.2 架构模式

#### 模式1: 单 Agent 监督者

**适用场景**: 有界任务，如客服 Agent、知识库问答

#### 模式2: 初始化器-执行器分离

**Anthropic 的长代码任务方案**:
- 初始化器：设置持久项目环境、创建文件夹结构、生成功能列表
- 执行器：增量实现、运行测试、Git 提交

#### 模式3: 多 Agent 协调

- 协调器 Agent + 研究/写作/审核 Agent
- 上下文过滤、历史清理、状态传递

---

## 第三部分: Agent Harness vs Prompt Engineering vs Context Engineering

### 3.1 三者关系

```
┌─────────────────────────────────────────────────────────────────────┐
│                        AGENT HARNESS                                │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                  CONTEXT ENGINEERING                         │   │
│  │  ┌───────────────────────────────────────────────────────┐  │   │
│  │  │              PROMPT ENGINEERING                        │  │   │
│  │  │  "You are an expert X. Do Y like Z."                   │  │   │
│  │  └───────────────────────────────────────────────────────┘  │   │
│  └─────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

### 3.2 详细对比

#### Prompt Engineering (提示工程)

| 维度 | 描述 |
|------|------|
| **焦点** | 编写清晰的单次任务指令 |
| **范围** | 单输入-输出对 |
| **思维方式** | "我该说什么？" |
| **适用场景** | 一次性任务、演示原型 |
| **调试方法** | 重写和猜测 |

#### Context Engineering (上下文工程)

| 维度 | 描述 |
|------|------|
| **焦点** | 策划整个信息环境 |
| **范围** | 模型看到的一切 |
| **思维方式** | "模型应该知道什么？" |
| **适用场景** | 生产系统、多用户应用 |

**关键洞察** (Andrej Karpathy):
> "在每个工业级 LLM 应用中，上下文工程是将正确的信息以正确的格式填充到上下文窗口的精妙艺术和科学。"

#### Harness Engineering (Harness 工程)

| 维度 | 描述 |
|------|------|
| **焦点** | 控制 Agent 运行的环境 |
| **范围** | 整个系统基础设施 |
| **思维方式** | "如何让错误不可能发生？" |
| **适用场景** | 企业级长期运行的 Agent |

### 3.3 关系总结

```
PROMPT ENGINEERING = CONTEXT ENGINEERING 的子集
                     = AGENT HARNESS 的子集

层次关系：
- Prompt: 说什么
- Context: 给什么信息
- Harness: 在什么环境中运行
```

**类比**:
- **Prompt Engineering** = 给实习生一份详细的待办清单
- **Context Engineering** = 提供完整的项目资料、历史记录和工具
- **Harness Engineering** = 建立公司制度、检查流程和防错机制

---

## 第四部分: Harness Engineering 实践指南

### 4.1 核心原则

#### 原则1: 系统性修复而非重试

```python
# 错误：提示词重试
for attempt in range(3):
    result = llm.generate(prompt)
    if is_valid(result):
        return result
    prompt += "\nPlease try again."

# 正确：工程化修复
class ValidatedTool:
    def execute(self, params):
        self.validate_schema(params)
        result = self.run(params)
        self.verify_output(result)
        return result
```

#### 原则2: AGENTS.md 知识积累

```markdown
# AGENTS.md - 项目级 Agent 指令

## 架构规则
- 每个领域分为固定层：Types → Config → Repo → Service → Runtime → UI
- 依赖方向严格验证
- 超出架构的代码无法提交

## 已知失败模式

### F-001: 忘记运行测试
- 症状: Agent 宣称功能完成但未验证
- 修复: 每次提交前必须运行测试
```

#### 原则3: 机械可验证性

```python
class ScreenshotVerifier:
    """截图验证工具"""
    def capture_and_verify(self, selector):
        screenshot = browser.screenshot(selector)
        analysis = vision_model.analyze(screenshot)
        return VerificationResult(passed=analysis.is_correct)
```

### 4.2 三层防御架构

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

### 4.3 OpenAI 实践案例

**项目规模**: 100万行代码，3.5 PR/工程师/天

**三大支柱**:

1. **上下文工程**: 仓库作为单一真相源
2. **架构约束**: 刚性分层架构 + 自定义 linter
3. **熵管理**: 背景 Agent 持续扫描

---

## 第五部分: 行业案例

### 5.1 Stripe Minions 系统

**规模**: 每周1,300个 AI 生成的 PR

**关键设计决策**:
- **窄范围任务**: 每个 Minion 处理独立任务
- **并行执行**: 数百个 Minion 同时运行
- **人机回环**: PR 仍需人工审查

### 5.2 Anthropic Claude Code

**设计理念**:
- 更少的工具胜过更多的工具
- 渐进式披露优于前期加载
- Harness 必须随模型进化

### 5.3 GitHub Copilot Workspace

**演进**:
- 原始 Copilot: 自动完成工具
- Copilot Workspace: 完整 Agent Harness

---

## 第六部分: 工具生态

### 6.1 主流 Harness 实现

| 类型 | 工具 |
|------|------|
| 编码专用 | Claude Code, Codex CLI, GitHub Copilot Workspace, Cursor Agent, SWE-agent |
| 通用/可配置 | LangChain DeepAgents, LlamaIndex Agent, AutoGPT |
| 多 Agent 编排 | Vibe Kanban, Emdash, Desplega Agent Swarm |

### 6.2 评估指标

| 指标 | 描述 | 目标值 |
|------|------|--------|
| **Merge Rate** | AI PR 被合并的比例 | >80% |
| **Review Cycle Time** | AI PR 审查时间 vs 人工 PR | <1.5x |
| **Test Pass Rate** | 首次测试通过率 | >70% |
| **Revert Rate** | 合并后被回退的比例 | <5% |
| **Session Continuity** | 跨会话任务成功率 | >90% |

---

## 第七部分: 职业分析

### 7.1 角色定义

**核心公式**:
```
Agent = LLM Model + Harness
         (大脑)      (身体+环境)

Harness = 工具 + 知识 + 观察 + 行动接口 + 权限
```

### 7.2 核心职责

```
1. 实现工具
   - 文件读写、Shell执行、API调用、浏览器控制

2. 策划知识
   - 产品文档、架构决策记录、风格指南

3. 管理上下文
   - 子Agent隔离、上下文压缩、任务持久化

4. 控制权限
   - 沙箱访问、破坏性操作审批

5. 收集过程数据
   - 感知-推理-行动轨迹
```

### 7.3 AI工程角色金字塔

```
层4: 架构/战略层
├── AI Architect (AI架构师)
├── AI Product Manager (AI产品经理)
└── Head of AI (AI负责人)

层3: 系统/平台层
├── Harness Engineer ⭐
├── Platform Engineer (AI平台工程师)
└── MLOps Engineer

层2: 应用/实现层
├── AI Engineer
├── AI Agent Engineer
└── LLM Engineer

层1: 交互/优化层
├── Context Engineer
└── Prompt Engineer
```

### 7.4 薪资基准 (2026)

| 经验水平 | 薪资范围 |
|----------|----------|
| **Junior (0-2年)** | $120K - $160K |
| **Mid-level (2-5年)** | $160K - $220K |
| **Senior (5+年)** | $220K - $300K+ |
| **Lead/Architect** | $280K - $400K+ |

### 7.5 6个月学习路线图

| 月份 | 主题 | 里程碑 |
|------|------|--------|
| 1 | AI Agent 基础 | 构建使用工具完成多步骤任务的 Agent |
| 2 | Agent 设计模式 | 为任务选择正确模式并解释 |
| 3 | 验证与测试 | 构建自动化评估管道 |
| 4 | 生产基础设施 | 拥有完整 Harness 的生产 Agent |
| 5 | 高级模式 | 多 Agent 系统设计与操作 |
| 6 | 作品集与求职 | 展示技能的作品集 |

---

## 附录: 术语表

| 术语 | 定义 |
|------|------|
| **Agent Harness** | 围绕 LLM 的软件基础设施 |
| **Harness Engineering** | 将每次 Agent 失败视为工程问题永久修复 |
| **Context Engineering** | 设计动态系统提供正确信息 |
| **Context Rot** | 上下文增加导致性能下降 |
| **Mechanical Verification** | 通过工具使正确行为可自动验证 |
| **Progressive Disclosure** | 渐进式披露上下文 |

---

## 参考资源

### 开创性文献

1. Mitchell Hashimoto - "Harness Engineering" (2026年2月)
2. OpenAI - "Harness Engineering: Leveraging Codex in an Agent-First World"
3. Anthropic - "Building Effective Agents" (2025)
4. ICML 2025 - "General Modular Harness for LLM Agents"
5. Stanford - "Lost in the Middle" (2023)

### 相关 Awesome Lists

- [awesome-agent-harness](https://github.com/AutoJunjie/awesome-agent-harness)
- [awesome-cli-coding-agents](https://github.com/bradAGI/awesome-cli-coding-agents)
- [awesome-context-engineering](https://github.com/yzfly/awesome-context-engineering)

---

*报告完成时间: 2026年3月*
*本报告基于公开技术文献、研究论文和行业实践案例整理*
