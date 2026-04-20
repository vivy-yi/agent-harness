# anvil.el v0.3.0 — Multi-Agent Orchestrator, Consensus, Live Streaming

**来源**: [Reddit /r/emacs](https://www.reddit.com/r/emacs/comments/1sq9q3a/anvilel_v030_multiagent_orchestrator_consensus/)

**日期**: 2026-04-19

**核心内容**:

Emacs MCP server anvil.el 发布 v0.3.0，在 6 天内从 v0.1 跳跃到 v0.3.1（dogfood 故事：开发者自己使用新版本发布新版本）。

**v0.3.0 核心新功能 — `anvil-orchestrator`**:
- 将同一 prompt 同时发送给多个 AI Agent（Claude Code 等通过 MCP 接入）
- 通过共识（consensus）机制汇聚多个 Agent 的结果
- Live Streaming：实时流式输出多 Agent 协作结果
- 可从 Emacs 内部或 Claude Code 通过 MCP 调用

**技术亮点**:
- 单 MCP server 将 Emacs 变为 AI workbench
- Multi-agent fanning：一点对多点 prompt 分发
- Consensus gathering：多 Agent 结果汇聚与协调
- v0.3→v0.3.1 快速迭代展示实际使用中的成熟度

**Harness 关联**: 这是 Agent Harness 在 Emacs 生态中的实现示例——以 Emacs 为宿主环境，通过 MCP 协议连接多个 Agent，形成多 Agent 协作的 harness 框架。

**标签**: `#emacs` `#mcp` `#multi-agent` `#orchestrator` `#consensus` `#live-streaming` `#harness`
