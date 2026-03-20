# Git Remote - Manage set of remote repositories

Manage the set of repositories ("remotes") whose branches you track with your local repository.

## Why Use Remotes?

### Collaboration
Remotes allow multiple developers to work on the same project. Each person pushes and pulls changes to a shared remote (typically `origin`) to stay in sync.

### Forks and Upstream
When you **fork** a repository (e.g., on GitHub), you create your own copy of someone else's repo. Your fork becomes `origin`, and the original repo is typically added as `upstream`:

```bash
$ git remote -v
origin    https://github.com/you/repo.git (fetch)       # your fork
upstream  https://github.com/original-author/repo.git (fetch)  # the original
```

This lets you:
- **Pull from `upstream`** to keep your fork up to date with the original project
- **Push to `origin`** to save your changes on your fork
- **Open pull requests** from your fork back to the original repo

### Multiple Remotes
You aren't limited to one remote. Common reasons for multiple remotes include:
- **`upstream`** — tracking the original repo you forked from
- **`staging` / `production`** — deployment targets (e.g., Heroku, Azure)
- **`backup`** — a mirror on another hosting platform (e.g., GitLab)
- **Teammate forks** — fetching a colleague's branches directly

### Fetch vs Push URLs
Each remote stores separate URLs for fetching and pushing. Usually they're the same, but they can differ — for example, fetching over HTTPS (public) and pushing over SSH (authenticated).

## Options

```bash
$ git remote -v
origin  https://github.com/user/repo.git (fetch)
origin  https://github.com/user/repo.git (push)
```

## Commands

With no arguments, the command `git remote` shows a list of existing remotes.

### add

```bash
git remote add <name> <url>
```

Adds a new remote with the given name and URL.

### remove

```bash
git remote remove <name>
```

Removes the remote and all associated tracking branches.

### rename

```bash
git remote rename <old> <new>
```

Renames a remote from `<old>` to `<new>`.

### set-url

```bash
git remote set-url <name> <newurl>
```

Changes the URL for an existing remote.

### show

```bash
git remote show <name>
```

Displays detailed information about a remote, including tracked branches.
