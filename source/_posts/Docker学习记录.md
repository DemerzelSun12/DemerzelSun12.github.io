---
title: Docker学习记录
top: false
cover: false
toc: true
mathjax: true
date: 2021-01-14 10:53:28
author: Demerzel
img: /medias/featureimages/18.jpg
coverImg:
password:
summary: 学习Docker的一些记录
tags: [Docker,程序员的自我修养]
categories: Docker
---

## 前言

为了给hitreport写CI，github的CI实在用不明白了，于是乎自己学一下Docker，记录一下。

Docker是一个开源项目，最开始是dotCloud公司内部的一个业余项目，基于Go语言实现。后来项目加入了Linux基金会，使用Apache 2.0协议，代码在Github维护。

Docker的目标是实现轻量级的操作系统虚拟化解决方案。Docker的基础是Linux容器（LXC）等技术。在LXC的基础上Docker进行了进一步封装，让用户不关心容器的管理，使操作简便。操作Docker容器类似于操作一个快速轻量级的虚拟机。

Docker与传统虚拟化不同的地方在于，Docker是在操作系统层面实现虚拟化，直接复用本地主机的操作系统，而传统方式是在硬件层面实现虚拟化。

## Docker基本概念

### 镜像 Image

Docker镜像是一个只读的模板，例如，一个镜像可以包含一个完整的ubuntu操作系统环境，里面仅安装Apache或者用户需要的其他应用程序。

镜像可以用来创建Docker容器。

### 容器 Container

Docker容器用来运行应用。容器是从镜像创建的运行实例，容器可以被启动、开始、停止、删除。每个容器都是相互隔离的、保证安全的。

容器可以看做一个简易的Linux环境（包括root用户权限、进程空间、用户空间和网络空间等）并且可以在其中运行相应的应用程序。

注：**镜像是只读的，容器在启动时创建一层可写层作为最上层。**


### 仓库 Repository

仓库是集中存放镜像文件的场所。有时候会将**仓库**和**仓库注册服务器**混为一谈。实际上，仓库注册服务器上往往存放多个仓库，每个仓库中又包含了多个镜像，每个镜像有不同的标签。

仓库分为公开仓库（public）和私有仓库（private），用户创建自己的镜像之后使用`push`命令上传到公有或者私有仓库，在另一台机器上使用这个镜像的时候，只需要从仓库上`pull`即可。

注：**Docker仓库的概念类似git，注册服务器可以理解为Github托管服务。**


## 常用命令

### 获取镜像

例：

```bash
docker pull ubuntu:20.04
```

相当于

```bash
sudo docker pull registry.hub.docker.com/ubuntu:20.04
```

即从registry.hub.docker.com的ubuntu仓库中下载标记为20.04的镜像。

如果不从官方仓库注册服务器下载，需要从其他仓库下载，此时需要指定完整的仓库注册的服务器地址。

```bash
sudo docker pull dl.docker.com:5050/ubuntu:20.04
```

完成后，即可使用镜像，例如创建一个容器，其中运行bash应用。

```bash
sudo docker run -t -i ubuntu:20.04 /bin/bash
demerzel@DESKTOP-70JN6UI:/#
```

### 列出本地镜像

```bash
sudo docker images
REPOSITORY      TAG     IMAGE ID    CREATED     VIRTUAL SIZE      
```
