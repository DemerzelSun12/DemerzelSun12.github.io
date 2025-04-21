---
title: Docker学习记录
draft: false
math: true
date: 2021-01-14 10:53:28
cover: images/featureimages/18.jpg
summary: 学习Docker的一些记录
tags: [Docker,程序员的自我修养]
categories: [Docker]
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

字段含义：

- REPOSITORY：来源的仓库
- TAG：镜像的标记
- IMAGE ID：ID号，唯一
- CREATED：创建时间
- VIRTUAL SIZE：镜像大小

其中，镜像的ID唯一的标识了镜像，相同ID的镜像为相同。TAG信息用来标记同一个仓库的不同镜像，例如ubuntu仓库中有多个镜像，通过TAG区分发行版本，例如10.04、12.04、12.10等。例如如下命令指定使用镜像ubuntu:14.04启动一个容器。

```bash
sudo docker run -t -i ubuntu:14.04 /bin/bash
```

如果不指定具体标记，则默认使用latest标记信息。

### 创建镜像

用户可以从Docker Hub获取已有镜像并更新，或者利用本地文件系统新建一个镜像。

#### 修改已有镜像

首先下载镜像启动器

```bash
sudo docker run -t -i training/sinatra /bin/bash
root@0b2616b0e5a8:/#
```

在容器中添加json和gem两个应用。

```bash
root@0b2616b0e5a8:/# gem install json
```

结束后，使用exit退出，使用`docker commit`提交更改后的副本。

```bash
sudo docker commit -m "Added json gem" -a "Docker Newbee" 0b2616b0e5a8 ourusr/sinatra:4f166bd27a9ff0f6dc2a830403925b5360bfe0b93d476f7fc3231110e7f71b1c
```

其中，-m指定提交说明信息，与VCS相同；-a可以指定更新用户信息；之后是用来创建容器的ID；最后是指定目标镜像的仓库名和tag信息。创建成功后会返回镜像的ID信息。

#### 利用DockerFile创建镜像

首先创建一个Dockerfile，包含一些如何创建镜像的指令。

```bash
mkdir sinatra
cd sinatra
touch Dockerfile
```

DockerFile中每一条指令代表创建镜像的一层，例如：

```bash
# This is a comment
FROM ubuntu:14.04
MAINTAINER demerzelsun <demerzelsun@gmail.com> # 现在已弃用
LABEL maintainer="demerzelsun@gmail.com"
RUN apt-get -qq update
RUN apt-get -qqy install ruby ruby-dev
RUN gem install sinstra
```

Dockerfile的基本语法是

- 使用 # 作为注释
- FROM告诉Docker使用哪个镜像作为基础
- 维护者信息，现在MAINTAINER标签已被弃用，应该使用更易flexible的LABEL指令
- RUN开头的指令会在创建时运行


