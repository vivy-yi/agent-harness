# Synthetic Computers at Scale for Long-Horizon Productivity Simulation

**arXiv ID:** 2604.28181
**Published:** 2026-04-30
**Authors:** Tao Ge, Baolin Peng, Hao Cheng
**Categories:** cs.AI, cs.CL, cs.HC

## Summary

Realistic long-horizon productivity work is strongly conditioned on user-specific computer environments, where much of the work context is stored and organized through directory structures and content-rich artifacts. This paper introduces "Synthetic Computers at Scale," a scalable methodology for creating realistic computer environments with folder hierarchies and content-rich artifacts (documents, spreadsheets, presentations).

Conditioned on each synthetic computer, the authors run long-horizon simulations: one agent creates productivity objectives specific to the computer's user and requiring multiple professional deliverables (~1 month of human work); another agent then acts as that user, navigating the filesystem, coordinating with simulated collaborators, and producing professional artifacts.

## Key Contribution

- **Synthetic Computer Generation:** Scalable creation of realistic computer environments
- **Long-Horizon Simulation:** Agents work across simulated months of productivity
- **Multi-Agent Setup:** One agent creates objectives, another executes
- **Ground Truth:** Realistic directory structures and content-rich artifacts

## Relevance to Agent Harness

- Directly relevant to agent productivity simulation and training
- Shows how to generate realistic training data for agent systems
- The "synthetic user" pattern is valuable for agent evaluation
- Highlights filesystem/context importance for agent performance