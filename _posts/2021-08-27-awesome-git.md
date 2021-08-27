---
layout: post
title: git 使用命令
categories: git
description: 常用的 git 使用命令
keywords: git, git
---

# git 与 github

## [Git 的设置](https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/)
```bash
git config --global user.name "[name]"
git config --global user.email "[email address]"
git config --global core.editor "code --wait"
git config --global core.autocrlf input
```
## 创建仓库
```bash
git init
git remote add origin  <REMOTE_URL> 
git remote set-url origin  <REMOTE_URL> 
git remote -v
git remote rm origin
git remote add upstream  <THEIR_REMOTE_URL> 
```

## 推送提交到远程仓库
```bash
git push  <REMOTENAME> <BRANCHNAME> 
git push origin main
git push  <REMOTENAME> <LOCALBRANCHNAME>:<REMOTEBRANCHNAME> 
git push  <REMOTENAME> :<BRANCHNAME> 
```
## 从远程仓库获取更改
与远程仓库交互时，这些命令非常有用。 `clone` 和 `fetch` 用于从仓库的远程 URL 将远程代码下载到您的本地计算机，merge 用于将其他人的工作与您的工作合并在一起，而 `pull` 是 `fetch` 和 `merge` 的组合。

```bash
git fetch remotename
git fetch origin
git merge remotename/branchname
git merge origin YOUR_BRANCH_NAME
git pull origin YOUR_BRANCH_NAME
git pull remotename branchname
```




## 如何使用 SSH

```bash
ls -al ~/.ssh
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
open ~/.ssh/config
touch ~/.ssh/config
```

```
#~/.ssh/config
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```
```bash
ssh-add -K ~/.ssh/id_ed25519
```