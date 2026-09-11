# Performance & Rendering Optimization

React re-renders are cheap *until they aren't*. The default contract: when a component's state changes, it re-renders — and so do all its children, even if their props didn't change. **`React.memo`**, **`useMemo`**, and **`useCallback`** are the levers to skip unnecessary work. Code splitting and keys do more for real apps than micro-optimizations ever will.

**The Intuition:** Rendering is like re-printing a newspaper every time one story changes. React's default is to re-print every page (re-render everything) because it can't know which pages changed. `React.memo` is a post-it that says "only re-print if my stories actually changed." `useMemo`/`useCallback` make the *identity* of your data stable so those post-its work.

## When re-renders happen

```text
A component re-renders when:
1. its state changes (useState/useReducer)
2. its props change (parent re-rendered and passed new values)
3. its context value changes
4. its parent re-renders (children follow unless memoized)
```

The sneaky one is #4: a parent re-rendering re-renders the *whole subtree*. `React.memo` puts a boundary around a component so it only re-renders when its own props (by reference comparison) change.

## React.memo — skip re-renders on unchanged props

```jsx
const ExpensiveList = React.memo(function ExpensiveList({ items, onToggle }) {
  // only re-runs when items or onToggle actually change
  return <ul>{items.map(i => <li key={i.id} onClick={() => onToggle(i.id)}>{i.name}</li>)}</ul>;
});

function App() {
  const [query, setQuery] = useState('');
  const items = useMemo(() => getItems(), []);          // stable array
  const onToggle = useCallback((id) => toggle(id), []); // stable function

  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      {/* typing re-renders App, but ExpensiveList SKIPS because props are identical */}
      <ExpensiveList items={items} onToggle={onToggle} />
    </>
  );
}
```

**The catch:** `React.memo` compares props with `Object.is`. If you pass a *new* array or function every render (`items={filter(...)}`), the comparison always fails and memo is pointless — hence the `useMemo`/`useCallback` pairing.

## useMemo vs useCallback

```jsx
// useMemo — cache a VALUE (recompute only when deps change):
const total = useMemo(() => items.reduce((s, i) => s + i.price, 0), [items]);

// useCallback — cache a FUNCTION (stable identity until deps change):
const handleSave = useCallback(() => saveDraft(id), [id]);

// Both are just "memoize based on deps" — useCallback(fn, deps) ≡ useMemo(() => fn, deps)
```

**Rule of thumb:** these exist to keep *references stable* for memoized children and effect deps. They are **not** free performance wins — each adds overhead. Reach for them when you can point at the expensive child or the effect loop they prevent.

## The render pipeline & when NOT to optimize

```text
Measure first → identify the hot path → then memoize.

Don't:
- wrap every component in React.memo preemptively
- useMemo trivial arithmetic ("useMemo every computation")
- add deps-array gymnastics for a 3-item list
```

Real wins usually come from:
1. **Keys** — stable unique keys let React reuse DOM nodes and skip re-mounting
2. **Lifting state down** — put state as close as possible to where it changes
3. **Code splitting** — don't ship/parse code users never see
4. **Lists** — virtualization (`react-window`) for thousands of rows

## Code splitting — lazy + Suspense

```jsx
// Instead of one giant bundle, load routes/pages on demand:
import { lazy, Suspense } from 'react';

const Dashboard = lazy(() => import('./Dashboard'));   // split point

function App() {
  return (
    <Suspense fallback={<Spinner />}>
      <Dashboard />          {/* fetched only when first rendered */}
    </Suspense>
  );
}
```

**The intuition:** a big bundle is like loading the entire library before you enter. Code splitting loads each chapter when you walk into that room. Vite/Next do this automatically for route-level splits; `lazy` gives you component-level control.

## Automatic batching & the concurrent features

```jsx
// React 18+ batches updates even in async code — one render for several sets:
async function fetchUser() {
  setLoading(true);
  const data = await api.get('/user');
  setLoading(false);          // batched with setUser into one render
  setUser(data);
}

// useTransition — mark a state update as "non-urgent" (keep typing responsive):
const [isPending, startTransition] = useTransition();
startTransition(() => setFilter(query));   // UI stays responsive while filter recomputes
```

---

**Setup:** A search input that filters a 5,000-row table, without the table re-rendering while you type.

**Solution:**
```jsx
const Table = React.memo(function Table({ rows }) {
  return <tbody>{rows.map(r => <tr key={r.id}>...</tr>)}</tbody>;
});

function SearchPage() {
  const [query, setQuery] = useState('');
  const [rows] = useState(() => generateRows(5000));

  const visibleRows = useMemo(
    () => rows.filter(r => r.name.toLowerCase().includes(query.toLowerCase())),
    [rows, query]
  );

  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <Table rows={visibleRows} />
    </>
  );
}
```

**Key insight:** the *filter* is `useMemo`'d (the expensive part) and the *table* is memoized (so it only re-renders when `visibleRows` changes — which only happens when the query changes the filtered result). Typing re-renders `SearchPage`, but the 5,000-row table only re-renders when the result set actually changes.

---

**Setup:** A child with an interval-based animation must NOT re-render when the parent's unrelated state changes.

**Solution:**
```jsx
const Animation = React.memo(function Animation() {
  useEffect(() => {
    const t = setInterval(() => { /* animate */ }, 16);
    return () => clearInterval(t);
  }, []);
  return <canvas ref={...} />;
});

function App() {
  const [count, setCount] = useState(0);
  return (
    <>
      <button onClick={() => setCount(c => c + 1)}>{count}</button>
      <Animation />   {/* memo: does NOT re-render when count changes */}
    </>
  );
}
```

**Key insight:** without `React.memo`, every `count` change re-renders `Animation`. The memo boundary makes the animation independent of its parent's state churn. Note the effect is still mounted once — memo only affects renders, not lifecycle.

---

**Setup:** Why does `useCallback(() => x, [])` with empty deps freeze the captured value?

**Solution:** Empty deps = "stable forever" — the function closes over the *first render's* variables. Any value the callback reads must be in the deps, or it reads stale data. Same stale-closure contract as `useMemo` and `useEffect`.

**Key insight:** deps arrays are *contracts*. The exhaustive-deps linter exists to keep you honest — "fixing" it with `// eslint-disable-next-line` is where stale-state bugs breed.

---

## Practice (try before peeking)

1. Does `React.memo` compare props deeply or by reference?
2. When is `useMemo` genuinely worth it?
3. What's the actual first step of React performance work?

<details><summary>Answers</summary>

1. By reference (`Object.is` on each prop). Deep comparison would itself cost more than the re-renders it saves — that's why you must keep prop identities stable with `useMemo`/`useCallback`.
2. When the computation is expensive (large arrays, parsing, transforms) *and* it runs on every render with unchanged inputs — or when the memoized value's stable identity prevents a memoized child/effect from re-running. For trivial arithmetic it's overhead, not help.
3. Measure (React DevTools Profiler / `<Profiler>`) and find the actual hot path. Optimizing before measuring is guessing — most re-renders are cheap.

</details>

---

**Common traps:**
- Memoizing everything preemptively — overhead with no measured gain
- Passing new objects/functions to memoized children (kills the memo)
- Using the array index as a key on reorderable lists
- `useMemo`/`useCallback` with empty deps that read changing values (stale closures)
- Forgetting that memoization is per-component-instance — it doesn't share across parents

---
