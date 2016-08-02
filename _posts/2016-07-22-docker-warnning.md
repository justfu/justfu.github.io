---
layout: post
title: "docker使用时应注意的一些问题"
keywords: ["docker使用时应注意的一些问题"]
description: "docker使用时应注意的一些问题"
category: "docker"
tags: ["docker]
---
{% include JB/setup %}

#### 将容器制作成新的 docker镜像

```
docker commit -a 'Author' -m 'push is message' < containerId > < new-images-name >
docker push 
```

注意： 要注意`dockefile`所编排的，不要与修改的地方产生冲突