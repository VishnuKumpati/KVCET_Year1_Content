# Merge Conflicts

Git merges most changes automatically. Two people editing different files, or different parts of the same file, need no intervention.

A **merge conflict** happens when two branches changed **the same lines** of the same file. Git cannot know which version is wanted, so it stops and asks.

A conflict is not a failure or a mistake. It is Git declining to guess, which is the correct behaviour — the alternative is silently discarding someone's work.

## Causing One

`main` and `feature/hindi` both changed line 3 of `config.py`:

```
git merge feature/hindi
```

**Output:**

```
Auto-merging config.py
CONFLICT (content): Merge conflict in config.py
Automatic merge failed; fix conflicts and then commit the result.
```

The merge is now **in progress** and incomplete. Nothing is broken and nothing is lost — the repository is in a state waiting for a decision.

```
git status
```

**Output:**

```
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   config.py

no changes added to commit (use "git add" and/or "git commit -a")
```

Git says exactly what to do. "Unmerged paths" lists the files needing attention, and the hints give both routes out — resolve and commit, or abort.

```
git status --short
```

**Output:**

```
UU config.py
```

`UU` means unmerged on both sides. Files not listed merged cleanly and need nothing.

## The Conflict Markers

Git edits the conflicted file, putting both versions in with markers:

```python
TIMEOUT = 10
RETRIES = 3
<<<<<<< HEAD
GREETING = "Welcome"
=======
GREETING = "Namaste"
>>>>>>> feature/hindi
```

Three markers divide it:

| Marker | Means |
| --- | --- |
| `<<<<<<< HEAD` | start of the version on your current branch |
| `=======` | the divider |
| `>>>>>>> feature/hindi` | end of the version from the branch being merged |

So the top section is **yours** — what `main` had — and the bottom is **theirs**, from `feature/hindi`. `HEAD` always means the branch you are on.

Lines 1 and 2 are outside the markers, because both branches left them alone. Only the disputed lines are wrapped.

`git diff` during a conflict shows a combined view:

```
git diff
```

**Output:**

```
diff --cc config.py
index a797ac0,68e23d7..0000000
--- a/config.py
+++ b/config.py
@@@ -1,3 -1,3 +1,7 @@@
  TIMEOUT = 10
  RETRIES = 3
++<<<<<<< HEAD
 +GREETING = "Welcome"
++=======
+ GREETING = "Namaste"
++>>>>>>> feature/hindi
```

## Resolving

Resolving means editing the file until it is what you want, **including deleting all three marker lines**.

There are four possible answers:

**Keep yours** — delete the markers and their version.

**Keep theirs** — delete the markers and your version.

**Keep both** — if the changes are compatible, keep both lines and remove the markers.

**Write something new** — often the right answer, when both changes point at a requirement neither fully met.

Here the branch's version is wanted:

```python
TIMEOUT = 10
RETRIES = 3
GREETING = "Namaste"
```

No markers remain. The file is ordinary Python again.

**Leaving a marker in is the classic mistake.** `<<<<<<< HEAD` is not valid Python, so the file will not even parse — which is why running the tests after resolving catches it immediately. Searching the project for `<<<<<<<` before committing is a good habit.

## Completing the Merge

`git add` on a conflicted file is how you tell Git it is resolved:

```
git add config.py
git status --short
```

**Output:**

```
M  config.py
```

`UU` became `M`. Once every conflicted file is staged, commit:

```
git commit --no-edit
```

**Output:**

```
[main 2f24558] Merge branch 'feature/hindi'
```

Git prepares a merge commit message automatically; `--no-edit` accepts it. Without the flag, an editor opens for you to adjust it — worth doing when the resolution involved a real decision.

```
git log --oneline --graph
```

**Output:**

```
*   2f24558 Merge branch 'feature/hindi'
|\
| * 3c0e414 Greet in Hindi
* | d45b7b5 Greet with Welcome
|/
* 74ee4a9 Add configuration
```

Both branches are in the history, joined by the merge commit.

## Aborting

If the conflict is larger than expected, or you want to start again, abort:

```
git merge --abort
git status
```

**Output:**

```
On branch main
nothing to commit, working tree clean
```

Everything returns to how it was before the merge began. The markers are gone, the files are restored, and nothing is lost.

`git merge --abort` is the safety net worth knowing early. A conflict can always be undone, which makes attempting one low-risk.

## Taking One Side Wholesale

For a file where one version is simply correct — a generated file, or a lock file — the whole side can be taken:

```
git checkout --ours config.py
```

**Output:**

```
Updated 1 path from the index
```

`--ours` is your current branch, `--theirs` is the branch being merged. The file is replaced entirely, markers and all.

Use this only when you are certain the other side has nothing worth keeping. For source code, read both versions.

Be careful with the words: during a `merge`, "ours" is the branch you are on. During a `rebase` they are swapped, which is a genuine trap.

## Reducing Conflicts

Conflicts cannot be eliminated and can be made rare.

**Pull often.** Most conflicts come from working against a stale `main` for days. `git pull` before starting anything.

**Merge `main` into your branch regularly.** Several small conflicts along the way beat one enormous one at the end.

**Keep branches short.** A branch merged within a day or two rarely conflicts. One open for a month conflicts with everything.

**Keep commits small and focused.** A conflict in a fifty-line commit is easier to reason about than one in a five-hundred-line commit.

**Agree who is working where.** Two people rewriting the same module in the same week will conflict, however good the tooling. That is a coordination problem, not a Git problem.

**Run a formatter.** Consistent formatting means fewer changes that are only whitespace, and whitespace-only differences cause conflicts as readily as real ones.

## Resolving Well

**Understand both sides before choosing.** `git log --oneline main..feature/hindi` shows what the other branch was doing and why.

**Ask, when it is not your code.** The person who wrote the other version knows what it was for. A two-minute conversation beats a guess.

**Run the tests afterwards.** A resolution can be syntactically fine and logically wrong — keeping your version and silently discarding a bug fix, for instance. The tests are what catch that.

**Resolve in the smallest steps.** One file at a time, checking each, rather than editing everything and hoping.

Pull requests help here too: the platform shows conflicts before merging, and simple ones can often be resolved in the browser. Anything involving a real decision is better done locally, where the tests can run.

## Further Reading

- **The Pro Git book on basic merge conflicts** — https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging
- **GitHub: resolving a merge conflict** — https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts

Git stops rather than guessing, marks both versions in the file, and waits; edit out the markers, `git add` to mark it resolved, commit — or `git merge --abort` to undo the whole thing. Next, the newest tool in the workflow and the care it needs.
