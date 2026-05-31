# Git — A Complete Course (Beginner → Expert)

> **Read this repo top to bottom and you'll go from "what is a commit?" to expert-level branching,
> history rewriting, recovery, and team workflows — confident on the command line and in code review,
> and ready for any Git interview.**

This isn't a command dump — it's an **ordered curriculum**. The files are numbered `01 → 19`. Read them
in order and each topic only relies on what came before. Every file starts with plain-English
**intuition**, shows the *painful before* (a lost commit, a tangled history, a merge conflict), then the
**mental model** with a diagram, then the **actual commands**, and ends with **exercises solved in
full**.

🎯 **In a hurry before an interview?** → [Interview Cheat Sheet](INTERVIEW_CHEATSHEET.md).
🧭 **New to Git?** → [What Is Git & Version Control](01-What-Is-Git-and-Version-Control.md).
🧠 **The "aha" file most people skip:** → [How Git Works (the Object Model)](03-How-Git-Works-Internals.md)
— understand this and everything else clicks.

## 🏷️ Conventions used everywhere

| Symbol | Means |
|--------|-------|
| `$ git ...` | a command you run in the terminal |
| `# ...` | a comment / explanation |
| `❌ / ✅` | the wrong way vs the right way |
| 🧠 / 🎯 / 📐 / 💻 | intuition / the problem / how it works / commands |

> The golden rule of this repo: **Git tracks snapshots, not files; almost everything is a commit, and a
> branch is just a movable label pointing at one.** Internalize that and Git stops being scary.

---

## 🗺️ The Curriculum

### Part 1 — Fundamentals (get productive)
| # | Topic | What you'll learn |
|---|-------|-------------------|
| **01** | [What Is Git & Version Control](01-What-Is-Git-and-Version-Control.md) | VCS, centralized vs distributed, why Git won |
| **02** | [Setup & Configuration](02-Setup-and-Configuration.md) | Install, `git config`, identity, SSH keys, aliases |
| **03** | [How Git Works (the Object Model)](03-How-Git-Works-Internals.md) | Blobs, trees, commits, SHAs, the DAG, the `.git` folder |
| **04** | [The Three Areas & File Lifecycle](04-The-Three-Areas-and-Lifecycle.md) | Working tree, staging/index, repository; HEAD; file states |
| **05** | [The Basic Workflow](05-The-Basic-Workflow.md) | `init`/`clone`, `status`, `add`, `commit`, `.gitignore` |
| **06** | [Branching & Merging](06-Branching-and-Merging.md) | Branches as pointers, fast-forward vs 3-way merge, conflicts |

### Part 2 — Core skills (work like a pro)
| # | Topic | What you'll learn |
|---|-------|-------------------|
| **07** | [Rebase vs Merge](07-Rebase-vs-Merge.md) | Rebasing, the golden rule, when to use each |
| **08** | [Remotes & Collaboration](08-Remotes-and-Collaboration.md) | `fetch`/`pull`/`push`, tracking branches, `origin` |
| **09** | [Undoing Things](09-Undoing-Things.md) | `restore`, `reset` (soft/mixed/hard), `revert`, the **reflog** |
| **10** | [Stashing & Cleaning](10-Stashing-and-Cleaning.md) | `stash`, `clean` — park and discard work safely |
| **11** | [Inspecting History](11-Inspecting-History.md) | `log`, `diff`, `show`, `blame`, `bisect`, `grep` |
| **12** | [Tagging & Releases](12-Tagging-and-Releases.md) | Lightweight vs annotated tags, semver, releases |

### Part 3 — Advanced & team workflows (mastery)
| # | Topic | What you'll learn |
|---|-------|-------------------|
| **13** | [Rewriting History](13-Rewriting-History.md) | `amend`, interactive rebase, squash/fixup, `cherry-pick`, `filter-repo` |
| **14** | [Workflows & Branching Strategies](14-Workflows-and-Branching-Strategies.md) | GitHub Flow, Git Flow, trunk-based development |
| **15** | [Pull Requests & Code Review](15-Pull-Requests-and-Code-Review.md) | Forks, PRs, review etiquette, merge options |
| **16** | [Hooks & Automation](16-Hooks-and-Automation.md) | Client/server hooks, pre-commit, CI integration |
| **17** | [Advanced Tools](17-Advanced-Tools.md) | Worktrees, submodules, LFS, sparse-checkout, `bisect run` |
| **18** | [Troubleshooting & Recovery](18-Troubleshooting-and-Recovery.md) | Recover lost commits, detached HEAD, fix common disasters |
| **19** | [Best Practices](19-Best-Practices.md) | Commit messages, atomic commits, `.gitignore`, never commit secrets |

Also at the repo root: the one-page [Interview Cheat Sheet](INTERVIEW_CHEATSHEET.md) (command reference +
interview Q&A + study plan).

---

## 🧭 How to Study This

1. **Don't skip [internals (03)](03-How-Git-Works-Internals.md).** Everyone who finds Git "confusing"
   skipped the object model. Once you see that a commit is a snapshot and a branch is a pointer, `reset`,
   `rebase`, and recovery become obvious instead of magic.
2. **Type every command.** Make a throwaway repo (`git init test && cd test`) and run the examples. Git is
   muscle memory — reading isn't enough.
3. **Feel the pain first.** Each file opens with a painful *before* — a lost commit, a tangled merge. The
   command only makes sense against the problem it solves.
4. **Learn the [reflog (09)](09-Undoing-Things.md) early.** It's the safety net that makes you fearless —
   almost nothing in Git is truly lost.
5. **Do the 🎯 Practice.** Each topic ends with exercises — attempt before peeking at the solution.
6. **Use the self-check questions** as your "ready to move on?" gate.

A roadmap with time estimates lives in the [Study Plan](INTERVIEW_CHEATSHEET.md#-study-plan).

---

## 📐 How Each Topic Is Structured

Every file follows the same shape — intuition → the painful *before* → how it works (diagram) →
**actual commands** → when-to-use/trade-offs → mistakes & gotchas → real-world use → **practice with full
solutions** → takeaways + self-check.

> 💡 The mantra for this whole course: **commit early, commit often; branch freely; and never fear —
> the reflog remembers.**
