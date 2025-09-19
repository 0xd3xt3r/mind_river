---
aliases:
  - git
tags:
  - cheatsheet
---


## Alias

```bash

git config --global alias.co checkout
git config --global alias.cob 'checkout -b'
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.ss 'status -s'

git config --global alias.last 'log -10 --pretty=format:"%C(yellow)%h | %C(green)%ad%Cred%d | %Creset%s%Cblue | [%cn]" --decorate --date=short --graph'

git config --global alias.unstage 'reset HEAD --'

a = commit --amend
allfiles = "!f() { git log --name-only --diff-filter=A --pretty=format: | sort -u; }; f"
cfg = config --list
changedfiles = "diff-tree --no-commit-id -r --name-only"
cam = commit -am
cm = commit -m
co = checkout
cob = checkout -b
discard = reset HEAD --hard
discardchunk = checkout -p 
ol = "log --all --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
others = "ls-files --others --ignored --exclude-from=.gitignore"
rmuntracked = clean -df
root = rev-parse --show-toplevel
s = status
searchfiles = "log --name-status --source --all -S" 
searchtext = "!f() { git grep \"$*\" $(git rev-list --all); }; f"
uncommit = reset --soft HEAD^
unstage = reset HEAD --
wip = "!f() { git add . && git commit -m 'Work in progress'; }; f"

```

## Workflow


Upstream syncing

