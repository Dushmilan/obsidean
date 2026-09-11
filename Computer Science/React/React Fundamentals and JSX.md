# React Fundamentals & JSX

React is a **declarative** UI library: you describe what the screen *should look like* for a given state, and React figures out how to get there. The two pillars are **components** (reusable pieces) and **JSX** (HTML-like syntax that compiles to function calls). The virtual DOM makes updates efficient without you touching the DOM directly.

**The Intuition:** jQuery-era code said *how*: "find this element, change its text." React says *what*: "render `Score` as 42." When the score changes, React re-runs your render functions, diffs the result against the previous one (the virtual DOM), and patches only what changed. You stop manipulating the DOM and start describing it.

## The mental model

```text
State → Render → UI
  ↑                  │
  └── event / fetch ─┘

Change the state, React re-renders the component tree.
The virtual DOM diff (reconciliation) finds the minimal set
of real-DOM changes.
```

## JSX — the syntax

JSX looks like HTML but compiles to `React.createElement` calls:

```jsx
// JSX:
const element = <h1 className="title">Hello, {name}!</h1>;

// Compiles to:
const element = React.createElement(
  'h1',
  { className: 'title' },
  'Hello, ',
  name,
  '!'
);
```

**JSX rules:**
- One root element per return (or fragments `<>...</>`)
- Expressions in `{}` — any JavaScript expression: `{count + 1}`, `{items.map(...)}`
- Attributes: `className` (not `class`), `htmlFor` (not `for`), camelCase events (`onClick`)
- Comments: `{/* comment */}`
- Conditional rendering: `{isLoggedIn ? <Logout/> : <Login/>}` or `{isLoggedIn && <Dashboard/>}`

## Components — the building blocks

```jsx
// FUNCTION component — the modern standard
function Greeting({ name }) {          // props destructured
  return <h1>Hello, {name}!</h1>;
}

// Usage — components are capitalized, self-closing or paired:
<Greeting name="Ada" />

// Components can compose — a UI is a tree of components:
function App() {
  return (
    <header>
      <Greeting name="Ada" />
      <Score value={42} />
    </header>
  );
}
```

**Component rules:**
- Capitalized names (`greeting` would be treated as an HTML tag)
- Props flow **down** (parent → child), never up
- A component is a *pure function of its props* — same props, same output

## Props — read-only inputs

```jsx
function Score({ value, max = 100 }) {      // destructure + defaults
  return <progress value={value} max={max} />;
}

// Passing anything:
<Card
  title="Stats"
  items={[1, 2, 3]}          // array
  onSave={() => save()}      // function (callback)
  config={{ theme: 'dark' }} // object
/>
```

**Props are immutable** — a component must never assign to its props. If data needs to change, the parent owns it as state.

## Create a project

```bash
npm create vite@latest my-app -- --template react   # Vite — fast, modern
cd my-app
npm install
npm run dev          # dev server with HMR
npm run build        # production bundle
npm run preview      # preview the build
```

---

**Setup:** Build a `Counter` component with a button.

**Solution:**
```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);     // state: count, setter

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

**Key insight:** `useState` returns `[value, setter]`. Calling `setCount` *re-renders* the component with the new value — you never touch the DOM. The arrow `() => setCount(count + 1)` is the event handler; React wires it up.

---

**Setup:** Render a list of items with a key.

**Solution:**
```jsx
function ShoppingList({ items }) {
  return (
    <ul>
      {items.map(item => (
        <li key={item.id}>{item.name}</li>   // key = identity for diffing
      ))}
    </ul>
  );
}
```

**Key insight:** The `key` tells React which list items persist across re-renders. Use a *stable unique id* — never the array index (reordering breaks state and animations). Keys missing → React warns and diffing degrades.

---

**Setup:** Conditional render a loading state.

**Solution:**
```jsx
function Profile({ user }) {
  if (!user) {
    return <p>Loading…</p>;          // early return
  }
  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}
```

**Key insight:** Components are just functions — early returns and `&&`/`?:` are ordinary JavaScript. Conditional rendering is *not a special feature*; it's regular code inside the render function.

---

**Setup:** Compose props into a reusable card.

**Solution:**
```jsx
function Card({ title, children, footer }) {
  return (
    <div className="card">
      <h3>{title}</h3>
      <div className="card-body">{children}</div>   {/* children = what's between tags */}
      {footer && <div className="card-footer">{footer}</div>}
    </div>
  );
}

// Usage — children make components composable:
<Card title="Welcome" footer={<button>OK</button>}>
  <p>Body content goes here.</p>
</Card>
```

**Key insight:** `children` is the implicit prop for everything between the component's tags — the mechanism behind layout/wrapper components. Optional props (`footer && ...`) render conditionally.

---

## Practice (try before peeking)

1. Why `className` instead of `class` in JSX?
2. What's wrong with `<li key={index}>`?
3. Can a child modify its props?

<details><summary>Answers</summary>

1. `class` is a JavaScript keyword; `className` avoids the collision in JSX (which is JS).
2. Index keys break when items are inserted/reordered/sorted — React reuses state for the wrong elements. Use stable unique ids.
3. No — props are immutable. The child must call a callback prop (`onChange`) to ask the parent to change state.

</details>

---

**Common traps:**
- Forgetting the key on mapped items
- Mutating props (props are read-only — the parent owns the data)
- Multiple root elements without a fragment
- `class` instead of `className` (silent style bug)
- Calling a component (`<Greeting/>` vs `Greeting()`) — JSX treats them differently for hooks

---
