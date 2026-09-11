# State & useState

State is the data that *changes* — and when it changes, the component re-renders. `useState` is the primary hook: it declares a piece of state and its setter. The subtle rules — state updates are async and batched, state is immutable (replace, don't mutate) — are where most React bugs live.

**The Intuition:** A component is a function: props in, JSX out. State is the *mutable memory* the function carries between renders. `const [count, setCount] = useState(0)` says "this component remembers a number, starting at 0." Every `setCount` schedules a re-render with the new value.

## The useState contract

```jsx
const [state, setState] = useState(initialValue);
// state    — the current value (read-only — don't mutate!)
// setState — replaces the value and schedules a re-render
```

**Two iron rules:**

**1. Never mutate state — replace it.**
```jsx
// BAD — mutating an object in place:
setUser(user.name = 'Ada');        // wrong on two counts
user.name = 'Ada';                 // mutates — React won't know

// GOOD — a new object:
setUser({ ...user, name: 'Ada' }); // fresh object, changed field
```

**2. State updates are async and batched.**
```jsx
const [count, setCount] = useState(0);

setCount(count + 1);
setCount(count + 1);
// After the batch, count is 1 — NOT 2!
// Both set calls saw the same stale `count` (0).

// If you need the LATEST value, use the updater form:
setCount(prev => prev + 1);
setCount(prev => prev + 1);        // now count becomes 2 ✓
```

## State with arrays & objects

```jsx
// Arrays — never push/splice in place:
setItems([...items, newItem]);                  // append
setItems(items.filter(i => i.id !== id));       // remove
setItems(items.map(i => i.id === id ? {...i, done: true} : i));  // update

// Objects — never assign fields in place:
setUser({ ...user, age: user.age + 1 });

// Nested — copy each level you change:
setConfig({ ...config, theme: { ...config.theme, dark: !config.theme.dark } });
```

## Lazy initialization

```jsx
// The initializer runs on EVERY render if passed directly:
const [token] = useState(computeToken());     // computeToken() runs each render

// Lazy form — runs ONCE, only on mount:
const [token] = useState(() => computeToken());   // ← the right way for expensive work
```

## The derived-state pattern

```jsx
// Don't store what you can compute:
const [items, setItems] = useState([]);
const total = items.reduce((s, i) => s + i.price, 0);   // derived — computed each render

// BAD: a second state that must be kept in sync:
// const [total, setTotal] = useState(0);  — the classic sync bug
```

## The batching behavior (React 18+)

```jsx
function handleClick() {
  setCount(c => c + 1);      // 3 updates batched into ONE re-render
  setCount(c => c + 1);
  setCount(c => c + 1);
}

// In async callbacks, React 18 batches too (wasn't always true):
async function load() {
  const data = await fetch('/api');
  setLoading(false);          // these two re-renders are also batched
  setData(data);
}
```

---

**Setup:** A toggle button (true/false state).

**Solution:**
```jsx
function Toggle() {
  const [on, setOn] = useState(false);

  return (
    <button onClick={() => setOn(prev => !prev)}>
      {on ? 'ON' : 'OFF'}
    </button>
  );
}
```

**Key insight:** The updater form `prev => !prev` is *required* when the new value depends on the current one — it reads the latest state even in batches. `setOn(!on)` would work for a single click but breaks under rapid/doubled updates.

---

**Setup:** An array state with add, remove, and toggle-complete.

**Solution:**
```jsx
const [todos, setTodos] = useState([]);

const addTodo = (text) =>
  setTodos(prev => [...prev, { id: nextId(), text, done: false }]);

const removeTodo = (id) =>
  setTodos(prev => prev.filter(t => t.id !== id));

const toggleTodo = (id) =>
  setTodos(prev => prev.map(t =>
    t.id === id ? { ...t, done: !t.done } : t
  ));
```

**Key insight:** Every operation returns a *new array* — filter/map/spread create new arrays, leaving the old state untouched. This immutability is what lets React detect changes cheaply (reference comparison).

---

**Setup:** A form that accumulates multiple fields.

**Solution:**
```jsx
const [form, setForm] = useState({ name: '', email: '', age: '' });

const update = (field) => (event) =>
  setForm(prev => ({ ...prev, [field]: event.target.value }));

<input value={form.name} onChange={update('name')} />
<input value={form.email} onChange={update('email')} />
```

**Key insight:** Computed property names (`[field]: value`) + the updater spread pattern gives one handler for all fields. The `{ ...prev, [field]: value }` copy-then-override is the canonical object-state update.

---

**Setup:** Why does `setCount(count + 1)` twice not double the count?

**Solution:** React batches the two calls — both read the *same* stale `count` (0) from this render's closure, so both set 1. The updater form `setCount(c => c + 1)` reads the *queued* value, so two calls give 2.

**Key insight:** State in the component body is a *snapshot* of this render. Setters with updater functions are the only way to chain off the latest value. This "stale closure" idea also explains `useEffect` dependency bugs.

---

## Practice (try before peeking)

1. What's the output of three `setCount(count + 1)` in one handler?
2. How do you safely append to a state array?
3. When does the lazy initializer `useState(() => expensive())` actually run?

<details><summary>Answers</summary>

1. The count increments by 1 (one render, stale value read three times). Use updaters for +3.
2. `setItems(prev => [...prev, newItem])` — new array, never mutate.
3. Once, on the initial mount only — React calls the function lazily. Passing the *result* (`useState(expensive())`) runs it every render.

</details>

---

**Common traps:**
- Mutating state in place (`items.push(x)`) — React can't see the change
- Async staleness — setState from stale closures
- Storing derived values that can just be computed
- Calling the initializer eagerly (`useState(expensive())` not `useState(() => expensive())`)
- Reading state immediately after setting it — the update hasn't happened yet

---
