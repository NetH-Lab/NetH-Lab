---
title: FATE - A System for Feterated Learning
summary: 
type: project
tags: 
- 工程项目
- Feterated Learning
authors:
- Minel Huang

date: "2021-04-18T00:00:00Z"
lastmod: "2021-04-18T00:00:00Z"

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

更新时间：2021/04/18

# 1 Overview
FATE是微众银行开发的一款用于联邦学习的开源系统，本博客用于记录对FATE框架的分析以及源码阅读，希望在FATE的基础上开发新的功能。

# 2 FATE Architecture
前提：部署FATE standalone版本
部署请参考FATE Github：[https://github.com/FederatedAI/FATE](https://github.com/FederatedAI/FATE)

笔者研究开源系统的步骤是，安装 - 熟悉并使用用户态 - 阅读用户态代码 - 溯源到内核代码 - 根据内核代码分析系统架构 - 阅读内核代码并修改

在[Architecture of FATE](https://neth-lab.netlify.app/publication/21-3-12-architecture-of-fate/)中，笔者从用户态开始，逐步分析FATE系统框架，这篇博客记录了笔者的阅读与分析历程。

# 3 部署FATE Cluster
笔者的工作内容为部署/维护FATE Cluster，并将standalone版本中的代码移植到cluster上，并进行测试，故在本章节，记录FATE Cluster的部署过程。
本章节主要有两类内容：
1. docker基础：[Overview of docker](https://neth-lab.netlify.app/publication/21-4-9-overview-of-docker/)
2. FATE Cluster部署：[Deployment of FATE Cluster](https://neth-lab.netlify.app/publication/21-4-9-deployment-of-fate-cluster/)

# 4 FATE测试系统搭建