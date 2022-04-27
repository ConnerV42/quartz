---
title: "Useful Git Commands"
tags:
- software-engineering
disableToc: false
---

## Stash Commands
- Stash tracked files with a stash message
```shell
git stash save "stash message"
```
--- 
- Stash tracked and untracked files (`-u` is shorthand for `--include-untracked`) with a stash message
```bash
git stash save -u "stash message"
```
--- 
- View all stashes
```bash
git stash list
```
--- 
- Apply stash by id (Doesn't delete from stash)
```bash
git stash apply stash@{0}
```
--- 
- Unstash a single, specific file from a stash
```bash
git diff stash@{0}^1 stash@{0} -- <filename>
```
---
- The `-p` and `-u`  flag step-by-step walks you through each changed/new file in your repo and asks "Do you want to stash this?"
```
git stash save -p -u
```
---
## Cleaning / Deletion
- Remove a file from source control without deleting it:
```
git rm --cached mylogfile.log
```
- Remove a directory from source control without deleting it:
```
git rm --cached mylogfile.log
```
- In case you have **_not_** pushed the commit publicly yet, and want to undo it, keeping the changes in your working directory:
```
git reset HEAD~1 --soft   
```
---
- Dry (`-n`) run for git clean for recursive directory (`-d`) deletion of untracked files:
```shell
git clean -n -d
```
---
- Delete all untracked files from the repository (ignores files in .gitignore):
```shell
git clean -f
```
---
- Discard uncommitted, local changes
```shell
git reset --hard
```
---
- To delete a local branch
```shell
git branch -d <local-branch>
```
---
## Merging
- Merge main into feature 
```
git checkout main
git pull
git checkout feature
git merge main
git push
```
---
## Rebasing
- Does an interactive rebase, starting at the most recent commit, and working to the Nth commit from HEAD to produce a squashed commit. The squashed commit replaces each commit starting with HEAD, and ending at N, inclusive.
```shell
git rebase -i HEAD~N
```
---
## Misc. Commands
- Revert singular files back to their state in a previous commit hash
```shell
git checkout $COMMIT_SHA -- file1/to/restore file2/to/restore
```
- To view local and remote branches
```shell
git branch -a
```
--- 
- To view local and remote branches, and the last commit to each 
```shell
git branch -a -v
```
---

- Show staged changes
```shell
git diff --cached
```
or
```shell
git diff HEAD
```
---
- See number of commits by each author for a branch and their email
```bash
git shortlog -sne 
```
--- 
- See number of commits since a certain date:
```bash
- git shortlog -s -n --since "JAN 1 2022"
```
--- 


- git cheat sheet:
- https://gitexplorer.com/