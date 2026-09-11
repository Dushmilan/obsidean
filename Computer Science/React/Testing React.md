# Testing React

Testing a React app means testing *behavior*, not implementation. The standard stack is **Vitest** (or Jest) + **React Testing Library**: render a component, interact with it the way a user would, assert on what's visible. `userEvent` simulates real interactions (typing, clicking); the testing-library queries find elements the way a user finds them — by role, label, and text — never by internal state.

**The Intuition:** A unit test for a function checks inputs → outputs. A React test checks the same thing one level up: render the component, *do something a user would do* (click, type), and assert the DOM shows what the user should see. You never inspect `state`, never call internal methods — the test is blind to implementation, which is exactly why it survives refactors.

## The basic test

```jsx
// Button.test.jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Counter } from './Counter';

test('increments when clicked', async () => {
  const user = userEvent.setup();
  render(<Counter />);

  const button = screen.getByRole('button', { name: /increment/i });
  await user.click(button);

  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

**The query ladder — prefer in this order:**
1. `getByRole` — what assistive tech / users actually perceive
2. `getByLabelText` — form fields (label = name)
3. `getByPlaceholderText`, `getByText`, `getByTestId` (last resort)

`getBy*` throws if missing; `queryBy*` returns null; `findBy*` waits (for async).

## Testing interactions

```jsx
test('form submits entered text', async () => {
  const user = userEvent.setup();
  const onSubmit = vi.fn();
  render(<TodoForm onSubmit={onSubmit} />);

  await user.type(screen.getByLabelText(/task/i), 'Buy milk');
  await user.click(screen.getByRole('button', { name: /add/i }));

  expect(onSubmit).toHaveBeenCalledWith({ text: 'Buy milk' });
});
```

`userEvent` is the key: it fires events in a realistic order (focus, input, change, blur) and respects delays. `fireEvent` is the blunt hammer — use it only when `userEvent` is too slow or can't express the scenario.

## Testing async behavior

```jsx
test('shows users after fetch resolves', async () => {
  vi.spyOn(api, 'fetchUsers').mockResolvedValue([{ id: 1, name: 'Ada' }]);
  render(<UserList />);

  expect(screen.getByText(/loading/i)).toBeInTheDocument();   // initial state

  const item = await screen.findByText('Ada');                 // waits for async
  expect(item).toBeInTheDocument();
  expect(screen.queryByText(/loading/i)).not.toBeInTheDocument();
});
```

**The pattern:** assert the loading state first (synchronous), then `findBy*` for the resolved state (waits up to a timeout). Mock the network/data layer at the boundary — `vi.mock` a module or `mockResolvedValue` a fetch wrapper — so tests are fast, deterministic, and offline.

## Mocking modules

```jsx
// Mock a whole module:
vi.mock('../lib/api', () => ({
  fetchUser: vi.fn(() => Promise.resolve({ id: 7, name: 'Grace' })),
}));

// Or partial mock — keep the real module's other exports:
vi.mock('../lib/utils', async (importOriginal) => {
  const actual = await importOriginal();
  return { ...actual, formatDate: vi.fn(() => 'Aug 16') };
});
```

**The intuition:** mocks replace *dependencies*, not the code under test. The component is real, its API calls are fake — so the test asserts the component's behavior given a known response, without touching the network or a database.

## Testing hooks & reducers

```jsx
// Reducers are pure — test them directly, no rendering needed:
test('cartReducer adds an item', () => {
  const state = cartReducer({ items: [] }, { type: 'add', item: { id: 1 } });
  expect(state.items).toHaveLength(1);
});

// Custom hooks need a render host — use renderHook:
import { renderHook, act } from '@testing-library/react';

test('useLocalStorage persists', () => {
  const { result, unmount } = renderHook(() => useLocalStorage('key', 'default'));
  act(() => result.current[1]('saved'));
  expect(localStorage.getItem('key')).toBe('"saved"');
});
```

## Snapshot tests — use sparingly

```jsx
test('matches snapshot', () => {
  const { container } = render(<Header user={{ name: 'Ada' }} />);
  expect(container).toMatchSnapshot();
});
```

Snapshots are a double-edged sword: they lock in *any* change (even accidental whitespace), and developers tend to `--update` without reviewing. They're useful as a regression tripwire for large generated output — not as a replacement for behavioral assertions.

---

**Setup:** Test that a login form validates empty fields and calls `onLogin` only with valid input.

**Solution:**
```jsx
test('blocks empty submit, then submits valid credentials', async () => {
  const user = userEvent.setup();
  const onLogin = vi.fn();
  render(<LoginForm onLogin={onLogin} />);

  await user.click(screen.getByRole('button', { name: /log in/i }));
  expect(screen.getByText(/email is required/i)).toBeInTheDocument();
  expect(onLogin).not.toHaveBeenCalled();

  await user.type(screen.getByLabelText(/email/i), 'ada@example.com');
  await user.type(screen.getByLabelText(/password/i), 'secret123');
  await user.click(screen.getByRole('button', { name: /log in/i }));

  expect(onLogin).toHaveBeenCalledWith({ email: 'ada@example.com', password: 'secret123' });
});
```

**Key insight:** the test drives the form exactly like a user — empty submit shows validation, then real input passes. Note `getByLabelText` finds inputs by their `<label>` (accessible naming) — no fragile CSS/class selectors.

---

**Setup:** Test a component that fetches data on mount, with loading → success states.

**Solution:**
```jsx
vi.mock('../api', () => ({ getStats: vi.fn() }));

import { getStats } from '../api';

test('renders stats after fetch', async () => {
  getStats.mockResolvedValue({ views: 42, likes: 7 });
  render(<StatsCard />);

  expect(screen.getByText(/loading/i)).toBeInTheDocument();
  expect(await screen.findByText('42')).toBeInTheDocument();
  expect(screen.getByText('7')).toBeInTheDocument();
});

test('renders error state', async () => {
  getStats.mockRejectedValue(new Error('boom'));
  render(<StatsCard />);

  expect(await screen.findByText(/failed to load/i)).toBeInTheDocument();
});
```

**Key insight:** two tests, two mocked outcomes — the component's success and failure branches both get covered without a server. `findBy*` handles the async resolution; if the promise never resolves, the test times out and fails loudly.

---

**Setup:** Why do the guidelines say to test by role/text instead of `data-testid`?

**Solution:** Role/text queries reflect what users and assistive tech actually experience — a change in label or semantic element is caught by the test. `data-testid` is an internal contract: it survives markup changes that should break the test, and it gives no signal about accessibility.

**Key insight:** testing the *user-visible* surface is what makes React tests refactor-proof. If the component is rewritten from classes to hooks, the tests still pass — because the visible behavior didn't change.

---

## Practice (try before peeking)

1. `getByText`, `queryByText`, `findByText` — when do you use each?
2. Why prefer `userEvent` over `fireEvent`?
3. What's the one thing a good React test never does?

<details><summary>Answers</summary>

1. `getBy*` — element must exist right now (synchronous, throws otherwise). `queryBy*` — element may not exist (returns null, for asserting absence). `findBy*` — element will appear after async work (retries until timeout, rejects if never found).
2. `userEvent` simulates the full realistic event sequence (pointer down/up, focus, input events with delays) — closer to what a browser does, which catches timing and handler bugs that `fireEvent`'s single synthetic event misses.
3. It never inspects implementation — no reading `state`, no calling internal handlers, no checking prop values directly. It asserts what the user sees and can do.

</details>

---

**Common traps:**
- Testing implementation details (state, internal functions) — brittle and meaningless
- Over-mocking — mocking the component's own logic makes the test assert nothing real
- `getByText` on async content (use `findByText`) — flaky "element not found"
- Snapshot-update as a reflex instead of reviewing the diff
- Forgetting `act()` warnings with state updates in effects

---
