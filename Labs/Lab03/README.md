# Lab 03 - Working with Git History, Stashing, and Reverting Commits

Course: CS-649 Cloud DevOps
Student: Yusra Faisal (GitHub: faisal-yusra)

Practice repository (where the Git tasks were run): https://github.com/faisal-yusra/DevOps_Lab3_Practice
Live GitHub Pages site (Task 11): https://faisal-yusra.github.io/

## Contents

| File | Description |
|---|---|
| `README.md` | This file |
| `temp.txt` | File created for Task 8 (commit, then revert) |
| `compose.yaml` | Docker Compose file used to run Gitea (Task 10) |
| `Lab3_Solution.pdf` | Report with the screenshots for Tasks 1-11 |
| `screenshots/` | All screenshots, named as in the lab handout |

## Tasks completed

1. Local and remote commit conflicts: `git pull --no-rebase` and `git pull --rebase`
2. Manual merge conflict resolution during a rebase
3. `.gitignore` and `git rm -r --cached`
4. `git stash` and `git stash pop`
5. Checking out a specific commit (detached HEAD)
6. Soft vs hard reset
7. `git commit --amend`
8. `git revert` on a pushed commit
9. Force push on a separate branch (`test-force`)
10. Gitea with Docker Compose in GitHub Codespaces
11. GitHub Pages portfolio site

## Notes

- Tasks 1-9 were done in the practice repository `DevOps_Lab3_Practice` so that rewriting history could not affect the submission repository.
- In Task 10 the forked Gitea repository no longer has a `codespace` branch and the Gitea Codespace has no Docker installed, so the same Gitea setup (`compose.yaml`) was run from a Codespace on the practice repository.
