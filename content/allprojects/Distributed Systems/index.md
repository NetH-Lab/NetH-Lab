---
title: Research on Distributed Systems
summary: 分布式系统学习与研究总结
type: project
tags: 
- 科研项目
- Distributed Machine Learning
- Distributed Systems
- Datacenter Networks
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

# 研究目的

我们通常使用的计算机都为单机系统，它很强大，可以运行复杂的程序，快速的处理计算任务，足够去完成我们日常生活中的任务，比如看视频、玩游戏等等。然而，随着互联网的不断发展，人类世界中产生的数据与日俱增，这些数据需要被处理，需要存储，同时带来了更大的计算任务。比如百度要在极短的时间内在海量的数据中查询我们搜索的关键词，并且这种搜索请求也是海量的。

单机系统不足以完成这样的任务，所以我们考虑使用多台机器去处理这种任务，单机系统也变成了分布式系统。然而，相较于单机系统，分布式系统是十分复杂的，故而衍生出了一个新的研究领域。作为一个系统研究人员，笔者希望在分布式系统领域进行一番研究，故开展此博客簇，以记录我的学习历程和研究方法。

# 研究方向

现如今，数据中心大多为分布式系统，以处理海量的数据。这种系统是紧耦合的，即机器之间的距离很近，带宽很大，大量的算法被应用于此。如今运用在紧耦合分布式系统包括Hadoop，Spark，MapReduce等，如何优化现有系统框架以提高处理速度为一类研究方向。

随着Machine Learning的运用日益广泛，数据中心如何加速Machine Learning成为一类问题，故需要研究如何优化数据中心网络或分布式系统以支持或加速ML过程。

在数据中心之外，大量的计算机（如个人电脑）之间通过Internet连接，由个人电脑组成的P2P网络是松耦合的，并且有很强的异构性，那么是否可以制作松耦合的分布式提供，以利用这种分散的计算资源呢？

综上所述，笔者将从这三个方向寻找研究机会：

1. 对现有分布式系统（Hadoop，Spark等）进行分析研究，寻找其优化点以加速数据中心网络处理速度。
2. 对分布式机器学习（如联邦学习）进行分析研究，改善现有分布式机器学习系统（如Spark，FATE等）。
3. 对软耦合分布式系统进行分析研究，探寻搭建该系统的可能性，与在该系统运行软件的可行性（如分布式游戏）。

# 研究计划

分布式系统是和工程应用紧密相关的，所以笔者将从三个方面“修炼”分布式系统，理论研究与工程系统搭建同步进行，以搭建出“属于”自己的分布式系统：

1. 分布式系统基础学习与经典论文研读。

   MIT 6.824 Distributed Systems学习笔记：[Link](https://neth-lab.netlify.app/publication/2021-1-1-mit-distributed-systems/)

   Raft论文精读：[Link](https://neth-lab.netlify.app/publication/20-11-15-summary-of-raft/)

2. 联邦学习理论研究。

3. 搭建研究Hadoop、Spark、FATE系统。