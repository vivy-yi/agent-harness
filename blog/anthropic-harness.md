# Anthropic说：不要在等下一代模型了，立刻马上做Harness！

> 作者：猕猴桃 | 来源：探索AGI | 发布日期：2026年3月25日 11:51

---

同一个模型，什么都没换。数据没换，提示词没换，只换了模型外面包的那层运行环境，编程基准的成功率从 42% 跳到了 78%。

这个数据来自 Nate B Jones 的研究。变量只有一个：模型外面的壳。

今天，Anthropic也分享了一篇工程博客，用自己的实验给了另一组数据：同一句提示词，同一个模型，跑 20 分钟花 9 美元，出来的东西核心功能是坏的；换一套运行方式，跑 6 小时花 200 美元，出来一个能玩的游戏。

现在业内共识，当一个技术被Anthropic以博客的形式分享出来，说明这个技术是必需，且成熟的方案了。

这层壳，现在有了一个正式的名字：**Harness**。

围绕它展开的工程实践，叫 **Harness Engineering**。2026 年 AI 圈最热的话题之一。

---

## 三代进化

要理解 Harness Engineering 解决什么问题，得先看它的前两代。

2022 到 2024 年，关键词是 **Prompt Engineering**。核心关注是怎么写好一条指令。few-shot、chain-of-thought、角色扮演，本质上都在打磨单次输入。

2025 年，Andrej Karpathy 和 Shopify CEO Tobi Lütke 推了个新概念：**Context Engineering**。单条 prompt 不够了，你得为模型动态构建整个上下文——相关文件、历史对话、工具定义，知识库检索结果——让模型做每一个决策时，都能看到它需要的信息。

2026 年 2 月，**Harness Engineering** 来了。

![三代进化](./images-2/harness-01.jpg)

---

打个比方：

Prompt Engineering 是写好一封邮件。Context Engineering 是把相关附件都带上。Harness Engineering，是搭建整个工作环境——让 Agent 能持续，稳定、高质量地工作。它包含了上下文工程，但在更高的层面运作：约束、反馈循环、架构规则、工具链、生命周期管理。

这个词最早来自 Mitchell Hashimoto——HashiCorp 联合创始人、Terraform 的缔造者。他今年 2 月写了篇博客，把自己使用 AI 编程的进化拆成了六个阶段。第五个阶段叫 **Engineer the Harness**。

定义只有一句话：

> 每当你发现 Agent 犯了一个错误，你就花时间去工程化一个解决方案，让它再也不会犯同样的错。

他在 Ghostty 项目里实践了这个理念。AGENTS.md 文件里的每一行规则，背后都对应一个 Agent 曾经犯过的错。他说，那个文件里的每一行都基于一次 Agent 的不良行为，而且几乎完全解决了这些问题。

几天后 OpenAI 跟进了一篇重磅博文，Martin Fowler 团队的 Birgitta Böckeler 接着分析。几周之内，这个词火遍了整个 AI 工程圈。

---

## 百万行代码的验证

OpenAI 的 Codex 团队做了一个实验，让整个行业重新理解"工程师"这个角色。

从一个空的 git 仓库开始，5 个月，大约 100 万行代码，1500 个 PR，全部由 Agent 生成。人类一行代码都没写。

团队一开始只有 3 个工程师，后来扩到 7 个。用 GPT-5 驱动的 Codex CLI 从零构建了一个完整的生产级应用。平均每位工程师每天合并 3.5 个 PR。如果用传统方式手写，工期大概是现在的 10 倍。

项目的核心工程师 Ryan Lopopolo 写了一句话：

> Agent 不难，Harness 才难。

5 个月实战里他们总结了几条硬规则：

- 仓库是 Agent 唯一的知识来源。
- 代码不仅要对人类可读，更要对 Agent 可读。
- 架构约束不靠 prompt，靠 linter。
- 自主性得一步步给。
- 如果 PR 需要大改才能合并，问题不在 Agent，在 Harness。

OpenAI 团队给自己的角色总结是：我们现在最大的挑战，在于设计环境、反馈循环和控制系统。

不写代码了。写规则。

![百万行代码验证](./images-2/harness-02.jpg)

---

Cursor 团队也做过一次长程的Coding实验，他们发现一个反直觉的问题：

**约束解空间，反而让 Agent 更有生产力。**

当模型可以生成任何东西时，会浪费 token 探索死胡同。当 Harness 定义了清晰的边界，Agent 反而更快收敛到正确答案。

---

## 不止 OpenAI

如果只有 OpenAI 一家在做，Harness Engineering 可能只是个噱头。但事实是，几家公司几乎同时走向了同一个方向。

**Stripe** 的内部系统「Minions」，每周合并 1300 多个 PR，全部由无人值守的 Agent 完成。

![Stripe Minions架构](./images-2/harness-03.jpg)

它的架构有个值得注意的设计：Blueprint 编排系统，把工作流拆成确定性节点和 Agentic 节点。

- 确定性节点——跑 linter、推送更改——按固定路径执行，不调用大模型。
- Agentic 节点——实现功能、修复 CI——让模型判断。

Stripe 还有一个硬规则：CI 最多跑两轮。第一轮失败，Agent 自动修复再跑一次。还失败，直接转交人类。不允许 Agent 在 CI 上无限重试。

他们内部的工具平台挂了大约 500 个 MCP 工具，但给每个 Agent 的只是精心筛选过的子集。因为他们发现了一条规律：更多的工具并不等于更好的表现。

Stripe 工程团队的总结：

> 成功取决于可靠的开发者环境、测试基础设施和反馈循环，跟模型选择关系不大。

---

**Cursor** 的「Self-Driving Codebases」研究走得更远——每小时约 1000 个 commit，一周超过 1000 万次工具调用，启动后无需人工干预。

![Cursor演进历程](./images-2/harness-04.jpg)

但他们走过的弯路，本身就是 Harness 设计难度的证明：

- 第一版单 Agent，复杂任务扛不住。
- 第二版多 Agent 共享状态文件，锁竞争严重，Agent 之间互相打架。
- 第三版结构化角色分工，太僵硬。
- 第四版持续执行器，角色过载。

最终版本是递归 Planner-Worker 模型。

他们还发现了一个有点黑色幽默的现象：一条模糊的初始指令，在数百个并发 Agent 之间会被放大。一个 Agent 犯的错，乘以几百个并发。结果可想而知。

---

## 模型不会反省

上面这些例子都在解决同一个问题：怎么让 Agent 持续，稳定地产出高质量的代码。但有一个更底层的问题，直到 Anthropic 这篇博客才被拆清楚。

**模型不会评价自己的工作。**

Anthropic 的工程师发现，当你让 Agent 自己评估刚写完的东西时，它会自信地表示写得很好。

即使在人类看来，质量明显不行。这个问题在主观任务上（比如前端设计，页面到底好不好看）尤其严重，因为没有二元的对错标准。但即使在有客观标准的任务上，Agent 的自我判断也会出问题。

这才是 Harness 需要存在的底层原因之一。模型能力够了，但它对自己的能力没有准确的认知。

---

Anthropic 的解法借鉴了 GAN（生成对抗网络）的思路：

把生成和评估拆成两个独立的 Agent。一个 generator 负责写，一个 evaluator 负责评。evaluator 不是看截图打分，而是用 Playwright 真的去点页面、查 API，看数据库状态，像一个真人 QA 一样操作完再给反馈。

但 evaluator 也不是天生就靠谱。

**开箱即用的 Claude 是一个很差的 QA Agent。**

早期的 evaluator 会发现问题，然后说服自己这不是大问题，接着就批准了。它也倾向于做表面测试，不去探边界。他们花了好几轮来校准 evaluator 的判断标准，让它的严苛程度符合人类预期。

关键在于，**让一个独立的 evaluator 变得严格，远比让 generator 学会自我批评容易得多。** 这就是拆分的价值。

![Evaluator设计](./images-2/harness-06.jpg)

---

他们做了一些全栈开发测试，用一套3个Agent的架构：planner 把一句话需求展开成完整的产品规格，generator 按 sprint 逐个功能实现，evaluator 在每个 sprint 结束后真机验收。

Solo 模式的输出看起来不错，但是功能都是坏的，界面完全看不出来。

![Solo模式输出](./images-2/harness-11.jpg)

harness 模式出来的版本，编辑器更完整，游戏能玩，物理引擎有些粗糙但核心交互是通的。

![Harness模式输出](./images-2/harness-12.jpg)

evaluator 的 bug 报告精确到哪行代码出了什么问题。

---

## 数据调研

我用deepresearch做了关于很多企业，发布的harness产生的价值。

**LangChain**：同一模型（gpt-5.2-codex），只改 Harness，Terminal Bench 2.0 成绩从 52.8% 升到 66.5%。排名从三十名开外直接进前五。

![Harness提升数据](./images-2/harness-07.jpg)

**Nate B Jones**：同一模型，Harness 不同，成功率 42% vs 78%。换壳带来的提升相当于换一代模型。

**Anthropic**：solo 模式 20 分钟 / $200（功能可用）。后续优化后的 harness，一次完整构建约 4 小时 / $125。

**Terminal Bench 2.0**：Claude Opus 4.6 在 Claude Code Harness 上排名第 33，换一个 Harness 冲到第 5。同一个模型，同一个基准，排名差了 28 位。

![Terminal Bench排名对比](./images-2/harness-08.jpg)

**Pi Research**：一个下午内，仅通过修改 Harness，提升了 15 个不同 LLM 的编程能力。论文标题就是《Improving 15 LLMs at Coding in One Afternoon. Only the Harness Changed》。

还有很多，都指向了一个结论：

**在当前这个节点，优化模型外面的壳，回报率可能比等下一代模型更高。**

---

## 当然也有人不同意。

OpenAI 的 Noam Brown 在 Latent Space 的一次访谈中直接说：

> Harness 就像一根拐杖，我们终将超越它。

他的理由是： 在推理模型出现之前，开发者在 GPT-4o 上搭了大量复杂的 Agentic 系统来模拟推理能力。

路由器、编排器、multi-agent 协作。然后推理模型一出来，这些东西一夜之间不需要了。在很多场景下，它们反而让效果更差了。

他的预判是，OpenAI 将走向一个统一模型的未来。你不应该需要在模型上面再加路由器。他给开发者的建议只有一句话：

> 别花六个月搭建一个可能六个月后就被淘汰的东西。

---

METR 的数据也给了 Harness 阵营一记闷击。

他们找了 scikit-learn、Sphinx、pytest 三个项目的 4 位活跃维护者，审查了 296 个 AI 生成的 PR。结果：维护者的合并率大约只有自动化评分通过率的一半。

自动评分器说 Agent 能独立完成约 50 分钟的任务，但维护者实际愿意合并的 PR 对应的任务范围只有 8 分钟左右——7 倍的能力高估。

Latent Space 给这一派起了个名字，叫 Bitter Lesson 阵营。出自 Rich Sutton 那篇著名的 AI 随笔：别在工程花活上下太多功夫，算力的增长终究会碾平一切。

---

## 但壳在进化

Noam Brown 说得对不对？对了一半。

Anthropic 自己的实验就是最好的证据。

他们最早的 harness 用 Opus 4.5，需要把工作拆成 sprint，一个 sprint 做一个功能，每个 sprint 之间做 context reset。

![Harness迭代历程](./images-2/harness-09.jpg)

因为 4.5 有一种"上下文焦虑"，当它感觉自己快用完上下文窗口时，会提前开始收尾工作，质量急剧下降。

整套 sprint + context reset 的机制就是为了对付这个问题。

然后 Opus 4.6 出来了。这个版本规划更谨慎、长任务更稳定、长上下文检索更好、自己 debug 的能力更强。

于是 Anthropic 直接砍掉了 sprint 结构，让 generator 连续跑完整个构建——一次跑了两个多小时，没有崩溃。

Sprint 被淘汰了。Noam Brown 说的"拐杖"，在这个案例里确实被扔掉了。

但 evaluator 没有被扔掉。

因为即使 Opus 4.6 变强了，它的能力边界只是往外推了一些，边界本身并没有消失。

在那个边界以内的任务，generator 可以独立搞定，evaluator 变成了多余的开销。但在边界附近的任务上，evaluator 依然能抓出 generator 自己看不到的问题——功能只实现了表面、API 路由定义顺序导致的隐蔽 bug、交互逻辑的缺失。

Anthropic 给出的判断是：

**harness 的可能性空间不会随着模型进步而缩小。它会平移。**

模型变强了，旧的约束可以拆掉，但新的、更高阶的约束空间打开了。以前你要教 Agent 怎么管理上下文窗口，现在不用了；但你可以开始让它做四小时的自主开发任务，那就需要新的反馈和验收机制。

---

不只是 Anthropic 在做这件事。Manus 6 个月内重构了 5 次 Harness。LangChain 一年内重新架构了 3 次研究型 Agent。Vercel 砍掉了 80% 的 Agent 工具。

这说明一件事：

**Harness 不是一次性工程，它是一个持续演化的系统。**

---

Anthropic 那篇博客的最后一句话是这么写的：

> 有意思的 harness 组合空间不会随着模型进步而缩小。它只是在移动。而 AI 工程师真正要做的，是持续找到下一个有效的组合。

---

## 写在最后

OpenAI 的 Codex 团队不写代码了，写的是架构规则、linter 配置和 AGENTS.md。Stripe 的工程师不写代码了，写的是 Blueprint 编排和 CI 限速策略。Anthropic 的工程师不写代码了，写的是 evaluator 的打分标准和校准逻辑。

**写代码正在变成一件成本很低的事。**

设计那套让 Agent 持续，稳定、高质量写代码的系统，才是真正贵的部分。

而这套系统本身也不是一劳永逸的。每一代模型出来，都得重新审视哪些约束还管用，哪些该拆掉，哪些新的空间被打开了。

真正稀缺的能力，不在模型里面，在模型外面。而且它每隔几个月就得重写一次。
