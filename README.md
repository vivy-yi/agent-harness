# Agent Harness 全面技术分析报告 | Comprehensive Technical Analysis

> **研究日期**: 2026年3月 | **Research Date**: March 2026
> **核心主题**: Agent Harness 技术架构、实现模式及其与 Prompt Engineering、Context Engineering 的关系
> **Core Topic**: Agent Harness architecture, implementation patterns, and its relationship with Prompt Engineering and Context Engineering

---

## 执行摘要 | Executive Summary

**Agent Harness** 是2025-2026年 AI 工程领域最具影响力的概念之一，由 Mitchell Hashimoto（Terraform 创始人）正式提出并命名。它代表了从"提示工程"向"系统工程"转变的重要里程碑。

**Agent Harness** is one of the most influential concepts in the AI engineering field from 2025-2026, formally proposed and named by Mitchell Hashimoto (Terraform founder). It represents a significant shift from "prompt engineering" to "systems engineering."

### 关键洞察 | Key Insights

| 维度 | 传统方法 | Agent Harness 方法 |
|------|---------|-------------------|
| **焦点** | 优化单次 LLM 调用 | 构建完整运行时环境 |
| **Focus** | Optimize single LLM calls | Build complete runtime environment |
| **失败处理** | 重试提示词 | 工程化修复系统问题 |
| **Failure Handling** | Retry prompts | Engineering fix for system issues |
| **状态管理** | 无状态/单次会话 | 跨会话持久化 |
| **State Management** | Stateless/single session | Cross-session persistence |
| **模型依赖** | 紧密耦合 | 可插拔架构 |
| **Model Dependency** | Tightly coupled | Pluggable architecture |

**OpenAI 实证数据**: 3名工程师使用 Harness Engineering，在5个月内产出约1500个PR，平均每人每天3.5个PR，代码库规模达100万行，零人工编写代码。

**OpenAI Data**: 3 engineers using Harness Engineering produced ~1500 PRs in 5 months, averaging 3.5 PRs/engineer/day, codebase of 1 million lines, zero human-written code.

---

## 第一部分: Agent Harness 核心定义 | Part 1: Agent Harness Core Definition

### 1.1 什么是 Agent Harness? | What is Agent Harness?

Agent Harness 是围绕 LLM 的软件基础设施层，负责管理除模型推理本身之外的一切：

Agent Harness is the software infrastructure layer surrounding LLMs, responsible for managing everything except model inference itself:

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
                    │  (CPU核心)  │  ← Inference only
                    └─────────────┘
```

**权威定义对比 | Authoritative Definitions**:

| 来源 | 定义 | Definition |
|------|------|------------|
| **Mitchell Hashimoto** | "Agent Harness 是将 Agent 能力'约束'和'引导'到特定任务的软件基础设施" | "Software infrastructure that constrains and guides Agent capabilities to specific tasks" |
| **Anthony Alcaraz** (O'Reilly) | "管理上下文生命周期的完整架构系统：从意图捕获、规范、编译、执行、验证到持久化" | "Complete architectural system managing context lifecycle: intent capture, specification, compilation, execution, verification, persistence" |
| **Anthropic** | "一个强大的通用 Agent Harness，擅长编码和其他需要模型使用工具收集上下文、规划和执行的任务" | "A powerful general-purpose Agent Harness excelling at coding and tasks requiring models to use tools to gather context, plan, and execute" |
| **Phil Schmid** | "模型是 CPU，上下文窗口是 RAM，Agent Harness 是操作系统" | "Models are CPU, context window is RAM, Agent Harness is the operating system" |

### 1.2 为什么需要 Agent Harness? | Why Agent Harness is Needed?

LLM 的核心限制 | Core LLM Limitations:

1. **无状态性**: 每次新会话都是"盲人" | **Statelessness**: Every new session is "blind"
2. **上下文窗口有限**: 即使100万token也会填满 | **Limited Context**: Even 1M tokens gets filled
3. **工具调用幻觉**: 可能调用不存在的API | **Tool Hallucination**: May call non-existent APIs
4. **状态丢失**: 网络中断或重启后进度归零 | **State Loss**: Progress resets after interruption
5. **Context Rot**: 长上下文中重要信息被淹没 | Important info gets drowned in long context

---

## 第二部分: Agent Harness 技术架构 | Part 2: Technical Architecture

### 2.1 核心组件详解 | Core Components

#### 2.1.1 工具集成层 | Tool Integration Layer

```python
# 典型工具层架构 | Typical tool layer architecture
class ToolIntegrationLayer:
    """
    定义 Agent 在世界中能做什么：
    - 文件读写
    - 代码执行沙箱
    - 数据库查询
    - API 调用
    - Web 访问

    Define what Agents can do in the world:
    - File read/write
    - Code execution sandbox
    - Database queries
    - API calls
    - Web access
    """

    def execute_tool(self, tool_call: ToolCall) -> ToolResult:
        # 1. 验证调用参数 | Validate call parameters
        self.validate_parameters(tool_call)

        # 2. 在沙箱中执行 | Execute in sandbox
        with Sandbox() as sandbox:
            result = sandbox.run(tool_call)

        # 3. 清理和格式化输出 | Clean and format output
        cleaned = self.sanitize_output(result)

        # 4. 注入回上下文 | Inject back to context
        return ToolResult(cleaned)
```

**关键设计原则 | Key Design Principles**:
- 模型永不直接接触外部系统 | Models never directly contact external systems
- 所有调用都经过验证 | All calls are validated
- 输出被清理和结构化 | Output is cleaned and structured
- 错误可被重试 | Errors can be retried

#### 2.1.2 内存与状态管理 | Memory & State Management

三层内存架构 | Three-layer memory architecture:

```
┌─────────────────────────────────────────────────────────┐
│  LONG-TERM MEMORY (长期记忆)                              │
│  - 向量存储                                               │
│  - 结构化文件 (JSON > Markdown)                          │
│  - 知识图谱                                               │
│  - 跨任务持久化                                           │
│                                                         │
│  - Vector storage                                        │
│  - Structured files (JSON > Markdown)                   │
│  - Knowledge graphs                                      │
│  - Cross-task persistence                                │
└─────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────┐
│  SESSION STATE (会话状态)                                 │
│  - 工具结果日志                                           │
│  - 已完成子任务                                           │
│  - 进度笔记                                               │
│  - 当前任务持续期间                                       │
│                                                         │
│  - Tool result logs                                      │
│  - Completed subtasks                                    │
│  - Progress notes                                        │
│  - Current task duration                                │
└─────────────────────────────────────────────────────────┘
                            │
┌─────────────────────────────────────────────────────────┐
│  WORKING CONTEXT (工作上下文)                             │
│  - 即时提示词                                             │
│  - 当前推理步骤                                           │
│  - 易失性                                                 │
│                                                         │
│  - Immediate prompts                                     │
│  - Current reasoning steps                               │
│  - Volatile                                             │
└─────────────────────────────────────────────────────────┘
```

**Anthropic 发现 | Anthropic Discovery**: JSON 比 Markdown 更适合功能跟踪文件，因为模型不太可能意外覆盖或重新格式化 JSON。 | JSON is more suitable than Markdown for feature tracking files, as models are less likely to accidentally overwrite or reformat JSON.

#### 2.1.3 上下文工程与压缩 | Context Engineering & Compression

```python
class ContextManager:
    """
    在每次模型调用时决定：包含什么、压缩什么
    Decide what to include and compress on each model call
    """

    def prepare_context(self, task: Task, history: History) -> Context:
        # 1. 压缩旧历史 | Compress old history
        if history.token_count > THRESHOLD:
            history = self.compact(history)

        # 2. RAG检索：仅拉取相关文档 | RAG retrieval: only fetch relevant docs
        relevant_docs = self.rag_retrieve(task.query)

        # 3. 应用 "Lost in the Middle" 研究结论 | Apply "Lost in the Middle" research
        context = Context(
            system_prompt=self.get_system_prompt(),  # 开头 | beginning
            retrieved_docs=relevant_docs,            # 中间（可压缩）| middle (compressible)
            user_query=task.query,                   # 结尾（最近）| end (most recent)
        )

        return context
```

**Stanford "Lost in the Middle" 研究发现 | Stanford Research Findings**:
- 关键内容 buried 在长提示词中间时，性能显著下降 | Performance significantly degrades when key content is buried in the middle
- 模型对开头和结尾内容注意力最强 | Models have strongest attention to beginning and end
- 应用到 Harness 设计：静态决策上下文放开头，动态操作上下文放结尾 | Apply to Harness design: static context at beginning, dynamic context at end

#### 2.1.4 验证与护栏 | Verification & Guardrails

```python
class VerificationLayer:
    """
    生产级 Harness 在将工作标记为完成前验证输出
    Production-grade Harness verifies output before marking work complete
    """

    def verify_completion(self, work: Work) -> VerificationResult:
        # 编码 Agent：运行测试套件 | Coding Agent: run test suite
        if work.type == "code":
            test_results = run_test_suite(work)
            if not test_results.passed:
                return VerificationResult(
                    passed=False,
                    feedback=test_results.errors
                )

        # 敏感操作：人机回环 | Sensitive operations: human-in-the-loop
        if work.risk_level > HIGH:
            return self.request_human_approval(work)

        return VerificationResult(passed=True)
```

### 2.2 架构模式 | Architecture Patterns

#### 模式1: 单 Agent 监督者 | Pattern 1: Single-Agent Supervisor

适用场景: 有界任务，如客服 Agent、知识库问答 |适用: Bounded tasks like customer service agents, Q&A

#### 模式2: 初始化器-执行器分离 | Pattern 2: Initializer-Executor Split

Anthropic 的长代码任务方案 | Anthropic's long-code task solution:
- 初始化器：设置持久项目环境、创建文件夹结构、生成功能列表 | Initializer: Set persistent project environment, create folder structure, generate feature list
- 执行器：增量实现、运行测试、Git 提交 | Executor: Incremental implementation, run tests, Git commit

#### 模式3: 多 Agent 协调 | Pattern 3: Multi-Agent Coordination

- 协调器 Agent + 研究/写作/审核 Agent | Coordinator Agent + Researcher/Writer/Reviewer Agents
- 上下文过滤、历史清理、状态传递 | Context filtering, history cleanup, state transfer

---

## 第三部分: Agent Harness vs Prompt Engineering vs Context Engineering | Part 3: Comparison

### 3.1 三者关系 | Relationship

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

### 3.2 详细对比 | Detailed Comparison

#### Prompt Engineering (提示工程)

| 维度 | 描述 | Description |
|------|------|-------------|
| **焦点** | 编写清晰的单次任务指令 | Write clear single-task instructions |
| **范围** | 单输入-输出对 | Single input-output pair |
| **思维方式** | "我该说什么？" | "What should I say?" |
| **适用场景** | 一次性任务、演示原型 | One-time tasks, demo prototypes |
| **调试方法** | 重写和猜测 | Rewrite and guess |

#### Context Engineering (上下文工程)

| 维度 | 描述 | Description |
|------|------|-------------|
| **焦点** | 策划整个信息环境 | Design entire information environment |
| **范围** | 模型看到的一切 | Everything the model sees |
| **思维方式** | "模型应该知道什么？" | "What should the model know?" |
| **适用场景** | 生产系统、多用户应用 | Production systems, multi-user apps |

**关键洞察 | Key Insight** (Andrej Karpathy):
> "在每个工业级 LLM 应用中，上下文工程是将正确的信息以正确的格式填充到上下文窗口的精妙艺术和科学。"
> "In every industrial-grade LLM application, context engineering is the subtle art and science of filling the context window with the right information in the right format."

#### Harness Engineering (Harness 工程)

| 维度 | 描述 | Description |
|------|------|-------------|
| **焦点** | 控制 Agent 运行的环境 | Control the environment where Agents operate |
| **范围** | 整个系统基础设施 | Entire system infrastructure |
| **思维方式** | "如何让错误不可能发生？" | "How to make errors impossible?" |
| **适用场景** | 企业级长期运行的 Agent | Enterprise-level long-running Agents |

### 3.3 关系总结 | Relationship Summary

```
┌─────────────────────────────────────────────────────────────┐
│  PROMPT ENGINEERING = 子集 of CONTEXT ENGINEERING         │
│                     = subset of                             │
│                     = 子集 of AGENT HARNESS                 │
│                     = subset of                             │
│                                                             │
│ 层次关系 | Hierarchy:                                       │
│  - Prompt: 说什么 | What to say                            │
│  - Context: 给什么信息 | What information to provide      │
│  - Harness: 在什么环境中运行 | What environment to run    │
└─────────────────────────────────────────────────────────────┘
```

**类比 | Analogy**:
- **Prompt Engineering** = 给实习生一份详细的待办清单 | Give an intern a detailed todo list
- **Context Engineering** = 提供完整的项目资料、历史记录和工具 | Provide complete project materials, history, and tools
- **Harness Engineering** = 建立公司制度、检查流程和防错机制 | Establish company policies, check processes, and error prevention mechanisms

---

## 第四部分: Harness Engineering 实践指南 | Part 4: Practice Guide

### 4.1 核心原则 | Core Principles

#### 原则1: 系统性修复而非重试 | Principle 1: Systematic Fix Instead of Retry

```python
# ❌ 错误：提示词重试 | Wrong: prompt retry
for attempt in range(3):
    result = llm.generate(prompt)
    if is_valid(result):
        return result
    prompt += "\nPlease try again."

# ✅ 正确：工程化修复 | Correct: engineering fix
class ValidatedTool:
    def execute(self, params):
        self.validate_schema(params)
        result = self.run(params)
        self.verify_output(result)
        return result
```

#### 原则2: AGENTS.md 知识积累 | Principle 2: AGENTS.md Knowledge Accumulation

```markdown
# AGENTS.md - 项目级 Agent 指令 | Project-level Agent instructions

## 架构规则 | Architecture Rules
- 每个领域分为固定层：Types → Config → Repo → Service → Runtime → UI
- 依赖方向严格验证 | Strict dependency direction validation
- 超出架构的代码无法提交 | Code outside architecture cannot be committed

## 已知失败模式 | Known Failure Patterns

### F-001: 忘记运行测试 | Forget to run tests
- 症状: Agent 宣称功能完成但未验证 | Symptom: Agent claims completion without verification
- 修复: 每次提交前必须运行测试 | Fix: Must run tests before every commit
```

#### 原则3: 机械可验证性 | Principle 3: Mechanical Verifiability

```python
# 构建工具使正确行为可验证 | Build tools that make correct behavior verifiable
class ScreenshotVerifier:
    """截图验证工具 | Screenshot verification tool"""
    def capture_and_verify(self, selector):
        screenshot = browser.screenshot(selector)
        analysis = vision_model.analyze(screenshot)
        return VerificationResult(passed=analysis.is_correct)
```

### 4.2 三层防御架构 | Three-Layer Defense Architecture

```
LAYER 1: 架构约束 | Architecture Constraints
- 严格的领域分层 | Strict domain layering
- 依赖方向限制 | Dependency direction limits
- 通过 linter 和结构测试强制执行 | Enforced via linter and structural tests

LAYER 2: 知识系统 | Knowledge System
- AGENTS.md 项目级指令 | Project-level instructions
- 渐进式上下文披露 | Progressive context disclosure
- 失败模式库 | Failure pattern library

LAYER 3: 工具验证 | Tool Verification
- 机械验证工具 | Mechanical verification tools
- 人机回环中断 | Human-in-the-loop interruption
- 自动测试执行 | Automated test execution
```

### 4.3 OpenAI 实践案例 | OpenAI Practice Case

**项目规模 | Project Scale**: 100万行代码，3.5 PR/工程师/天 | 1M lines of code, 3.5 PRs/engineer/day

**三大支柱 | Three Pillars**:

1. **上下文工程**: 仓库作为单一真相源 | Context Engineering: Repository as single source of truth
2. **架构约束**: 刚性分层架构 + 自定义 linter | Architecture Constraints: Rigid layered architecture + custom linter
3. **熵管理**: 背景 Agent 持续扫描 | Entropy Management: Background agents continuously scan

---

## 第五部分: 行业案例 | Part 5: Industry Cases

### 5.1 Stripe Minions 系统 | Stripe Minions System

**规模 | Scale**: 每周1,300个 AI 生成的 PR | 1,300 AI-generated PRs/week

**关键设计决策 | Key Design Decisions**:
- **窄范围任务**: 每个 Minion 处理独立任务 | Narrow scope: Each Minion handles independent tasks
- **并行执行**: 数百个 Minion 同时运行 | Parallel execution: Hundreds of Minions run simultaneously
- **人机回环**: PR 仍需人工审查 | Human-in-the-loop: PRs still require human review

### 5.2 Anthropic Claude Code

**设计理念 | Design Philosophy**:
- 更少的工具胜过更多的工具 | Fewer tools beat more tools
- 渐进式披露优于前期加载 | Progressive disclosure beats upfront loading
- Harness 必须随模型进化 | Harness must evolve with models

### 5.3 GitHub Copilot Workspace

**演进 | Evolution**:
- 原始 Copilot: 自动完成工具 | Original Copilot: Autocomplete tool
- Copilot Workspace: 完整 Agent Harness | Complete Agent Harness

---

## 第六部分: 工具生态 | Part 6: Tool Ecosystem

### 6.1 主流 Harness 实现 | Major Harness Implementations

| 类型 | 工具 | Tools |
|------|------|-------|
| 编码专用 | Claude Code, Codex CLI, GitHub Copilot Workspace, Cursor Agent, SWE-agent |
| Coding-specific | |
| 通用/可配置 | LangChain DeepAgents, LlamaIndex Agent, AutoGPT |
| General/Configurable | |
| 多 Agent 编排 | Vibe Kanban, Emdash, Desplega Agent Swarm |
| Multi-Agent Orchestration | |

### 6.2 评估指标 | Evaluation Metrics

| 指标 | 描述 | Description | 目标值 | Target |
|------|------|-------------|--------|--------|
| **Merge Rate** | AI PR 被合并的比例 | AI PR merge rate | >80% |
| **Review Cycle Time** | AI PR 审查时间 vs 人工 PR | AI PR review time vs human PR | <1.5x |
| **Test Pass Rate** | 首次测试通过率 | First test pass rate | >70% |
| **Revert Rate** | 合并后被回退的比例 | Post-merge revert rate | <5% |
| **Session Continuity** | 跨会话任务成功率 | Cross-session task success | >90% |

---

## 第七部分: Harness Engineer 职业分析 | Part 7: Career Analysis

### 7.1 角色定义 | Role Definition

**核心公式 | Core Formula**:
```
Agent = LLM Model + Harness
         (大脑)      (身体+环境)
         (brain)     (body+environment)

Harness = 工具 + 知识 + 观察 + 行动接口 + 权限
         = Tools + Knowledge + Observation + Action Interface + Permissions
```

### 7.2 核心职责 | Core Responsibilities

```
1. 实现工具 | Implement Tools
   - 文件读写、Shell执行、API调用、浏览器控制
   - File read/write, shell execution, API calls, browser control

2. 策划知识 | Curate Knowledge
   - 产品文档、架构决策记录、风格指南
   - Product docs, architecture decisions, style guides

3. 管理上下文 | Manage Context
   - 子Agent隔离、上下文压缩、任务持久化
   - Sub-agent isolation, context compression, task persistence

4. 控制权限 | Control Permissions
   - 沙箱访问、破坏性操作审批
   - Sandbox access, destructive operation approval

5. 收集过程数据 | Collect Process Data
   - 感知-推理-行动轨迹
   - Perception-reasoning-action trajectories
```

### 7.3 AI工程角色金字塔 | AI Engineering Role Pyramid

```
层4: 架构/战略层 | Layer 4: Architecture/Strategy
├── AI Architect (AI架构师)
├── AI Product Manager (AI产品经理)
└── Head of AI (AI负责人)

层3: 系统/平台层 | Layer 3: System/Platform
├── Harness Engineer ⭐
├── Platform Engineer (AI平台工程师)
└── MLOps Engineer

层2: 应用/实现层 | Layer 2: Application/Implementation
├── AI Engineer
├── AI Agent Engineer
└── LLM Engineer

层1: 交互/优化层 | Layer 1: Interaction/Optimization
├── Context Engineer
└── Prompt Engineer
```

### 7.4 薪资基准 (2026) | Salary Benchmark (2026)

| 经验水平 | Experience Level | 薪资范围 | Salary Range |
|----------|-------------------|----------|---------------|
| **Junior (0-2年)** | Junior (0-2 years) | $120K - $160K |
| **Mid-level (2-5年)** | Mid-level (2-5 years) | $160K - $220K |
| **Senior (5+年)** | Senior (5+ years) | $220K - $300K+ |
| **Lead/Architect** | | $280K - $400K+ |

### 7.5 6个月学习路线图 | 6-Month Learning Roadmap

| 月份 | Month | 主题 | Topic | 里程碑 | Milestone |
|------|-------|------|-------|--------|-----------|
| 1 | | AI Agent 基础 | AI Agent Basics | 构建使用工具完成多步骤任务的 Agent | Build agent that uses tools to complete multi-step tasks |
| 2 | | Agent 设计模式 | Agent Design Patterns | 为任务选择正确模式并解释 | Choose correct pattern for task and explain |
| 3 | | 验证与测试 | Verification & Testing | 构建自动化评估管道 | Build automated evaluation pipeline |
| 4 | | 生产基础设施 | Production Infrastructure | 拥有完整 Harness 的生产 Agent | Production agent with complete Harness |
| 5 | | 高级模式 | Advanced Patterns | 多 Agent 系统设计与操作 | Multi-Agent system design and operation |
| 6 | | 作品集与求职 | Portfolio & Job Search | 展示技能的作品集 | Portfolio showcasing skills |

---

## 附录: 术语表 | Appendix: Glossary

| 术语 | Term | 定义 | Definition |
|------|------|------|------------|
| **Agent Harness** | | 围绕 LLM 的软件基础设施 | Software infrastructure surrounding LLMs |
| **Harness Engineering** | | 将每次 Agent 失败视为工程问题永久修复 | Treat every Agent failure as permanent engineering fix |
| **Context Engineering** | | 设计动态系统提供正确信息 | Design dynamic systems to provide right information |
| **Context Rot** | | 上下文增加导致性能下降 | Performance degradation as context grows |
| **Mechanical Verification** | | 通过工具使正确行为可自动验证 | Make correct behavior verifiable via tools |
| **Progressive Disclosure** | | 渐进式披露上下文 | Progressive context disclosure |

---

## 参考资源 | References

### 开创性文献 | Seminal References

1. Mitchell Hashimoto - "Harness Engineering" (2026年2月 | Feb 2026)
2. OpenAI - "Harness Engineering: Leveraging Codex in an Agent-First World"
3. Anthropic - "Building Effective Agents" (2025)
4. ICML 2025 - "General Modular Harness for LLM Agents"
5. Stanford - "Lost in the Middle" (2023)

### 相关 Awesome Lists

- [awesome-agent-harness](https://github.com/AutoJunjie/awesome-agent-harness)
- [awesome-cli-coding-agents](https://github.com/bradAGI/awesome-cli-coding-agents)
- [awesome-context-engineering](https://github.com/yzfly/awesome-context-engineering)

---

*报告完成时间: 2026年3月 | Report Completed: March 2026*
*本报告基于公开技术文献、研究论文和行业实践案例整理 | Based on public technical literature, research papers, and industry practice*
