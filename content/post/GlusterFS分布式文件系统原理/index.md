---
title: GlusterFS分布式文件系统原理
draft: false
math: true
date: 2021-03-01 21:13:28
cover: images/featureimages/22.jpg
summary: GlusterFS分布式文件系统原理简介
tags: [程序员的自我修养,分布式系统]
categories: [分布式系统]
---

## 概览

以 Docker 为代表的容器技术在云计算领域正扮演着越来越重要的角色，甚至一度被认为是虚拟化技术的替代品。企业级的容器应用常常需要将重要的数据持久化，方便在不同容器间共享。为了能够持久化数据以及共享容器间的数据，Docker 提出了 Volume 的概念。单机环境的数据卷难以满足 Docker 集群化的要求，因此需要引入分布式文件系统。目前开源的分布式文件系统有许多，例如 GFS，Ceph，HDFS，FastDFS，GlusterFS 等。GlusterFS 因其部署简单、扩展性强、高可用等特点，在分布式存储领域被广泛使用。本文主要介绍了如何利用 GlusterFS 为 Docker 集群提供可靠的分布式文件存储。

## GlusterFS 分布式文件系统简介

### GlusterFS 概述

GlusterFS (Gluster File System) 是一个开源的分布式文件系统，主要由 Z RESEARCH 公司负责开发。GlusterFS 是 Scale-Out 存储解决方案 Gluster 的核心，具有强大的横向扩展能力，通过扩展能够支持数PB存储容量和处理数千客户端。GlusterFS 借助 TCP/IP 或 InfiniBand RDMA 网络将物理分布的存储资源聚集在一起，使用单一全局命名空间来管理数据。GlusterFS 基于可堆叠的用户空间设计，可为各种不同的数据负载提供优异的性能。

GlusterFS 总体架构与组成部分如图1所示，它主要由存储服务器（Brick Server）、客户端以及 NFS/Samba 存储网关组成。不难发现，GlusterFS 架构中没有元数据服务器组件，这是其最大的设计这点，对于提升整个系统的性能、可靠性和稳定性都有着决定性的意义。

GlusterFS 支持 TCP/IP 和 InfiniBand RDMA 高速网络互联。
客户端可通过原生 GlusterFS 协议访问数据，其他没有运行 GlusterFS 客户端的终端可通过 NFS/CIFS 标准协议通过存储网关访问数据（存储网关提供弹性卷管理和访问代理功能）。
存储服务器主要提供基本的数据存储功能，客户端弥补了没有元数据服务器的问题，承担了更多的功能，包括数据卷管理、I/O 调度、文件定位、数据缓存等功能，利用 FUSE（File system in User Space）模块将 GlusterFS 挂载到本地文件系统之上，实现 POSIX 兼容的方式来访问系统数据。

