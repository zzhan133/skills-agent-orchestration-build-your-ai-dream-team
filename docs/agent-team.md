# Project Pulse agent team

Mona's Project Pulse dashboard will be built by a coordinated team of custom agents defined in `.github/agents/`.

| Agent | Model | Source file | Responsibility |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 | `.github/agents/orchestrator.agent.md` | Coordinates the team, turns the plan into phased work with explicit file ownership, manages dependencies, and verifies the integrated result. |
| Planner | Claude Opus 4.7 | `.github/agents/planner.agent.md` | Researches the repository and requirements, identifies risks and edge cases, and produces an implementation plan with dependencies and validation expectations. |
| Designer | Gemini 3.1 Pro | `.github/agents/designer.agent.md` | Defines the dashboard's UI/UX, accessibility, information hierarchy, responsive behavior, and polished Project Pulse visual direction. |
| Coder | GPT-5.5 | `.github/agents/coder.agent.md` | Implements the assigned dashboard files with explicit, testable behavior and creates runnable support configuration when assigned. |

The Orchestrator will first ask the Planner for a Project Pulse implementation strategy. It will then assign non-overlapping design and implementation work to the Designer and Coder, running independent work in parallel and dependent work sequentially. Finally, the Orchestrator will verify that the dashboard, including its launch configuration, works as one coherent result.
