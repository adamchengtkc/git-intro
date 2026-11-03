# Git for Everyone

A friendly, hands-on introduction to Git for people who don't write code. Use Git to keep track of changes to documents, plans, spreadsheets, and other files—and to recover or collaborate with confidence.

**You will learn to:** save checkpoints, see what changed, try a separate version, and share work. No programming experience is needed.

## Git and GitHub: what is the difference?

- **Git** is a tool on your computer that records changes to files.
- **GitHub** is a website where you can store Git projects and work with other people. You can learn the Git basics without a GitHub account.

Think of Git as a history-aware folder. Unlike repeatedly saving files as `plan-final-v2-really-final.docx`, Git keeps named checkpoints in one project.

## The three places to know

Git gives you a chance to review changes before saving a checkpoint:

```text
Your project folder        Staging area             Saved history
┌──────────────────┐       ┌─────────────────┐      ┌───────────────┐
│ Edit and create  │ ────▶ │ Choose changes  │ ───▶ │ Commit: save  │
│ files here       │  add  │ for next save   │ save │ a checkpoint  │
└──────────────────┘       └─────────────────┘      └───────────────┘
```

In Git, a saved checkpoint is called a **commit**. You can make one whenever you reach a useful stopping point.

## A first project, step by step

This example uses a terminal (also called a command line), a text-based way to give your computer instructions. Copy one command at a time. Replace the example folder name if you like.

### 1. Create a practice folder

```bash
mkdir weekend-plan
cd weekend-plan
git init -b main
```

`git init -b main` starts tracking this folder and names the first branch `main`. It does not upload anything.

### 2. Make a file

Open a text editor, write a short plan, and save it in the `weekend-plan` folder as `plan.txt`. For example:

```text
Weekend plan
- Visit the farmers market
- Have a picnic
```

### 3. Review and choose what to save

```bash
git status
```

Git will show that `plan.txt` is new. Now tell Git to include it in your next checkpoint:

```bash
git add plan.txt
git status
```

The file is staged: selected for the next checkpoint. Staging is like putting selected papers in an envelope before sealing it.

### 4. Save your first checkpoint

```bash
git commit -m "Add first weekend plan"
```

The message in quotation marks is a short note to your future self about what this checkpoint contains.

### 5. Make a change and save another checkpoint

Edit `plan.txt` to add `- Bring a blanket`, then run:

```bash
git status
git diff
git add plan.txt
git commit -m "Add picnic reminder"
git log --oneline
```

`git diff` previews what changed. `git log --oneline` shows your saved checkpoints, newest first.

Your project now has a history:

```text
oldest                                              newest
● Add first weekend plan ───▶ ● Add picnic reminder
             checkpoint 1                 checkpoint 2
```

## Try a separate idea with a branch

A **branch** is a separate line of work. It lets you experiment without changing the version you consider your main one.

```bash
git switch -c rainy-day
```

Now edit `plan.txt` to add a rainy-day activity, and save that change:

```bash
git add plan.txt
git commit -m "Add rainy-day activity"
```

Switch back to the original branch with:

```bash
git switch main
```

The rainy-day change is still saved on its branch; it has not been added to `main`. Branches are useful for trying options, not just for software.

```text
                         ● Rainy-day activity (rainy-day)
                        /
● First plan ───▶ ● Picnic reminder (main)
```

If you are using an existing project, its original branch may be called `master`. Use that name instead of `main` throughout this guide. Run `git branch` to list the branches; the star marks the current one.

## Merge your idea back into main

A **merge** brings changes from another branch into the branch you are currently on. When you are happy with the rainy-day idea, bring it into `main`:

```bash
git status
git switch main
git merge rainy-day
git status
```

Before switching or merging, make sure `git status` reports a clean working tree. If it does not, finish and commit your changes first.

Open `plan.txt`: the rainy-day activity is now part of `main`. The direction matters: switching to `main` before running `git merge rainy-day` brings the changes **into main**. It does not delete the `rainy-day` branch.

In this example, `main` has not changed since you created `rainy-day`, so Git can simply move `main` forward. Git calls this a **fast-forward** merge. If both branches have new commits and their changes can be combined automatically, Git creates a merge commit instead. If an editor opens for its message, save and close it to finish.

Check the combined history with:

```bash
git log --oneline --graph --all
```

## Resolve a merge conflict

Changes on `main` do not always cause a conflict. A **merge conflict** happens when Git cannot automatically combine changes, such as when two branches edit the same line differently. Git pauses the merge so you can choose the final text.

Try this after merging `rainy-day`. On `main`, add this line to `plan.txt`:

```text
- Activity: visit the park
```

Save it, commit it, and create another branch:

```bash
git add plan.txt
git commit -m "Add activity choice"
git switch -c museum-plan
```

On `museum-plan`, change that line to `- Activity: visit the museum`, save the file, and commit:

```bash
git add plan.txt
git commit -m "Choose the museum"
git switch main
```

Back on `main`, change the same line to `- Activity: visit the beach`, save the file, and commit. Then try the merge:

```bash
git add plan.txt
git commit -m "Choose the beach"
git merge museum-plan
git status
```

Git reports a conflict in `plan.txt`. Open it in your text editor and find:

```text
<<<<<<< HEAD
- Activity: visit the beach
=======
- Activity: visit the museum
>>>>>>> museum-plan
```

The text above `=======` is from your current branch (`HEAD`, here `main`). The text below it is from the branch you are merging (`museum-plan`).

Choose either version or combine them. Replace the whole marked block, including all three marker lines, with the text you want, for example:

```text
- Activity: visit the beach, then the museum
```

Save the file, then mark it resolved and finish the merge:

```bash
git add plan.txt
git diff --cached
git commit -m "Merge museum plan and settle activity choice"
git status
```

`git add` tells Git you have resolved this file; `git diff --cached` lets you review the staged result. If Git lists other conflicted files, resolve and stage each one before committing. After the commit, `git status` should report a clean working tree. Open `plan.txt` to check the final plan.

To cancel instead, run `git merge --abort` **before** completing the merge commit. This returns you to the state before the merge attempt when you started with a clean working tree. Your branch commits are still saved, so you can try again later.

## Share a project

Git can track a project on your computer without an internet connection. To share it, connect your local repository to another repository, such as one hosted on GitHub. Git calls that connection a **remote**.

### Connect a remote named origin

**`origin`** is the conventional nickname for your project's main remote. It is not a branch or a special GitHub command: it is a local name for a repository address. `main` names a branch; `origin` names a remote.

Create an empty repository named `weekend-plan` on GitHub. Do not initialize it with a README, license, or `.gitignore`, because your local project already has its own history. Replace `YOUR-USERNAME` below with your GitHub username:

```bash
git remote add origin https://github.com/YOUR-USERNAME/weekend-plan.git
git remote -v
```

`git remote add` saves the connection; it does not upload files. `git remote -v` shows the addresses used to receive changes (`fetch`) and send them (`push`).

If `origin` already exists, inspect it with `git remote -v` instead of adding it again. If its address is wrong, correct it with `git remote set-url origin https://github.com/YOUR-USERNAME/weekend-plan.git`. Projects downloaded with `git clone` normally already have an `origin` remote.

### Push your saved work

**`git push`** sends your local commits to a remote repository. Publish `main` for the first time with:

```bash
git status
git switch main
git push -u origin main
```

Start with a clean working tree. This sends your local `main` branch to `origin`. The `-u` option remembers the matching remote branch as its **upstream**, so later pushes from `main` can simply use `git push`. Open your repository on GitHub to check that the plan is there.

Pushing sends commits, not unsaved edits or changes you have only staged. You need permission to write to the GitHub repository and may need to sign in using your GitHub tools. Never put a password or access token in the remote URL.

### Pull changes from others

**`git pull`** downloads changes from the remote and integrates them into your current branch. It does not just download files. After the first push above, update your local `main` with:

```bash
git status
git switch main
git pull --ff-only
```

Commit any unfinished work before pulling. The upstream set earlier tells Git to use `origin` and its `main` branch. `--ff-only` allows the update only when Git can move your branch forward without combining separate lines of work.

If both you and someone else have made new commits, `git pull --ff-only` stops without merging them. It has still downloaded the remote history. To combine those changes, run:

```bash
git merge origin/main
```

`origin/main` is your local record of `main` on `origin`, updated when Git downloads changes. If this merge reports conflicts, follow the conflict-resolution steps above, then push once the merge is complete. If an editor opens for a merge message, save and close it to finish.

A typical routine on `main`, once the remote and upstream are set, is:

1. Run `git pull --ff-only` to get the latest work before editing.
2. Edit your files, review with `git diff`, then stage and commit your changes.
3. Run `git push` to share your commits.

If a push is rejected because the remote has newer commits, pull again, combine any separate histories as described above, and retry the push. Do not use `git push --force` to bypass this: it can overwrite other people's shared history.

## Exercises

Try these in your `weekend-plan` folder. Use `git status` whenever you are unsure what Git sees.

1. **Make a checkpoint:** Add a favorite snack to `plan.txt`, stage the file, and commit it with a message. Check that the new checkpoint appears in `git log --oneline`.
2. **Inspect a change:** Add one more line but do not stage it yet. Run `git diff` and identify the line you added. Then stage and commit it.
3. **Try a branch:** Create a branch called `birthday-idea`, add a birthday activity, and commit it. Switch back to `main` (or `master`) and check that the birthday activity is not in that version.
4. **Merge a branch:** Merge `birthday-idea` into your original branch. Check that the birthday activity is now in `plan.txt` and that `git status` reports a clean working tree.
5. **Resolve a conflict:** Repeat the conflict walkthrough above with a new branch called `snack-plan` and a line for your snack choice. Commit different changes to that same line on both branches, merge into your original branch, and resolve the conflict. Check the final text and working-tree status.
6. **Explain the diagram:** In your own words, describe the difference between a project folder, the staging area, and a commit.

### Check your understanding

- `git status` tells you what is new, changed, or staged.
- `git add plan.txt` selects `plan.txt` for the next commit; it does not save a commit by itself.
- `git commit` saves the staged changes in your local history.
- A branch lets you keep a separate version of work.
- `git merge rainy-day` brings changes from `rainy-day` into your current branch.
- A merge conflict pauses a merge until you choose the final content, stage each resolved file, and commit.
- `origin` is a remote nickname; `main` is a branch name.
- `git push -u origin main` publishes `main` and sets its upstream for later pushes and pulls.
- `git push` sends commits; `git pull` downloads and integrates remote changes. Neither is needed for local practice.

## A few helpful habits

- Make commits small enough that their messages can describe them clearly.
- Run `git status` before and after staging or committing.
- Git records changes to files, not every action you take. A change must be staged and committed before it is in the saved history.
- Avoid putting passwords or private information in files you plan to share.
