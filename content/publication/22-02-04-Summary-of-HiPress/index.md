---
title: Summary of HiPress
summary: '[分布式机器学习][通信优化]HiPress - Compress-awareness分布式机器学习framework学习笔记'
authors:
- Minel Huang
date: “2022-02-04T00:00:00Z”
publishDate: "2022-02-04T00:00:00Z"

publication_types: ["0"]

tags: 
- Distributed Systems
- Distributed Machine Learning
- Distributed Machine Learning Framework
- Communication Efficiency
featured: false
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Feb.04 2022</i></font></h4>
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
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍场景和Problems。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>Background</b></font></a>：介绍背景知识和motivation。
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
  <h4>Scenario and Problems</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当前state-of-art的scaling DNN场景为，使用数据并行 + PS或Ring-AllReduce架构完成training的一次迭代，为了减少其中gradients的通信开销，通常引入gradient compression方法。其中，PS，AllReduce也代表了parameters的同步方式。文章的作者发现，gradient synchronizaiton和gradient compression之间的协调是低效的，也即communication和computation（后文简称Comm. and Comp.）之间的协同低效。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;State-of-art的梯度同步策略为BytePS，在BytePS中，gradient synchronization所占的时间高达63.6%和76.8%（训练Bert-large和Transformer model）。我们可以引入gradient compression来降低通信开销，然而在实验中表明，training时间仅提高了1.3X，38.1% lower than expected performance。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;那么为什么Comp. and Comm.之间的协调效率过低呢？这是因为压缩后的gradients并不能直接进行聚合 - 因为其和主流的optimization（例如gradient partitioning和batching）不匹配。那么将这种compression之间应用在DNN框架内后，其带来的额外computational overhead被忽视了，并且gradient synchronazation path被放大（笔者认为这句话的意思是，gradient compression带来的计算开销并没有均匀的分布式化，故体现在部分path过长，导致整个集群的等待）。因此，第一个challenge为如何在gradient sychronization过程中分摊Comm中额外的计算开销（例如encode和decode），那么我们需要首先identify合适的调度颗粒度。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;第二个challenge是缺少一个compression-awareness的系统支持。没有这样的支持，DNN开发人员的开发过程将十分复杂，这些人员必须要精通底层架构（例如通信框架和接口）和压缩算法才能将其运用。<br>

  <h4>Contributions</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在本节我们概括性的总结文章的贡献。本文提出一个可组合的梯度同步架构，成为CaSync，其包含一个copression-awareness的同步系统，由解耦合的communication，aggregation和compression原语组成。这种细颗粒度的组成成分使得我们可以在高效的pipelining between Comm. and Comp.和高效的bulky execution of smaller tasks之间保持平衡。并且，CaSync提供了可选择的compresion and partitioning mechanism来决定是否compress每一个gradient，以及如何partition large gradients（在压缩前）以最佳优化pipeline和parallel processing。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;其次，文章提出了on-GPU gradient compression toolkit，称为CompLL，其提供on-GPU梯度压缩API。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;最后，文章建立了一个compression-aware data parallel DNN training framework，称为HiPress，由CaSync和CompLL组成。HiPress是和主流的DNN系统例如MXNet，TensorFlow和PyTorch兼容的。
</div>

<h2><a name="section2">2. Background</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;由于本文章涉及内容较多，包括gradient compression，distributed DNN framework，pipeline等，故在此章节中对其分别概括叙述。同时，第一章并不能彻底讲明白为什么Comm.和Comp.协调低效，故在此章带着这个问题去讲解背景知识，充分叙述motivation。
  
</div>

