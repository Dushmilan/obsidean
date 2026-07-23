---
date: 2026-07-18
type: index
tags: [vault-map, navigation]
---

# OpenCode Vault — Index

**Agent Entry Point:** Read this file first to understand the vault structure.

## Quick Navigation

| Folder            | Purpose                              | When to Read              |
| ----------------- | ------------------------------------ | ------------------------- |
| `Profile/`        | Who Dushmilan is, preferences, stack | Every session             |
| `Memory/`         | Rules, protocols, session state      | Every session             |
| `Knowledge Base/` | Technical concepts, patterns, papers | On-demand                 |
| `Decisions/`      | Architectural/design reasoning       | When making decisions     |
| `Projects/`       | Active project status                | When working on a project |
| `Sessions/`       | Session logs                         | When reviewing past work  |
| `Inbox/`          | Raw ideas, follow-ups                | To process pending items  |

## Profile Files (Always Load)

- `Profile/identity.md` — Name, background, personality, lifestyle
- `Profile/tech-stack.md` — Languages, frameworks, AI/ML, tools
- `Profile/coding-style.md` — Architecture, conventions, naming
- `Profile/design-philosophy.md` — Aesthetic, animation, UI principles
- `Profile/goals.md` — Projects, career goals, communication preferences

## Memory Files (Always Load)

- `Memory/rules.md` — Session protocols, dumping rules, workflow
- `Memory/session-handoff.md` — Current session state (update at session end)

## On-Demand Context

Load these only when the task requires them:

- `Knowledge Base/Concepts/*.md` — Technical concepts
- `Knowledge Base/Code Patterns/*.md` — Reusable patterns
- `Knowledge Base/Papers/*.md` — Academic papers
- `Decisions/*.md` — Past architectural decisions
- `Projects/*.md` — Specific project status

## Skill Routes

| Task Type | Skills to Load |
|-----------|---------------|
| Frontend design / UI | `frontend-design`, `ui-ux-pro-max`, `high-end-visual-design` |
| Animation / motion | `gsap` |
| Code review | `caveman-review` |
| Prototyping | `prototype` |
| Web app testing | `webapp-testing`, `playwright-visual-testing` |
| Documents (Word) | `docx` |
| Documents (PDF) | `pdf` |
| Presentations | `pptx` |
| Spreadsheets | `xlsx` |
| MCP servers | `mcp-builder` |
| Cloudflare / Workers | `cloudflare`, `workers-best-practices`, `wrangler` |
| Creating new skills | `skill-creator` |
| Planning / stress-testing | `grill-me` |
| Breaking into issues | `to-issues` |
| Brand / visual identity | `brand-guidelines`, `brandkit` |
| TDD | `tdd` |
| Architecture improvement | `improve-codebase-architecture` |

## Session Protocol

1. **Start:** Read `Profile/` + `Memory/rules.md` + `Memory/session-handoff.md`
2. **During:** Save to `Knowledge Base/`, `Decisions/`, or `Inbox/` as needed
3. **End:** Update `Memory/session-handoff.md` with current state
