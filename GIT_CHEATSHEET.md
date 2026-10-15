# Git Team Workflow Cheat Sheet

How to work on your own branch, keep it up to date, and get your work merged
**without ever touching `main` directly**. Only the manager merges into `main`.

---

## 0. The big picture

```
main  ───●────────●──────────────●───────   (protected, manager merges here)
          \                      ↑
           \                     │  Pull Request (PR), reviewed + merged by manager
your-branch ●───●───●───●────────┘
               (your commits)
```

- `main` = the official code. Nobody commits to it directly.
- Everyone creates **their own branch** from `main`, works there, pushes it.
- They open a **Pull Request (PR)** on GitHub: "please merge my branch into main".
- The manager reviews and clicks **Merge**.

---

## 1. One-time setup (each new person does this once)

```bash
# Tell git who you are (shows on every commit)
git config --global user.name  "Your Name"
git config --global user.email "you@company.com"

# Optional but nice
git config --global init.defaultBranch main
git config --global pull.rebase false      # "git pull" merges (simplest for beginners)

# Get the company repo onto your computer
git clone https://github.com/<org>/<repo>.git
cd <repo>
```

Check it worked:

```bash
git config --list | grep user
git remote -v          # shows "origin" = the GitHub repo
```

> If GitHub asks for a password, use a **Personal Access Token** (GitHub →
> Settings → Developer settings → Tokens) or set up an SSH key.

---

## 2. Create your own branch

Always start from an up-to-date `main`:

```bash
git checkout main
git pull origin main                 # get latest code
git checkout -b feature/my-task      # create + switch to your branch
```

Branch naming ideas: `feature/login-page`, `fix/crash-on-save`, `yourname/dev`.

```bash
git branch            # list local branches, * = the one you are on
git branch -a         # include remote branches
git checkout <name>   # switch branch   (newer: git switch <name>)
```

---

## 3. Daily loop: change → add → commit → push

```bash
git status                      # what changed? (run this ALL the time)
git diff                        # see exact line changes

git add file1.py file2.py       # stage specific files
git add .                       # or stage everything in this folder

git commit -m "Add login form validation"   # save a snapshot locally

git push -u origin feature/my-task          # first push of a new branch
git push                                    # later pushes (after -u once)
```

What each step means:

| Step       | Where it lives            |
|------------|---------------------------|
| edit file  | your working folder only  |
| `git add`  | "staging area" (ready)    |
| `git commit` | your local history      |
| `git push` | GitHub (others can see)   |

Good commit message: short, present tense, says *what* and *why*:
`Fix crash when cart is empty` — not `changes` or `asdf`.

---

## 4. Open a Pull Request (ask manager to merge)

1. Push your branch (step 3).
2. Go to the repo on GitHub → it shows **"Compare & pull request"** → click it.
3. **base:** `main`  ←  **compare:** `feature/my-task`
4. Write a title + what you changed, assign the manager as reviewer.
5. Click **Create pull request**.

If the manager asks for changes: just edit, `add`, `commit`, `push` again on the
**same branch** — the PR updates automatically.

---

## 5. Keep your branch up to date with main

Others' work gets merged into `main` while you work. Pull it into your branch
regularly (and always before opening a PR):

```bash
git checkout feature/my-task
git fetch origin
git merge origin/main          # bring main's new commits into your branch
git push
```

This changes **your branch only**, never `main`. Safe.

---

## 6. Merge conflicts (don't panic)

A conflict = you and someone else edited the same lines. Git shows:

```
<<<<<<< HEAD
your version of the line
=======
the version from main
>>>>>>> origin/main
```

Fix it:

```bash
git status                     # lists "both modified" files
# open each file, keep the right code, DELETE the <<<<<<< ======= >>>>>>> lines
git add <fixed-file>
git commit                     # finishes the merge
git push
```

Changed your mind mid-merge? `git merge --abort` puts everything back.

---

## 7. How merging works (what the manager does)

**On GitHub (normal way):** open the PR → review "Files changed" → **Merge pull request**.
Options:
- *Create a merge commit* – keeps all your commits + a merge commit.
- *Squash and merge* – all your commits become one clean commit on main.
- *Rebase and merge* – replays your commits on top of main, no merge commit.

**Same thing from the command line** (only whoever owns `main` does this):

```bash
git checkout main
git pull origin main
git merge feature/my-task
git push origin main
```

After merge, clean up your branch:

```bash
git checkout main
git pull origin main
git branch -d feature/my-task              # delete local branch
git push origin --delete feature/my-task   # delete remote branch (optional)
```

Then start the next task with a fresh branch (step 2).

---

## 8. Protect main (manager does once on GitHub)

GitHub repo → **Settings → Branches → Add branch protection rule** (or *Rulesets*):
- Branch name pattern: `main`
- ✅ Require a pull request before merging
- ✅ Require approvals (1)
- ✅ Do not allow bypassing / block force pushes

Now nobody can push directly to `main`, even by accident.
Give teammates **Write** access (Settings → Collaborators) so they can push their own branches.

---

## 9. Undo / oops commands

| Problem | Command |
|---|---|
| Undo changes in a file (not yet added) | `git restore file.py` |
| Unstage a file (after `git add`) | `git restore --staged file.py` |
| Fix last commit message (not pushed yet) | `git commit --amend -m "new msg"` |
| Undo last commit, keep the changes | `git reset --soft HEAD~1` |
| Undo a commit that's already pushed (safe) | `git revert <commit-id>` |
| Save work temporarily to switch branch | `git stash` … later `git stash pop` |
| Committed on main by mistake (not pushed) | `git branch feature/x` → `git reset --hard origin/main` → `git checkout feature/x` |

⚠️ Never `git push --force` on `main` or on someone else's branch.

---

## 10. Looking around

```bash
git log --oneline --graph --all     # pretty history of all branches
git log -p file.py                  # history of one file
git show <commit-id>                # what a commit changed
git blame file.py                   # who wrote each line
git diff main..feature/my-task      # difference between branches
```

---

## 11. The whole flow in 10 lines (pin this)

```bash
git checkout main && git pull origin main      # 1. fresh main
git checkout -b feature/my-task                # 2. own branch
# ... edit code ...
git status                                     # 3. check
git add .                                      # 4. stage
git commit -m "Describe change"                # 5. commit
git fetch origin && git merge origin/main      # 6. stay updated
git push -u origin feature/my-task             # 7. push
# 8. GitHub → open Pull Request into main
# 9. Manager reviews + merges
git checkout main && git pull && git branch -d feature/my-task   # 10. clean up
```
