# Git Fundamentals

Git is a **distributed version control system**: every clone is a full copy of the entire history. Its model — content-addressed objects linked into a commit graph, with branches as movable labels — is what makes it fast, safe, and confusing until you see the model.

**The Intuition:** Git stores snapshots, not diffs. Every commit is a full tree of files (compressed), plus parent links and metadata. Branches are just *names pointing at commits*. HEAD is "where am I." Once you see that commits are immutable and branches are stickers, the whole tool makes sense.

## The object model

```text
BLOB  — file content (hashed by SHA-1)
TREE  — a directory: maps names → blobs/trees
COMMIT — tree + parent(s) + author + message
REF   — a name → commit (branches, tags, HEAD)

Commits are immutable; history = the graph of commits.
```

```
commit  →  tree  →  blobs (files)
   │
   └── parent → commit (previous)
```

## The three areas

```text
Working directory  — your files (editable)
Staging area (index) — what you're about to commit
Repository (.git) — committed history

git add  →  working → staged
git commit → staged → committed
```

## The core commands

```bash
git init                    # create a repo
git clone <url>             # full copy of a remote repo
git status                  # what changed / what's staged
git add <file>              # stage a change (or . for everything)
git commit -m "message"     # snapshot the staged changes
git log --oneline           # commit history (one line each)
git diff                    # unstaged changes
git diff --staged           # staged changes
git show <commit>           # what a commit changed
git rm / git mv             # remove / rename tracked files
```

## The commit lifecycle

```bash
# New feature flow:
git checkout -b feature-x     # create + switch branch
# ... edit files ...
git status                    # see what changed
git add src/app.py tests/     # stage relevant files
git commit -m "add feature x"
git push -u origin feature-x  # publish the branch
```

## Fixing mistakes

```bash
git commit --amend            # edit the LAST commit's message (local only!)
git reset --soft HEAD~1       # undo commit, keep changes staged
git reset --hard HEAD~1       # undo commit AND throw away changes (DANGER)
git checkout -- <file>        # discard unstaged changes to a file
git restore <file>            # same as checkout -- (modern)
git revert <commit>           # NEW commit that undoes an old one (safe on shared)
git stash                     # save WIP, clean working dir
git stash pop                 # restore it
git clean -fd                 # remove untracked files (DANGER)
```

## Reading history

```bash
git log --oneline --graph --all     # the tree view
git log -p <file>                   # history of a file, with diffs
git blame <file>                    # who changed each line, when
git show <commit>:<file>            # file content at a commit
git diff HEAD~3 HEAD                # range comparison
```

---

**Setup:** Undo the last commit but keep the work.

**Solution:**
```bash
git reset --soft HEAD~1
# HEAD moved back one; the changes are staged, ready to recommit.
git status    # changes still there, staged
git commit -m "better message"
```

**Key insight:** `--soft` moves the branch pointer only — nothing else changes. `--hard` would also wipe the working directory. On *shared* branches use `revert` instead (rewriting pushed history breaks everyone).

---

**Setup:** You accidentally committed a secret (API key). Remove it from history.

**Solution:**
```bash
# 1. Remove the secret from the working files, commit the fix.
# 2. Rewrite history to purge it (ONLY if never pushed / local):
git filter-branch --tree-filter 'rm -f .env' HEAD
# or: git filter-repo --invert-paths --path .env
# 3. Force-push, and ROTATE the key — assume it's compromised.
```

**Key insight:** Once pushed, the secret is effectively public — rewriting history doesn't un-leak it. The industry rule: *rotate the credential*; history rewriting is cleanup, not security. Also never commit `.env` files — use `.gitignore`.

---

**Setup:** Find the commit that introduced a bug among the last 200.

**Solution:**
```bash
git bisect start
git bisect bad                # current HEAD is broken
git bisect good v1.0          # a known-good tag
# Git checks out the midpoint; you run the failing test:
git bisect good|bad           # repeat ~log2(200) ≈ 8 times
git bisect reset
```

**Key insight:** `git bisect` is *binary search on history* — $O(\log n)$ commits checked instead of scanning. This is the same divide-and-conquer as the DSA binary search, applied to commit graphs.

---

**Setup:** Why does `git pull` sometimes complain about local changes?

**Solution:** `pull` = fetch + merge. If your local changes touch files the merge wants to update, Git refuses (or conflicts). Fix: commit/stash your changes first (`git stash`), pull, then `git stash pop`. Or rebase: `git pull --rebase` replays your commits on top of the remote.

**Key insight:** Git never silently overwrites uncommitted work — it errors loudly. The conflict-resolution chore is the price of concurrent editing, and it's why *small, frequent commits* make merges painless.

---

## Practice (try before peeking)

1. `git reset --soft HEAD~1` vs `git revert HEAD` — which is safe on a shared branch?
2. Where do staged-but-uncommitted changes live?
3. What does `git clone` give you that `git init` doesn't?

<details><summary>Answers</summary>

1. `revert` is safe on shared branches (it adds a new commit); `reset` rewrites history, so only on local, unpushed commits.
2. The staging area (index) — a file in `.git/index` listing what the next commit will contain.
3. `clone` copies an existing repository *including all history and remotes*; `init` creates an empty new one.

</details>

---

**Common traps:**
- `git reset --hard` discarding uncommitted work (use `--soft` or stash first)
- Amending pushed commits — history mismatch for everyone
- Committing secrets — rotate, don't rely on cleanup
- Huge files in git — Git isn't a blob store; use Git LFS or object storage
- `git pull` overwriting work — commit/stash before pulling

---
