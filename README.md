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
git init
```

`git init` starts tracking this folder. It does not upload anything.

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

If Git says that `main` does not exist, your project may call its original branch `master`. Use `git branch --show-current` to see the current branch name.

## Share a project

Git can track a project on your computer without an internet connection. To share it through GitHub, create an empty repository on GitHub, then follow the setup commands GitHub gives you. Those commands connect your local project to its online copy.

Once connected, the usual sharing commands are:

- `git push` — send your new checkpoints to the online copy.
- `git pull` — bring other people's saved changes down to your computer.

You need permission to access the online repository. If you are new to Git, practice the local steps above before connecting or sharing a project.

## Exercises

Try these in your `weekend-plan` folder. Use `git status` whenever you are unsure what Git sees.

1. **Make a checkpoint:** Add a favorite snack to `plan.txt`, stage the file, and commit it with a message. Check that the new checkpoint appears in `git log --oneline`.
2. **Inspect a change:** Add one more line but do not stage it yet. Run `git diff` and identify the line you added. Then stage and commit it.
3. **Try a branch:** Create a branch called `birthday-idea`, add a birthday activity, and commit it. Switch back to `main` (or `master`) and check that the birthday activity is not in that version.
4. **Merge and resolve a conflict:** In `plan.txt`, add a line `- Activity: visit the park` and commit it on your original branch (`main` or `master`). Create a branch with `git switch -c museum-plan`, change that same line to `- Activity: visit the museum`, and commit. Switch back to your original branch, change the same line to `- Activity: visit the beach`, and commit. Now merge the other branch:

   ```bash
   git merge museum-plan
   git status
   ```

   Git reports a conflict because both branches changed the same line differently. Open `plan.txt`; Git marks the competing versions like this:

   ```text
   <<<<<<< HEAD
   - Activity: visit the beach
   =======
   - Activity: visit the museum
   >>>>>>> museum-plan
   ```

   Choose or combine the ideas in your text editor, and delete the `<<<<<<<`, `=======`, and `>>>>>>>` marker lines. For example, leave `- Activity: visit the park and museum`. Save the file, then mark the conflict resolved and finish the merge:

   ```bash
   git add plan.txt
   git commit -m "Merge museum plan"
   git status
   ```

   Check that Git reports a clean working tree. A conflict is just Git asking you to choose what the final text should say; your resolution is saved in the merge commit.
5. **Explain the diagram:** In your own words, describe the difference between a project folder, the staging area, and a commit.

### Check your understanding

- `git status` tells you what is new, changed, or staged.
- `git add plan.txt` selects `plan.txt` for the next commit; it does not save a commit by itself.
- `git commit` saves the staged changes in your local history.
- A branch lets you keep a separate version of work.
- `git push` and `git pull` share changes with an online copy; neither is needed for local practice.

## A few helpful habits

- Make commits small enough that their messages can describe them clearly.
- Run `git status` before and after staging or committing.
- Git records changes to files, not every action you take. A change must be staged and committed before it is in the saved history.
- Avoid putting passwords or private information in files you plan to share.
