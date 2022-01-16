---
title: Summary of Gaia
summary: '[分布式机器学习][Geo-Distributed][通信优化]Gaia学习笔记'
authors:
- Minel Huang
date: “2022-01-16T00:00:00Z”
publishDate: "2022-01-16T00:00:00Z"

publication_types: ["0"]

tags: 
- Distributed Systems
- Distributed Machine Learning
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Jan.16 2022</i></font></h4>
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
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍Gaia的应用场景和Problems。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section1"><font color="blue"><b>Architecture</b></font></a>：介绍Gaia中的组件。
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
  <h4>场景和Problem</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Gaia所对应的使用场景为geo-distributed machine learning，指有多个data centers同时参加一次model training。这是因为由于成本或隐私等原因，将全世界的数据集中到一个DC中是不现实的，故数据依旧仅存放于各个local datacenter中。各数据中心通过WAN进行通信，交换updates。于是，和在LAN中训练最为不同的一点是，WAN的带宽是珍贵的，跨WAN进行通信的成本非常高，于是成为训练瓶颈。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;为证明其motivation，Gaia提供了跨DC模型训练中，communication的时延测试，如下图：
  <img src="pic/1.1.png" style="margin: 0 auto;"><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;可以看到WAN的带宽要比LAN的带宽小15X（平均），在最坏的情况下甚至小于60X。故传统的distributed framework如MapReduce、Tensorflow等并不能适用于此场景。<br>

  <h4>Solutions and Contributions</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Gaia的目标是，尽可能的缩小跨WAN的数据传输，即reduct communication size。Gaia发现，并不是所有的updates都会对parameters的变化有巨大的影响，如下图：<br>
  <img src="pic/1.2.png" style="margin: 0 auto;"><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;可以看到，甚至有60%以上的updates对parameters的影响在0.05%以下，这给了我们解决通信量大的启发，即只传那些会大幅影响parameters的updates，称为significant updates。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Gaia的主要贡献可以总结为：解耦合了于DC内部的updates同步，和DC间的updates同步，这里的同步是指相较于BSP、SSP等并行方法中的同步，在Gaia中使用的并行方法为<b>ASP (Approximate Synchronous Parallel)</b>。基于这种新的并行方法，Gaia不需要在每次迭代中都使用全部的updates，论文中给出了该并行方法同样可以使模型达到收敛的证明。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;此外，Gaia具有泛用性，其适用于大多数machine learning算法，并可以与Parameter Sever架构的系统一同使用。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;关于Parameter Server架构的分布式机器学习框架，可以参考<a href="https://neth-lab.netlify.app/publication/21-10-04-summary-of-parameter-server/">Summary of Parameter Server</a>
</div>

<h2><a name="section2">2. Architecture</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Parameter Server系统框图如下：<br>
  <img src="pic/2.1.png" style="margin: 0 auto;"><br>
  <br>
  &nbsp;&nbsp;&nbsp;&nbsp;Gaia运行在Parameter Servers中间，在本节笔者将对其组件进行分别介绍。<br>

  <h4>The significance filter</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;该组件用来评估每个update的重要性，故需要一个significance function和initial significance threshold，只有大于阈值的updates才会在DC间share。
</div>