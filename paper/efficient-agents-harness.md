---
title: 高效Agents的尽头是，Harness
author: PaperAgent
date: "2026-03-24 16:08:26"
source: "https://mp.weixin.qq.com/s/HabQGb7GGtHYRPDV5xzHNA"
---

# 高效Agents的尽头是，Harness

大家好，我是PaperAgent，不是Agent！

玩过**Agent**的人都有这种体验：前五轮，它像个天才；十五轮后，开始犯蠢；二十轮后，它连自己最开始要干嘛都忘了。

很多人把**Agent**当成一个“**模型+工具**”的简单组合。但**跑长了**就知道，这**远远不够**。

philschmid打了个比方：模型是CPU，上下文是内存，而**Harness**是操作系统。没有**操作系统**，再强的CPU也撑不起复杂任务。[没想到，又一个Code Agents瓶颈，被美团&上交大彻底撕开了~](https://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247505531&idx=1&sn=d7c3900a4612693a2d8be5fb36da5e84&scene=21#wechat_redirect)

![https://www.philschmid.de/agent-harness-2026](images/img_001.png)https://www.philschmid.de/agent-harness-2026

- **模型**是CPU：提供原始处理能力
- **上下文窗口**是RAM：有限的、易失性工作内存
- **Agent Harness**是操作系统：管理上下文，处理"启动"序列（提示词、钩子），并提供标准驱动（工具处理）
- **Agent**是应用程序：运行在操作系统之上的特定用户逻辑

目前，通用型**Harness**还很罕见。**OpenDev**是这一新兴类别的典型代表，他们把Agent跑长跑会遇到的坑，一个一个填平了。

![Image](images/img_002.png)

AI编程助手正在经历一场根本性转变。从Copilot的代码补全，到Claude Code、Aider等终端原生Agent，开发者们发现：**真正的自动化需要直接运行在开发者管理源码、执行构建、部署环境的地方——终端**。

但构建一个**生产级的终端Agent**绝非易事。长周期会话中的上下文窗口爆炸、破坏性命令的安全风险、以及多步骤任务的推理退化，这些都是IDE插件时代不必面对的挑战。

![系统架构总览](images/img_003.png)*Figure 2: OPENDEV的四层系统架构（Entry & UI → Agent → Tool & Context → Persistence）*

OpenDev团队在这篇论文中提出了一个核心观点：**Coding Agent的架构应该分为两个明确的阶段——Scaffolding（构建期）和 Harness（运行时）**。这篇论文不仅贡献了代码，更贡献了一整套工程化的设计哲学。[Code Agents的评估瓶颈，终于还是被美团&上交大彻底撕开了](https://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247505531&idx=1&sn=d7c3900a4612693a2d8be5fb36da5e84&scene=21#wechat_redirect)

## 2 核心理念：Scaffolding + Harness

传统Agent往往混淆了两个不同的阶段：

- **Scaffolding（脚手架）**：在第一个Prompt到达之前完成的工作——组装System Prompt、构建Tool Schema、注册Subagent。这部分应该**eager construction（即时构建）**，确保Agent在运行时已是完全就绪状态。

![Agent Harness架构](images/img_004.png)*Figure 4: Agent Harness的详细视图，展示了ReAct循环周围的七个支撑子系统*

- **Harness（马具/运行时框架）**：第一个Prompt之后的所有工作——工具调度、上下文管理、安全强制执行、会话持久化。这是Agent的**运行时编排层**。

> **关键洞察**：将构建期与运行时分离开，意味着你可以独立演化每个部分。新增工具只需修改Registry，而变更压缩策略只需调整Harness。

## 3 Extended ReAct：不只是"思考-行动"

**Agent Harness架构**实现了一个扩展的ReAct循环，包含六个阶段：

1. **Pre-check & Compaction**：检查注入队列，执行上下文压缩
2. **Thinking**：独立的思考阶段（无工具访问），防止过早行动
3. **Self-Critique**：自批判阶段（仅在HIGH级别启用）
4. **Action**：主LLM调用，产生工具调用
5. **Tool Execution**：并行/串行执行工具
6. **Post-processing**：学习记录、会话保存

![Extended ReAct Loop](images/img_005.png)*Figure 8: Extended ReAct执行循环的详细视图，展示了从Initialization到Iteration的完整流程*

特别值得注意的是**Phase 0的Staged Context Management**。系统监控Token使用率，在70%、80%、85%、90%、99%五个阈值采取不同策略：

![Image](images/img_006.png)*Algorithm 1: Extended ReAct Loop with Five-Stage Compaction and Doom-Loop Detection*

## 4 Context Engineering：上下文是一等公民

在长周期终端会话中，**上下文管理不是优化项，而是基础性工程问题**。OpenDev提出了Context Engineering Layer，包含四个关键子系统：

### 4.1 动态System Prompt组装

通过Priority-ordered conditional composition，根据运行时环境动态加载Prompt片段。例如，只有在Git仓库中才加载Git工作流指南，只有启用Todo跟踪时才加载任务管理指令。

![Image](images/img_007.png)

*Figure 10: PromptComposer的工作流程：Filter → Sort → Load → Join*

### 4.2 自适应上下文压缩（ACC）

不同于简单的"到达阈值就总结"，ACC采用渐进式策略：

- **Active State**：最近的消息，完整保留
- **Faded State**：较旧的消息，替换为引用指针
- **Archived State**：归档到磁盘，完全移出上下文

![Image](images/img_008.png)

*Figure 13: 五级压缩管道（Warning → Masking → Pruning → Aggressive Masking → Full Compaction）*

### 4.3 系统提醒（System Reminders）

解决"指令衰减"问题：随着会话变长，模型对初始System Prompt的关注度下降。OPENDEV通过**事件驱动的即时提醒**（Event-driven System Reminders）在关键时刻注入短小的user-role消息：

- 检测到未完成Todo却调用task\_complete时
- 连续5次只读操作后（防止探索螺旋）
- 工具调用失败后

![Image](images/img_009.png)

*Figure 11: 系统提醒如何防止过早完成任务（左：无提醒导致用户信任丧失；右：有提醒确保任务完成）*

### 4.4 双记忆架构（Dual-Memory）

为Thinking模型设计的特殊架构：

- **Episodic Memory**：LLM生成的长程对话摘要（每5条消息更新一次）
- **Working Memory**：最近6条消息的原文

这既避免了长上下文超限，又保留了关键细节。

![Image](images/img_010.png)

*Figure 14: Agentic Context Engineering (ACE) 记忆管道，包含Bullet Selector、Reflector、Curator和Playbook*

## 5. 防御纵深：五层安全架构

终端Agent能执行任意Shell命令，这带来了巨大风险。OPENDEV采用**五层独立的安全机制**：

1. **Prompt-Level Guardrails**：System Prompt中的安全策略
2. **Schema-Level Tool Restrictions**：通过Tool Schema过滤（如Planner Subagent只能看到只读工具）
3. **Runtime Approval System**：Manual/Semi-Auto/Auto三级审批
4. **Tool-Level Validation**：危险模式检测（rm -rf /等）、陈旧读取检测
5. **Lifecycle Hooks**：用户自定义的Pre/Post工具钩子

![Image](images/img_011.png)*Figure 3: 防御纵深安全架构的五层独立防线*

**关键设计**：让危险工具"不可见"而非"被阻止"。当Tool Schema中根本不包含写工具时，LLM甚至不会尝试生成写操作，这比运行时权限检查更 robust。

## 6. 复合AI系统：多模型路由

OPENDEV采用Compound AI System架构，将不同工作负载路由到不同模型：

| Model Role | Purpose | Fallback Chain |
| --- | --- | --- |
| **Action** | 主要执行模型 | - |
| **Thinking** | 深度推理（无工具） | Action |
| **Critique** | 自评估 | Thinking → Action |
| **Vision** | 图像处理 | Action (if capable) |
| **Compact** | 快速总结 | Action |

![Image](images/img_012.png)*Figure 1: OPENDEV的并发会话架构，每个Session包含多个Subagent，各自绑定到LLM Pool中的不同模型*

这种设计实现了**模型无关性**：切换提供商只需改配置，无需改代码。

## 7. 工具系统：延迟发现与高效扩展

### 7.1 MCP（Model Context Protocol）延迟发现

为避免100个外部工具消耗20,000 Token的上下文，OPENDEV采用**Lazy Discovery**：只有在Agent调用`search_tools`或直接调用某个MCP工具时，该工具的Schema才被加载到上下文中。

![Image](images/img_013.png)*Figure 19: ToolRegistry的延迟发现机制，将MCP集成的基线Token成本降至接近零*

### 7.2 9-Pass模糊匹配编辑

LLM生成的代码编辑往往有微小偏差（空格、缩进、转义序列）。`edit_file`工具实现了9级渐进式匹配链，从精确匹配到上下文感知匹配，大幅降低了"内容未找到"的错误率。

![Image](images/img_014.png)

*Table D: Edit Tool的九级模糊匹配链*

### 7.3 完整工具目录

![Image](images/img_015.png)

*Table 1: OPENDEV内置工具完整目录（按Handler分类）*

## 8. 关键经验：五个设计权衡

OpenDev总结了五个跨领域的设计张力：

![Image](images/img_016.png)

## 最后，Harness作为新的抽象层

OpenDev的贡献不仅是又一个开源Coding Agent，而是提出了**Harness**这一关键抽象——将Agent的运行时编排从业务逻辑中分离出来。

在终端原生Agent的时代，我们需要的不是更聪明的Prompt，而是更健壮的**Scaffolding + Harness**架构：

- **Scaffolding**确保Agent以正确的能力组合启动
- **Harness**确保Agent在长周期、高风险的终端环境中安全、高效、持续地运行

正如论文所言："The design space for terminal-native agentic tools remains largely underexplored." 而**Harness**，或许就是这个设计空间中最关键的 missing piece。

```
https://arxiv.org/pdf/2603.05344v3
https://github.com/opendev-to/opendev
Building Effective AI Coding Agents for the Terminal: Scaffolding, Harness, Context Engineering, and Lessons Learned                   https://openai.com/index/harness-engineering/
```

[动手设计AI Agents：（编排、记忆、插件、workflow、协作）](https://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247492838&idx=2&sn=1e25832e7300ef312721325d0def30b4&scene=21#wechat_redirect)

[分享两篇Claude Skills最新论文，有3个核心结论](https://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247502780&idx=1&sn=2671e0e0e6e15dd5a2020b1fc1281cf7&scene=21#wechat_redirect)

[会学习的龙虾，才是好龙虾：OpenClaw-RL](https://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247505531&idx=1&sn=d7c3900a4612693a2d8be5fb36da5e84&scene=21#wechat_redirect)
[2026，做Agentic AI，绕不开这两篇开年综述](https://mp.weixin.qq.com/s?__biz=Mzk0MTYzMzMxMA==&mid=2247502666&idx=1&sn=d6a467896c6753c8d8634c7400d8dbb4&scene=21#wechat_redirect)

---

每天一篇大模型Paper来锻炼我们的思维~已经读到这了，不妨点个👍、❤️、↗️三连，加个星标⭐，不迷路哦~
