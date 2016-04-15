---
layout: post
title: "git-remote 添加多个远程分支"
keywords: ["git-remote 添加多个远程分支"]
description: "git-remote 添加多个远程分支"
category: "git-remote"
tags: ["git-remote"]
---
{% include JB/setup %}

### 使用 `git-remote` 添加多个远程分支的命令

```
git remote	-v 

git remote	set-url --add git@***.git

git remote set-url --add [--push] <name> <newurl>

git remote set-url --delete [--push] <name> <url>
```