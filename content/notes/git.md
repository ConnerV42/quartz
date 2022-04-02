---
title: "Useful Git Commands"
tags:
- software-engineering
disableToc: false
---

## Stash Commands
- Stash tracked files
```shell
git stash save "stash message"
```
--- 
- Stash tracked and untracked files
```bash
git stash save -u "stash message"
```
--- 
- View stashes
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
## Cleaning / Deletion
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
## ~/.gitconfig
```properties
[alias]
gr = log --graph --full-history --all --color --date=short --pretty=format:\\\"'%x1b[31m%h%x09%x1b[32m%d%x1b[0m%x20%ad %s'\\\"
st = status
co = checkout
ci = commit
br = branch --sort=committerdate
# Show the changes in a stash (usage: `git ss 0`): 
ss = "!f() { git stash show 'stash@{'$1'}'; }; f"`
# Stash Pop `git sp 1`
sp = "!f() { git stash pop 'stash@{'$1'}'; }; f"
# Stash drop `git sd 0`
sd = "!f() { git stash drop 'stash@{'$1'}'; }; f"
# Diff to stash `git dts 3`
dts = "!f() { git difftool 'stash@{$1}..HEAD'; }; f"
# Apply a stash without dropping it afterward `git sa 2`
sa = "!f() { git stash apply 'stash@{'$1'}'; }; f"
# Stash list
sl = stash list
# See what changed
wc = whatchanged -m
po = push origin
# Checkout, Pull Origin
cpo = "!f() { git fetch && git checkout $1 && git pull origin $1; }; f"
# Pull, Push
pp = "!f() { git pull origin `git cb` && git push origin `git cb`; }; f"
# See what will get pushed
gonnapush = !sh -c 'git diff --stat origin/`git cb`' -
# Pull from origin
p = "!f() { git pull origin `git cb`; }; f"
# Add All changed files as staged
aa = "!f() { git st $1 | awk '/modified/ && $2 !~ /header/ {print $2}' | xargs git add; git st; }; f"
# Delete All (any file that was deleted from filesystem but not from git will be added as staged to delete.)
da = "!f() { git st --porcelain | grep '^ D' | cut -c4- | xargs git rm -f; git st; }; f"
# Unstage All (If you accidentilly did an add all and need to reverse it.)
usa = "!f() { git restore --staged `git st | awk '$0 ~ /modified/ {print $2}'`; git st; }; f"
dt = difftool -y --ignore-space-change
mt = mergetool
# Shows the date of all tags
tagdate = "log --tags --simplify-by-decoration --pretty='format:%ci %d'"
# Branch Delete - Deletes a branch on Origin - be careful!
bD = "!f() { git push origin --delete $1 && git branch -D $1; }; f"
# Show local branches no on master
bnom = "!f() { git branch --no-merged `git rev-parse master` --sort=committerdate; }; f"
# Show local branches no on branch X `git bno develop`
bno = "!f() { git branch --no-merged `git rev-parse $1` --sort=committerdate; }; f"
# Show local branches that have merged to master (branch on master)
bom = "!f() { git branch --merged `git rev-parse master` --sort=committerdate; }; f"
# Show local branches that have merged to branch X `git bo develop`
bo = "!f() { git branch --merged `git rev-parse $1` --sort=committerdate; }; f"
```

- git cheat sheet:
- https://gitexplorer.com/