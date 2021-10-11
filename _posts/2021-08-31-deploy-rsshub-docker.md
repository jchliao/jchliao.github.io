---
layout: post
title: 在服务器上部署rsshub
categories: web
description: 在GitHub上部署jekyll
keywords: rsshub, rss, docker
---

## rsshub docker 部署

```bash
docker pull diygod/rsshub
docker run -d --name rsshub -p 1200:1200 diygod/rsshub
docker run -d --name rsshub -p 1200:1200  -e CACHE_EXPIRE=300 -e DEBUG_INFO=false diygod/rsshub
docker run -d --name rsshub -p 1200:1200 diygod/rsshub --restart==always
docker update --restart==always rsshub
docker stop rsshub
```
