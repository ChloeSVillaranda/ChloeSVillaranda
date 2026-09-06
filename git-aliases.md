# Git Aliases Chloe

These are the aliases that I use in Git.

## Installation

Run the following command to configure all aliases at once:

```bash
git config --global alias.st status && \
git config --global alias.co checkout && \
git config --global alias.br branch && \
git config --global alias.ci commit && \
git config --global alias.cm 'commit -m' && \
git config --global alias.sw switch && \
git config --global alias.new 'switch -c' && \
git config --global alias.unstage 'restore --staged' && \
git config --global alias.lg 'log --oneline --graph --decorate --all' && \
git config --global alias.last '!f() { git log -"$1" --oneline --decorate; }; f' && \
git config --global alias.df diff && \
git config --global alias.stale '!f() { git fetch --prune && git branch -vv | awk '\''/gone\]/ {print $1}'\''; }; f' && \
git config --global alias.cleanup '!f() { git fetch --prune && git branch -vv | awk '\''/gone\]/ {print $1}'\'' | xargs -r git branch -d; }; f' && \
git config --global alias.cleanup-force '!f() { git fetch --prune && git branch -vv | awk '\''/gone\]/ {print $1}'\'' | xargs -r git branch -D; }; f'
```

## Aliases

| Alias | Command | Description |
| --- | --- | --- |
| `st` | `status` | Show the current working tree status |
| `co` | `checkout` | Switch branches or restore files |
| `br` | `branch` | List, create, or delete branches |
| `ci` | `commit` | Create a commit |
| `cm` | `commit -m` | Create a commit with a message |
| `sw` | `switch` | Switch branches |
| `new` | `switch -c` | Create and switch to a new branch |
| `unstage` | `restore --staged` | Remove files from the staging area |
| `lg` | `log --oneline --graph --decorate --all` | Show a compact visual commit history |
| `last` | `log -N --oneline --decorate` | Show the last N commits |
| `df` | `diff` | Show changes that have not been staged |
| `stale` | Shell function | Find local branches whose upstream branch no longer exists |
| `cleanup` | Shell function | Delete stale local branches if they have been merged |
| `cleanup-force` | Shell function | Force-delete stale local branches, even if unmerged |

## Examples

### Status

```bash
git st
```

Equivalent to:

```bash
git status
```

### Commit with a message

```bash
git cm "Add authentication"
```

Equivalent to:

```bash
git commit -m "Add authentication"
```

### Create a new branch

```bash
git new feature/login
```

Equivalent to:

```bash
git switch -c feature/login
```

### View commit history

```bash
git lg
```

### View the last N commits

The `last` alias accepts a number as an argument:

```bash
git last 1
git last 5
git last 10
git last 20
```

For example:

```bash
git last 5
```

is equivalent to:

```bash
git log -5 --oneline --decorate
```

### Unstage a file

```bash
git unstage file.txt
```

Equivalent to:

```bash
git restore --staged file.txt
```

## Cleaning Up Stale Branches

Sometimes a remote branch gets deleted from GitHub, but the corresponding local branch still exists.

For example, you might have:

```text
origin/main
origin/feature/login
```

and locally:

```text
main
feature/login
feature/old
```

If `feature/old` was deleted from GitHub, your local copy may still exist.

### Find stale branches

Run:

```bash
git stale
```

This fetches the latest information from the remote and lists local branches whose upstream branch no longer exists.

### Delete stale branches

Run:

```bash
git cleanup
```

This will:

1. Fetch from the remote.
2. Prune deleted remote-tracking branches.
3. Find local branches whose upstream is gone.
4. Delete those branches using `git branch -d`.

Because `-d` is used, Git will **not delete branches containing unmerged changes**.

### Force-delete stale branches

If you are certain that you do not need the branches:

```bash
git cleanup-force
```

This uses:

```bash
git branch -D
```

instead of:

```bash
git branch -d
```

`-D` does not check whether the branch has been merged, so use it carefully.

## View Configured Aliases

To see all of your global Git aliases:

```bash
git config --global --get-regexp '^alias\.'
```

## Notes

The `last`, `stale`, `cleanup`, and `cleanup-force` aliases use Git's shell alias syntax (`!`), which allows them to execute shell functions and accept arguments.

These aliases are intended for Bash/Zsh-style shells.
