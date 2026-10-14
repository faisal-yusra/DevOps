# Lab 02: Hands-On Git and Version Control

**Student:** Yusra Faisal
**GitHub username:** faisal-yusra
**Course:** CS-649 Cloud DevOps
**Environment:** Ubuntu Server 26.04.1 LTS (VMware Workstation), Git 2.53.0
**Practice repository:** `DevOps_Lab2_Practice` (private)
**Submission repository:** `DevOps` -> `Labs/Lab02`

All commands were run in the Ubuntu terminal whose prompt shows `faisal-yusra@ubuntu`. Evidence screenshots are in `screenshots/` and the full write-up is in `Lab2_Solution.pdf`.

## Files in this submission

| File | Purpose |
|---|---|
| `README.md` | This summary |
| `notes.txt` | File created and edited in Task 6 (and Q1) |
| `main.py` | File edited on `feature-1` in Task 9 |
| `Lab2_Solution.pdf` | Step-by-step solution with screenshots |
| `screenshots/` | Evidence screenshots (exact required filenames) |

## Task summary

| Task | What was done | Key commands | Screenshots |
|---|---|---|---|
| Getting Started | Installed Git | `sudo apt install git -y`, `git --version` | `git_installation.png` |
| 1 | Created private repo `DevOps_Lab2_Practice` | GitHub web UI | `repo_private.png` |
| 2 | SSH key, added to GitHub, cloned over SSH | `ssh-keygen -t ed25519`, `ssh -T git@github.com`, `git clone git@github.com:faisal-yusra/DevOps_Lab2_Practice.git` | `ssh_keygen.png`, `github_sshkey.png`, `ssh_clone.png` |
| 3 | Configured Git identity | `git config --global user.name/user.email`, `git config --list` | `git_identity.png`, `git_config_list.png` |
| 4 | Explored `.git` | `ls -a`, `ls -a .git` | `git_folder.png` |
| 5 | Deleted `.git`, re-initialised, first commit and push | `rm -rf .git`, `git init`, `git add`, `git commit`, `git remote add origin`, `git branch -M main`, `git push -u origin main` | `delete_git.png`, `git_init.png`, `first_commit.png`, `first_push.png` |
| 6 | File status and staging | `git status`, `git add notes.txt`, `git commit` | `status1.png`, `commit_notes.png` |
| 7 | Branch created in GitHub GUI and pulled locally | `git pull origin bugfix/user-auth-error`, `git checkout`, `git branch` | `bugfix_branch_gui.png`, `bugfix_branch_local.png` |
| 8 | Branch created and pushed from terminal | `git checkout -b feature/db-connection`, `git push origin feature/db-connection` | `feature_db_branch.png` |
| 9 | Branching and merging | `git checkout -b feature-1`, edit `main.py`, `git merge feature-1`, `git push` | `branch_create.png`, `feature_commit.png`, `merge.png`, `push_branches.png` |
| 10 | Pull request, merge and branch delete (GitHub GUI) | PR `feature/db-connection` -> `main` | `pr_creation.png`, `pr_merge.png`, `branch_delete.png` |
| 11 | develop / staging strategy | `git merge --no-ff` feature -> develop -> staging -> main | `branch_strategy.png`, `branch_merges.png`, `final_merge.png` |
| 12 | Code review workflow (second account as reviewer) | PR, reviewer, request changes, fix, approve, merge | `pr_create_details.png`, `pr_assigned_reviewer.png`, `pr_request_changes.png`, `pr_rejected.png`, `pr_updated_with_commits.png`, `pr_merge_confirm.png`, `pr_merged.png`, `pr_branch_deleted.png` |
| 13 | Branch cleanup | GitHub trash icon, `git push origin --delete feature-1`, `git fetch --prune`, `git branch -d` | `remote_branch_deleted.png`, `remote_branch_delete_cmd.png` |

## Why does `git status` fail after `rm -rf .git`?

The `.git` folder is the repository itself. It stores the commit history, branches, remote settings and configuration. After it is deleted, the folder is only an ordinary directory, so Git cannot find a repository and prints `fatal: not a git repository (or any of the parent directories): .git`. Running `git init` creates a fresh `.git` folder, but the old history and the remote link are gone, so the remote has to be added again.

## Exam practice

- **Q1:** branch `q1-branch`, commit, `git merge --no-ff` into `main`, and `git log --oneline --graph` to verify the merge.
- **Q2:** `main`, `develop`, `staging`; feature branch `feature/q2-profile` merged into `develop`, then `staging`, then `main`; `git show <branch>:profile.txt` proves each stage has the change.
- **Q3:** `collab-a` and `collab-b` edit the same line of `team.txt`; the merge conflict is captured, resolved by keeping both lines, and committed.

## Notes

- Merging directly into `main` locally is risky on a team; the pull request flow (Tasks 10 and 12) is the safer approach.
- No passwords, tokens or private keys appear in any screenshot. Only the public SSH key and fingerprint are shown.
