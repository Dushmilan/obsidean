# Modules & npm

Node organizes code into **modules** — files that export values and import others'. Two systems coexist: **CommonJS** (`require`/`module.exports`, the classic default) and **ES Modules** (`import`/`export`, the modern standard). **npm** is the package manager: it installs dependencies, resolves versions, and locks them for reproducible builds.

**The Intuition:** Modules are like chapters in a book — each file has a defined scope (nothing leaks out except what you explicitly export) and a clear table of contents (`require`/`import`). npm is a library with a card catalog: `package.json` is your library card list, `package-lock.json` is the exact shelf location of every book so two machines build the same library.

## CommonJS — the classic system

```js
// math.js
const PI = 3.14159;

function area(r) {
  return PI * r * r;
}

module.exports = { area, PI };        // export an object

// main.js
const { area } = require('./math');   // import — relative path
console.log(area(2));                 // 12.56636
```

**Rules:**
- `require()` is synchronous and **cached** — a module executes once; later requires return the same exports object
- One module = one file; `module.exports` is what `require` returns
- Bare specifiers (`require('express')`) resolve from `node_modules`; relative paths (`./math`) resolve from the file

## ES Modules — the modern standard

```js
// math.js  (package.json needs "type": "module", or file .mjs)
export const PI = 3.14159;

export function area(r) {
  return PI * r * r;
}

export default function greet(name) {     // default export — one per module
  return `Hello, ${name}`;
}

// main.js
import greet, { area, PI } from './math.js';   // default + named
```

**Differences from CommonJS:**
- `import` is **hoisted** and can be static-analysed (enables tree-shaking)
- **Top-level `await`** works in ESM without an async wrapper
- ESM is the spec-compliant standard; CommonJS is Node's historical system

| | CommonJS | ES Modules |
|---|---|---|
| Syntax | `require` / `module.exports` | `import` / `export` |
| Loading | synchronous, cached | static, hoisted |
| File signals | `.js` (default) | `.mjs` or `"type": "module"` |
| Tree-shaking | no | yes (static analysis) |
| Top-level await | no | yes |

## package.json — the manifest

```json
{
  "name": "my-api",
  "version": "1.0.0",
  "type": "module",
  "main": "src/index.js",
  "scripts": {
    "dev": "node --watch src/index.js",
    "start": "node src/index.js",
    "test": "node --test"
  },
  "dependencies": {
    "express": "^4.19.0"
  },
  "devDependencies": {
    "vitest": "^1.0.0"
  }
}
```

- `dependencies` — runtime packages (deployed with the app)
- `devDependencies` — build/test/lint tooling (not installed in production)
- `scripts` — your command aliases; `npm run dev`, `npm test`

## Semantic versioning & the lockfile

```text
"express": "^4.19.0"
             │  │  └─ patch  — bug fixes       (4.19.1, 4.19.2...)
             │  └──── minor — backward-compatible features (4.20, 4.21...)
             └─────── major — breaking changes (5.0.0 would NOT be allowed)

^4.19.0  →  4.x.x but not 5.x (caret: allow minor & patch)
~4.19.0  →  4.19.x (tilde: allow patch only)
4.19.0   →  exactly this version
```

**The lockfile** (`package-lock.json`) pins the *exact* resolved version of every dependency and transitive dependency. Two machines running `npm ci` get byte-identical trees. `package-lock.json` should be committed; `node_modules` never should.

## npm in practice

```bash
npm init -y                    # create package.json
npm install express            # add to dependencies + install
npm install -D vitest          # add to devDependencies
npm uninstall express          # remove
npm ci                         # clean install from lockfile (CI use)
npm run dev                    # run a script
npm ls --depth=0               # top-level installed packages
npm outdated                   # see which deps have updates
```

**`npm ci` vs `npm install`:** `ci` wipes `node_modules` and installs *exactly* what the lockfile says — reproducible, fast, correct. `install` may update the lockfile. Always `ci` in CI/CD and when onboarding.

## node_modules & resolution

```text
my-app/
  package.json
  package-lock.json
  node_modules/          ← installed deps (never commit)
    express/
    ...
  src/
```

Resolution order for `require('express')`: look in `./node_modules`, then `../node_modules`, walking up until the filesystem root. That's how nested dependencies of different versions can coexist.

---

**Setup:** Split a utility module and import it in two places.

**Solution:**
```js
// src/format.js
export function formatDate(ts) {
  return new Date(ts).toISOString().slice(0, 10);
}
export function formatMoney(n) {
  return `$${n.toFixed(2)}`;
}

// src/users.js
import { formatDate } from './format.js';
export function userRow(u) {
  return `${u.name} — joined ${formatDate(u.joinedAt)}`;
}

// src/orders.js
import { formatDate, formatMoney } from './format.js';
```

**Key insight:** one module, named exports, imported anywhere. ESM's static analysis means bundlers can tree-shake — unused exports (`formatMoney` where it's not imported) get dropped from production bundles.

---

**Setup:** A Node script that reads a JSON config at startup, using ESM with top-level await.

**Solution:**
```js
// config.js  ("type": "module")
import { readFile } from 'node:fs/promises';

const config = JSON.parse(
  await readFile(new URL('./config.json', import.meta.url), 'utf8')
);

export default config;

// app.js
import config from './config.js';
console.log(config.port);          // config is ready before any other code runs
```

**Key insight:** top-level `await` lets you load async resources *before* the rest of the module body runs — no async wrapper needed. `import.meta.url` gives the current file's URL, which is the correct way to build absolute paths in ESM (there's no `__dirname`).

---

**Setup:** You cloned a repo — what commands get it running?

**Solution:**
```bash
npm ci        # exact deps from package-lock.json
npm run dev   # start the dev server
```

**Key insight:** `npm ci` guarantees the same dependency tree as the team's — no "works on my machine" version drift. Never use `npm install` for onboarding; it can silently update the lockfile with newer compatible versions.

---

## Practice (try before peeking)

1. What does `^` mean in `"express": "^4.19.0"` — and would `npm install` upgrade to 5.x?
2. `npm ci` vs `npm install` — which for CI and why?
3. CommonJS `require` vs ESM `import` — which enables tree-shaking?

<details><summary>Answers</summary>

1. Caret allows minor and patch updates within the same major version: `4.19.x`, `4.20.x`, etc. — but never `5.x`, because the major bump means breaking changes. `npm install` may resolve to the newest allowed (`4.x`), while the lockfile pins the exact installed version.
2. `npm ci` — it installs strictly from the lockfile, produces identical trees, and fails fast if the lockfile is out of sync. `npm install` mutates the lockfile and is for development when you're intentionally changing dependencies.
3. ESM `import` — its static, hoisted form lets tools know exactly which exports are used and drop the rest. CommonJS's dynamic `require` can't be analysed this way.

</details>

---

**Common traps:**
- Committing `node_modules` (huge, machine-specific — gitignore it)
- Not committing `package-lock.json` (breaks reproducible installs)
- Using `npm install` in CI instead of `npm ci`
- Mixing `require` and `import` in the same file without a build step
- `^0.x` versioning gotcha — `0.x.y` semver treats minor as breaking, so `^0.2.0` allows `0.2.x` only, not `0.3.0`

---
