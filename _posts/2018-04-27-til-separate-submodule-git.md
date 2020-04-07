---
layout: post
title: "[TIL][GIT] How to correctly separate subdirectories from git repo"
description: ""
category: 
- TodayILearn
- Kubernetes
tags: ["TIL", "git", "filter-branch", "subtree"]
---



# Problem 

If you have big repo, you want to separate it into several small repos. 

There are two approach you might google it, here is different

# Subtree



```bash
git subtree split -P feature_a -b "branchA"
```

 It will separate your code, but `it will include whole git history`. It will slow-down your CI/CD flow if your original repo history is very huge. 

# Filter-Branch

which rewrites the repo history picking up only those commits that actually affect the content of a specific subdirectory. (refer to refer 2)

```
git filter-branch --prune-empty --subdirectory-filter SUBDIR -- --all
```

# Reference



- [Splitting a subfolder out into a new repository](https://help.github.com/articles/splitting-a-subfolder-out-into-a-new-repository/)
- [Difference between git filter branch and git subtree?](https://stackoverflow.com/questions/38735205/difference-between-git-filter-branch-and-git-subtree)
- [Using Git subtrees for repository separation](https://makingsoftware.wordpress.com/2013/02/16/using-git-subtrees-for-repository-separation/)