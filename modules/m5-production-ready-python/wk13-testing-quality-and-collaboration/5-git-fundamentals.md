# Git Fundamentals

Keeping old versions by copying folders — `project`, `project_final`, `project_final_v2`, `project_final_ACTUAL` — works for about a week. It cannot say what changed between two of them, cannot combine two people's work, and cannot recover the version from last Tuesday.

**Git** is a version control system. It records the history of a project as a series of snapshots, so any past state can be recovered, any change can be inspected, and several people can work on the same code without overwriting each other.

It is not a backup system and not a file-syncing service. It is a record of **changes**, with a reason attached to each one.

## Starting a Repository

`git init` turns a directory into a repository:

```
git init -b main
```

**Output:**

```
Initialized empty Git repository in C:/Users/anita/project/.git/
```

`-b main` names the first branch `main`. Older versions of Git used `master`; `main` is now the convention.

That creates a hidden `.git` directory holding the entire history. Delete it and the history is gone; the rest of the files are untouched.

`git status` is the command to run constantly — it says where things stand:

```
git status
```

**Output:**

```
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

## The Three Areas

Git has three places a change can be, and understanding them makes every command make sense.

```mermaid
flowchart LR
    A["Working directory<br>your files"] -->|"git add"| B["Staging area<br>the next commit"]
    B -->|"git commit"| C["Repository<br>permanent history"]
```

- The **working directory** is the files you edit.
- The **staging area** is what you have chosen to include in the next commit.
- The **repository** is the committed history.

The staging area is the part other systems do not have. It lets you commit *some* of your changes — the bug fix now, the half-finished experiment later — rather than everything at once.

## Tracking a File

Create `calculator.py`, then:

```
git status --short
```

**Output:**

```
?? calculator.py
```

`??` means untracked: Git can see the file and is not recording it.

`git add` moves it to the staging area:

```
git add calculator.py
git status --short
```

**Output:**

```
A  calculator.py
```

`A` means added — staged, ready to commit.

```
git commit -m "Add the add function"
```

**Output:**

```
[main (root-commit) 52bb550] Add the add function
 1 file changed, 2 insertions(+)
 create mode 100644 calculator.py
```

`52bb550` is the start of the commit's hash, its unique identifier. Yours will differ — hashes are computed from the content, the author and the time.

```
git status
```

**Output:**

```
On branch main
nothing to commit, working tree clean
```

"Working tree clean" means everything is committed. It is the state to be in before switching branches or finishing for the day.

## Seeing What Changed

Add a `subtract` function to the file, then:

```
git diff
```

**Output:**

```
diff --git a/calculator.py b/calculator.py
index 4693ad3..3b474e9 100644
--- a/calculator.py
+++ b/calculator.py
@@ -1,2 +1,6 @@
 def add(a, b):
     return a + b
+
+
+def subtract(a, b):
+    return a - b
```

Lines starting with `+` were added, `-` would mean removed, and unmarked lines are context. `@@ -1,2 +1,6 @@` says the change is at line 1, replacing 2 lines with 6.

`git diff` shows **unstaged** changes. After `git add .` it shows nothing, because there is nothing left unstaged:

```
git add .
git diff
```

**Output:**

```
```

The staged changes are shown by `git diff --staged`:

```
git diff --staged
```

**Output:**

```
diff --git a/calculator.py b/calculator.py
index 4693ad3..3b474e9 100644
--- a/calculator.py
+++ b/calculator.py
@@ -1,2 +1,6 @@
 def add(a, b):
     return a + b
+
+
+def subtract(a, b):
+    return a - b
```

Run `git diff --staged` before every commit. It is the last chance to notice a debugging `print()` or a stray file.

`git add .` stages everything changed, which is convenient and is how unwanted files get committed. Check the status first.

## History

```
git commit -m "Add the subtract function"
git log --oneline
```

**Output:**

```
f91a271 Add the subtract function
52bb550 Add the add function
```

Newest first. `--oneline` is the readable form; plain `git log` gives the full entry:

```
git log -1
```

**Output:**

```
commit f91a271974973a8e7943130c68507579fad3cf9a
Author: Anita Sharma <anita@example.com>
Date:   Wed Sep 16 10:00:00 2026 +0000

    Add the subtract function
```

| Command | Shows |
| --- | --- |
| `git log --oneline` | one line per commit |
| `git log -5` | the last five |
| `git log --graph` | a diagram of branches |
| `git log -- filename` | commits touching one file |
| `git show <hash>` | one commit's full changes |

Set your identity once, before your first commit, or the author on every commit will be wrong:

```
git config --global user.name "Anita Sharma"
git config --global user.email "anita@example.com"
```

## Commit Messages

The message is the only explanation the next reader gets. "update", "fix", "stuff" and "asdf" waste the entire mechanism.

The convention: a short summary line in the **imperative mood**, as though completing the sentence "this commit will…":

```
Add the subtract function
Fix off-by-one error in last_three
Remove unused pandas dependency
```

Not "Added", not "Adds". The imperative matches the messages Git generates itself.

Keep the summary under about 50 characters. If more explanation is needed, leave a blank line and write a paragraph below it — that is where **why** belongs. The diff already shows what changed; it cannot show why.

Commit **one logical change at a time**. A commit containing a bug fix, a new feature and a rename cannot be described in one line, cannot be reviewed, and cannot be undone without losing the other two.

## .gitignore

Some files should never be committed: virtual environments, caches, secrets, editor settings, build output. `.gitignore` lists them:

```
venv/
.venv/
__pycache__/
*.pyc
.env
*.log
.DS_Store
.vscode/
```

With that in place, those files stop appearing in `git status`:

```
git status --short
```

**Output:**

```
?? .gitignore
```

The `__pycache__` directory that existed was ignored, and only `.gitignore` itself remains untracked — so commit it. `.gitignore` belongs in the repository, so everyone working on the project ignores the same things.

Two warnings.

**Patterns are literal.** `.env` matches a file called exactly `.env`. It does **not** match `secrets.env` — that needs `*.env`. Check with `git status` rather than assuming.

**Ignoring does not untrack.** A file already committed keeps being tracked no matter what `.gitignore` says. Remove it from tracking with `git rm --cached <file>`, and remember that its history remains — which is why a committed password must be treated as compromised and changed, not merely deleted.

## Undoing Things

| Situation | Command |
| --- | --- |
| Discard unstaged changes to a file | `git restore <file>` |
| Unstage a file, keeping the changes | `git restore --staged <file>` |
| Change the last commit's message | `git commit --amend` |
| Undo a commit, keeping the changes staged | `git reset --soft HEAD~1` |
| Undo a commit and the changes | `git reset --hard HEAD~1` |

`git restore` and `git reset --hard` **destroy work permanently**. There is no undo, because the changes were never committed and so were never recorded. Check `git status` and `git diff` before running either.

Never amend or reset a commit that has been pushed and that others may have pulled — it rewrites history that other people already have.

## Remotes

A remote is a copy of the repository somewhere else, usually on GitHub. It is what makes Git a collaboration tool rather than a personal undo history.

Starting from an existing remote:

```
git clone https://github.com/anita/project.git
```

Connecting an existing local repository to an empty remote:

```
git remote add origin https://github.com/anita/project.git
```

`origin` is the conventional name for the main remote.

```
git remote -v
```

**Output:**

```
origin  https://github.com/anita/project.git (fetch)
origin  https://github.com/anita/project.git (push)
```

`git push` sends your commits:

```
git push -u origin main
```

**Output:**

```
To https://github.com/anita/project.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

`-u` links the local branch to the remote one, so later pushes are just `git push`.

`git pull` fetches others' commits and merges them into your branch:

```
git pull
```

**Output:**

```
Already up to date.
```

Commits are local until pushed. A commit is safe from your own mistakes; it is not safe from a lost laptop until it is on the remote.

## The Daily Routine

```
git pull                      # start with everyone else's work
# ... make a change ...
git status                    # what have I touched?
git diff                      # what exactly changed?
git add calculator.py         # stage deliberately
git diff --staged             # last check
git commit -m "Add the multiply function"
git push
```

Commit often and in small pieces. A commit is a point you can return to, and one commit at the end of a week is one point in a week.

## Further Reading

- **The Pro Git book** — https://git-scm.com/book/en/v2
- **GitHub's Git documentation** — https://docs.github.com/en/get-started/using-git

`git add` stages, `git commit` records, `git status` and `git diff` show where things stand, and `git push` shares them. Next, working on something without disturbing the main line.
