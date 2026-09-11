# React Router & SPAs

A **Single-Page Application** loads once and swaps views via JavaScript — no full page reloads. **React Router** manages the URL↔component mapping: the URL is state, and navigation re-renders the right component. The result feels like a native app: instant transitions, preserved state, deep-linkable URLs.

**The Intuition:** In a traditional site, each URL loads a new HTML page. In an SPA, one HTML page loads and the router *pretends* the URL changed — swapping components while keeping the app alive. The browser's back/forward still works because the router syncs with the URL history API.

## The core pieces

```jsx
import { BrowserRouter, Routes, Route, Link, useNavigate, useParams } from 'react-router-dom';

// 1. The router wraps the app:
<BrowserRouter>
  <App />
</BrowserRouter>

// 2. Routes map URL → component:
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/about" element={<About />} />
  <Route path="/users/:id" element={<UserProfile />} />   // :id = URL param
  <Route path="*" element={<NotFound />} />               // catch-all
</Routes>

// 3. Navigation — <Link> (anchor that doesn't reload):
<Link to="/about">About</Link>
<Link to={`/users/${user.id}`}>{user.name}</Link>

// 4. Programmatic navigation:
const navigate = useNavigate();
navigate('/login');               // push a new entry
navigate('/dashboard', { replace: true });   // replace current entry
navigate(-1);                     // back
```

## URL params & query strings

```jsx
function UserProfile() {
  const { id } = useParams();            // from /users/:id
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetch(`/api/users/${id}`).then(r => r.json()).then(setUser);
  }, [id]);                              // re-fetch when id changes

  // query string: /search?q=react
  const [searchParams, setSearchParams] = useSearchParams();
  const q = searchParams.get('q') ?? '';
}

// Navigate with query:
navigate(`/search?q=${encodeURIComponent(term)}`);
```

## Nested routes & layout routes

```jsx
<Route path="/dashboard" element={<DashboardLayout />}>   {/* layout wraps */}
  <Route index element={<Overview />} />                   {/* /dashboard */}
  <Route path="reports" element={<Reports />} />           {/* /dashboard/reports */}
  <Route path="settings" element={<Settings />} />
</Route>
```

The layout component renders children via `<Outlet />`:
```jsx
function DashboardLayout() {
  return (
    <div>
      <Sidebar />
      <main><Outlet /></main>    {/* nested route renders here */}
    </div>
  );
}
```

## Protected routes — the auth pattern

```jsx
function RequireAuth({ children }) {
  const { user } = useAuth();               // your auth context
  const location = useLocation();

  if (!user) {
    // Redirect to login, remembering where they wanted to go:
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  return children;
}

<Route path="/profile" element={
  <RequireAuth>
    <Profile />
  </RequireAuth>
} />
// After login: navigate(location.state?.from?.pathname ?? '/')
```

## Lazy loading — code splitting per route

```jsx
import { lazy, Suspense } from 'react';

const Settings = lazy(() => import('./Settings'));   // loaded on demand

<Route path="/settings" element={
  <Suspense fallback={<Spinner />}>
    <Settings />
  </Suspense>
} />
```

**Why:** the initial bundle only contains what the first view needs; route chunks load as visited. This is the #1 SPA performance win.

---

**Setup:** A layout with sidebar and three sections.

**Solution:**
```jsx
function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Layout />}>
          <Route index element={<Home />} />
          <Route path="blog" element={<Blog />} />
          <Route path="blog/:slug" element={<BlogPost />} />
          <Route path="*" element={<NotFound />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}
```

**Key insight:** The layout route is the shared shell; `<Outlet />` is where the nested view renders. URL structure (nested paths) mirrors component structure — this nesting is what keeps the URL in sync with the UI.

---

**Setup:** Fetch data keyed by the URL param.

**Solution:**
```jsx
function BlogPost() {
  const { slug } = useParams();

  useEffect(() => {
    setPost(null);                       // reset while loading
    fetch(`/api/posts/${slug}`)
      .then(r => r.json())
      .then(setPost);
  }, [slug]);                            // re-run when slug changes
  ...
}
// Navigating /blog/a → /blog/b re-fetches — same component, new param.
```

**Key insight:** `useParams` returns the *current* params; putting them in the effect deps makes navigation re-fetch automatically. This is the "URL as state" pattern — the component derives its data from the URL.

---

**Setup:** Redirect after login.

**Solution:**
```jsx
async function handleLogin(e) {
  e.preventDefault();
  const ok = await api.login(form);
  if (ok) {
    // return to the page they wanted, or default home:
    navigate(location.state?.from?.pathname ?? '/', { replace: true });
  }
}
```

**Key insight:** The `state={{ from: location }}` on the redirect *carries* the intended destination through the redirect — the post-login UX of "return where you were" needs that memory. `replace: true` prevents the login page from lingering in history.

---

**Setup:** Server-side rendering / static export — when BrowserRouter won't work.

**Solution:** For SPAs served by a static host, the *server* must send `index.html` for every route (SPA fallback) — otherwise refreshing `/about` 404s. Options:
- Configure the host's fallback (Netlify `_redirects`, Nginx `try_files`)
- Use `HashRouter` (URLs become `/#/about`) — no server config, uglier URLs
- Move to a framework (Next.js) for true SSR/SSG

**Key insight:** BrowserRouter URLs (`/about`) are clean but need server cooperation; HashRouter works anywhere but pollutes the URL. The "refresh 404" problem is the classic SPA deployment gotcha.

---

## Practice (try before peeking)

1. What's the difference between `<Link>` and `<a href>`?
2. How do you read `/users/42`'s 42?
3. Why lazy-load routes?

<details><summary>Answers</summary>

1. `<Link>` intercepts the click and updates the URL via history API — no page reload, app state preserved. `<a>` triggers a full browser navigation.
2. `const { id } = useParams()` — from the `:id` segment of the matched route.
3. Code-splitting — each route's bundle loads only when visited, shrinking the initial load.

</details>

---

**Common traps:**
- Forgetting the server SPA-fallback → refresh 404s
- Using `<a>` for internal links → full reloads, state loss
- Not re-fetching when params change (missing `[id]` in deps)
- Routes declared out of order — React Router ranks by specificity, but put catch-all `*` last
- Nested routes without `<Outlet />` → nothing renders

---
