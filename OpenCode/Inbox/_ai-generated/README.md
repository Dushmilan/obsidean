---
date: 2026-07-23
type: readme
tags: [ai-generated, workflow]
---

# AI Generation Zone

**Purpose:** Staging area for AI-generated content before review and integration.

## Workflow

1. OpenCode generates content into `physics-unit-{N}/`
2. Review each file for accuracy
3. Move validated files to `Physics/{Unit-Folder}/`
4. Delete or archive rejected generations
5. Git commit after each batch

## Naming Convention

- Physics: `{NN.M}-{Topic-Name}.md` (matches Physics Unit 1 pattern)
- Linear Algebra: `{NN}-{Cluster-Name}.md`

## What Goes Here

- Physics concept notes (Units 2-7)
- Linear Algebra clusters (L16-L35, if generated)
- Audio ML research summaries (future)

## What Doesn't Go Here

- Profile updates → `OpenCode/Profile/`
- Project decisions → `OpenCode/Decisions/`
- Raw ideas → `OpenCode/Inbox/` (root level)
