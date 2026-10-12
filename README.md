# Git Revert, Git Restore and Git Reset

This repository is created to understand the difference between the `git revert` and `git reset` commands in Git.

Both commands help us undo changes, but they work differently. Understanding them is important when working on personal projects or collaborating with a team.

## 1. Git Revert

The `git revert` command undoes the changes introduced by a particular commit by creating a **new commit**.

It does not delete the original commit from the commit history. This makes it a safer option when working on shared repositories.

### Syntax

```bash
git revert <commit-id>
```

### How to Find a Commit ID

We can find the commit ID on the Code or Commits page of a GitHub repository. We can also find it using these terminal commands:

```bash
git log
```

Displays detailed information about previous commits.

```bash
git log --oneline
```

Displays commit IDs and commit messages in a shorter format.

### Example

Suppose our commit history looks like this:

```text
a1b2c3d Add new feature
e4f5g6h Update homepage
i7j8k9l Initial commit
```

If we want to undo the changes introduced by `a1b2c3d`, we can run:

```bash
git revert a1b2c3d
```

Git creates a new commit that reverses the changes introduced by that commit.

**Important:** `git revert` does not simply restore the entire repository to an earlier state. It reverses the changes made by the specified commit while preserving subsequent commits, where possible.

If conflicts occur, we must resolve them before completing the revert.

## 2. Git Reset

The `git reset` command moves the current branch's `HEAD` and can change the staging area and working directory, depending on the option we use.

Unlike `git revert`, `git reset` can remove commits from the current branch's visible history.

### Syntax

```bash
git reset <commit-id>
```

By default, `git reset` uses the `--mixed` option.

There are three commonly used reset options:

- `--soft`
- `--mixed`
- `--hard`

## 3. Git Reset --soft

The `--soft` option moves `HEAD` to the specified commit but keeps the changes from the removed commits **staged**.

This means we can make additional changes or create a new commit without staging the existing changes again.

### Syntax

```bash
git reset --soft <commit-id>
```

### Example

```bash
git reset --soft HEAD~1
```

This moves `HEAD` back by one commit. The changes introduced by that commit remain staged and ready to commit again.

**Use case:** When we want to undo a commit but keep its changes staged.

## 4. Git Reset --mixed

The `--mixed` option moves `HEAD` to the specified commit and unstages the changes from the commits being removed from the current branch's history.

The changes remain in our working directory, so we can edit them before committing again.

This is the default behavior of `git reset`.

### Syntax

```bash
git reset --mixed <commit-id>
```

We can also write:

```bash
git reset <commit-id>
```

### Example

```bash
git reset --mixed HEAD~1
```

This moves `HEAD` back by one commit. The changes remain in our files, but they are no longer staged.

**Use case:** When we want to undo a commit and edit its changes before making a new commit.

## 5. Git Reset --hard

The `--hard` option moves `HEAD` to the specified commit and resets both the staging area and working directory to match that commit.

**Warning:** Changes in tracked files that are not present in the target commit can be permanently lost.

### Syntax

```bash
git reset --hard <commit-id>
```

### Example

```bash
git reset --hard HEAD~1
```

This moves `HEAD` back by one commit and discards the changes from that commit in the working directory and staging area.

**Use case:** When we intentionally want to discard changes and return to an earlier state.

Use this command carefully because recovering discarded work may be difficult.

## 6. Difference Between Git Revert and Git Reset

| Feature | `git revert` | `git reset --soft` | `git reset --mixed` | `git reset --hard` |
|---|---|---|---|---|
| Creates a new commit | Yes | No | No | No |
| Moves `HEAD` backward | No | Yes | Yes | Yes |
| Keeps changes staged | Not applicable | Yes | No | No |
| Keeps changes in working directory | Reverses specified changes | Yes | Yes | No |
| Can rewrite branch history | No | Yes | Yes | Yes |
| Safe for shared branches | Usually yes | Generally avoid on shared branches | Generally avoid on shared branches | Risky on shared branches |

## 7. Which Command Should We Use?

- **Use `git revert`** when a commit has already been pushed and you want to undo its changes without rewriting shared history.
- **Use `git reset --soft`** when you want to undo a local commit but keep all its changes staged.
- **Use `git reset --mixed`** when you want to undo a local commit while keeping its changes unstaged.
- **Use `git reset --hard`** when you intentionally want to discard changes and return to a previous commit.

## 8. Important Note About Pushed Commits

When working with other developers, avoid resetting commits that have already been pushed and shared, because rewriting history can cause problems for collaborators.

If you need to undo a change on a shared branch, `git revert` is usually the better choice.

For local commits that have not been pushed, `git reset` can be useful when you need to change your commit history.

## Conclusion

Both `git revert` and `git reset` are useful Git commands, but they serve different purposes.

- `git revert` undoes changes by creating a new commit and preserves the existing commit history.
- `git reset --soft` moves the branch pointer while keeping changes staged.
- `git reset --mixed` moves the branch pointer and keeps changes unstaged.
- `git reset --hard` moves the branch pointer and discards affected working-directory changes.

Understanding these commands helps us manage Git history, recover from mistakes, and collaborate more safely.

## 9. Git Restore

The `git restore` command is used to discard changes in files or restore files from a particular commit. It helps us recover a file's content or remove unwanted changes.

Unlike `git reset`, `git restore` does not move the current branch's `HEAD` or remove commits from commit history.

### Basic Syntax

```bash
git restore <file-name>
```

This restores the specified file in the working directory to its content in the index (staging area). Any unstaged changes to that file are discarded.

### Example

Suppose we modify `index.html` but want to discard our changes and return to the staged version.

```bash
git restore index.html
```

**Important:** This command can discard your unstaged changes to the specified file. Make sure you do not need those changes before running it.

## 10. Git Restore Options

### A. `git restore --worktree`

The `--worktree` option restores a file in the working directory. It is the default behavior when no destination option is specified.

```bash
git restore --worktree index.html
```

This replaces the working copy of `index.html` with the version in the staging area.

You can also use the shorter command:

```bash
git restore index.html
```

### B. `git restore --staged`

The `--staged` option unstages a file without deleting the changes from the working directory.

```bash
git restore --staged index.html
```

**Example:** You have staged a file using `git add`, but now you want to remove it from the staging area while keeping your edits.

This command restores the staged version to match `HEAD`, while your working-directory changes remain untouched.

### C. `git restore --source`

The `--source` option lets us choose which commit or reference to restore a file from.

```bash
git restore --source=<commit-id> index.html
```

**Example:**

```bash
git restore --source=HEAD~1 index.html
```

This restores `index.html` in the working directory to the version from one commit before `HEAD`.

It does not move the branch pointer or remove any commits.

### D. `git restore --staged --worktree`

We can use both options together to restore a file in the staging area and working directory.

```bash
git restore --staged --worktree index.html
```

This resets the staged and working copies of the file to match `HEAD`, discarding changes to that file in both places.

**Warning:** Changes to the file that are not committed can be lost.

### E. `git restore --patch` (`-p`)

The `--patch` option allows us to select individual parts of a file's changes to restore, instead of discarding all changes in the file.

```bash
git restore --patch index.html
```

Git shows sections of changes and asks which ones we want to discard.

This is useful when we want to keep some edits but undo others.

### F. `git restore --ours` and `git restore --theirs`

These options are useful when resolving merge conflicts.

```bash
git restore --ours index.html
```

Restores the file from our side of the merge.

```bash
git restore --theirs index.html
```

Restores the file from the other side of the merge.

These options apply to unmerged paths and are used to choose one side's version of a conflicted file. They are not general-purpose ways to restore any previous commit.

## 11. Difference Between Git Restore, Git Reset, and Git Revert

| Command | Main purpose |
|---|---|
| `git restore <file>` | Discards unstaged changes to a file by restoring its staged version. |
| `git restore --staged <file>` | Unstages a file while keeping its working-directory changes. |
| `git reset --soft <commit>` | Moves `HEAD` while keeping changes staged. |
| `git reset --mixed <commit>` | Moves `HEAD` and leaves changes unstaged in the working directory. |
| `git reset --hard <commit>` | Moves `HEAD` and discards affected tracked-file changes. |
| `git revert <commit>` | Creates a new commit that reverses the changes introduced by an earlier commit. |

## Conclusion

Remember these simple rules:

- **`git revert`** — undo a commit by creating another commit.
- **`git reset`** — move the branch pointer and optionally change the staging area and working directory.
- **`git restore`** — restore file content in the working directory or staging area without moving `HEAD`.

Always check your changes with `git status` before using commands that can discard work.

```bash
git status
git diff
git diff --staged
```