---
layout: post
title: "CentOs7.2搭建PHP环境"
keywords: ["CentOs7.2搭建PHP环境"]
description: "CentOs7.2搭建PHP环境"
category: "CentOs7.2搭建PHP环境"
tags: ["php环境"]
---
{% include JB/setup %}

> 第一步 添加`yum`的源 在 `/etc/yum.repos.d`

```
-r--r--r--  1 root root 1887 7月  14 11:19 CentOS-Base.repo
-r--r--r--  1 root root  825 7月  14 11:19 epel.repo
```

> 第二步 添加必需类库
比如我系统中缺少`libxml2` 和 `gcc`的类库

```
configure: error: xml2-config not found. Please check your libxml2 installation.
# yum install gcc 
# yum install libxml2
# yum install libxml2-devel -y
```

> 第三步 然后重新编译一次

```
Thank you for using PHP. #出现这个标识语，表示编译成功
# make && make install
```

http://www.zabbix.cc/know/1420/

http://www.csdn.net/article/2012-07-05/2807142