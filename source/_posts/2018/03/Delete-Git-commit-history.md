---
layout: post
title: 刪除過往 Github Repo 的提交紀錄
date: 2018-03-12 19:23:22
categories: 
tags: [Git, Github]
mathjax: false
---

# 前言

[Github](https://github.com/) 是著名的代碼託管網站，配合版本控制軟體 Git 可以很好地記錄提交紀錄，但有時因為沒有經驗或是純粹開始作為玩玩而已，會有許多提交紀錄有點「醜」…

<!--more-->

# 解決方法

透過以下 Git Command 可以將過往的提交紀錄刪除，並保持當前的代碼狀態：

```bash
git checkout --orphan [New Branch]  # checkout
git add -A                          # add all files and commit them
git commit -am "Clear History"
git branch -D master                # deletes the master branch
git branch -m master                # rename the current branch to master
git push -f origin master           # force push master branch to github
git gc --aggressive --prune=all     # remove the old files
```

## `git checkout --orphan`

命令 `git checkout` 經常用以創建分支或切換到既有分支：

```bash
git branch [name]                   # create a new branch
git checkout [name]                 # swich to the branch

# the commands above can be written as below
git checkout -b [name]              # create a new branch and switch to
```

有時在某分支上累積了無數次提交，但又懶得花時間整理，可以使用 `--orphan` 參數基於當前所在的分支創建一個沒有任何提交紀錄的分支：

```bash
git checkout --orphan [Name]
```

# 參考資料

- [stackoverflow | How to delete all commit history in github?](https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github)
- [stackoverflow | Make the current commit the only (initial) commit in a Git repository.](https://stackoverflow.com/questions/9683279/make-the-current-commit-the-only-initial-commit-in-a-git-repository)
- [Gist | Repo Reset](https://gist.github.com/heiswayi/350e2afda8cece810c0f6116dadbe651)