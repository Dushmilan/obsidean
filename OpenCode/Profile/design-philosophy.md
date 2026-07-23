---
date: 2026-07-18
type: profile
tags: [design, philosophy, animation, ui]
---

# Design Philosophy

## Core Principles
- **Warm, boutique aesthetic** — beige/canvas backgrounds, rose/sage accents, never pure black/white
- **Intentional, non-templated** — custom color systems per project
- **Unique visual identity per project** — anti-generic, anti-template
- **Offline-first PWA** approach (IndexedDB, service workers)
- **Dark-by-default** themes with light mode support

## UI Style
- **Pill-shaped UI** — `rounded-full` for buttons/badges, `rounded-card` (16px) for containers
- **Glassmorphism** (acrylic) in WSO2 work, clean flat in personal projects
- **Grain/noise texture** overlays for paper feel
- **Typography:** Geist, Playfair Display, Public Sans

## Animation Style
- GSAP timelines in `useEffect` (imperative, not declarative CSS)
- Custom cubic-bezier easings (`spring: cubic-bezier(0.16, 1, 0.3, 1)`)
- Scroll-triggered reveals (ScrollTrigger plugin)
- 3D card tilt on hover (perspective transforms)
- Magnetic button effects (cursor-follow + spring-back)
- Staggered child animations

## Design Tokens
- Custom brand colors via `tailwind.config.js` `extend.colors`
- No CSS modules, no styled-components — Tailwind only

## WSO2 Oxygen-UI Exposure
- MUI v7 + Emotion CSS-in-JS
- ExtendTheme with CSS variables (`--oxygen-*`)
- 9 built-in themes: Acrylic, Classic, HighContrast, Pale variants, WSO2
- Compound component pattern (Shell: Navbar + Sidebar + Main)
- Contribution: fix text overflow (#529)
