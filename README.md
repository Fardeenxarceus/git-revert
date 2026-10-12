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