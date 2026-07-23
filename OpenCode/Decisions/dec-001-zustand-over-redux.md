---
date: 2026-07-18
type: decision
status: active
tags: [state-management, frontend]
---

# Decision: Zustand over Redux

## Context
Chose a state management solution for Twinkle-Hearts and CodeCoach-AI frontends.

## Options Considered
- **Redux Toolkit:** Heavy boilerplate, lots of ceremony for simple state
- **Zustand:** Minimal API, built-in persist middleware, works outside React
- **Jotai/Recoil:** Atomic state — overkill for current needs

## Chosen Option
**Zustand** — zero boilerplate, persist middleware for localStorage/IndexedDB, works with vanilla JS for cart sync module, simpler mental model.

## Consequences
- State lives in stores with standalone selectors
- Persist middleware handles hydration automatically
- Can use Zustand stores outside React (cart-sync worker)

## Related
- `[[twinkle-hearts]]`
- `[[codecoach-ai]]`
- `[[Profile/design-philosophy]]`
