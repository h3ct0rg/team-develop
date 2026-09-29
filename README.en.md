# Team Dev — an efficient AI-agent development team

[Español](README.md) | **English**

Team Dev defines a development team in plain Markdown. It is language-, model-, and harness-agnostic, with coordination designed not to become the main token cost.

Any environment able to read/write files and run commands can use it. Subagents are optional; without them, one AI takes the required roles in sequence.

## Efficient by default

The CTO does not staff from a fixed complexity quota. An extra agent requires evidence of need: independent work with no shared files, a missing specialty, risk-driven independent coverage, or a clear reduction in waiting time.

| Risk | Initial team |
|---|---|
| Low: localized, tested change | 1 developer + automated gate |
| Medium: multiple modules or internal contract | 1 developer + 1 reviewer |
| High: migration, security, data, concurrency, or public API | 1 developer + 1 reviewer + 1 QA |

Start with at most two active developers, one reviewer, and one QA. Every expansion must be justified in the team record. UX is used only for a visual interface or human-facing flow change.

## Compact communication

The task directory is the source of truth. The CTO owns a canonical state file; other roles write only deltas: acceptance criteria covered, paths/symbols changed, evidence, new decisions, and blockers.

- Briefs, plans, and full histories are not resent between agents.
- Clean reports stay short; detail is reserved for reproducible defects, risks, or irreversible decisions.
- One integrated gate runs build, lint, and the full suite. Review and QA reuse valid evidence.
- A rejection returns only the affected lane; approved work is not restarted.

For migrations, split work at stable boundaries — project, library, or layer — rather than per file or class.

## Roles

| File | Role | Used when |
|---|---|---|
| [cto.md](cto.md) | CTO | Always: triage, staffing, coordination |
| [project-manager.md](project-manager.md) | PM | Ambiguous, broad, or dependent scope |
| [senior-developer.md](senior-developer.md) | Developer | Implementation by file boundary |
| [code-reviewer.md](code-reviewer.md) | Reviewer | Medium/high risk or warning signal |
| [qa-engineer.md](qa-engineer.md) | QA | Integration gate or high risk |
| [ux-designer.md](ux-designer.md) | UX | Visual interface changes |

## Workflow

```text
Task → CTO triage → [PM plan when needed] → smallest team
     → developer(s) on independent boundaries → proportional review
     → integrated gate (build/lint/tests + relevant QA/UX) → close
```

Independent lanes may run in parallel; lanes sharing a contract are sequenced. The limits are three fix cycles per lane and two gate rejections before escalating to the user.

## Task files

```text
tareas/<date>-<slug>/
  00-brief.md       objective, risk, ACs, boundaries, commands
  00-estado.md      canonical state; CTO writes it
  00-equipo.md      agents used and staffing rationale
  01-plan.md        optional; only for non-trivial scope
  01-ux-<K>.md      optional UX criteria
  02-dev-<K>-<N>.md compact development delta
  03-review-<K>-<N>.md focused verdict and findings
  04-qa-<K>-<M>.md integration gate
  05-resumen.md     result and coordination metrics
```

Templates live in [tareas/_plantilla](tareas/_plantilla).

## Usage

```text
Read <path-to-team-develop>/cto.md and act as CTO for this task:
"<task description>"
Project: <project path>
```

The final report includes agents actually used, justified expansions, estimated handoffs/words, and reused checks. No dependencies or provider-specific APIs are required.
