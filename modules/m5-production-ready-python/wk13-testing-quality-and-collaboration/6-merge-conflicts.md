# Merge Conflicts

When two programmers change **different** parts of a file, Git combines their work automatically. But when two programmers make changes to the **same or overlapping parts** of a file, Git may not be able to combine their work automatically. It does not guess. Instead, it stops and asks a person to decide. This situation is called a **merge conflict**.

> **Merging is the process of combining the changes from two lines of development, such as your commits and another programmer's commits, into one.**

> **A merge conflict happens when Git cannot combine two sets of changes automatically because the changes overlap or contradict each other. A person must then decide what the final version should contain.**

Merge conflicts are a normal part of working in a team. They do not mean anything is broken, and no work is lost. Git simply needs your decision.

## How a Merge Conflict Happens

Ravi and Priya have both cloned `calculator_project`, which contains:

**File: `calculator.py`**

```python
def add(a, b):
    return a + b
```

Each of them changes the **same line**, the `return` line, in a different way:

| Programmer | New Line | Steps |
| --- | --- | --- |
| **Priya** | `return round(a + b, 2)` | Commits and pushes first |
| **Ravi** | `return float(a + b)` | Commits his change locally, then runs `git pull` to get Priya's change |

Ravi runs `git pull` to get Priya's commit, as taught in **Working with Remote Repositories**.

**Possible output:**

```
Auto-merging calculator.py
CONFLICT (content): Merge conflict in calculator.py
Automatic merge failed; fix conflicts and then commit the result.
```

- **`Auto-merging calculator.py`** means Git tried to combine the two versions of the file.
- **`CONFLICT (content)`** means both changed the same content, so Git could not combine them.
- **`Automatic merge failed`** means the merge is waiting for Ravi to resolve the conflict.

## Checking the Conflict with `git status`

```
git status
```

**Possible output (shortened):**

```
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
    both modified:   calculator.py
```

- **`unmerged paths`** means some files still have conflicts.
- **`both modified: calculator.py`** shows the file with the conflict: both Ravi and Priya changed it.
- Git also lists the next steps: fix the conflict, then run `git add` and `git commit`.

## Reading the Conflict Markers

Git writes **both** versions into the file, separated by special lines called **conflict markers**. Ravi's `calculator.py` now looks like this:

```
def add(a, b):
<<<<<<< HEAD
    return float(a + b)
=======
    return round(a + b, 2)
>>>>>>> f6b8c5ffc52e398a3b08d6ca4a76f1e53cebac6d
```

> **Conflict markers are lines that Git adds to a file to show where a conflict is and which version of the conflicting lines came from each side.**

| Marker | Meaning |
| --- | --- |
| `<<<<<<< HEAD` | The start of **your** version. **`HEAD`** means the commit you are currently on |
| `=======` | The line separating your version from the incoming version |
| `>>>>>>> f6b8c5f...` | The end of the **incoming** version, which is the change Git is trying to merge into your current branch. The long code after the marker is the ID of Priya's commit. Yours will be different. When merging a branch, the branch name appears here instead |

So, between `<<<<<<< HEAD` and `=======` is **Ravi's** line. Between `=======` and `>>>>>>>` is **Priya's** line. The line outside the markers, `def add(a, b):`, had no conflict, so it appears only once.

**The file cannot run while the markers are in it**, because they are not valid Python.

## Resolving a Merge Conflict

> **Resolving a merge conflict means editing the file to the correct final version, removing the conflict markers, and then committing the result.**

### Step 1: Decide What the Line Should Be

There are three choices:

- **Keep your version:** `return float(a + b)`
- **Keep the incoming version:** `return round(a + b, 2)`
- **Combine both**, if both changes are useful: `return round(float(a + b), 2)`

Combining both changes is not always as simple as putting both lines together. Understand what each change is intended to do before deciding on the final code. If you are unsure what the other programmer intended, **ask them** before deciding.

Ravi decides to combine both changes.

### Step 2: Edit the File and Remove the Markers

Ravi edits `calculator.py` so it contains only the final version:

```python
def add(a, b):
    return round(float(a + b), 2)
```

All three marker lines, `<<<<<<<`, `=======` and `>>>>>>>`, are deleted. **Every conflict marker must be removed**, and the file should be saved.

### Step 3: Stage the Resolved File

```
git add calculator.py
```

`git add` tells Git that the conflict in this file is **resolved**. Running `git status` now shows `All conflicts fixed but you are still merging.`

### Step 4: Commit the Merge

```
git commit -m "Merge rounding and float changes in add"
```

This creates the **merge commit**, which records the combined result.

### Step 5: Push the Result

```
git push
```

The resolved version is now on GitHub, and Priya can pull it.

## Resolving Conflicts in VS Code

VS Code highlights conflicts and shows clickable options directly above them:

| Option | What It Does |
| --- | --- |
| **Accept Current Change** | Keeps your version, the `HEAD` side |
| **Accept Incoming Change** | Keeps the incoming version |
| **Accept Both Changes** | Keeps both versions, one after the other |

Clicking an option removes the conflict markers for you. Always **check the result**, because "Accept Both Changes" simply places both lines one after the other, which may not be correct code. After choosing, save the file, then complete Steps 3 to 5.

## Cancelling a Merge with `git merge --abort`

If a conflict is confusing and you want to go back to how things were before the pull, run:

```
git merge --abort
```

This cancels the merge and attempts to return your files to the state they were in before the merge started. Your existing commits are kept. To avoid losing uncommitted work, commit or safely save your changes before starting a merge. You can then talk to the other programmer and try again later.

## Merge Conflicts with Branches

Conflicts also happen when combining **branches**. To bring the commits from a branch into `main`, switch to `main` and use **`git merge`**:

```
git switch main
git merge add-subtract
```

**Possible output:**

```
Updating e3f4b7f..81ae7df
Fast-forward
 calculator.py | 4 ++++
 1 file changed, 4 insertions(+)
```

**`git merge <branch>`** combines the named branch into the branch you are currently on. Here, `main` received the `subtract()` function from `add-subtract`.

The output shows **`Fast-forward`**. Git can complete a merge using a fast-forward when the current branch has no new commits of its own. If both branches have separate commits, Git may need to create a merge commit or report a conflict.

If both branches changed the same lines, `git merge` reports a conflict exactly like `git pull` did, and you resolve it with **the same five steps**.

## Avoiding Merge Conflicts

Conflicts cannot always be avoided, but they can be made rare and small:

- **Pull often**, especially before you start working, so you build on the latest code.
- **Commit and push small changes often**, so your work and others' work do not drift far apart.
- **Talk to your team** about who is working on which file or function.
- **Use branches** for separate pieces of work, as shown in **Git Fundamentals**.

## Command Summary

| Command | What It Does |
| --- | --- |
| `git status` | Shows which files have conflicts, listed under **Unmerged paths** |
| `git add <file>` | Marks a file's conflict as resolved |
| `git commit -m "message"` | Creates the merge commit after all conflicts are resolved |
| `git merge --abort` | Cancels the merge and returns to the state before it started |
| `git merge <branch>` | Combines a branch into the current branch |

## Points to Remember

- **A merge conflict** happens when two sets of changes overlap or contradict each other, so Git cannot combine them automatically.
- **Conflict markers** show your version, between `<<<<<<< HEAD` and `=======`, and the incoming version, between `=======` and `>>>>>>>`.
- **To resolve a conflict**, edit the file to the correct version, remove every marker, then run `git add`, `git commit` and `git push`.
- **`git merge --abort`** cancels a merge if you need to start again.

## Further Reading

- 📎 **Official Git book: Basic Merge Conflicts** — https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging
- 📎 **GitHub guide to resolving a merge conflict using the command line** — https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line

You can now test, log, track and share code as part of a team. In the final topic, **AI-Assisted Development**, you'll learn how to use AI tools to help write, explain and review code, while staying responsible for its quality.
