---
layout: post
title: "git将开发分支合并到master主分支"
keywords: ["git将开发分支合并到master主分支"]
description: "git将开发分支合并到master主分支"
category: "git"
tags: ["git", "命令"]
---
{% include JB/setup %}

### 工作流程

在开发过程中，我们一般的工作流程是：

1. 先pull下master的最新代码

```ssh
git pull origin master
```

2. 然后创建一个开发分支

```ssh
git checkout -b dev_sixian
```

3. 进入开发分支进行开发

```ssh
git branch dev_sixian
```
4. 修改后 进行 `commit` 、`add`


### 合并开发分支到主分支

1. 先进入master分支

```ssh
git checkout master
```

2. 执行合并操作

```ssh
git merge dev_sixian
```
