# Effects & Lifecycle

`useEffect` runs side effects after rendering — fetching data, subscribing to events, timers, DOM updates outside React. It's React's way of coordinating with the *outside world*. The dependency array controls when it runs; getting it wrong causes the infinite-loop and stale-closure bugs that plague React apps.

**The Intuition:** Rendering should be pure (props/state → JSX). Effects are the "impure" part: anything touching the world beyond React. `useEffect(callback, deps)` says: "after React renders, if the deps changed since last time, run the callback." It replaces the old lifecycle methods (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`) with one unified mechanism.

## The three use cases (old lifecycle → effect)

```jsx
// MOUNT (runs once):  fetch data, subscribe, start timer
useEffect(() => {
  fetchData();
}, []);                    // empty deps = run once on mount

// UPDATE (deps change): react to prop/state change
useEffect(() => {
  saveToDraft(title);      // runs whenever `title` changes
}, [title]);

// UNMOUNT (cleanup): clear timers, unsubscribe
useEffect(() => {
  const timer = setInterval(tick, 1000);
  return () => clearInterval(timer);   // cleanup runs on unmount (and before re-run)
}, []);
```

## The dependency array rules

```text
NO array:         runs after EVERY render
[] (empty):       runs once on mount (+ cleanup on unmount)
[deps]:           runs when any dep changes
```

**Every value used inside the effect must be in the deps** — React's linter (`exhaustive-deps`) enforces this. Missing deps = stale closures; extra deps = too many runs.

## The effect lifecycle in detail

```jsx
useEffect(() => {
  // 1. mount: effect runs
  // 2. deps change: CLEANUP from the previous run, then effect re-runs
  // 3. unmount: cleanup runs (final)
  const sub = api.subscribe(onEvent);
  return () => sub.unsubscribe();      // the cleanup
}, [api]);
```

The cleanup-return pattern makes effects *self-healing*: every run undoes the previous one before redoing.

## Fetching data — the canonical effect

```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false;                     // ignore stale responses
    setUser(null);                             // reset while loading

    fetch(`/api/users/${userId}`)
      .then(r => r.json())
      .then(data => { if (!cancelled) setUser(data); })
      .catch(e => { if (!cancelled) setError(e); });

    return () => { cancelled = true; };        // cleanup: mark stale
  }, [userId]);                                // re-fetch when userId changes

  if (error) return <p>Failed</p>;
  if (!user) return <p>Loading…</p>;
  return <h1>{user.name}</h1>;
}
```

**Key insight:** The `cancelled` flag prevents *state updates from stale requests* — if `userId` changes quickly, the old response must not overwrite the new one. This is the classic race-condition guard.

## The infinite-loop bug

```jsx
// BAD — infinite re-render loop:
const [data, setData] = useState([]);
useEffect(() => {
  setData(fetchSomething());     // setData → re-render → effect runs again → setData...
}, [data]);                      // data changes → effect re-runs → ...

// FIX — depend on the trigger, not the result:
useEffect(() => {
  fetchSomething().then(setData);
}, [userId]);                    // stable deps — runs only when userId changes
```

## Effects vs event handlers — the distinction

```text
EVENT HANDLER (onClick, onSubmit):
  Runs BECAUSE the user did something. Not an effect.

EFFECT (useEffect):
  Runs because RENDERING happened and deps changed.
  Use for: subscribing, fetching, timers, syncing with external systems.

Rule: if the action is a direct response to an event, put it in the handler.
Only use effects to sync with the world outside React.
```

---

**Setup:** A clock that updates every second.

**Solution:**
```jsx
function Clock() {
  const [now, setNow] = useState(new Date());

  useEffect(() => {
    const timer = setInterval(() => setNow(new Date()), 1000);
    return () => clearInterval(timer);      // cleanup on unmount
  }, []);

  return <p>{now.toLocaleTimeString()}</p>;
}
```

**Key insight:** The empty deps + cleanup pattern: start the timer on mount, kill it on unmount. Without cleanup, every mount leaks a timer — a classic memory bug (and in StrictMode dev, double-mount makes it visible).

---

**Setup:** Sync a prop change to localStorage.

**Solution:**
```jsx
function ThemeToggle({ theme }) {
  useEffect(() => {
    localStorage.setItem('theme', theme);
  }, [theme]);                    // runs only when theme changes

  return <button>{theme}</button>;
}
```

**Key insight:** This is a *reactive sync* — when the prop changes, persist it. The dep array limits the effect to actual changes (no-op re-renders don't rewrite storage).

---

**Setup:** Fetch when a search term changes, with debounce.

**Solution:**
```jsx
useEffect(() => {
  if (!query) return;
  const timeout = setTimeout(() => {
    fetchResults(query).then(setResults);
  }, 300);                        // debounce: wait 300ms after last keystroke

  return () => clearTimeout(timeout);    // cancel previous pending debounce
}, [query]);
```

**Key insight:** The cleanup cancels the *previous* pending timer — each keystroke resets the debounce. Cleanup isn't just for unmount; it runs *before every re-run*, which is exactly what debounce/cancel patterns need.

---

**Setup:** Why does the effect run twice in development?

**Solution:** React's **StrictMode** intentionally double-invokes effects in development to surface bugs (mount → cleanup → mount). It's a *feature*: it forces you to write correct cleanup. Production runs effects once.

**Key insight:** Write every effect as if it must survive mount-unmount-remount. If the double-run breaks something (duplicate subscriptions, double fetch), your cleanup is wrong — fix the cleanup, don't remove StrictMode.

---

## Practice (try before peeking)

1. `useEffect(fn)` with no array — how often does fn run?
2. What does the returned function do?
3. How do you fetch only when `userId` changes?

<details><summary>Answers</summary>

1. After *every* render — no dep array means no gating.
2. It's the cleanup — runs before the next effect run and on unmount (clear timers, unsubscribe, cancel fetches).
3. Put `[userId]` in the dependency array — the effect runs on mount and whenever userId changes.

</details>

---

**Common traps:**
- Empty-deps effect that reads state — stale closure (the state is frozen at mount)
- Forgetting cleanup → timer/subscription leaks
- Setting state inside an effect with no deps → infinite loop
- Fetching in an effect without a cancel guard → race conditions
- Putting the effect's *result* in deps instead of the *trigger*

---
