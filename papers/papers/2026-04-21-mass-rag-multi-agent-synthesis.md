---
title: "MASS-RAG: Multi-Agent Synthesis Retrieval-Augmented Generation"
source: ArXiv (cs.CL)
date: 2026-04-20
url: https://arxiv.org/abs/2504.XXXXX
tags: [rag, multi-agent, retrieval, llm, synthesis]
summary: "MASS-RAG：多 Agent 协作合成 RAG 系统，解决噪声、碎片化外部知识的整合问题。"
---

# MASS-RAG: Multi-Agent Synthesis Retrieval-Augmented Generation

## 论文信息

- **作者**：ArXiv (2026-04-20)
- **类别**：cs.CL

## 核心问题

当检索上下文**噪声、碎片化或不完整**时，LLM 难以有效整合外部知识。

## 解决方案：MASS-RAG

**Multi-Agent Synthesis RAG** — 多 Agent 协作合成 RAG：

1. **多 Agent 分工**：不同 Agent 处理不同类型的知识片段
2. **知识合成**：Agent 间协作整合碎片化信息
3. **质量提升**：减少噪声影响，提高答案质量

## 关键创新

- **Agent Specialization**：专业分工处理不同知识类型
- **Cross-Reference Synthesis**：跨文档知识关联
- **Uncertainty-aware Retrieval**：处理不完整/噪声上下文

## 应用场景

- 复杂研究问题回答
- 多文档综合分析
- 知识库问答系统

## 相关工作

- [LangChain Multi-Agent](https://github.com/langchain-ai/langchain)
- [Agent RAG Patterns](https://github.com/vivy-yi/agent-harness)
