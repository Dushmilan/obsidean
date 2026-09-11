# React Patterns & Error Boundaries

Beyond the hooks, there's a toolbox of *composition patterns*: **error boundaries** (catch render crashes), **`children` composition** (layout and slots), **render props** and **HOCs** (code reuse — now mostly replaced by hooks), **compound components** (shared implicit state), and **`lazy` + `Suspense`** for loading states. Knowing when to use each keeps component trees simple and reusable.

**The Intuition:** Components are like LEGO — but the real skill is knowing *which connectors exist*. `children` is the universal slot. Error boundaries are airbags: one crash shouldn't total the whole car (the whole app). Compound components are like a `<select>` and its `<option>`s — a family that shares state without you wiring it manually.

## Error boundaries — catch render crashes

```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {          // render phase — set the fallback flag
    return { hasError: true };
  }

  componentDidCatch(error, info) {             // commit phase — log / report
    logError(error, info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback ?? <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

// Usage — wrap fragile regions, not the whole app:
<ErrorBoundary fallback={<p>Profile failed to load</p>}>
  <UserProfile />
</ErrorBoundary>
```

**Key facts:** error boundaries are the *only* place React lets you catch errors thrown during rendering, lifecycle methods, and constructors of descendants. They **don't** catch event handlers, async code, or errors in the boundary itself. They must be **class components** — there's no hook equivalent (yet).

## children — the composition slot

```jsx
function Layout({ header, sidebar, children, footer }) {
  return (
    <div className="layout">
      <header>{header}</header>
      <aside>{sidebar}</aside>
      <main>{children}</main>
      <footer>{footer}</footer>
    </div>
  );
}

<Layout
  header={<BrandBar />}
  sidebar={<Nav />}
  footer={<Footer />}
>
  <PageContent />
</Layout>
```

**The intuition:** passing JSX as props (or `children`) turns components into *templates with slots*. The parent decides what fills each slot; the layout component decides where it goes. This is how you build reusable page shells without prop-drilling 20 props.

## Render props — share logic via a function prop

```jsx
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  useEffect(() => {
    const onMove = (e) => setPos({ x: e.clientX, y: e.clientY });
    window.addEventListener('mousemove', onMove);
    return () => window.removeEventListener('mousemove', onMove);
  }, []);
  return render(pos);           // the CHILD decides what to render
}

<MouseTracker render={({ x, y }) => <p>Mouse at {x}, {y}</p>} />
```

Historically the go-to for sharing stateful logic. **Hooks replaced this pattern** for most cases — a `useMousePosition()` hook does the same thing with less nesting. You'll still see render props in older libraries (React Router v5, some form libs), so it's worth recognizing.

## Higher-order components — wrap to enhance

```jsx
function withLoading(Wrapped) {
  return function WithLoading({ isLoading, ...rest }) {
    return isLoading ? <Spinner /> : <Wrapped {...rest} />;
  };
}

const UserWithLoading = withLoading(User);
// <UserWithLoading isLoading={true} user={...} />
```

**The intuition:** an HOC is a function that takes a component and returns an enhanced one — decoration. Same story as render props: hooks (`useLoading`, custom hooks) mostly superseded HOCs because they compose without wrapping. HOCs still appear in libraries (Redux's `connect`, React Router's `withRouter` legacy).

## Compound components — a family sharing implicit state

```jsx
const AccordionContext = createContext(null);

function Accordion({ children }) {
  const [openIndex, setOpenIndex] = useState(0);
  return (
    <AccordionContext.Provider value={{ openIndex, setOpenIndex }}>
      <div className="accordion">{children}</div>
    </AccordionContext.Provider>
  );
}

function Item({ index, title, children }) {
  const { openIndex, setOpenIndex } = useContext(AccordionContext);
  const open = openIndex === index;
  return (
    <div className="accordion-item">
      <button onClick={() => setOpenIndex(open ? -1 : index)}>{title}</button>
      {open && <div className="accordion-body">{children}</div>}
    </div>
  );
}

// Usage — parent and children coordinate with zero wiring:
<Accordion>
  <Accordion.Item index={0} title="What is React?">...</Accordion.Item>
  <Accordion.Item index={1} title="Why hooks?">...</Accordion.Item>
</Accordion>
```

**The intuition:** like native `<select>` + `<option>` — the family shares state (which option is open) through an internal context, invisible to the consumer. The consumer just nests them and they work.

## lazy + Suspense for loading states

```jsx
const Chart = lazy(() => import('./Chart'));

<Suspense fallback={<ChartSkeleton />}>
  <Chart />
</Suspense>
```

Used together: `lazy` defers loading a component until it renders; `Suspense` shows a fallback while it loads. This composes with **data fetching** too — a component can suspend while loading data and React shows the nearest `Suspense` fallback.

---

**Setup:** Wrap sections of a dashboard so one failing widget doesn't blank the page.

**Solution:**
```jsx
<Dashboard>
  <ErrorBoundary fallback={<p>Revenue chart unavailable</p>}>
    <RevenueChart />
  </ErrorBoundary>
  <ErrorBoundary fallback={<p>User list unavailable</p>}>
    <UserList />
  </ErrorBoundary>
  <ErrorBoundary fallback={<p>Settings unavailable</p>}>
    <SettingsPanel />
  </ErrorBoundary>
</Dashboard>
```

**Key insight:** boundaries at *feature granularity* (not app-wide) mean a crash in one widget degrades gracefully — the rest of the page keeps working. Each boundary is a separate safety cell.

---

**Setup:** A modal that reuses the same overlay/skeleton but lets every caller define its content.

**Solution:**
```jsx
function Modal({ open, title, onClose, children }) {
  if (!open) return null;
  return (
    <div className="overlay" onClick={onClose}>
      <div className="modal" onClick={e => e.stopPropagation()}>
        <header>{title}</header>
        <section>{children}</section>
        <footer>
          <button onClick={onClose}>Close</button>
        </footer>
      </div>
    </div>
  );
}

<Modal open={editing} title="Edit profile" onClose={() => setEditing(false)}>
  <ProfileForm user={user} />
</Modal>
```

**Key insight:** the modal owns its shell (overlay, header, footer, ESC/close behavior); callers supply only the *body* via `children`. One modal component serves every dialog in the app — that's composition paying for itself.

---

**Setup:** Why can't a hook catch render errors?

**Solution:** Hooks run *during* render and can't intercept errors thrown by a component's own render — there's no "wrap the component in try/catch" from inside it (a `try/catch` around JSX would break the hooks call order). React's answer is the class-based error boundary, which sits *above* the failing subtree.

**Key insight:** error boundaries are structurally outside — they wrap, they don't live inside. That's why they need a class (lifecycle hooks: `getDerivedStateFromError` + `componentDidCatch`) and why a function component can't be one.

---

## Practice (try before peeking)

1. What do error boundaries catch — and what three things do they NOT catch?
2. When is `children` the right tool vs explicit props?
3. Render props vs custom hooks — when do you still see the old pattern?

<details><summary>Answers</summary>

1. They catch errors in render, lifecycle methods, and constructors of descendants. They don't catch event handlers, async code (promises, timers, `setTimeout`), or errors thrown inside the boundary itself — handle those with try/catch or promise `.catch`.
2. `children` when the slot is "whatever the parent wants" (layout bodies, modal content). Explicit props when the structure is known and typed (title, footer) — they give names and validation that `children` can't.
3. In legacy libraries and codebases (React Router v5, older Redux form libraries, some icon/table libs). For your own logic, custom hooks are simpler — same reuse with no wrapper nesting.

</details>

---

**Common traps:**
- One error boundary around the whole app — one crash kills everything; put boundaries at feature level
- Trying to use a hook as an error boundary (impossible — classes only)
- Prop-drilling through 5 levels when `children`/Context would flatten it
- Building HOC/render-prop layers when a custom hook would do (unnecessary nesting)
- Forgetting `lazy` imports need a `Suspense` boundary — or you get a blank screen during load

---
