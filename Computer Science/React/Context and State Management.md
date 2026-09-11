# Context & State Management

Prop drilling is passing data through every component in a chain. **Context** lets you broadcast a value to any component in a subtree without threading props manually. **`useReducer`** gives you Redux-style state transitions with plain functions. For big apps, dedicated stores (Zustand, Redux Toolkit) add selectors, persistence, and devtools — but Context + `useReducer` covers most needs.

**The Intuition:** Props are like handing a note down a line of people. Context is like putting a note on a shared bulletin board — anyone in the building reads it directly, no passing required. The catch: *everyone re-renders when the board changes*, so Context is for *slow-changing* data (theme, auth, locale), not high-frequency updates (every keystroke of a form).

## Creating and consuming context

```jsx
// 1. Create the context (the "bulletin board"):
const ThemeContext = createContext('light');

// 2. Provide a value to a subtree:
function App() {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

// 3. Consume it — any descendant, no props:
function Toolbar() {
  const { theme, setTheme } = useContext(ThemeContext);
  return <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
    Switch to {theme === 'light' ? 'dark' : 'light'}
  </button>;
}
```

**The contract:** `createContext` makes the board, `<Provider value={...}>` broadcasts, and `useContext` reads the *nearest* provider above the component. No provider? You get the default value from `createContext(defaultValue)`.

## Splitting contexts — avoid the mega-provider

```jsx
// BAD — one giant object: ANY state change re-renders EVERY consumer
<AppContext.Provider value={{ theme, user, cart, filters }}>

// GOOD — separate contexts so consumers only re-render for their slice:
<ThemeContext.Provider value={theme}>
  <UserContext.Provider value={user}>
    <CartContext.Provider value={cart}>
      {children}
    </CartContext.Provider>
  </UserContext.Provider>
</ThemeContext.Provider>
```

**Why:** Context re-renders *all* consumers when the value reference changes. A new `value={{...}}` object every render defeats memoization. Keep each context small and stable.

## useReducer — state transitions as functions

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    case 'reset':     return { count: 0 };
    default: throw new Error(`Unknown action: ${action.type}`);
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });
  return (
    <>
      <p>{state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </>
  );
}
```

**The intuition:** `useReducer` is `useState` with a *policy layer*. Instead of scattering `setState` calls, every change becomes an `action` (a plain object). The `reducer` is a pure function: `(state, action) → newState`. Pure means *same input, same output, no side effects* — that purity is what makes state transitions testable in isolation.

```jsx
// Testing the reducer is just testing a pure function — no component needed:
const result = reducer({ count: 0 }, { type: 'increment' });
assert(result.count === 1);
```

## Combining Context + useReducer

```jsx
const AuthContext = createContext(null);

function authReducer(state, action) {
  switch (action.type) {
    case 'login':  return { user: action.user, status: 'authenticated' };
    case 'logout': return { user: null, status: 'anonymous' };
    default: throw new Error(`Unknown action: ${action.type}`);
  }
}

function AuthProvider({ children }) {
  const [state, dispatch] = useReducer(authReducer, { user: null, status: 'anonymous' });
  // The whole API is a stable object — useMemo keeps the reference stable:
  const value = useMemo(() => ({ state, dispatch }), [state]);
  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

// Consumers:
function LogoutButton() {
  const { dispatch } = useContext(AuthContext);
  return <button onClick={() => dispatch({ type: 'logout' })}>Log out</button>;
}
```

**Key insight:** This is the "mini-Redux" pattern — all auth logic lives in one reducer, exposed through one provider, consumed anywhere via `useContext`. It's the standard answer before you need a real store.

## When to reach for a store (Zustand / Redux Toolkit)

| Need | Tool |
|------|------|
| Shared *slow-changing* data (theme, locale, auth) | Context |
| Local complex transitions (form wizard, game state) | `useReducer` |
| Server cache (fetch results, invalidation) | TanStack Query / SWR |
| Large client state with many slices, devtools, persistence | Zustand / Redux Toolkit |

```jsx
// Zustand — a store outside React, so components subscribe to slices:
import { create } from 'zustand';

const useStore = create((set) => ({
  count: 0,
  inc: () => set((s) => ({ count: s.count + 1 })),
}));

function Counter() {
  const count = useStore((s) => s.count);      // subscribe to ONE slice
  const inc = useStore((s) => s.inc);
  return <button onClick={inc}>{count}</button>;
}
```

**The difference from Context:** the store lives *outside* React, so components subscribe to specific slices and only re-render when *their* slice changes — no provider tree, no "whole subtree re-renders" cost.

---

**Setup:** A theme toggle shared across a navbar and a page body, without prop drilling.

**Solution:**
```jsx
const ThemeContext = createContext(null);

function App() {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Navbar />
      <Page />
    </ThemeContext.Provider>
  );
}

function Navbar() {
  const { theme, setTheme } = useContext(ThemeContext);
  return <button onClick={() => setTheme(t => t === 'light' ? 'dark' : 'light')}>
    {theme}
  </button>;
}

function Page() {
  const { theme } = useContext(ThemeContext);
  return <main className={`page page--${theme}`}>Content</main>;
}
```

**Key insight:** `Page` and `Navbar` are siblings — props would need `App` to thread theme through both. Context jumps the hierarchy directly. The provider wraps only what needs the value; components outside the provider fall back to the default.

---

**Setup:** A shopping cart using `useReducer` with add/remove/clear.

**Solution:**
```jsx
function cartReducer(state, action) {
  switch (action.type) {
    case 'add':
      return { items: [...state.items, action.item] };
    case 'remove':
      return { items: state.items.filter(i => i.id !== action.id) };
    case 'clear':
      return { items: [] };
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

function Cart() {
  const [state, dispatch] = useReducer(cartReducer, { items: [] });
  return (
    <>
      <ul>{state.items.map(i => <li key={i.id}>{i.name}</li>)}</ul>
      <button onClick={() => dispatch({ type: 'add', item: { id: 1, name: 'Apple' } })}>
        Add
      </button>
      <button onClick={() => dispatch({ type: 'clear' })}>Clear</button>
    </>
  );
}
```

**Key insight:** Every action is a *description of intent* (`'add'`, `'remove'`) plus data (`item`, `id`) — the reducer owns the "how". Handlers become one-liners: `dispatch({...})`. This keeps components dumb and the logic centralized.

---

**Setup:** Why does passing `value={{ theme, setTheme }}` re-render everything on every state change?

**Solution:** The object literal is *new* on every render, so the provider's value reference changes, and React re-renders all consumers even if the data didn't meaningfully change. Fix: `useMemo` the value, or split contexts so only the affected slice changes.

**Key insight:** Context value identity matters as much as its content. Memoize values you hand to providers; otherwise your Context becomes a performance leak.

---

## Practice (try before peeking)

1. What's the main downside of putting a frequently-changing value in Context?
2. Why must a reducer be pure?
3. `useReducer` vs `useState` — when does one win?

<details><summary>Answers</summary>

1. Every value change re-renders all consumers of that context — high-frequency updates (typing, mouse movement, live data) belong in local state or a store with per-slice subscriptions, not Context.
2. Purity makes transitions predictable and testable — same `(state, action)` always yields the same next state, which is what enables devtools replay, time-travel, and unit testing without a UI.
3. `useReducer` wins when updates follow a *family* of related actions with non-trivial logic (form wizards, carts, auth) or when the next state depends on the previous in complex ways. `useState` wins for single independent values.

</details>

---

**Common traps:**
- Creating the provider value inline (new object every render → all consumers re-render)
- One giant context object for everything (any change re-renders everyone)
- Prop drilling instead of Context — or Context instead of props for one-off data
- Reducers with side effects (logging, API calls, timers inside the reducer — breaks purity)
- `useContext` returning `undefined` and crashing — always provide a default or throw a clear error

---
