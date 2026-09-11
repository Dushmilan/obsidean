---
date: 2026-08-24
type: index
tags: [vault-map, navigation]
---

# Vault Index

> Master entry point. This page links **only** to the six main hubs — every hub owns its own detail. `University/Semester 1/` → `Physics/` 2026-09-01; `Knowledge Base/` → `Computer Science/` tracks 12–15 2026-09-01.

**Hierarchy rule:** `Vault-Index → main hub → section index → topic index → notes`. No skipped levels.

## 🌐 Main Vaults

| # | Subject | Hub | Notes | Status |
|---|---------|-----|-------|--------|
| 1 | 📐 Mathematics — A/L Pure & Applied · Uni modules · Linear Algebra · Info Theory | [[Maths]] | 308 | ✅ Pure & Applied done · LA L16–L35 pending |
| 2 | ⚛️ Physics — GCE A/L, 7 units | [[Physics_Index]] | 55 | ✅ Complete |
| 3 | 📊 Statistics — Probability · Regression · Inference | [[Stats_Index]] | 23 | ✅ Complete |
| 4 | 💻 Computer Science — Fundamentals · Languages · Databases · DSA · ML/DL/HF | [[Computer-Science_Index]] | ~203 | ✅ Complete — Knowledge Base merged as tracks 12–15 (2026-09-01) |
| 5 | 🤖 CS 188 — Intro to AI | [[CS-188_Index]] | 3 | 🟡 Started |
| 6 | 🛠️ Projects & Operations | [[OpenCode/INDEX|INDEX]] | 22 | 🔵 Active |

---

## 🧭 How to navigate

- **Know the subject?** Jump straight to its hub above.
- **Lost?** Any note → its `_Index` → hub → here.
- **Cross-subject links** live in each hub's `🌐 Main Vault Network` table.


*Last updated: 2026-09-01*

## 🕒 Recently Updated (live via Dataview)

```dataview
TABLE WITHOUT ID file.link AS "Note", dateformat(file.mtime, "yyyy-MM-dd HH:mm") AS "Modified"
WHERE contains(file.path, "Computer Science") OR contains(file.path, "Maths")
SORT file.mtime DESC
LIMIT 8
```

> Requires the **Dataview** community plugin (Settings → Community plugins → Browse → "Dataview" → Enable). Without it this block shows as plain code.
