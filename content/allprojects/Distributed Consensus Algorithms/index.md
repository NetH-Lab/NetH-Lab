---
title: Research on Distributed Consensus Algorithms
summary: 分布式一致性算法研究总结
type: project
tags: 
- 科研项目
- Distributed Games
- Distributed Consensus Algorithm
- Distributed Systems
- Datacenter Networks
authors:
- Minel Huang

date: "2020-11-01T00:00:00Z"
lastmod: "2020-11-01T00:00:00Z"

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

## **问题描述**

研究分布式一致性算法的初衷是，我想搭建一个无服务器，或者可以描述为不需要计算服务器的一个游戏。玩家通过计算机直接组网，将计算任务分布在各个玩家计算机上，实现多人游戏。

这个问题很庞大，所以我们可以先将问题简化。

首先，一个单机游戏是不需要服务器的资源的，那么在包含N个玩家的peer-to-peer（P2P）游戏中，完全可以做成N个单机游戏，我们只需要让N个玩家电脑运行状态一致即可。当然，对于一个多人联机游戏而言，延迟是很关键的一项指标，故本研究可以将研究目标简化为低延迟的分布式一致性算法与低延迟组网的研究。

进一步思考，在上述众多目标中，一致性是最为关键的。N台电脑组成了一个软耦合的分布式系统，我们需要先找到一个健全的分布式算法，之后再考虑如何在算法层面中降低延迟，即整个网络的一致完成时间T最小。

次要目标为组网，该组网需要适配上述的一致性算法，但我们需要考虑到，路由/组网是由运营商主导的，玩家或者游戏公司并不能干预运营商网络，所以第二个研究问题应运而生：如何修改运营商路由器，来实现P2P组网的低延迟。这个研究可以归纳为网络层次上的延迟优化。

网络层次的优化同样分为路由算法优化与协议栈优化，故我们需要在第一研究完成后，尝试在这两个方向上进行加速。

## **研究步骤**

根据一中的描述，研究步骤可归纳为如下几点：

1. 分析现有共识算法，总结其优缺点与运用在松耦合分布式系统的可能性

2. 寻找松耦合分布式系统一致性算法，并总结优缺点

3. 仿真松耦合一致性算法，并尝试改进

4. 使用SDN对运营商网络进行仿真，并将改进后的松耦合一致性算法融入进去，查看其可行性。

5. 尝试优化网络协议或路由算法。

## **现有共识算法的研究**

Raft算法：[Link](https://neth-lab.netlify.app/publication/20-11-15-summary-of-raft/)