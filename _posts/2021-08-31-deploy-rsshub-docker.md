---
layout: post
title: 在服务器上部署rsshub
categories: web
description: 在GitHub上部署jekyll
keywords: rsshub, rss, docker
---

# 在服务器上部署rsshub

## 安装docker

```bash
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

## Docker 部署
```bash
docker pull diygod/rsshub
docker run -d --name rsshub -p 1200:1200 diygod/rsshub
docker run -d --name rsshub -p 1200:1200  -e CACHE_EXPIRE=3600 diygod/rsshub
docker run -d --name rsshub -p 1200:1200 diygod/rsshub --restart==always
docker stop rsshub
```