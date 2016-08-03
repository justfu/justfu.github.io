---
layout: post
title: "linux下添加用户及用户组和home目录"
keywords: ["linux下添加用户及用户组和home目录"]
description: "linux下添加用户及用户组和home目录"
category: "php-fpm"
tags: ["php-fpm"]
---
{% include JB/setup %}

```
useradd -g dengsixian -d /home/dengsixian -s /bin/bash dengsixian
```

添加环境变量

```
 PATH=/opt/:$PATH
 . /etc/profile
```