# Git Branching & Collaboration

Branches are the collaboration engine — isolated workspaces that merge cleanly when done. The two integration strategies, **merge** and **rebase**, produce different histories. And the daily rituals — pull requests, code review, conflict resolution — are the actual workflow of professional teams.

**The Intuition:** A branch is a movable label on a commit. Creating one is instant (a sticker, not a copy). Multiple people working on their own branches diverge; merging or rebasing brings them back together. The conflict is just Git saying "two people changed the same lines — you decide."

## Branch mechanics

```bash
git branch                # list branches
git branch feature-x      # create (pointer at current commit)
git checkout feature-x    # switch to it
git switch feature-x      # modern equivalent
git checkout -b feature-x # create + switch in one
git branch -d feature-x   # delete (only if merged)
git branch -D feature-x   # force delete (unmerged)
```

## Merge vs Rebase — the trade-off

```text
MERGE:
  - Preserves branch topology (a merge commit records "two histories joined")
  - History is truthful but noisy (diamond shapes in the graph)
  - Safe on shared branches
  - Conflict resolved ONCE at the merge point

REBASE:
  - Replays your commits on top of another branch → LINEAR history
  - History is clean ("feature built on latest main")
  - Rewrites commit hashes → ONLY for local/unpushed branches
  - Conflicts resolved PER-COMMIT (replayed one at a time)
```

```bash
# Merge:
git checkout main
git merge feature-x

# Rebase:
git checkout feature-x
git rebase main        # replay feature commits on top of main
git checkout main
git merge feature-x    # now a fast-forward (linear)
```

## The golden rule

> **Never rewrite history you've shared.** Rebase local work; merge (or force-push with coordination) for shared.

## Pull requests — the collaboration unit

```text
1. Branch from main:        git checkout -b feature/login
2. Commit small, frequent:  git commit -m "add login form"
3. Push the branch:         git push -u origin feature/login
4. Open a Pull Request (GitHub/GitLab) — the review unit
5. Reviewers comment → you push more commits to the same branch
6. CI runs tests on every push
7. Merge (squash merge keeps history clean) or rebase-merge
```

## Conflict resolution

```bash
git merge feature-x
# CONFLICT (content): Merge conflict in src/app.py
git status          # shows "both modified: src/app.py"
# Open the file: find the markers:
<<<<<<< HEAD
...your version...
=======
...their version...
>>>>>>> feature-x
# Edit to the desired result, delete the markers, then:
git add src/app.py
git commit          # complete the merge
```

**Reduce conflicts:** small commits, frequent pulls/rebases, don't both edit the same files, keep feature branches short-lived.

## Common workflows

**GitHub Flow (simple):** main is always deployable; feature branches → PRs → merge to main.
**Git Flow (release-oriented):** main + develop + feature/ + release/ + hotfix/ branches.
**Trunk-based (CI-friendly):** everyone merges to main daily in small PRs — merge conflicts almost vanish.

## Collaboration hygiene

```bash
git fetch                   # download remote refs WITHOUT merging
git pull --rebase           # rebase local commits onto updated main
git log origin/main..HEAD   # what I have that's not on main yet
git diff main...feature     # cumulative feature diff (three-dot)
git push --force-with-lease # safer force-push (refuses if remote moved)
```

---

**Setup:** A teammate pushed to main while you were working. Integrate cleanly.

**Solution:**
```bash
git fetch origin
git rebase origin/main       # replay your local commits on the new main
# resolve any conflicts per-commit, then:
git push --force-with-lease
```
Or, if you prefer merge: `git merge origin/main` then push normally.

**Key insight:** Rebase keeps your feature branch's history linear and replayable. `--force-with-lease` protects against clobbering someone else's push (it fails if the remote advanced since your last fetch).

---

**Setup:** A PR has 40 tiny commits ("wip", "fix typo"). Merge it cleanly.

**Solution:** Squash merge — the PR's commits collapse into ONE commit on main:
```bash
git merge --squash feature-x
git commit -m "feat: login flow (#123)"
```
or use the GitHub "Squash and merge" button.

**Key insight:** Squash merges trade granular history for readability — the feature becomes one logical change on main. The original commits stay on the branch (for the reviewer) but don't pollute main.

---

**Setup:** Two branches changed `config.py` differently — resolve the conflict.

**Solution:**
```bash
# After the merge reports a conflict:
git status                 # both modified: config.py
# Open config.py, see <<<<<<< ======= >>>>>>> markers.
# Decide the correct merged content (sometimes BOTH changes belong:
# one added a setting, the other changed a value).
# Remove markers, save:
git add config.py
git commit
```

**Key insight:** Conflict resolution is a *design decision*, not a mechanical pick — the merged result must preserve both features' intent. This is why reviewing the conflict (not just "taking ours") matters. Tools like VS Code's merge editor make this visual.

---

**Setup:** You started from an outdated main and now have conflicts everywhere.

**Solution:** Rebase onto the freshest main *early and often*:
```bash
git fetch
git rebase origin/main
# small conflicts, commit by commit — much easier than one giant merge at the end
```
This is the "integrate continuously" strategy — the longer you diverge, the more painful the merge.

**Key insight:** Conflict pain ∝ divergence time. Short-lived branches + frequent rebases make conflicts rare and small. This is why CI + trunk-based development reduces merge friction.

---

## Practice (try before peeking)

1. Rebase rewrites what — and why does that matter for shared branches?
2. `git fetch` vs `git pull`?
3. When is a merge commit created?

<details><summary>Answers</summary>

1. Commit hashes (and thus history). Anyone else who based work on those commits gets mismatched history — hence "never rewrite shared history."
2. `fetch` downloads refs only; `pull` fetches *and* integrates (merge/rebase).
3. When the merged branch isn't a descendant of the target — the histories diverged, so Git creates a merge commit joining them (fast-forward skips it).

</details>

---

**Common traps:**
- Force-pushing over teammates' work (use `--force-with-lease`)
- Rebasing shared branches — history desync for everyone
- Letting branches live for weeks — merge pain grows
- Committing secrets or build artifacts — use `.gitignore`
- Resolving conflicts by blindly picking "ours/theirs" — data loss

---
