# Forms & Controlled Components

React forms use **controlled components**: the input's value *is* state, and every keystroke flows through `onChange` → state → value. This makes the form's data a single source of truth — validation, disabling, and submission all read from the same state.

**The Intuition:** In plain HTML, the input *owns* its value and React can't see it. In a controlled component, React owns it: `value={state}` and `onChange={setState}`. Every keystroke updates state, re-renders, and writes back. You can transform the value as it's typed (uppercase, format), validate live, and submit exactly what state holds.

## Controlled inputs — the pattern

```jsx
function NameForm() {
  const [name, setName] = useState('');

  return (
    <input
      value={name}                        // state → input
      onChange={(e) => setName(e.target.value)}   // input → state
    />
  );
}
```

The round trip: type → onChange → setState → re-render → value. **The input can never hold a value React doesn't know about** — that's the whole point.

## Each input type

```jsx
// Text / textarea
<input value={text} onChange={e => setText(e.target.value)} />
<textarea value={bio} onChange={e => setBio(e.target.value)} />

// Select
<select value={city} onChange={e => setCity(e.target.value)}>
  <option value="london">London</option>
  <option value="paris">Paris</option>
</select>

// Checkbox — checked, not value:
<input type="checkbox" checked={agreed} onChange={e => setAgreed(e.target.checked)} />

// Radio
<input type="radio" checked={color === 'red'} onChange={() => setColor('red')} />

// Number — value is a string; convert:
<input type="number" value={qty} onChange={e => setQty(Number(e.target.value))} />
```

## Validation patterns

```jsx
const [form, setForm] = useState({ email: '', password: '' });
const [errors, setErrors] = useState({});

const validate = (values) => {
  const e = {};
  if (!values.email.includes('@')) e.email = 'Invalid email';
  if (values.password.length < 8) e.password = 'Too short';
  return e;
};

function handleSubmit(ev) {
  ev.preventDefault();                    // stop the native page reload
  const errs = validate(form);
  if (Object.keys(errs).length > 0) {
    setErrors(errs);                      // show errors, don't submit
    return;
  }
  api.createUser(form);
}
```

**Live validation:** validate in `onChange` (or on blur) — `errors.email && <p className="error">{errors.email}</p>` right under the field.

## The big three form libraries

| Library | Approach | When |
|---------|----------|------|
| Controlled (hand-rolled) | useState + handlers | simple forms, learning |
| React Hook Form | uncontrolled + refs, register() | performant, complex forms |
| Formik | state-based, Yup validation | structured, validation-heavy |

## Uncontrolled (the escape hatch)

```jsx
// Uncontrolled: the DOM owns the value; React reads it on demand:
const inputRef = useRef(null);
function submit() {
  const value = inputRef.current.value;   // read when needed
}
<input ref={inputRef} defaultValue="prefilled" />
// Use for: file inputs, large forms with zero live logic, integrating legacy
```

---

**Setup:** A login form with email + password, validating on submit.

**Solution:**
```jsx
function LoginForm({ onSubmit }) {
  const [form, setForm] = useState({ email: '', password: '' });
  const [error, setError] = useState('');

  const handleChange = (field) => (e) =>
    setForm(prev => ({ ...prev, [field]: e.target.value }));

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!form.email || !form.password) {
      setError('All fields are required');
      return;
    }
    setError('');
    onSubmit(form);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="email" value={form.email}
             onChange={handleChange('email')} placeholder="Email" />
      <input type="password" value={form.password}
             onChange={handleChange('password')} placeholder="Password" />
      {error && <p className="error">{error}</p>}
      <button type="submit">Log in</button>
    </form>
  );
}
```

**Key insight:** The lifted `onSubmit(form)` callback is how a child form reports data to a parent — props down, events up. `ev.preventDefault()` stops the browser's native form submission (page reload).

---

**Setup:** Enable a submit button only when valid (live).

**Solution:**
```jsx
const valid = form.email.includes('@') && form.password.length >= 8;
<button type="submit" disabled={!valid}>Sign up</button>
```

**Key insight:** `valid` is *derived* from state — computed each render, never stored separately (avoids sync bugs). The disabled button gives instant feedback without any submission.

---

**Setup:** A dynamic list of inputs (e.g., multiple emails).

**Solution:**
```jsx
const [emails, setEmails] = useState(['']);

const updateEmail = (index) => (e) =>
  setEmails(prev => prev.map((em, i) => i === index ? e.target.value : em));

const addEmail = () => setEmails(prev => [...prev, '']);

{emails.map((em, i) => (
  <div key={i}>
    <input value={em} onChange={updateEmail(i)} />
    <button onClick={() => setEmails(prev => prev.filter((_, idx) => idx !== i))}>
      Remove
    </button>
  </div>
))}
<button onClick={addEmail}>Add email</button>
```

**Key insight:** Array state + index-based updaters handle dynamic fields. The *index* as key is acceptable here because the list is append/remove-only at the end and each row is stateless — but it would break with reordering.

---

**Setup:** Controlled vs uncontrolled for a large form — decision.

**Solution:**
- **Controlled**: every keystroke re-renders the whole form. Fine for < 30 fields.
- **Uncontrolled / React Hook Form**: reads values via refs at submit — no re-render per keystroke. Better for very large forms or high-frequency typing.

**Key insight:** The trade is *live feedback* (controlled: validation-as-you-type, dependent fields) vs *performance* (uncontrolled: no per-keystroke render). React Hook Form is the popular middle ground — uncontrolled under the hood, controlled ergonomics via `register`.

---

## Practice (try before peeking)

1. What does `ev.preventDefault()` do in a submit handler?
2. Checkbox uses `value` or `checked`?
3. When is an uncontrolled input the right choice?

<details><summary>Answers</summary>

1. Stops the browser's native form submission — the page would reload and lose all state.
2. `checked` — checkboxes are boolean; `value` is the option's submitted data.
3. File inputs (no value control), or massive forms where per-keystroke renders cost too much.

</details>

---

**Common traps:**
- Forgetting `value={...}` → uncontrolled input with a confusing hybrid
- `Number(e.target.value)` for numeric inputs — the raw value is a string
- Not calling `preventDefault()` — the page reloads on submit
- Storing derived validity as state — compute it instead
- `defaultValue` + controlled `value` together — React warns about the conflict

---
