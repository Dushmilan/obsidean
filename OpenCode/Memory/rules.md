---
date: 2026-07-18
type: memory
tags: [rules, protocol, workflow]
---
	
# Rules & Protocols

## Vault Structure
```
OpenCode/
  INDEX.md                    # Vault map (read first)
  Profile/                    # Who Dushmilan is
  Memory/                     # Rules, session state
  Knowledge Base/Concepts/    # Technical concepts
  Knowledge Base/Code Patterns/ # Reusable patterns
  Knowledge Base/Papers/      # Academic papers
  Decisions/                  # Architectural decisions
  Projects/                   # Active project status
  Sessions/                   # Session logs
  Inbox/                      # Raw ideas, follow-ups
```

## Session Workflow
1. **Start:** Read `INDEX.md` → `Profile/` → `Memory/rules.md` → `Memory/session-handoff.md`
2. **During:** Save to `Knowledge Base/`, `Decisions/`, or `Inbox/` as needed
3. **End:** Update `Memory/session-handoff.md` with current state

## Dumping Protocol
1. **Analyze** — separate signal from noise, identify new vs repeat
2. **Filter** — strip filler, deduplicate, remove empty sections
3. **Categorize** — distribute to correct folders
4. **Update individually** — append to existing notes, create new only for novel topics
5. **Confirm** — show which folders were updated

## Rules
- No raw dumps. Everything gets filtered before saving.
- No JSON exports or AI-format artifacts in the vault.
- Empty/null sections are stripped, not saved.
- Every significant decision gets its own file in `Decisions/`
- Every technical concept/paper/pattern goes in `Knowledge Base/`
- Random mid-session thoughts go in `Inbox/` immediately
- All notes use Obsidian wikilinks for graph connectivity
- Never write AI-sounding language in PRs, commits, comments, or any public-facing communication. Messages must sound human, direct, and natural — no boilerplate, no robotic phrasing, no filler.

## Communication Rules

Concrete anti-patterns that sound AI-generated. Avoid all of these in
commits, PRs, comments, or any public-facing text.

- **No hallucinated keywords** — Never reference features or changes not
  actually in the diff (e.g., "group fetching" when no group code was touched)
- **No fabricated terminology** — Don't call something "optimistic" if the
  code waits for API success before applying state. Use accurate terms.
- **No parenthetical filler** — Avoid "(security fix)", "(sensitive data)",
  "to prevent X" after every bullet. Let the diff speak. If it needs explanation,
  write a sentence, not a tag.
- **No mechanical spec-speak** — Don't write "validate/enforce/idempotent/
  refactor" as a checklist. Write natural sentences a teammate would say.
- **No terminal garbage** — Never paste raw output with ANSI escape codes
  (`[103;5u`) into PR bodies or commit messages.
- **No meta-commentary** — No "Key changes from the AI version" or
  process notes in PR descriptions. The PR is the artifact.
- **No padding sections** — Every section must describe actual changes in
  the diff. No "Debug Account Switcher" or invented feature sections.

## Coding Discipline (Karpathy)

1. **Think Before Coding:** Ask, don't guess. Surface assumptions. If multiple interpretations exist, present them — don't pick silently.
2. **Simplicity First:** No speculation, no unrequested abstractions. Single-use code stays single-use. If you wrote 200 lines and it could be 50, rewrite it.
3. **Surgical Changes:** Touch only scope. Match existing style. Clean your own mess. One logical change per commit.
4. **Goal-Driven:** Define success upfront. Verify done before shipping. Convert vague tasks to measurable criteria.

### Pre-Code Checklist
Before writing any code:
1. What is the exact requirement?
2. What am I uncertain about?
3. Is there a simpler way?
4. Should I ask or assume?

### Post-Code Quality Gate
Before finishing:
1. Does every line serve the stated requirement?
2. Could this be half as long?
3. Are there any abstractions nobody asked for?
4. Did I match the existing code style?
5. Did I leave any debug code or temporary variables?

## Scope Gate (Overcommitment Mitigation)

Before starting new work, ask:
1. How many active tasks am I juggling?
2. Does this new task displace an existing one?
3. Is this excitement-driven or deadline-driven?
4. Can this be deferred to next session?

If starting a 4th+ parallel task: STOP. Finish one first.

## Ship Checklist

Before marking anything "done":
1. Did I verify the success criterion?
2. Did I run the relevant tests?
3. Did I check for regressions?
4. Is there debug code or temp variables to clean?
5. Could this be split into smaller changes?

## Pre-Commit/PR Protocol (Caveman Review)

Before ANY commit, push, or PR:
1. Run `git diff` or `git diff --staged` to see what's changing
2. Load the `caveman-review` skill
3. Review the diff using caveman-review format (one line per finding: location, problem, fix)
4. Fix any 🔴 bugs or 🟡 risks found before proceeding
5. Then commit/push/PR

**No exceptions.** This is not optional. Even for "small" changes.

## Decision Log Format

Every file in `Decisions/` must include:
- **Decision:** What was decided
- **Rationale:** Why this over alternatives
- **Confidence:** 1-10
- **Supersedes:** (optional) which prior decision this reverses
- **Review date:** When to re-check if still sound

## Auto-Project Rule

Starting a new project → auto-create `Projects/<project-name>.md` with goals + progress log.
Applies to personal projects AND open-source contributions.
