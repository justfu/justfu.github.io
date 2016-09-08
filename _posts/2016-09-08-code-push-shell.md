---
layout: post
title: "代码发布脚本规范"
keywords: ["代码发布脚本规范"]
description: "代码发布脚本规范"
category: "shell"
tags: ["shell"]
---
{% include JB/setup %}

### 背景：
长时间不去管的项目时发现要找到，代码网站相关资料很费时间

### 建议
在写代码发布脚本时写上注释

如下

git 代码发布脚本

```
#!/bin/sh

# 代码发布详情
# =============================================
# 项目名称：欢乐剧场 游戏 开发分支
# 线上网址：http://vip.test.xinyuemin.com
# 更新日期：2016-09-08
# 发布者author: dengsixian
# 代码负责人： 技术部
# 代码服务器： ssh65key
# nginx： /etc/nginx/conf.d/dengsixian.conf
# 代码路径 ： git@git.oschina.net

cd /root/git_code/test/weikanjia
#git reset --hard
git checkout -- *
git pull origin dev
```


svn 代码发布脚本

````
#!/bin/sh

# 代码发布详情
# =============================================
# 项目名称：***
# 线上网址：http://baidu.com/
# 更新日期：2016-09-08
# 发布者author: ***
# 代码负责人： ****
# 代码服务器： server158
# nginx： /etc/nginx/conf.d/dengsixian.conf
# 代码路径 ： git@git.oschina.net

cd /root/svn_code/front_end/jichangactivity
svn up
. /root/.bash_alias
/usr/bin/rsync  -vzrtopg --progress --delete   '/root/svn_code/front_end/jichangactivity'  lane@rsync_server158:/data_sixian/www_svn/front_end/jichangactivity  --exclude '.svn'
```