---
layout: post
title: "git线上发布代码脚本"
keywords: ["git线上发布代码脚本"]
description: "git线上发布代码脚本"
category: "git"
tags: ["git", "命令"]
---
{% include JB/setup %}


```sh
#!/bin/sh

cd /data/svn/product/
#git reset --hard
git checkout -- *
git pull
chown -R www.www /data/svn/product/
chmod -R 755 /data/svn/product/
rsync -av --exclude='.git/' --exclude='Doc/'  /data/svn/product/ /data/www/product/

find /data/www/product/Application/ -name common~runtime.php -ok rm -rf {} \;
#rm -fr /data/www/product/Application/Runtime*
#kill -USR2 `cat /data/logs/php-fpm.pid`
#service php-fpm reload
```