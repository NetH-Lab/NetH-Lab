---
title: Research on Distributed Systems
summary: 分布式系统研究过程纪要
type: project
tags: 
- 科研项目
- 工程项目
- Distributed Systems
- Distributed Computing
- Scheduling
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
<head>
<style>
    img{margin-left: 20px; margin-right: 20px;}
    #table th{text-align:center;}
    #table td{text-align:center;}
    p{margin-left: 15px; margin-right: 15px;}
    .div_catalogue{padding: 10px 10px; font-size: 16px; background-color: #E0E0E0; word-spacing:0px;  border:1px solid black; border-radius: 10px;}
    .div_licence{font-size: 16px; word-spacing:0px; border:1px solid black;}
    .div_learning_post{font-size: 16px; word-spacing:0px;}
    .div_indicate_source{font-size: 18px; word-spacing:0px; background-color: #E0E0E0;}
    .div_learning_post_boder{padding: 10px 10px; font-size: 16px; word-spacing:0px;  border:1px solid black;}
</style>
<!--支持网页公式显示-->    
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>
</head>

<body>

<div align="center" class="div_indicate_source">
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Aug.02 2021</i></font></h4>
  <div align="left">
  <font size="2px">
  </font>
  </div>
</div>

<div class="div_licence">
  <br>
  <div align="center">
      <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0; margin-left: 20px; margin-right: 20px;" src="https://i.creativecommons.org/l/by-nc-nd/4.0/88x31.png" /></a>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本<span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Text" rel="dct:type">作品</span>由 <span xmlns:cc="http://creativecommons.org/ns#" property="cc:attributionName"><b>MinelHuang</b></span> 采用 <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><font color="red">知识共享署名-非商业性使用-禁止演绎 4.0 国际许可协议</font></a> 进行许可，在进行使用或分享前请查看权限要求。若发现侵权行为，会采取法律手段维护作者正当合法权益，谢谢配合。
  </p>
</div>

<br>

<div class="div_catalogue">
  <div align="center">
    <h2> 目录 </h2>
    <p>
  </div>
  <div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 0. <a href="#section0"><font color="blue"><b>前言</b></font></a>：简单介绍研究目的以及方向概述，维护当前研究进度和计划
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>知识补充</b></font></a>：记录笔者的学习路线，包括理论知识与工具学习
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>工程项目</b></font></a>：总结与分布式系统相关的项目开发过程
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 3. <a href="#section3"><font color="blue"><b>研究项目</b></font></a>：总结研究路线和各阶段进展
    <p>
  </div>
</div>

<h2><a name="section0">0. 前言</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本项目旨在记录笔者在<font color="red">MPhil阶段中的各类工作成果</font>，每项工作由单独的文档记录，可能是工程项目，可能是学习笔记。笔者希望通过此项目整理自己的工作成果，由此给出当前阶段的<font color="red">工作计划</font>。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;为与其他项目作区分，本项目仅总结归纳<b>分布式系统（Distributed System）</b>方向的工作，下面对该方向作简要介绍。

  <h3>0.1 研究目的 </h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们通常使用的计算机都为单机系统，它很强大，可以运行复杂的程序，快速的处理计算任务，足够去完成我们日常生活中的任务，比如看视频、玩游戏等等。然而，随着互联网的不断发展，人类世界中产生的数据与日俱增，这些数据需要被处理，需要存储，同时带来了更大的计算任务。比如百度要在极短的时间内在海量的数据中查询我们搜索的关键词，并且这种搜索请求也是海量的。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;单机系统不足以完成这样的任务，所以我们考虑使用多台机器去处理这种任务，单机系统也变成了分布式系统。然而，相较于单机系统，分布式系统是十分复杂的，故而衍生出了一个新的研究领域。作为一个系统研究人员，笔者希望在分布式系统领域进行一番研究，故开展此博客簇，以记录我的学习历程和研究方法。

  <h3>0.2 研究方向 </h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;关于如何寻找研究方向，请参考此文档<a href="https://neth-lab.netlify.app/publication/21-06-15-how_to_find_papers_and_find_research_topic_in_cs/">How to find papers and research topics in CS</a><br>
  当前的研究场景为：<font color="red">Distributed System for Privacy Computing</font>，故目前的项目除分布式计算系统外，还可能涉及部分隐私计算系统。关于隐私计算请参考另一个Project：<a href="https://neth-lab.netlify.app/allprojects/research-on-privacy-computing/">Research on Privacy Computing</a>

  <h3>0.3 研究计划</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;已完成的工作：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. 分布式计算系统Galaxy搭建<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;正在进行的工作：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. 经典分布式系统论文研读<br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. 近三年（2018-2021）会议论文分类<br>
  &nbsp;&nbsp;&nbsp;&nbsp;3. 分布式系统课程学习<br>
  &nbsp;&nbsp;&nbsp;&nbsp;4. 基础知识补充<br>
  
  <h3>0.4 本文档章节规划</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;文档大体将分为知识补充、工程项目、研究项目三个类别，其中知识补充重点在于总结基础知识学习笔记、经典论文学习笔记等；工程项目重点在于总结与分布式系统相关的项目过程文档与项目成果；研究项目重点在于根据研究方向记录重点research笔记。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;笔者将从此三个方向对个人知识结构进行梳理，你可以根据您的兴趣进行针对性阅读。
</div>

<h2><a name="section1">1. 知识补充</a></h2>
<div class="div_learning_post_boder">
  <h3>1.1 基础知识补充</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;1. <a href="https://neth-lab.netlify.app/publication/mit-distributed-systems/">MIT 6.824 Distributed Systems学习笔记</a> <br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. 大数据基础<font color="red">（正在进行）</font> <br>
  &nbsp;&nbsp;&nbsp;&nbsp;3. Linux系统知识学习<font color="red">（正在进行）</font>

  <h3>1.2 经典论文选读</h2>
  &nbsp;&nbsp;&nbsp;&nbsp;根据MIT 6.824课程，本章节总结了有关论文的学习笔记，同时针对行业广泛使用的系统论文做了针对性阅读。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. 一致性：<a href="https://neth-lab.netlify.app/publication/20-11-15-summary-of-raft/">Raft学习笔记</a> <br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. <a href="https://neth-lab.netlify.app/publication/21-1-4-summary-of-mapreduce/">MapReduce学习笔记</a> <br>
  &nbsp;&nbsp;&nbsp;&nbsp;3. <a href="https://neth-lab.netlify.app/publication/21-3-21-yarn-in-practice/">Hadoop YARN学习笔记</a> <br>
  &nbsp;&nbsp;&nbsp;&nbsp;4. <a href="https://neth-lab.netlify.app/publication/21-3-19-summary-of-apache-spark/">Spark学习笔记</a> <br>

  <h3>1.3 工具学习</h2>
  &nbsp;&nbsp;&nbsp;&nbsp;1. Docker技术学习笔记<font color="red">（未完成）</font> <br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. k8s技术学习笔记<font color="red">（未完成）</font> <br>
</div>

<h2><a name="section2">2. 工程项目</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;此节仅是对项目的整理归纳，创建索引表，具体项目内容将在github上或neth-lab其他project中介绍。<br>

  <h3>2.1 Galaxy系统开发 - 已完成</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Github地址：<a href="https://github.com/Huangxy-Minel/galaxy">https://github.com/Huangxy-Minel/galaxy</a><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;项目简介：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;该系统为笔者本科毕业设计，使用了三块树莓派模拟一个工作集群，并基于Hadoop YARN实现了一系列计算过程。在YARN的基础上，笔者引入了一种高扩展的scheduling框架，称为Galaxy，使其可以承载异构的工作负载。具体项目内容请见github文档<font color="red">（Github文档未完成）</font>。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;项目研究动机：<a href="https://neth-lab.netlify.app/publication/21-1-6-who-limits-the-resource-efficiency-of-my-datacenter-an-analysis-of-alibaba-datacenter-traces/">谁限制了数据中心的资源效率</a><br>
  &nbsp;&nbsp;&nbsp;&nbsp;根据该论文，笔者思考是否能通过资源规划的方法，对数据中心的计算资源进行数学建模，从而对异构的计算任务给予合理的资源分配，以这样的方式提高数据中心的资源效率。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;此过程产出两篇较为重要的过程文档，分别为：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. <a href="https://neth-lab.netlify.app/publication/21-3-15-build-hadoop-environment-on-cluster-of-raspberrypi/">树莓派Hadoop集群搭建</a><br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. <a href="https://neth-lab.netlify.app/publication/21-3-21-yarn-in-practice/">YARN in Practice</a><br>
  &nbsp;&nbsp;&nbsp;&nbsp;您可以参考此两篇文档，尝试从零搭建一个Hadoop YARN集群，并完成MapReduce任务。若您没有Hadoop相关的基础知识，您可以参考<font color="red">Section 1.2</font>中的MapReduce和Hadoop YARN学习笔记。

  <h3>2.2 分布式FATE异构计算系统开发<font color="red">（正在进行）</font></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;FATE为WeBank开发的一款面向联邦学习的计算系统，笔者当前的工作为引入GPU加速并将FATE部署在Spark环境中，以实现分布式运算。在此章节中，将详细描述如何将FATE移植到Spark环境中，并引入GPU异构加速的。关于FATE本体开发，请见<font color="red">Privacy Computing</font>项目中的内容。
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;过程文档记录：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. Spark集群搭建

</div>

<h2><a name="section3">3. 研究项目</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;该章节组织结构为，Section 3.1将描述总结当今研究热点和寻找idea工作相关的过程文档，Section 3.2将总结某一idea研究中的过程文档，Section 3.3中将总结论文撰写工作相关文档。

  <h3>3.1 近期研究热点与Idea</h3>
  &nbsp;&nbsp;&nbsp;&nbsp;本节的分类将以Scenario+Problem的方式对CCF A类部分会议论文进行分类，以总结研究热点。
  <h4> 3.1.1 Distributed Machine Learning</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>A.Scheduling</b><br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. 论文选读：<a href="https://neth-lab.netlify.app/publication/20-12-21-a-generic-communication-scheduler-for-distributed-dnn-training-acceleration/">A-generic-communication-scheduler-for-distributed-DNN-training-acceleration</a><br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. 论文选读：<a href="https://neth-lab.netlify.app/publication/20-12-20-summary-of-optimus/">Optimus: an efficient dynamic resource scheduler for deep learning clusters</a>
</div>

