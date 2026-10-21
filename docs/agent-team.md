# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the custom agent team building Mona's Project Pulse dashboard.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates the specialists, turns the plan into phases, assigns explicit file scopes, manages dependencies, and verifies the integrated result without implementing code itself. | [orchestrator.agent.md](../.github/agents/orchestrator.agent.md) |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository and documentation, then produces an ordered implementation plan with file assignments, dependencies, edge cases, and validation expectations. Does not write code. | [planner.agent.md](../.github/agents/planner.agent.md) |
| Coder | GPT-5.5 (copilot) | Implements dashboard logic and assigned runnable-app support, keeps errors explicit and behavior testable, and validates changes within its assigned file scope. | [coder.agent.md](../.github/agents/coder.agent.md) |
| Designer | Gemini 3.7 Flash (copilot) | Creates a polished, accessible, responsive dashboard with project cards, status badges, clear priorities, and consistent visual hierarchy within its assigned file scope. | [designer.agent.md](../.github/agents/designer.agent.md) |

The Orchestrator starts with the Planner's plan and delegates implementation to the Coder and Designer. Independent tasks can run in parallel only when their file scopes do not overlap and they have no data dependencies; dependent or overlapping work runs sequentially. The learner retains control of staging, committing, and pushing changes through Copilot CLI prompts.
