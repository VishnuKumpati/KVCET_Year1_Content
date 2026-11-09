# Working with Remote Repositories

In real projects, several programmers work on the same code. Each programmer keeps a copy of the repository on their own computer, and the shared copy lives online, such as on GitHub. When one programmer pushes new commits, those changes become available on GitHub. Other programmers need to pull them to update their local copies. Programmers need a way to download a repository, bring in changes others have made, and share their own changes without overwriting anyone's work.

| Term | Meaning |
| --- | --- |
| **Local repository** | The copy of a repository on your own computer, where you edit files and make commits |
| **Remote repository** | The shared copy of a repository stored online, such as on GitHub, that the whole team pushes to |
| **Cloning** | Downloading a complete copy of a remote repository to your computer |
| **Pulling** | Downloading new commits from the remote repository and adding them to your local repository |

## One-Time Setup for Pulling

Run this command **once** on your computer:

```
git config --global pull.rebase false
```

When both you and someone else have made new commits, `git pull` must combine the two sets of commits. This setting tells Git to combine them by **merging**, which is the simplest method for beginners. Without this setting, Git may ask you to specify how it should combine divergent branches, and some configurations stop with the message `fatal: Need to specify how to reconcile divergent branches`.

## Step 1: Clone a Repository

> **`git clone` creates a local copy of a remote repository, including all its files and its full commit history.**

On GitHub, open the repository, click the green **Code** button, and copy the **HTTPS** URL. Then, in the terminal, go to the folder where you keep your projects and run:

```
git clone https://github.com/ravikumar/calculator_project.git
```

**Possible output (shortened):**

```
Cloning into 'calculator_project'...
...
```

`git clone` does four things automatically:

1. Creates a new folder named after the repository, `calculator_project`.
2. Downloads all the files and the full commit history into it.
3. Connects the folder to the remote repository under the name **`origin`**.
4. Switches to the repository's default branch, which is often named `main`.

Move into the new folder before running any other Git command:

```
cd calculator_project
```

A cloned repository is already connected to GitHub, so **`git init` and `git remote add` are not needed**. You can check the connection with `git remote -v`, as in **Git Fundamentals**.

### Cloning Compared with `git init`

| | `git init` | `git clone` |
| --- | --- | --- |
| **Used when** | Starting a new project on your computer | Getting a copy of a project that already exists on GitHub |
| **Files** | Uses the files already in your folder | Downloads the files from GitHub |
| **Remote** | Must be connected with `git remote add` | Connected to `origin` automatically |

## Step 2: Work and Push as Usual

Inside the cloned repository, work exactly as in **Git Fundamentals**:

```
git status
git add .
git commit -m "Describe the change"
git push
```

When you clone a repository, Git automatically links your local branch to its corresponding remote branch. So, you do not need `-u` for the first push.

## Step 3: Pull Changes from the Remote Repository

> **`git pull` downloads new commits from the remote repository and combines them with your current branch, so your local repository is up to date.**

Suppose two programmers, Ravi and Priya, have both cloned `calculator_project`. Priya adds a `subtract()` function, commits it and pushes it to GitHub. Ravi's copy does not have this change yet.

### Why `git status` Does Not Show New Remote Commits

If Ravi runs `git status`, he sees:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

This looks up to date, but it is not. **`git status` does not check GitHub.** It compares Ravi's local branch with the latest remote information Git has downloaded. Since Ravi has not pulled Priya's new commit, Git does not know about it yet.

### Pulling the New Commit

Ravi runs:

```
git pull
```

**Possible output:**

```
From https://github.com/ravikumar/calculator_project
   ae3d61d..bb0fbcd  main       -> origin/main
Updating ae3d61d..bb0fbcd
Fast-forward
 calculator.py | 4 ++++
 1 file changed, 4 insertions(+)
```

- `ae3d61d..bb0fbcd` shows Ravi's copy moving from his last commit to Priya's new commit. Your commit IDs will be different.
- **`Fast-forward`** means Ravi had no new commits of his own, so Git simply moved his branch forward to include Priya's commit.
- `calculator.py | 4 ++++` shows that the pull brought four added lines into `calculator.py`.

Ravi's `calculator.py` now contains Priya's `subtract()` function.

## Pull Before You Push

Git normally **rejects** a push when the remote branch contains commits that your local branch does not have. This prevents your push from moving the remote branch backwards and leaving other people's commits out of its history.

Suppose Priya pushes another commit, and Ravi, without pulling first, commits his own change and tries to push:

```
git push
```

**Possible output:**

```
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/ravikumar/calculator_project.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
```

**`rejected`** means the push did not happen. The hint explains why: the remote repository contains Priya's new commit, which Ravi does not have.

To fix it, Ravi pulls first, then pushes:

```
git pull
git push
```

**Possible output of `git pull`:**

```
Merge made by the 'ort' strategy.
 calculator.py | 4 ++++
 1 file changed, 4 insertions(+)
```

- `calculator.py | 4 ++++` shows that the merge brought four added lines into `calculator.py`.
- Both Ravi and Priya had new commits, so Git **merged** them: it combined Priya's commit with Ravi's commit by creating a new **merge commit**.
- `'ort'` is the name of the method Git uses to combine the changes. You do not need to choose it.
- `git push` now succeeds, because Ravi's repository contains every commit that is on GitHub.

### If a Text Editor Opens

When `git pull` creates a merge commit, Git may open a text editor for the merge commit's message. Git has already written a suitable message, so you only need to save and close the editor:

- **In VS Code** or a similar editor, save the file and close its tab.
- **In Vim**, an editor that runs inside the terminal, type `:wq` and press **Enter**.

## The Everyday Team Workflow

When working with others, follow this order every time:

1. **`git pull`**: get the latest changes from GitHub **before** you start working.
2. **Edit** your files.
3. **`git add .`** and **`git commit -m "message"`**: save your changes locally.
4. **`git pull`**: get any changes others pushed while you were working.
5. **`git push`**: share your commits with the team.

Pulling at the start and again before pushing keeps your copy up to date and avoids rejected pushes.

## Command Summary

| Command | What It Does |
| --- | --- |
| `git config --global pull.rebase false` | Sets `git pull` to combine changes by merging. Run once |
| `git clone <url>` | Creates a local copy of a remote repository |
| `git pull` | Downloads new commits and combines them with your current branch |
| `git push` | Sends your commits to the remote repository |
| `git remote -v` | Shows which remote repository the local repository is connected to |

## Points to Remember

- **`git clone`** downloads a complete repository and connects it to `origin` automatically.
- **`git pull`** brings other people's commits into your local repository.
- **`git status` does not check GitHub**, so pull to get the latest changes.
- **Always pull before you push.** Git rejects a push when the remote repository has commits you do not have.

## Further Reading

- 📎 **Official Git book: Working with Remotes** — https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes
- 📎 **GitHub guide to cloning a repository** — https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository

In these examples, Git combined Ravi's and Priya's changes automatically, because they changed different parts of the code. But what happens when two people change the **same lines**? In the next topic, **Merge Conflicts**, you'll learn why conflicts happen and how to resolve them.
