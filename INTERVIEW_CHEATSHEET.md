# Git Interview Cheat Sheet & Command Reference

> One-page, high-yield reference: the commands you'll actually use, the mental models, the classic interview
> questions, and a study plan. Each section links to its full chapter.

---

## 🎯 The mental model (know this cold)

- Git stores **snapshots, not diffs**. A **commit** = a full snapshot (a *tree* of *blobs*) + parent(s) +
  message, named by the **SHA hash of its content** (integrity). Diffs are *computed*, not stored.
- A **branch is just a movable pointer** to a commit → branching is instant. **HEAD** = "where you are"
  (usually a branch).
- **Three areas:** working tree → (`git add`) → staging/index → (`git commit`) → repository.
- **Almost nothing is ever lost** — the **reflog** remembers every position HEAD held (~90 days).
- **Golden Rule:** never rewrite (rebase/reset/amend) **shared/pushed** history — use **`revert`** instead.

[Internals →](03-How-Git-Works-Internals.md) · [Three areas →](04-The-Three-Areas-and-Lifecycle.md)

---

## ⚡ The everyday commands

```bash
# SETUP (once)
git config --global user.name "Name"; git config --global user.email "you@x.com"
git config --global init.defaultBranch main

# START
git init                         # new repo here
git clone <url>                  # copy an existing repo (full history)

# THE LOOP
git status                       # what changed (most-run command)
git add <file> | git add . | git add -p   # stage (-p = hunk by hunk, precise)
git diff                         # working vs staged    | git diff --staged = staged vs last commit
git commit -m "message"          # save a snapshot
git commit -am "msg"             # stage tracked + commit (NOT new files)
git commit --amend               # fix the LAST (unpushed) commit (ch.13)

# BRANCH & MERGE
git switch -c feature            # create + switch (old: git checkout -b)
git switch main                  # switch
git branch / git branch -d x     # list / delete merged
git merge feature                # merge feature INTO current branch
git rebase main                  # replay current branch onto main (linear; LOCAL only)

# REMOTES
git fetch                        # download, update origin/* (no file changes)
git pull                         # fetch + merge (or --rebase)
git push                         # upload current branch
git push -u origin feature       # FIRST push of a new branch
git push --force-with-lease      # after rebasing YOUR branch (never plain --force)

# UNDO (ch.09)
git restore <file>               # discard unstaged edit (destructive to the edit)
git restore --staged <file>      # unstage (keep edit)
git reset HEAD~1                 # uncommit, keep changes (mixed)
git reset --soft HEAD~1          # uncommit, keep changes STAGED
git reset --hard HEAD~1          # uncommit + DISCARD changes ⚠️ (local only)
git revert <commit>              # NEW commit undoing one — SAFE for shared history
git reflog                       # recover "lost" commits → git reset --hard HEAD@{n}

# STASH / CLEAN (ch.10)
git stash [-u] / git stash pop   # park changes / restore (-u incl. untracked)
git clean -n  then  -fd          # dry-run, then delete untracked files/dirs

# HISTORY (ch.11)
git log --oneline --graph --all  # the DAG
git log -S"code" / --grep / --author / --since   # search history
git show <commit>                # one commit's message + diff
git blame -w <file>              # who last changed each line
git bisect start; git bisect bad; git bisect good <old>   # find the breaking commit

# TAGS (ch.12)
git tag -a v1.0.0 -m "Release"   # annotated tag
git push origin v1.0.0           # tags are NOT pushed by default!

# REWRITE LOCAL HISTORY (ch.13) — never on shared!
git rebase -i HEAD~5             # squash/reword/reorder/drop commits
git cherry-pick <commit>         # copy one commit to current branch
```

---

## 🔑 Command quick-reference by intent

| I want to… | Command |
|------------|---------|
| See what changed | `git status` / `git diff` / `git diff --staged` |
| Stage part of a file | `git add -p` |
| Undo an unstaged edit | `git restore <file>` |
| Unstage (keep edit) | `git restore --staged <file>` |
| Uncommit, keep work | `git reset HEAD~1` (or `--soft`) |
| Throw away commit + work | `git reset --hard HEAD~1` (local only) |
| Undo a **pushed** commit | `git revert <commit>` |
| Recover lost commits | `git reflog` → `git reset --hard HEAD@{n}` |
| Park work temporarily | `git stash` / `git stash pop` |
| New branch | `git switch -c name` |
| Update branch w/ latest main | `git rebase main` (local) or `git merge main` |
| Combine messy commits | `git rebase -i HEAD~N` (squash) |
| Find who broke a line | `git blame -w` → `git show <commit>` |
| Find which commit broke it | `git bisect` |
| Fix "push rejected" | `git pull` then `git push` |
| Get out of detached HEAD | `git switch -c name` (keep) / `git switch main` |

---

## 🧠 Merge vs Rebase (ch.07)

```text
MERGE  → joins branches with a MERGE COMMIT. Truthful, non-destructive, but clutters history.
REBASE → REPLAYS your commits onto another tip. Clean linear history, but REWRITES commits (new hashes).
GOLDEN RULE: never rebase commits others have (shared branches). Rebase LOCAL only; merge shared.
Pattern: rebase your feature locally to tidy it → merge into main.
After a rebase: git push --force-with-lease (never plain --force).
```

## 🔄 Reset modes (ch.09)

```text
git reset --soft  HEAD~1   → move pointer; changes stay STAGED
git reset --mixed HEAD~1   → move pointer; changes UNSTAGED (default)
git reset --hard  HEAD~1   → move pointer; changes DESTROYED ⚠️ (recoverable via reflog)
```

---

## 💬 Classic interview questions (crisp answers)

1. **Git vs GitHub?** Git = the version-control tool (runs locally, offline). GitHub = a hosting website that
   uses Git and adds collaboration (PRs, issues, CI).
2. **Does Git store diffs or snapshots?** Snapshots (commit → tree → blobs). Diffs are computed on demand;
   space saved by deduplication + packfiles.
3. **What is a branch?** A movable pointer to a commit (a tiny ref file) — that's why branching is instant.
4. **merge vs rebase?** Merge = merge commit, preserves history (can clutter). Rebase = replays commits,
   linear history, **rewrites hashes** — never on shared branches.
5. **`git fetch` vs `git pull`?** fetch downloads + updates `origin/*` (no file changes); pull = fetch +
   merge/rebase into your branch.
6. **`git reset` vs `git revert`?** reset moves the branch pointer (rewrites **local** history); revert adds a
   **new** commit that undoes one — **safe for shared** history.
7. **soft vs mixed vs hard reset?** soft = keep staged; mixed (default) = keep unstaged; hard = discard
   changes.
8. **Recover a commit after `reset --hard`?** `git reflog` → find the hash → `git reset --hard HEAD@{n}` (or
   branch it). Commits aren't deleted, just unreferenced.
9. **What is detached HEAD?** HEAD points at a commit, not a branch (from `checkout <hash>/<tag>`). Commits made
   there are orphaned if you leave → `git switch -c name` to keep them.
10. **Resolve a merge conflict?** Edit the `<<<<<<< ======= >>>>>>>` markers to the desired content, `git add`,
    `git commit` (or `git merge --abort`).
11. **`git push` rejected — why & fix?** The remote has commits you don't; `git pull` (integrate) then push.
12. **Stash vs commit?** Stash parks uncommitted work for short interruptions (not shared); a WIP commit is
    better for anything long-lived/visible.
13. **Undo a pushed bad commit?** `git revert <commit>` (never reset/force-push shared history).
14. **How to find which commit introduced a bug?** `git bisect` — binary-search between a known good and bad
    commit (~log₂N tests).
15. **Lightweight vs annotated tag?** Annotated = full object (tagger/date/message, signable) → use for
    releases; lightweight = bare pointer.
16. **Cherry-pick?** Apply a specific commit's changes onto the current branch as a new commit.
17. **Squash commits?** `git rebase -i` and mark commits `squash`/`fixup` to combine them (clean history) —
    local/unshared only.
18. **`.gitignore`?** Lists files Git shouldn't track (build output, deps, secrets). Set up before first
    commit.
19. **Accidentally committed a secret?** **Rotate the secret** (it's compromised), then scrub history
    (`filter-repo`) and gitignore it. Removing from history alone doesn't un-leak it.
20. **Fast-forward vs three-way merge?** FF = target hadn't moved → just advance the pointer (linear). 3-way =
    both diverged → a merge commit with two parents.

---

## 🚫 Top gotchas

```text
• reset --hard / rebase / amend on SHARED commits → use revert instead (Golden Rule).
• git restore <file> DISCARDS the edit; use --staged to merely unstage.
• Tags aren't pushed by default → git push origin <tag> / --follow-tags.
• Plain git stash skips untracked files → use -u.
• git clean -f deletes untracked files permanently → always -n (dry run) first.
• `git commit -am` skips NEW (untracked) files.
• Leaked secret pushed → ROTATE it; scrubbing history isn't enough.
• Use --force-with-lease, never --force.
```

---

## 📅 Study Plan

A roadmap from zero to confident. Type every command in a throwaway repo (`git init test`).

| Stage | Chapters | You can… | Time* |
|-------|----------|----------|-------|
| **1. Get productive** | [01](01-What-Is-Git-and-Version-Control.md)–[06](06-Branching-and-Merging.md): basics, internals, areas, workflow, branching | init/clone, commit with good messages, branch & merge, resolve conflicts | ~1 week |
| **2. Work like a pro** | [07](07-Rebase-vs-Merge.md)–[12](12-Tagging-and-Releases.md): rebase, remotes, undo, stash, history, tags | collaborate via remotes, undo anything, use the reflog, investigate history, tag releases | ~1–2 weeks |
| **3. Mastery** | [13](13-Rewriting-History.md)–[19](19-Best-Practices.md): rewriting, workflows, PRs, hooks, advanced, recovery, best practices | clean up history, run a team workflow, review PRs, automate, recover from disasters, write clean Git by habit | ~2 weeks |

\* *Self-paced; compress or stretch freely.*

**Milestone checks:**
- **After Stage 1:** branch off main, make atomic commits, merge back, resolve a conflict — from memory.
- **After Stage 2:** explain merge vs rebase + the Golden Rule; recover a commit with the reflog; fix a rejected
  push; bisect to a breaking commit.
- **After Stage 3:** squash a messy branch with interactive rebase; explain detached HEAD and escape it; do an
  open-source fork-and-PR; list 5 best-practice habits.

**How to study (so it sticks):**
1. **Don't skip [internals (ch.03)](03-How-Git-Works-Internals.md)** — it's the "aha" that makes everything
   else obvious.
2. **Type every command** in a throwaway repo and watch what `git status`/`git log --oneline --graph` show.
3. **Break things on purpose** in a test repo (reset --hard, delete a branch) and **recover** with the
   [reflog (ch.18)](18-Troubleshooting-and-Recovery.md) — this makes you fearless.
4. **Do the 🎯 Practice** in each chapter before peeking at solutions.
5. Use the **self-check questions** as your "ready to move on?" gate.

---
[Course home →](README.md) · Start: [What Is Git & Version Control →](01-What-Is-Git-and-Version-Control.md)
