# Team Dev — AI Agent Development Team

🌐 [Español](README.md) | **English**

A software development team defined in plain Markdown:
- **Language-agnostic**: adapts to each project's stack (Python, Go, Java, .NET, Node, etc.).
- **Tool-agnostic**: works with Claude Code, Cursor, GitHub Copilot, Codex, or any AI with file access.
- **Model- and provider-agnostic**: works with any AI harness, whether from OpenAI (GPT, Codex), Anthropic (Claude), DeepSeek, Google (Gemini), Mistral, Qwen, or local models (Ollama, LM Studio). It uses no provider-specific APIs, formats or features: it is just Markdown instructions that any model can follow.

> The role files are written in Spanish. Any current model can follow them, and you can write your tasks in English.

## Harness requirements
Any AI agent or harness works as long as it can:
1. **Read and write files** (the role files and the task folder).
2. **Run commands** in a terminal (the project's build, tests and lint).
3. *(Optional)* **Spawn subagents**. If it can, the CTO delegates each instance to a subagent and runs independent lanes in parallel. If not, the same agent plays each instance in turn.

Examples: Claude Code, OpenAI Codex CLI, Cursor, GitHub Copilot (agent mode), Aider, Cline, Roo Code, Continue, OpenHands, Gemini CLI, or a custom harness built on the OpenAI, Anthropic or DeepSeek APIs, among others.

## Roles
| File | Role | Instances | Responsibility |
|---|---|---|---|
| [cto.md](cto.md) | CTO | 1 | Receives the task, detects the stack, hires the team and coordinates the flow |
| [project-manager.md](project-manager.md) | Project Manager | 1 | Produces the plan: subtasks, lanes, complexity and acceptance criteria |
| [ux-designer.md](ux-designer.md) | UX Designer | 0–6 | Designs the interface before development and validates it during QA (only when there is a UI) |
| [senior-developer.md](senior-developer.md) | Senior Developer | 1–6 | Implements their lane as an expert in the stack assigned by the CTO |
| [code-reviewer.md](code-reviewer.md) | Code Reviewer | 1–6 | Reviews code by lane or specialty; approves it or sends it back |
| [qa-engineer.md](qa-engineer.md) | QA Engineer | 1–6 | Validates quality and requirements; approves or rejects |

## Scalable team (1 to 6 per role)
The CTO decides how many instances of each role to "hire" based on the plan's complexity:

1. The PM splits the work into **lanes** (C1, C2…): blocks with their own files and stack that a single developer carries from start to finish. The PM also estimates complexity (S/M/L/XL) and flags whether there is a UI.
2. The CTO sets up the team and records it in `00-equipo.md`. Each instance has an ID (`dev-1`, `rev-2`, `qa-1`, `ux-1`), a specialization (e.g. `dev-1` senior Go for the backend, `dev-2` senior React for the frontend) and a scope.

| Complexity | Dev | Review | QA | UX (if UI) |
|---|---|---|---|---|
| S | 1 | 1 | 1 | 1 |
| M | 2 | 1–2 | 1 | 1 |
| L | 3–4 | 2–3 | 2 | 1–2 |
| XL | 5–6 | 3–6 | 2–6 | 2–3 |

Rules: at most 6 per role; never more devs than lanes; each lane has 1 dev and at least 1 reviewer; UX = 0 when there is no interface. Independent lanes move in parallel if the harness supports subagents; otherwise, one after another.

**Examples**
- *"Fix the tax calculation in the API"* → complexity S, 1 lane: 1 dev, 1 review, 1 QA, 0 UX.
- *"User sign-up module: API, web screen and email notifications"* → complexity L, 3 lanes: 3 dev, 2 review (one of them for security), 2 QA, 1 UX.

## Flow
```
Task → CTO → PM → Hiring → UX Design → Devs ⇄ Review (max. 3 per lane) → QA + UX Validation (max. 2) → Close
                                          ↑                                     │
                                          └──── defects go back to the lane ────┘
```

- If the PM or UX raise blocking questions, the CTO stops and asks you.
- A lane that needs changes goes back only to its dev; approved lanes keep going.
- If the iteration limits are exceeded, the CTO stops and asks you for a decision.
- All communication between roles happens through files in `tareas/<date>-<slug>/` (K = instance number, N/M = iteration):

| File | Produced by |
|---|---|
| `00-brief.md` | CTO: task, project, stack and per-lane status |
| `00-equipo.md` | CTO: hired team and rationale |
| `01-plan.md` | Project Manager |
| `01-ux-<K>.md` | UX Designer (design) |
| `02-dev-<K>-<N>.md` | Senior Developer |
| `03-review-<K>-<N>.md` | Code Reviewer (verdict per lane) |
| `04-qa-<K>-<M>.md` | QA Engineer |
| `04-ux-<K>-<M>.md` | UX Designer (validation) |
| `05-resumen.md` | CTO: task wrap-up |

## Installation
```bash
git clone https://github.com/h3ct0rg/team-develop.git
```
No dependencies. You only need an AI tool that can read and write files and run commands (to run the project's build and tests).

## Running a task
Open your AI tool (ideally in the folder of the project you'll work on) and type:

```
Read <path-to-team-develop>/cto.md and, acting as the CTO, run the task:
"<task description>"
Project: <project path>
```

Example:
```
Read C:/repos/team-develop/cto.md and, acting as the CTO, run the task:
"Add a POST /users endpoint with email validation and tests"
Project: C:/repos/my-api
```

If you omit `Project`, the current working directory is used.

### By tool
- **Claude Code**: run `claude` in the project folder and paste the prompt. The CTO will use subagents for each role.
- **Cursor / Copilot (agent mode)**: open the project, add `team-develop` to the workspace or reference the path to `cto.md`, and paste the prompt into the agent chat.
- **Codex or other CLIs**: run it in the project folder with the same prompt.

If the tool doesn't support subagents, the same AI plays each role in turn, following its file.

## Customization
- Edit the role files to add your team's standards (conventions, minimum coverage, security checklist).
- Adjust the iteration limits and the team sizing table in [cto.md](cto.md).
- The brief, team and wrap-up templates are in [tareas/_plantilla/](tareas/_plantilla/).
