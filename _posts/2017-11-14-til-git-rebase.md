---
layout: post
title: "[TIL] Effective way for git rebase"
description: ""
category: 
- TodayILearn
- Kubernetes
tags: ["TIL", "git"]

---

## 心得:

這篇文章是講解到關於 git rebase 的部分，講著就提到一些 git 技巧．也提到該如何把不小心 commit 的檔案從 git 記錄內徹底刪除的方法．

## Rebase 

### Manually auto-rebase

Before start to rebase:

```
git fetch origin
```

Pull latest code and start rebase: (ex: rebase to `master`)

```
git rebase origin/master
```

Start rebase

```
git rebase --continue
```

... modify code.

```
git add your_changed_code
```

Force push because tree different: 

```
git push -f -u origin HEAD
```

- `-u`: for upstream sort term.
- `-f`: force (because you rebase your code history)

### Interactive rebasing 

Rebase with interactive 

```
git rebase -i (interactive) origin/develop
```

It will entry Select your change and make as `squash`, `pick`. 


## Check all commits.

```
git log stat
```

#### Reset 

```
git reset HEAD~ (rollback last change)
git log --stat --decorate
```


## Here is detail example how to rebase from evan/test1 to develop

```
git checkout -t origin/evan/test1
git log --stat --decorate
git fetch origin
git rebase -i origin/develop
vim .gitignore
git add -v .gitignore
git rebase --continue
git status
git submodule update
git log --stat
git push -f origin HEAD
```

### If your rebase target branch (origin/develop) has been rebased.

```
git rebase {from commit} --onto origin/develop 
```

### List previous 

List logs **before** `33322`

```
git log 33322~
```

## Clean git if you found mis-commit 

#### First step

```
#!/bin/bash
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | awk '/^blob/ {print substr($0,6)}' | sort --numeric-sort --key=2 | gcut --complement --characters=13-40 | gnumfmt --field=2 --to=iec-i --suffix=B --padding=7 --round=nearest
```

#### Second step (filter branch)

Have to do this before your filter branch.	
1. Remember to run this before you filter branch.

```
git clone --mirror
```

2. Then, just start to fitler your git branch.

```
#!/bin/bash
set -e
# git filter-branch --index-filter 'git rm --cached --ignore-unmatch filename' HEAD
git filter-branch --index-filter "git rm -r -f --cached --ignore-unmatch $*" --prune-empty --tag-name-filter cat -- --all
git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d
git reflog expire --expire=now --all
git reset --hard
git gc --aggressive --prune=now
```

## Clean related commands

### Prune remote branch which is not using anymore

```
git remote update --prune
git gc --aggressive --prune=now
```

### Cleanup merged branch

```
git branch --merged | grep -E -v 'master|rc|develop' | xargs -I{} git branch -d {}
```

