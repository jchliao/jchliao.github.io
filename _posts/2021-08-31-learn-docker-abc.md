---
layout: post
title: docker的一些使用方法
categories: Docker
description: Docker 的一些使用方法
keywords: Docker
---

Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。它是目前最流行的 Linux 容器解决方案。

## 安装docker

[Docker CE 的官方文档](https://docs.docker.com/engine/install/)

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
sudo usermod -aG docker $USER
sudo systemctl start docker
```

换个源
[中科大教程](https://mirrors.ustc.edu.cn/help/dockerhub.html)

## image 文件

Docker 把应用程序及其依赖，打包在 image 文件里面。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器。

[Docker 的官方仓库](https://hub.docker.com/)

## helloworld

```bash
docker image pull library/hello-world
docker image ls
docker run hello-world
docker container kill [containID]
```

## 容器文件

```bash
docker container ls
# 列出本机所有容器，包括终止运行的容器
docker container ls --all
docker container kill [containerID]
docker container rm [containerID]
```

## Dockerfile 文件

如何可以生成 image 文件？Dockerfile 文件。它是一个文本文件，用来配置 image。Docker 根据该文件生成二进制的 image 文件。

```bash
git clone https://github.com/ruanyf/koa-demos.git
cd koa-demos
```

新建一个文本文件`.dockerignore`

```docker
.git
node_modules
npm-debug.log
```

上面代码表示，这三个路径要排除，不要打包进入 image 文件。
新建一个文本文件 Dockerfile

```docker
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
```

- `FROM node:8.4`：该 image 文件继承官方的 node image，冒号表示标签，这里标签是8.4，即8.4版本的 node。
- `COPY . /app`：将当前目录下的所有文件（除了.dockerignore排除的路径），都拷贝进入 image 文件的/app目录。
- `WORKDIR /app`：指定接下来的工作路径为/app。
- `RUN npm install`：在/app目录下，运行npm install命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
- `EXPOSE 3000`：将容器 3000 端口暴露出来， 允许外部连接这个端口。

`docker image build` 命令创建 image 文件。

```bash
docker image build -t koa-demo .
# or
docker image build -t koa-demo:0.0.1 .
# 查看
docker image ls
# 生成容器
docker container run -p 8000:3000 -it koa-demo /bin/bash
```

- `-p` 参数：容器的 3000 端口映射到本机的 8000 端口。
- `-it` 参数：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。
- `koa-demo:0.0.1`：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）。
- `/bin/bash`：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell。

```bash
node demos/01.js
```

在容器的命令行，按下 Ctrl + c 停止 Node 进程，然后按下 Ctrl + d （或者输入 exit）退出容器。
此外，也可以用docker container kill终止容器运行。

```docker
# 在本机的另一个终端窗口，查出容器的 ID
docker container ls
# 停止指定的容器运行
docker container kill [containerID]

# 查出容器的 ID
docker container ls --all
# 删除指定的容器文件
docker container rm [containerID]

docker container run --rm -p 8000:3000 -it koa-demo /bin/bash
```

可以使用`docker container run`命令的`--rm`参数，在容器终止运行后自动删除容器文件。

```docker
docker container run --rm -p 8000:3000 -it koa-demo /bin/bash
```

```docker
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
CMD node demos/01.js
```

`CMD node demos/01.js`，它表示容器启动后自动执行`node demos/01.js`。
`RUN`命令与`CMD`命令的区别在哪里？`RUN`命令在 image 文件的构建阶段执行，执行结果都会打包进入 image 文件；`CMD`命令则是在容器启动后执行。另外，一个 Dockerfile 可以包含多个`RUN`命令，但是只能有一个`CMD`命令。

```bash
docker container run --rm -p 8000:3000 -it koa-demo:0.0.1
```

[Docker 入门教程](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

[Docker 微服务教程](https://www.ruanyifeng.com/blog/2018/02/docker-wordpress-tutorial.html)
