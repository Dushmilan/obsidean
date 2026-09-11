# Hooks Deep Dive

Beyond `useState` and `useEffect`, the hook toolbox includes `useRef` (mutable values that don't trigger re-renders), `useMemo`/`useCallback` (performance), and **custom hooks** (the real superpower — extracting reusable logic). Rules of Hooks govern them all.

**The Intuition:** Hooks are React's way of giving function components "state-like" superpowers while keeping rendering pure. `useRef` is a box that survives renders without causing them. `useMemo`/`useCallback` cache expensive work. Custom hooks let you package any logic — fetching, form handling, timers — into a reusable function starting with `use`.

## useRef — mutable, persistent, no re-render

```jsx
function Stopwatch() {
  const startTime = useRef(0);     // persists across renders
  const [elapsed, setElapsed] = useState(0);

  const start = () => {
    startTime.current = Date.now();     // mutate .current freely — no re-render
    setInterval(() => {
      setElapsed(Date.now() - startTime.current);
    }, 100);
  };
  // startTime.current changes do NOT trigger renders — that's the point.
}
```

**Three uses:**
1. **Persistent mutable value** that shouldn't trigger renders (timers, ids)
2. **DOM reference**: `const input = useRef(null); <input ref={input} />` then `input.current.focus()`
3. **Holding the latest value** to avoid stale closures

```jsx
// DOM ref:
function AutoFocus() {
  const inputRef = useRef(null);
  useEffect(() => { inputRef.current?.focus(); }, []);
  return <input ref={inputRef} />;
}

// Latest-value ref (avoids stale closures in effects/timers):
const latestProps = useRef(props);
useEffect(() => { latestProps.current = props; });   // update every render
// now an interval created once can read latestProps.current — always fresh
```

## useMemo — cache expensive calculations

```jsx
const expensive = useMemo(() => {
  return items.filter(i => i.qty > 0).reduce((s, i) => s + i.price * i.qty, 0);
}, [items]);       // recompute ONLY when items changes

// Without useMemo: recomputed on every render (even when items is the same)
```

**When it's worth it:** genuinely expensive computation (large arrays, parsing, transforms) that runs on re-renders. **When it's not:** trivial calculations — `useMemo` itself has overhead. Don't wrap everything.

## useCallback — stable function identity

```jsx
// Problem: a new function every render → child re-renders (if memoized):
const handleSave = useCallback(() => {
  saveDraft(id, text);
}, [id, text]);        // same function object until id/text change

<ExpensiveChild onSave={handleSave} />
// With React.memo(ExpensiveChild), a stable handleSave prevents re-renders
```

**The pairing:** `React.memo(Child)` + `useCallback(fn)` + `useMemo(value)` = skip unnecessary re-renders.

## Custom hooks — compose & reuse

```jsx
// Extract logic into a hook — the naming is the contract:
function useLocalStorage(key, initial) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored !== null ? JSON.parse(stored) : initial;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}

// Now any component gets synced localStorage in two lines:
function Preferences() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  return <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
    {theme}
  </button>;
}
```

**Custom hook rules:**
- Name starts with `use` (the linter needs it to enforce Rules of Hooks)
- It's just a function — you compose built-in hooks inside it
- Each *caller* gets independent state (hooks are per-component-instance)

## The Rules of Hooks

```text
1. Only call hooks at the TOP LEVEL (not in loops, conditions, or nested functions)
2. Only call hooks from React functions (components or custom hooks)

Why: React relies on CALL ORDER to match state to the right hook.
Breaking the order breaks the state machine:
```

```jsx
// BAD — conditional hook (breaks call order):
if (enabled) {
  const [x, setX] = useState(0);    // ERROR — conditional hook
}

// GOOD — the condition lives INSIDE, hooks stay unconditional:
const [x, setX] = useState(0);
useEffect(() => { if (enabled) setX(x + 1); }, [enabled]);
```

---

**Setup:** A form hook that manages all fields.

**Solution:**
```jsx
function useForm(initial) {
  const [values, setValues] = useState(initial);

  const handleChange = useCallback((event) => {
    const { name, value } = event.target;
    setValues(prev => ({ ...prev, [name]: value }));
  }, []);

  const reset = useCallback(() => setValues(initial), [initial]);

  return { values, handleChange, reset };
}

function Login() {
  const { values, handleChange, reset } = useForm({ email: '', password: '' });
  return (
    <form>
      <input name="email" value={values.email} onChange={handleChange} />
      <input name="password" value={values.password} onChange={handleChange} />
      <button type="button" onClick={reset}>Reset</button>
    </form>
  );
}
```

**Key insight:** One custom hook packages the entire "controlled form" logic — every form in the app reuses it. This is the real payoff of hooks: logic extraction without render-prop/context gymnastics.

---

**Setup:** Track how many times a component rendered.

**Solution:**
```jsx
function RenderCounter() {
  const renders = useRef(0);
  renders.current++;                      // mutate — no re-render loop
  return <p>Rendered {renders.current} times</p>;
}
```

**Key insight:** A `useRef` counter is the *only* way to count renders — `useState` would trigger another render every time you set it (infinite loop). This is the classic diagnostic tool for render loops.

---

**Setup:** Debounce a search input using a custom hook.

**Solution:**
```jsx
function useDebouncedValue(value, delay = 300) {
  const [debounced, setDebounced] = useState(value);

  useEffect(() => {
    const timeout = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(timeout);      // reset on every change
  }, [value, delay]);

  return debounced;
}

function Search() {
  const [query, setQuery] = useState('');
  const debouncedQuery = useDebouncedValue(query);   // lags by 300ms

  useEffect(() => {
    if (debouncedQuery) searchApi(debouncedQuery);
  }, [debouncedQuery]);
  ...
}
```

**Key insight:** The custom hook *owns* the debounce mechanics — every component gets it for free. The cleanup cancels the previous timer, so only the final keystroke's value survives.

---

**Setup:** Why does `useMemo(() => x, [])` with the empty array freeze x?

**Solution:** Empty deps = "never recompute" — the memoized value is computed once and cached forever, so it captures the *first* x. Same stale-closure principle as effects. Any value the computation reads must be in the deps.

**Key insight:** Deps arrays are contracts: *everything the callback touches must be listed.* The exhaustive-deps linter enforces this — trust it, don't suppress it.

---

## Practice (try before peeking)

1. `useRef` value changes trigger re-renders?
2. When is `useCallback` actually useful?
3. Why must hooks be called unconditionally?

<details><summary>Answers</summary>

1. No — ref changes are invisible to rendering (that's the point); only state changes re-render.
2. When the function is passed to a `React.memo`-wrapped child, or used in a hook's dependency array — stable identity prevents wasted re-renders and effect loops.
3. React matches each hook's state by its *call order* across renders; a conditional hook shifts the order and scrambles which state belongs to which hook.

</details>

---

**Common traps:**
- Overusing useMemo/useCallback — premature optimization; they add overhead
- Ref mutation during render — use refs in effects/handlers, not render
- Custom hooks not starting with `use` — the linter can't help you
- Hooks in loops/conditions — breaks call-order invariants
- `useMemo`/`useEffect` with empty deps reading changing values — stale closures

---
