# Agent Skills

A modular library of skills for AI coding agents (Claude Code, Cursor, Codex, Gemini, and other agent harnesses).

## Quick Start

Link the skills into your local agent directories (`~/.claude/skills` and `~/.agents/skills`):

```bash
./scripts/link-skills.sh
```

Every linked entry is a symlink into this repository, so keeping skills up to date is as simple as running `git pull`.

---

## Skills Catalog

### Engineering

Skills for daily software engineering workflows.

#### User-Invoked
Reachable when explicitly requested in conversation:

- **[implement-ticket](./skills/engineering/implement-ticket/SKILL.md)**: Multi-agent orchestration to ingest a ticket, branch (including stacked PRs via gh stack), implement with TDD, run two rounds of review and fixing, and open a PR.
- **[implement](./skills/engineering/implement/SKILL.md)**: Build work described by a spec or ticket, driving TDD at pre-agreed seams and closing out with code review.
- **[implement-spec](./skills/engineering/implement-spec/SKILL.md)**: Implement an entire spec on an integration branch with concurrent implementer subagents across the task frontier.
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)**: Interview session that sharpens ideas, aligns domain language, and records terminology in `GLOSSARY.md` and ADRs.
- **[show-me](./skills/engineering/show-me/SKILL.md)**: Explain concepts visually using concise diagrams, code sketches, or focused HTML artifacts.
- **[setup-skills](./skills/engineering/setup-skills/SKILL.md)**: Configure the repository for agent skills (issue tracker, triage labels, and domain doc layout).
- **[to-spec](./skills/engineering/to-spec/SKILL.md)**: Synthesize the current conversation into a comprehensive technical spec published to your issue tracker.
- **[to-tickets](./skills/engineering/to-tickets/SKILL.md)**: Break a spec or plan into tracer-bullet tickets with native blocking links on GitHub or relative markdown links locally.
- **[triage](./skills/engineering/triage/SKILL.md)**: Triage incoming bugs and feature requests through a structured state machine.
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)**: Audit the codebase for deepening opportunities and produce an actionable architecture report.
- **[thermo-nuclear-code-quality-review](./skills/engineering/thermo-nuclear-code-quality-review/SKILL.md)**: Run a strict maintainability review targeting giant files, abstraction health, and structural simplicity.
- **[retro](./skills/engineering/retro/SKILL.md)**: Analyze a coding session to suggest improvements to agent steering files, checks, and tooling.
- **[wayfinder](./skills/engineering/wayfinder/SKILL.md)**: Chart and resolve large, multi-session initiatives as a shared map of decision tickets.

#### Model-Invoked
Invoked directly by the user or reached autonomously by the agent:

- **[tdd](./skills/engineering/tdd/SKILL.md)**: Test-driven development with a red-green-refactor loop in vertical slices.
- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)**: Disciplined diagnosis loop for complex bugs and regressions before touching production code.
- **[code-review](./skills/engineering/code-review/SKILL.md)**: Two-axis review evaluating standards and spec compliance using parallel subagents.
- **[prototype](./skills/engineering/prototype/SKILL.md)**: Build throwaway prototypes to explore UI designs or validate logic.
- **[research](./skills/engineering/research/SKILL.md)**: Research questions against authoritative primary sources and produce cited notes.
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)**: Vocabulary and principles for designing deep modules with small interfaces and clean seams.
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)**: Build and maintain consistent domain models and architecture decision records.
- **[pr](./skills/engineering/pr/SKILL.md)**: Structured template for pull requests (summary visual, before/after evidence, blast radius).
- **[wizard](./skills/engineering/wizard/SKILL.md)**: Generate interactive bash wizards for tasks requiring human action.

---

### Productivity

General workflow and communication skills.

#### User-Invoked
- **[unslop](./skills/productivity/unslop/SKILL.md)**: Remove AI prose tells and fluff while preserving intended meaning and tone.
- **[grill-me](./skills/productivity/grill-me/SKILL.md)**: Stateless grilling interview to stress-test plans, decisions, or ideas.
- **[handoff](./skills/productivity/handoff/SKILL.md)**: Package conversation state into a portable summary for another session or collaborator.
- **[teach](./skills/productivity/teach/SKILL.md)**: Interactive teaching workspace for learning new concepts across sessions.
- **[to-questionnaire](./skills/productivity/to-questionnaire/SKILL.md)**: Create targeted questionnaires for external stakeholders.
- **[wait-what](./skills/productivity/wait-what/SKILL.md)**: Clarify confusing explanations in plain English using the project's glossary.

#### Model-Invoked
- **[grilling](./skills/productivity/grilling/SKILL.md)**: The underlying recursive interview engine for stress-testing requirements.
- **[writing-for-agents](./skills/productivity/writing-for-agents/SKILL.md)**: Guidelines for writing clear, actionable documents and skills for AI agents.

---

### In Progress

Experimental skills under active development:

- **[chief-of-staff](./skills/in-progress/chief-of-staff/SKILL.md)**
- **[claude-handoff](./skills/in-progress/claude-handoff/SKILL.md)**
- **[loop-me](./skills/in-progress/loop-me/SKILL.md)**
- **[setup-ts-deep-modules](./skills/in-progress/setup-ts-deep-modules/SKILL.md)**
- **[writing-beats](./skills/in-progress/writing-beats/SKILL.md)**
- **[writing-fragments](./skills/in-progress/writing-fragments/SKILL.md)**
- **[writing-shape](./skills/in-progress/writing-shape/SKILL.md)**

---

## License

[MIT](./LICENSE)
