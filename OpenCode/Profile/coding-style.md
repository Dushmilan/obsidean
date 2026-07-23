---
date: 2026-07-18
type: profile
tags: [coding-style, conventions, architecture]
---

# Coding Style

## Architecture Patterns
- **Full-stack monorepos** with workspace packages (npm/pnpm)
- **Clean Architecture / Hexagonal** in Python (Ports & Adapters + DI)
- **Feature-organized** frontend code (not by technical role)
- **Service + Repository layers** on backend
- **Zustand** for state management (no Redux)

## TypeScript Conventions
- Strict mode, full interfaces, no `any`
- `import type` for type-only imports
- `PascalCase` components, `camelCase` functions
- Absolute aliases (`@/`), groups: stdlib → third-party → local

## Python Conventions
- Full type annotations, Pydantic v2 schemas
- Module-level loggers
- `snake_case` for functions and variables
- Clean Architecture / Hexagonal pattern

## Formatting
- **Prettier:** 100 char, single quotes, trailing commas
- **Ruff:** Python linting and formatting
- **Pre-commit:** Husky + lint-staged (ESLint + Prettier on staged files)

## Import Organization
1. Standard library
2. Third-party packages
3. Local/project imports
- Separated by blank lines
