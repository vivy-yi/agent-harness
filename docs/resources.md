# Agent Harness Resources

A curated collection of Agent Harness related resources, articles, and references.

---

## Official References

### Seminal Papers & Articles

1. **Harness Engineering: Leveraging Codex in an Agent-First World**
   - Source: OpenAI
   - URL: https://openai.com/index/harness-engineering/

2. **Building Effective Agents**
   - Source: Anthropic
   - URL: https://www.anthropic.com/research/building-effective-agents

3. **Harness Engineering**
   - Source: Mitchell Hashimoto
   - Date: February 2026

4. **General Modular Harness for LLM Agents in Multi-Turn Gaming Environments**
   - Source: ICML 2025

5. **Lost in the Middle**
   - Source: Stanford (Liu et al.)
   - Date: 2023

### Industry Resources

- [awesome-agent-harness](https://github.com/AutoJunjie/awesome-agent-harness) - Comprehensive tool and framework list
- [awesome-cli-coding-agents](https://github.com/bradAGI/awesome-cli-coding-agents) - CLI coding agents collection
- [awesome-context-engineering](https://github.com/yzfly/awesome-context-engineering) - Context engineering resources

---

## Tools & Frameworks

### Coding Agents

| Tool | Description | GitHub |
|------|-------------|--------|
| Claude Code | Anthropic's coding agent | code.claude.com |
| Codex CLI | OpenAI's CLI coding agent | github.com/openai/codex |
| OpenCode | Open-source coding agent | github.com/sst/opencode |
| Gemini CLI | Google's CLI coding agent | github.com/google-gemini/gemini-cli |
| Cursor | AI-first code editor | cursor.sh |
| Aider | AI pair programming in terminal | github.com/paul-gauthier/aider |

### Agent Frameworks

| Framework | Description | GitHub |
|-----------|-------------|--------|
| LangChain | LLM application framework | github.com/langchain-ai/langchain |
| LangGraph | State-machine orchestration | github.com/langchain-ai/langgraph |
| CrewAI | Multi-agent framework | github.com/crewAIInc/crewAI |
| AutoGen | Multi-agent conversation | microsoft/autogen |
| LlamaIndex | Data indexing for LLMs | run-llama/llama_index |

### Agent Runtimes & Orchestrators

| Tool | Description | GitHub |
|------|-------------|--------|
| OpenClaw | AI agent runtime | github.com/openclaw/openclaw |
| Vibe Kanban | Kanban-based orchestrator | github.com/BloopAI/vibe-kanban |
| Emdash | Agentic development environment | github.com/generalaction/emdash |
| Composio | Agent orchestrator | github.com/ComposioHQ/agent-orchestrator |

---

## Concepts & Definitions

### Core Terms

| Term | Definition |
|------|------------|
| **Agent Harness** | Software infrastructure surrounding LLMs |
| **Harness Engineering** | Treat every Agent failure as permanent engineering fix |
| **Context Engineering** | Design dynamic systems to provide right information |
| **Context Rot** | Performance degradation as context grows |
| **Mechanical Verification** | Make correct behavior verifiable via tools |
| **Progressive Disclosure** | Progressive context disclosure |
| **Initializer-Executor Split** | Architecture pattern for long-running tasks |

---

## Architecture Patterns

### 1. Single-Agent Supervisor
- **Use Case**: Bounded tasks (customer service, Q&A)
- **Pattern**: LLM + Tools + Memory in a loop

### 2. Initializer-Executor Split
- **Use Case**: Long-running coding tasks
- **Pattern**: One-time setup + incremental execution

### 3. Multi-Agent Coordination
- **Use Case**: Complex projects
- **Pattern**: Coordinator + specialized agents

---

## Evaluation Metrics

| Metric | Description | Target |
|--------|-------------|--------|
| Merge Rate | AI PR merge rate | >80% |
| Review Cycle Time | AI PR review time vs human | <1.5x |
| Test Pass Rate | First test pass rate | >70% |
| Revert Rate | Post-merge revert rate | <5% |
| Session Continuity | Cross-session task success | >90% |

---

## Career Resources

### Salary Benchmark (2026 US)

| Level | Range |
|-------|-------|
| Junior (0-2 years) | $120K - $160K |
| Mid-level (2-5 years) | $160K - $220K |
| Senior (5+ years) | $220K - $300K+ |
| Lead/Architect | $280K - $400K+ |

---

## Contributing

To add resources:
1. Fork the repository
2. Add your resource to the appropriate section
3. Submit a PR

---

*Last Updated: March 2026*
