---
title: Summary of Sketching
summary: '[分布式机器学习][联邦学习][通信优化]分布式机器学习通信优化之Sketching'
authors:
- Minel Huang
date: “2022-01-19T00:00:00Z”
publishDate: "2022-01-19T00:00:00Z"

publication_types: ["0"]

tags: 
- Distributed Systems
- Distributed Machine Learning
- Federated Learning
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Jan.19 2022</i></font></h4>
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
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：Sketching overview。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>Sketched SGD</b></font></a>
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
</div>

<h2><a name="section2">2. Sketched SGD</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://arxiv.org/abs/1903.04488">Communication-efficient distributed sgd with sketching</a>. 2019. NIPS

  <h4>Intro & Related Work</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在分布式机器学习中，每个worker的计算时间和它身上的数据量正相关（data parallelism），但是通信时间于workers的数量无关。即使使用最佳的通信和计算交叉优化方法，总训练时间依旧依赖于最大的per-worker communication and computation time。所以，增加worker的数量到一定程度后，其对降低training time的作用将越来越少。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本文章中着重于降低每个worker的通信时间。对于SGD阶段，使用一种称为sketching的流算法，在保证收敛的条件下，每个worker只需发送O(log d)的gradient sketch，从而达到降低通信量的目标。

  <h4>Related Work</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在同步数据并行distributed SGD的情景下，一些研究专注于使用量化或稀疏化梯度来降低通信量。这类方法在降低通信量的同时，也增加了迭代次数。另一类稀疏化方法列入local worker只发送top-k coordinates的梯度，并对其做平均以获得一个近似的mini-batch gradient。这类方法在实践中很有效，但是目前依旧没有收敛性证明，因为近似的mini-batch gradient可能与真实的mini-batch gradient差距很大。第二个缺点是，即使使用一个很高的压缩率，其在传递parameters所需的通信量依旧是成O(W)增长的（W为worker数目）。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;另一方面，所有的梯度压缩方法都受限于biased or unbiased gradient estimates。对于biased压缩，最近的一些研究表明worker本地的累积压缩误差可以克服weight updates重的bias，只要压缩算法满足一些特性。本文中的方法将尊从这一点，并可以证明用sketches压缩梯度可以满足收敛性。

  <h4>Background - Count Sketch</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://zhuanlan.zhihu.com/p/369981005">Count-min Sketch 算法</a> <br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;该算法是一个用于寻找一个巨大vector中，元素出现次数的算法。一个intuitive的方法是，每进来一个元素x，如果则在hash(x)处增1。这样做的问题是，需要大量的存储空间，那有没有什么办法能只记录top-k个元素的计数呢？Count-min算法是其中一种，类似的算法还有Bloom filter等。下面我们简单叙述下count-min sketch的流程。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;假设我们希望只记录出现次数第一至第m的一组元素，即top-m，那我们需要准备k个hash函数，以及k*m的矩阵。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;对于任意元素x，计算`h_1(x), h_2(x), ..., h_k(x)`，在`k*m`中`(1, h_1(x)), (2, h_2(x)), ..., (k, h_k(x))`处自增1，此为插入过程。当所有的元素都插入完毕后，我们假设需要查询任意元素`y`出现了多少次，则需计算`h_1(y), h_2(y), ..., h_k(y)`，其中最大的值代表元素y的最多出现次数。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;很明显，查询y的结果不一定是正确的，count sketch实际上是一个概率模型，即查询结果正确的概率为`Pr(success) > 1 - \delta`，我们可以根据数据流大小、精度和合理的概率值计算出一个能容忍的m和k，从而达到缩小sketch的目的。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;总结来说，想要的错误范围越小，就要更大的m；想要更高的正确概率，就要更大的k，k*m即最后的矩阵大小。

  <h4>Sketched SGD</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;该算法用于Parameter Server架构，过程为：PS将所有的workers上传的sketches进行求和，而后通过summed sketch中恢复最大的（top-k）梯度元素的大小。为了提高sketching的压缩特性，我们还将进行第二轮通信，即parameter server请求前top-k的gradients的准确值，而后使用这些进行模型更新。该过程为Algorithm 1。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这样一来，每一轮worker只需传输k个values，而不是整个gradient vector。其余的d-k个梯度在理论上和实践上都是十分重要的。我们可以在local error accumulation vectors中将这些元素加起来，并加入下一轮的gradient中。该过程为Algorithm 2。<br>

  <img src="pic/2.1.png" style="margin: 0 auto;"><br>

</div>