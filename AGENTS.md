# Obsidian Vault — Agent Rules

Working directory: `/home/dushmilan/Documents/Obsidian Vault`
Entry point: `Vault-Index.md` → hub (e.g. `Computer Science/Computer-Science_Index.md`) → section `_Index.md` → notes.
Hierarchy rule: `Vault-Index → main hub → section index → topic index → notes`. Never skip levels.

## Session protocol
1. Start: read `OpenCode/INDEX.md` → `OpenCode/Profile/` → `OpenCode/Memory/rules.md` → `OpenCode/Memory/session-handoff.md`
2. During: save concepts to correct track, raw ideas to `OpenCode/Inbox/`, decisions to `OpenCode/Decisions/` with Decision/Rationale/Confidence/Supersedes/Review date
3. End: update `OpenCode/Memory/session-handoff.md`

## Note conventions
- Links: always `[[wikilinks]]`, never markdown links. Keep graph connected.
- Concept notes (e.g. `Computer Science/Node/HTTP Servers and the Request Lifecycle.md`): no frontmatter. Start with 1-sentence definition, then `**The Intuition:**` analogy, sections with code, `## Practice (try before peeking)` with `<details>` answers, `**Common traps:**` list, 1-line takeaway.
- Index notes (`*_Index.md`, `Vault-Index.md`): keep frontmatter (`date, type, tags`), tables, `**Up:** [[parent]]` footer.
- Templates live in `Templates/` (`IT-Note-Template.md`, `HF-LLM-Course-Template.md`). Match the closest existing note's style before creating.
- File naming: Title Case with spaces (e.g. `TLS-SSL Handshake.md` → `TLS-SSL Handshake.md` as `TLS/SSL Handshake` title inside ok, filename avoid `/`).
- "I learned X today" → identify track, `Read` sibling notes + section `_Index.md`, `Write` new note, update `_Index.md` table.
- Never leave `rejectUnauthorized: false`-style secrets, AI filler, or JSON artifacts in notes.

## Tooling
- Prefer `Read`/`Glob`/`Grep` over `cat`/`find`/`grep`. Use `workdir` param, never `cd`.
- Verify parent dir with `ls` before `mkdir`/`Write` of new paths.
- You have `dataview` + `remotely-save` plugins. Dataview blocks require the plugin — don't break them.
