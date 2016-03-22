---
layout: post
title: "mysql导入txt文件命令"
keywords: ["mysql导入txt文件命令"]
description: "mysql导入txt文件命令"
category: "mysql"
tags: ["mysql", "命令"]
---
{% include JB/setup %}


导入 demo

```php
load data local infile 'd:/mysql_txt/wbh_zy.txt'
into table qr_number(qr_pk_number, qr_number); 
```