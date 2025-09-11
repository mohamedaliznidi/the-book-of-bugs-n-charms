---
description: >-
  Master the mystical arts of Git sorcery including temporal version control concepts,
  branch weaving, merge rituals, and collaborative development enchantments for
  coordinating magical code evolution across multiple sorcerers.

theme: "magic"
---

# The Git Temporal Sorcery Grimoire

## The Ancient Knowledge

Git is a distributed temporal control system that tracks the mystical evolution of any collection of scrolls and incantations, primarily used for coordinating collaborative work among code sorcerers who weave source magic together during the sacred process of software enchantment development.


## When to Cast This Spell

1. **Tracking work** — record changes and share them with your future self.
2. **Branching** — create safe sandboxes for experiments or features.
3. **Collaboration** — sync with others across time zones and dimensions.
4. **Debugging history** — trace when/why a bug was introduced.
5. **Release management** — tag versions and maintain multiple stable lines.

## Preparing the Ritual

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# show helpful colors
git config --global color.ui auto

# common aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
```

## Your First Casting

### Initializing and cloning

```bash
git init            # create a new repo in current folder
git clone URL        # clone a remote repo
```

### Basic lifecycle

```bash
git status          # check repo state
git add file.js     # stage changes
git add .           # stage everything
git commit -m "msg" # commit staged changes
```

### Reviewing work

```bash
git log             # full history
git log --oneline   # condensed history
git diff            # unstaged changes
git diff --staged   # staged changes
```

## Apprentice Incantations

### Branching & switching realities

```bash
git branch feature-x      # create branch
git checkout feature-x    # switch to branch
# or, modern shortcut
git switch -c feature-x   # create + switch
```

### Merging timelines

```bash
git checkout main
git merge feature-x
```

### Rebasing (replaying commits)

```bash
git switch feature-x
git rebase main
```

**Tip:** Rebase is great for keeping history linear before merging, but avoid
rebasing public/shared branches.

### Undoing & cleaning up

```bash
git restore file.js       # discard changes (if uncommitted)
git reset HEAD file.js    # unstage a file
git commit --amend        # edit last commit (msg or staged content)
git revert <hash>         # make a new commit that undoes another
```

### Stashing (pocket dimension)

```bash
git stash save "msg"  # put changes aside
git stash list         # see stashes
git stash apply 0      # reapply stash
```

## Collaboration Rituals

### Syncing with remote

```bash
git remote add origin URL
git push -u origin main     # push first time
git push                    # push later commits
git pull                    # fetch + merge changes
git fetch                   # just fetch updates
```

### Tracking branches

```bash
git branch -r               # list remote branches
git checkout -b feat origin/feat
```

### Resolving conflicts

* Conflicts happen when Git cannot merge automatically.
* Open conflicted files, search for `<<<<<<<`, edit, then:

```bash
git add file.js
git commit
```

## Advanced Sorcery

### Tagging releases

```bash
git tag v1.0.0
git push origin v1.0.0
```

### Interactive rebase (rewriting history)

```bash
git rebase -i HEAD~5
```

* Allows squashing, reordering, or editing last 5 commits.
* Use carefully; don’t rewrite history of shared branches.

### Bisecting (hunting a cursed bug)

```bash
git bisect start
git bisect bad            # mark current as bad
git bisect good <hash>    # mark a known good commit
# Git checks out midpoint; test, mark good/bad, repeat until culprit found
git bisect reset
```

### Cherry-picking

```bash
git cherry-pick <hash>
```

Apply a commit from another branch onto your current branch.

## Wisdom of the Ancients — Tips & Best Practices

* **Commit often, but with purpose**: small, logical commits help bisecting and
  reviewing.
* **Write clear commit messages**: subject line (<50 chars), blank line, body.
* **Don’t commit secrets**: use `.gitignore` and environment variables.
* **Keep `main` clean**: branch for experiments, merge when stable.
* **Sync frequently**: avoid giant divergence from remote.
* **Rebase before merging** if you want a clean linear history.
* **Use stashes sparingly**: they are easy to forget.
* **Tag important versions**: helps when debugging old releases.
* **Use `git reflog`**: your safety net; you can recover almost anything.

## Common Curses & Their Remedies

### Curse: lost commit after reset

**Cause:** `git reset --hard` seems to wipe it.
**Counter-spell:** `git reflog` shows all recent commits/HEAD positions; find
the hash and `git checkout` or `git cherry-pick`.

### Curse: merge conflict chaos

**Cause:** merging diverged histories.
**Counter-spell:** rebase incrementally, resolve conflicts early, use visual
tools (`git mergetool`).

### Curse: pushing large files by mistake

**Cause:** no `.gitignore` or large binaries.
**Counter-spell:** remove with `git filter-repo` (or BFG tool), add to
`.gitignore`, use Git LFS.

### Curse: messy history with noise commits

**Cause:** committing frequently without cleanup.
**Counter-spell:** squash commits with interactive rebase before merging.

## Sacred Texts & Further Reading

* Official Git docs: [https://git-scm.com/doc](https://git-scm.com/doc)
* Git book (free): [https://git-scm.com/book/en/v2](https://git-scm.com/book/en/v2)
* Atlassian Git tutorials: [https://www.atlassian.com/git/tutorials](https://www.atlassian.com/git/tutorials)
* Oh Shit, Git!?!: [https://ohshitgit.com/](https://ohshitgit.com/)

## Quick Reference (cheat-sheet)

```text
git init
git clone URL

# basic lifecycle
git status
git add <file>|.
git commit -m "msg"
git log --oneline
git diff [--staged]

# branching
git branch <name>
git switch [-c] <name>
git merge <branch>
git rebase <branch>

# undoing
git restore <file>
git reset HEAD <file>
git commit --amend
git revert <hash>
git stash [save]

# collaboration
git remote add origin URL
git push [-u origin main]
git pull
git fetch

git tag vX.Y.Z
git rebase -i HEAD~N
git bisect start

git reflog
git cherry-pick <hash>
```


