# 🧠 Git Mastery — 50 Hands-On Exercises

A structured path to becoming a Git power user. Each directory contains a focused, real-world task with supporting materials. This is your lab. Dive in and get your hands dirty!

---

## 📁 Basics & Repositories
- [Initialize a Git repository and make your first commit](/repo/init-commit/)
- [Track and untrack files using `.gitignore`](/repo/gitignore/)
- [View detailed commit history with options like `--oneline`, `--graph`](/repo/log-options/)
- [Undo changes: reset, checkout, and restore](/repo/undo-changes/)
- [Create and clone a remote Git repository](/repo/remote-clone/)

---

## 🌿 Branching & Merging
- [Create a new branch and switch to it](/branching/new-branch/)
- [Make changes in a feature branch and merge to main](/branching/basic-merge/)
- [Handle merge conflicts and resolve them manually](/branching/merge-conflict/)
- [Fast-forward vs. no-fast-forward merges](/branching/ff-vs-noff/)
- [Delete local and remote branches](/branching/delete-branches/)
- [Rename branches locally and remotely](/branching/rename-branch/)
- [Compare branches with `diff` and `log`](/branching/compare-branches/)
- [Cherry-pick specific commits between branches](/branching/cherry-pick/)

---

## 🚦 Staging, Commits & History
- [Stage specific lines or hunks using `git add -p`](/staging/partial-add/)
- [Amend the most recent commit](/staging/amend-commit/)
- [Reorder, squash, and drop commits with interactive rebase](/staging/interactive-rebase/)
- [Tag versions with lightweight and annotated tags](/staging/tags/)
- [Find faulty commit using `git bisect`](/staging/bisect/)
- [View changes over time with `git blame`](/staging/blame/)

---

## 🧩 Remotes, Collaboration & Workflows
- [Add and remove remotes](/remote/remotes/)
- [Push and pull changes to/from remote repos](/remote/push-pull/)
- [Track upstream branches and understand HEAD](/remote/tracking-branches/)
- [Fork a repo and submit a pull request (PR)](/remote/fork-pr/)
- [Collaborate using feature branching and PRs](/remote/collab-feature-branches/)
- [Fetch vs. pull — and when to use each](/remote/fetch-vs-pull/)
- [Work with multiple remotes (e.g., `origin` and `upstream`)](/remote/multiple-remotes/)
- [Use `git pull --rebase` to keep history clean](/remote/rebase-pull/)
- [Review and resolve conflicts from PRs](/remote/conflict-review/)

---

## 🎯 Rewriting History & Danger Zone
- [Use `rebase -i` to clean up a messy commit history](/danger/rebase-cleanup/)
- [Undo a pushed commit using `revert`](/danger/revert-commit/)
- [Reset local changes with soft, mixed, and hard resets](/danger/reset-types/)
- [Force-push after a history rewrite](/danger/force-push/)
- [Rewrite author info in commits](/danger/change-author/)
- [Delete a commit from history (carefully)](/danger/delete-commit/)

---

## 🗂️ Submodules & Monorepos
- [Add and update a Git submodule](/submodules/add-update/)
- [Clone a repo with submodules](/submodules/clone-submodules/)
- [Remove submodules properly](/submodules/remove/)
- [Structure a monorepo with multiple projects](/submodules/monorepo-structure/)

---

## 🧪 Advanced & Git Internals
- [Explore `.git/` directory and how Git stores objects](/internals/git-dir/)
- [Create an alias for common commands](/internals/git-aliases/)
- [Inspect objects using `git cat-file`](/internals/cat-file/)
- [Use reflog to recover lost commits](/internals/reflog/)
- [Bundle and archive a Git repository](/internals/git-bundle/)
- [Create a bare Git repository and use it as a remote](/internals/bare-repo/)

---

## 🧰 Productivity & Tools
- [Setup Git hooks (pre-commit, pre-push)](/productivity/git-hooks/)
- [Use Git in VS Code or another GUI tool](/productivity/git-gui/)
- [Create a `.gitattributes` for cross-platform consistency](/productivity/gitattributes/)

---

## 🌐 GitHub/GitLab Actions & Automation
- [Trigger GitHub Actions on push or PR](/automation/github-actions-push/)
- [Automate testing with GitHub Actions CI](/automation/gh-actions-tests/)
- [Integrate Git with Jenkins or other CI tools](/automation/git-jenkins/)
