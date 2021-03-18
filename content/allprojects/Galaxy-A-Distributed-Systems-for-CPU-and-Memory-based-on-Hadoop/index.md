---
title: Galaxy - Research on Distributed Systems
summary: Galaxy分布式系统开发记录
type: project
tags: 
- 科研项目
- 工程项目
- Distributed Systems
- Datacenter Networks
- Distributed Machine Learning
authors:
- Minel Huang

date: "2020-12-31T00:00:00Z"
lastmod: "2020-12-31T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by MinelHuang
  focal_point: Smart

links:
#- icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/georgecushen
url_code: ""
url_pdf: ""
#url_slides: ""
#url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
#slides: example
---

更新时间：2021/03/15

# 1 Overview
## 1.1 研究目的

我们通常使用的计算机都为单机系统，它很强大，可以运行复杂的程序，快速的处理计算任务，足够去完成我们日常生活中的任务，比如看视频、玩游戏等等。然而，随着互联网的不断发展，人类世界中产生的数据与日俱增，这些数据需要被处理，需要存储，同时带来了更大的计算任务。比如百度要在极短的时间内在海量的数据中查询我们搜索的关键词，并且这种搜索请求也是海量的。

单机系统不足以完成这样的任务，所以我们考虑使用多台机器去处理这种任务，单机系统也变成了分布式系统。然而，相较于单机系统，分布式系统是十分复杂的，故而衍生出了一个新的研究领域。作为一个系统研究人员，笔者希望在分布式系统领域进行一番研究，故开展此博客簇，以记录我的学习历程和研究方法。

## 1.2 研究方向

现如今，数据中心大多为分布式系统，以处理海量的数据。这种系统是紧耦合的，即机器之间的距离很近，带宽很大，大量的算法被应用于此。如今运用在紧耦合分布式系统包括Hadoop，Spark，MapReduce等，如何优化现有系统框架以提高处理速度为一类研究方向。

随着Machine Learning的运用日益广泛，数据中心如何加速Machine Learning成为一类问题，故需要研究如何优化数据中心网络或分布式系统以支持或加速ML过程。

在数据中心之外，大量的计算机（如个人电脑）之间通过Internet连接，由个人电脑组成的P2P网络是松耦合的，并且有很强的异构性，那么是否可以制作松耦合的分布式提供，以利用这种分散的计算资源呢？

综上所述，笔者将从这三个方向寻找研究机会：

1. 对现有分布式系统（Hadoop，Spark等）进行分析研究，寻找其优化点以加速数据中心网络处理速度。
2. 对分布式机器学习（如联邦学习）进行分析研究，改善现有分布式机器学习系统（如Spark，FATE等）。
3. 对软耦合分布式系统进行分析研究，探寻搭建该系统的可能性，与在该系统运行软件的可行性（如分布式游戏）。

## 1.3 研究计划

分布式系统是和工程应用紧密相关的，所以笔者将从三个方面“修炼”分布式系统，理论研究与工程系统搭建同步进行，以搭建出“属于”自己的分布式系统：

1. 分布式系统基础学习与经典论文研读。
2. 搭建研究Hadoop、Spark、FATE系统。
3. 搭建自己的计算集群：Galaxy

# 2 基础知识补充与经典论文选读

## 2.1 分布式系统概述
1. MIT 6.824 Distributed Systems学习笔记：[Link](https://neth-lab.netlify.app/publication/2021-1-1-mit-distributed-systems/)

## 2.2 一致性
1. Raft论文精读：[Link](https://neth-lab.netlify.app/publication/20-11-15-summary-of-raft/)

## 2.3 分布式计算

# 3 开源分布式系统研究

## 3.1 Google MapReduce
1. MapReduce论文精读：[Link](https://neth-lab.netlify.app/publication/21-1-4-summary-of-mapreduce/)

## 3.2 Apache Hadoop YARN
1. Hadoop论文精读：[Summary of Apache Hadoop YARN](https://neth-lab.netlify.app/publication/summary-of-apache-hadoop-yarn/)
2. Hadoop系统搭建：[Configuration of Hadoop YARN Environment](https://neth-lab.netlify.app/publication/21-2-23-build-hadoop-yarn-environment/)

## 3.3 Apache Spark

## 3.4 Tensorflow

## 3.5 FATE
1. 系统源码阅读：[Architecture of FATE](https://neth-lab.netlify.app/publication/21-3-12-architecture-of-fate/)


# 2 Galaxy：一种优化CPU和内存利用的异构分布式计算任务处理系统
研究动机：[谁限制了数据中心的资源效率](https://neth-lab.netlify.app/publication/21-1-6-who-limits-the-resource-efficiency-of-my-datacenter-an-analysis-of-alibaba-datacenter-traces/)
根据该论文，笔者思考是否能通过资源规划的方法，对数据中心的计算资源进行数学建模，从而对异构的计算任务给予合理的资源分配，以这样的方式提高数据中心的资源效率。

## 2.1 Galaxy系统搭建

### 2.1.1 底层Hadoop系统搭建

1. [树莓派Hadoop集群搭建](https://neth-lab.netlify.app/publication/21-3-15-build-hadoop-environment-on-cluster-of-raspberrypi/)

## 2.2 资源调度研究

### 2.2.1 针对机器学习任务的资源调度
1. 论文选读：[A-generic-communication-scheduler-for-distributed-DNN-training-acceleration](https://neth-lab.netlify.app/publication/20-12-21-a-generic-communication-scheduler-for-distributed-dnn-training-acceleration/)
2. 论文选读：[Optimus: an efficient dynamic resource scheduler for deep learning clusters](https://neth-lab.netlify.app/publication/20-12-20-summary-of-optimus/)