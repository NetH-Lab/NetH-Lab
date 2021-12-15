---
title: Summary of Hoplite
summary: '[分布式系统][分布式机器学习][论文选读]Hoplite学习笔记'
authors:
- Minel Huang
date: “2021-12-15T00:00:00Z”
publishDate: "2021-12-15T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Distributed Systems
- Distributed Machine Learning
- AllReduce
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Dec.15 2021</i></font></h4>
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
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍Hoplite的应用场景和Problems。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>AllReduce</b></font></a>：介绍什么事allreduce技术。
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://dl.acm.org/doi/abs/10.1145/3452296.3472897">Hoplite: efficient and fault-tolerant collective communication for task-based distributed systems</a>. 2021. SIGCOMM<br>

  <h2>场景和Problems</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;该论文的应用场景为，task-based runtime distributed framework。与传统的task-based framework或data-based framework不同的是，runtime framework会在runtime中创建新的任务，并schedule某个worker去运行该任务。那么对于communication这一层而言，如何在runtime中运行collective collective communication如all-reduce是一个问题。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;传统的collective communication如OpenMPI, MPICH, Horovod, Gloo, NCCl等都需要在runtime之前先得知communication pattern，即app需要指出参与的workers会在runtime中如何通信。但在runtime framework中，对于新的任务我们是没法提前得知其会被部署在哪个worker上，也就无法得知该如何collective communication。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;所以，Hoplite允许参与者动态的运行collective communication，其计算data transfer schedule于任务或对象产生/到来时，并使用fine-grained pipelining运行low-latency data transfer scheduling。
</div>

<h2><a name="section2">2. AllReduce</a></h2>
<div class="div_learning_post_boder">
  <h4>AllReduce概述</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://zhuanlan.zhihu.com/p/79030485">腾讯机智团队分享--AllReduce算法的前世今生</a>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在分布式机器学习中，通常数据是分布在各个worker上的，即每个worker持有一部分的data partition。通常在一次迭代中，每个worker通过local computing得到gradients信息，而后需要使用所有的gradients来更新模型weights。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;那么由哪台worker收集所有的gradients而更新weights呢，一种思想如下图：<br>
  <img src="pic/2.1.jpeg" style="margin: 0 auto;"><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们可以通过Reduce + Broadcast的方法来实现，也即parameter server架构。我们现在尝试使用数学来定义上述过程的时间开销。设`a`代表两个通信节点之间的latency，`S`代表每个worker上的数据量，`B`代表带宽，`C`代表处理一个byte数据所耗费的时间，`N`代表worker总数。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;故在parameter server架构下，所需的总时间为`2*(\alpha +S/B) + N*S*C`<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在PS架构中我们发现，PS节点的带宽成为瓶颈，并且仅使用了PS节点的算力，我们希望通过一种手段能同时优化communication & computation，即让节点间的通信开销负载均衡，并在这一过程中能动用更多的算力。AllReduce实现了这一目标，下面我们以Ring AllReduce为例来阐述其如何实现的。<br>
  <img src="pic/2.2.jpeg" style="margin: 0 auto;"><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;AllReduce算法分为两步，第一步为让每一个worker都得到1/N的数据块，称为scatter-reduce，每一步通信耗时`\alpha + S/(NB)`，计算耗时`(S/N)*C`，共`N-1`步。该步对应上图左侧部分。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;第二步是使所有的worker上每一个`1/N`块都完整，称为allgather，每一步的通信耗时`\alpha + S/(NB)`，没有计算开销。该步对应上图右侧部分。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;那么，整体的耗时可以表示为，`2*(N-1)*[\alpha + S/(NB)] + (N-1)*[(S/N) * C]`。对比于Parameter Server模型，每一步一个worker仅会发送一次、接受一次`S/N`大小的数据块，于是我们可以说单步通信开销减少，并且负载均衡；在计算部分由于所有worker都参与了计算，故计算时间开销降低了`N`倍；甚至该方法节省了每台worker上的内存开销。后面笔者会描述，在Machine Learning领域，allreduce的用途和研究点。

  <h4>AllReduce in Machine Learning</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href = "https://andrew.gibiansky.com/blog/machine-learning/baidu-allreduce/">Bringing HPC Techniques to Deep Learning</a>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;假设我们使用data parallel stochastic gradient descent，其过程是，数据分布在各个worker上，每个worker计算出gradients后，与其他workers交换，最后使用所有的gradients更新本地模型。在这一情景中，数据是distributed，而每个worker持有全部的模型信息。首先明确的是，我们使用很庞大的模型，即parameters的数量庞大。这样导致每个worker产生的gradients数量也很庞大，由此导致较长的communication costs。我们尝试着，能否使用ring allreduce方式，同时达到distributed computation、降低单步communication cost和节省GPU空间三个目的。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在DNN反向传播中，每一层的weight更新都需要对该层的梯度信息进行求和，显然在分布式SGD中梯度信息是分布在各个worker上的。在这里我们可以使用allreduce来降低该部分的时间开销。
</div>