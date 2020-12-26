---
title: Research on Distributed Games
summary: Using distributed consensus algorithm to reduce the pressure on game servers.
type: project

tags: 
- Distributed Games
- Distributed Consensus Algorithm
- Distributed Systems
- Datacenter Networks
authors:
- Minel Huang

date: "2020-10-01T00:00:00Z"
lastmod: "2020-10-01T00:00:00Z"

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

## **Overview**

当我们在玩大型网络游戏时，常常会抱怨“服务器为什么这么卡”。由于同时在线人数过多，并且为了降低对玩家们的硬件设备的要求，大量的运算将在游戏厂商所维护的服务器上进行，综合导致了服务器资源不足，使得游戏延迟变高。对于小型游戏厂商来说，过高的数据中心成本使他们不得不制作单机游戏而非网络游戏，或是将游戏交给大型厂商如steam, Wegame运营。那么，有没有可能通过技术手段，将数据中心的压力分散到玩家的PC上，使小型游戏厂商同样可以制作网络游戏呢？

如今在游戏领域，运用了许多技术手段来降低数据中心的压力。例如使用分布式服务器进行运算，或者将大部分图形运算置于客户端上，但显然，数据中心的成本依然过高。所以，我想尝试着通过技术手段提高数据中心的性能，或者降低数据中心的成本。

## **研究方向**

我将这一研究目标拆解成三个研究课题：

1. 数据中心负载均衡、流量控制、分布式计算算法的研究：旨在通过提高数据中心的性能从而降低其整体成本。优化方法可以从应用层（分布式计算任务，任务规划，数据中心资源分配等）、传输层/网络层（负载均衡、流量控制等）和协议栈（改进现有的TCP/IP协议栈）等方向进行研究。
2. 一致性（共识）算法研究：假若没有中央服务器，可不可以通过P2P（peer-to-peer）的方式进行联网游戏？最简单的方式可能是所有的计算任务在网络边缘（本地），通过互联网交换自身状态，使用共识算法进行决策，从而达到每个节点状态一致的效果。现有的P2P应用包括Skype等。
3. 网络加速研究：对于分布式游戏而言，重要的一环是通过互联网交互信息，但在系统运行中，要求系统延迟是足够低的。也许我们可以寻找出适应一致性算法的最低时延的组网方式，但中间节点的路由是由运营商负责，是终端所不能掌控的。所以也许可以通过SDN等技术，对现有的运营商网络进行加速，优化路由算法与协议栈，尽可能地降低网络延迟。