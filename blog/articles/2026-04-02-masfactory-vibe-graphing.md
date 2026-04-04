---
title: 多智能体编排太繁琐？MASFactory用Vibe Graphing直接「话」出来了
author: PaperAgent
date: 2026-04-02
source: 微信公众号
url: https://mp.weixin.qq.com/s/7IsQJMPpMUDu4-Mtpe3gNA
tags: [MASFactory, Vibe Graphing, 多智能体, Agent,编排框架]
summary: 北京邮电大学开源MASFactory框架，提出以图为中心（Graph-Centric）架构和"Vibe Graphing"开发范式，用自然语言驱动多智能体工作流生成。
---

# 多智能体编排太繁琐？MASFactory用Vibe Graphing直接「话」出来了

**作者：PaperAgent**  
**发布日期：2026年3月27日**

大家好，我是PaperAgent，不是Agent！

在大模型能力不断跃升的当下，**多智能体系统（MAS）**已成为应对极具挑战性任务的关键支柱。然而，审视当前的 **MAS 编排领域**，开发者仍受困于陈旧的构建手段：要么投入高昂的工程成本，用硬编码手动维护复杂的通信逻辑；要么妥协于拖拽式画布框架，对于开发复杂的多智能体系统，工作量大且难以像Vibe Coding那样接入AI替代人类工作。

为打破这种低效的编排方式，北京邮电大学 正式开源了全新框架 **MASFactory**。该框架提出以图为中心（**Graph-Centric**）的架构来描述多智能体工作流，并引入了"Vibe Graphing"开发范式，将 MAS 的开发从手工组装推向了自然语言驱动的时代。

## Vibe Grahing

### 从"硬编码"到"意图驱动"的 Vibe Graphing

与传统的节点连线或编写底层逻辑不同，MASFactory 主张"先有全局意图，后有局部细节"。开发者只需用自然语言阐述系统的最终目标与角色分工期望，内置的 AI 引擎便会迅速推演出一套具备可行性的协作图结构。

针对AI生成过程中可能存在的幻觉不可控的问题，MASFactory引入"人在回路（Human-In-the-Loop）"的过程。开发者可以在每个阶段结束前审查AI的方案，并提出修改意见，直到AI的方案能够让开发者满意。

## 多智能体编排框架大横评

从多智能体系统的开发演进来看，当前主流方式大致可以分为三类：代码编写、可视化拖拽和 Vibe Graphing。

- **代码编写**：具备最高的灵活性和扩展性，适合构建复杂、多层次的多智能体协作系统，但对开发者能力要求较高，整体开发成本也更大；
- **可视化拖拽**：显著降低了使用门槛，让更多用户能够快速搭建基础工作流，但在复杂拓扑、细粒度控制和后续迭代方面仍存在限制。
- **Vibe Graphing**：进一步将多智能体系统设计从"手工实现"推进到"意图驱动生成"，用户无需编写大量代码或反复拖拽节点，只需清晰描述需求，并在交互过程中持续细化设计，即可快速完成系统构建与迭代。

也正因此，Vibe Graphing 更适合面向复杂需求下的快速原型设计和低人力成本开发。

## MASFactory系统架构

MASFactory 将庞杂的多智能体交互科学地抽象为四个层次：

1. **图骨架层**：像其他多智能体工作流框架一样，将 Node（节点）与 Edge（边）作为底层基础骨架，刻画智能体之间的协作与消息流动。

2. **组件层**：将基础骨架封装为开箱即用的模块，不仅包含执行任务的 Agent，还引入了用于动态路由的 Switch、处理多轮博弈的 Loop，以及人在回路的 Human 节点。更重要的是，任何 Graph 都可以作为子节点被无限嵌套，实现逻辑的极致复用。

3. **统一协议层**：借助适配器机制，无缝抹平不同通信协议的差异，并统一管理上下文环境，轻松接入 RAG、Memory 等增强能力。

4. **混合交互层**：为上层应用提供极其灵活的操作入口，包括兼容代码开发的声明式、命令式编排层、可视化的拖拽编排层和Vibe Graphing编排层，兼容不同开发者的使用习惯。

## 效果对比

为系统评估 MASFactory 的有效性，论文从两个层面进行了实验验证：

- 其一，检验 MASFactory 对已有代表性多智能体系统的复现能力；
-其二，验证 Vibe Graphing 所生成工作流的实际效果。

实验共覆盖 7 个公开基准，包括面向代码任务的 HumanEval、MBPP、BigCodeBench 和 SRDD，以及面向通用推理与工具使用的 MMLU-Pro、GAIA 和 GPQA。

整体来看，MASFactory 在 7 个公开基准上的结果表明，它既能较稳定地承载不同类型的多智能体工作流，也证明了"自然语言意图—可编辑规约—可执行图"这一路径是可行的。

同时，Vibe Graphing 生成的工作流也展现出较强竞争力。无论是基于 ChatDev 改造得到的 Vibe Graphing-ChatDev，还是直接面向任务生成的 Vibe Graphing-Task Specific，都在多个基准上取得了可观结果。其中，Task Specific 方案在 HumanEval、BigCodeBench 和 SRDD 上表现突出，说明通过自然语言驱动工作流生成，已经能够接近甚至达到人工设计系统的效果。

---

**代码仓库**：https://github.com/BUPT-GAMMA/MASFactory  
**论文地址**：https://arxiv.org/abs/2603.06007
