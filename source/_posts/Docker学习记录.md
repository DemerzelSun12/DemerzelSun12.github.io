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

## Docker安装

本来打算在电脑上装wsl的，结果C盘空间不太够，有几个比较大的软件还挪不了地方，点名Visual Studio，那就用虚拟机吧。

查看Ubuntu环境
```bash

```
查看
```bash

```



1. 更换国内软件源

应用程序$\rightarrow$软件和更新$\rightarrow$选择下载服务器$\rightarrow$选择最佳服务器

2. 更新源

```bash
sudo apt-get update
```

3. 安装依赖

```bash
sudo apt-get install apt-transport-https ca-certificates software-properties-common curl
```

4. 添加 GPG 密钥，并添加 Docker-ce 软件源

```bash
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
$(lsb_release -cs) stable"
```
