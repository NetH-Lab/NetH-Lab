---
title: Survey of Distributed Framework in Federated Learning
summary: '[机器学习][联邦学习][论文选读]联邦学习中的分布式框架调研与综述'
authors:
- Minel Huang
date: “2021-11-23T00:00:00Z”
publishDate: "2021-11-23T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Machine Learning
- Distributed Framework
- Federated Learning
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Dec.13 2021</i></font></h4>
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
  <p>
  <div align="center">
    <h2> 目录 </h2>
    <p>
  </div>
  <div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍近年federtaed learning system的场景分类。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>GAIA</b></font></a>：与distributed infrastructure层面上内存管理，其应用场景为interactive graph computing。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 3. <a href="#section3"><font color="blue"><b>Scaling FL System</b></font></a>：种为横向联邦学习设计的大规模FL架构。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 4. <a href="#section4"><font color="blue"><b>ParSync</b></font></a>：一种分布式的资源分配框架，使用partitioned synchronization方法降低了由contention on high-quality resources和staleness of local states带来的scheduling latency。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 5. <a href="#section5"><font color="blue"><b>Hoplite</b></font></a>：传统的collective communication方法是synchronous，无法适应asynchronous类的新应用，故此文提出一种distributed scheduling scheme for data transfer和fine-grained pipeline scheme，优化Ray系统的性能。
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本章将从综述的角度，介绍适用于Fedrated Learning的在分布式Framework方面的系统框架、分类与挑战。
</div>

<h2><a name="section2">2. GAIA</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://www.usenix.org/conference/nsdi21/presentation/qian-zhengping">GAIA: A System for Interactive Analysis on Distributed Graphs Using a High-Level Language</a>. NSDI 2021 <br>

  <h3>场景和Problems</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Gaia的应用场景为，在大型集群中对graph data进行迭代式分析。首先简单介绍一下什么是Graph data。<br>
  <img src="pic/2.1.png" style="margin: 0 auto;"><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Figure 1便是一个典型的graph data，其描述的是一个罪犯的现金流，例如在t1时，罪犯购买了一个网络商品，t2时银行将钱打给了3号商户。而后在t3时商户将这笔钱又打给了中间人账户，通过一系列中间人，最后再将这笔钱打回罪犯手中（t4）。Graph data analyze便是通过这样一个图数据，分析出是否有洗钱的嫌疑。通常，graph data由点和边组成，点代表着一个实体，边代表实体间的关系，在social networks，commerce transaction，online payments等领域存有大量的graph data。一份图数据可能包含billions of vertices，hundreds of billions to trillions of edges，所以一般需要在一个large cluster上进行处理。Gaia的应用场景是，对于这样一种大规模的图数据，用户会对其进行interactive query analyze，例如提交一个查询query以寻找某个顶点是否存在。故如何低时延的反馈是用户对cluster的需求。Gaia的场景可以概括为：scaling graph data，big cluster，interactive queries and low latency<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;那么先前的Graph data engines存在哪些问题呢？其一是Programming Model单一，例如Naiad，仅能scaling一些特定的图算法。作为一个distributed framework而言，其program接口没办法满足那些没有distributed computing知识的人。其二是memory management问题，过去的系统主要是基于bulk synchronous parallel (BSP)系统，即计算是迭代式的，每次迭代的运算是相同的。但是BSP系统并不适合interactive graph queries，因为interactive queries过程中通常需要维护大量的app states，这种states可能会随迭代指数倍增长，从而导致memory crisis；在interactive queries场景下，会出现多个queries共享有限内存的情况，当其为了提高效率能cache一部分input graph时，很有可能出现memory crash。

  <h3>内容概述</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;此文章中提出Gremlin，其提供了high-level的编程框架。用户提交Gremlin queries，GAIA系统将使用Scope Abstraction描述一个query中的data dependencies，这允许使用dataflow来描述Gremlin traversal，最终达到高效的parallel execution。为了防止memory crash，GAIA在runtime中优化了parallel graph traversal。GAIA的系统框架如下图所示：<br>
  <img src="pic/2.2.png" style="margin: 0 auto;"><br>
</div>

<h2><a name="section3">3. Scaling FL System</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://arxiv.org/abs/1902.01046">Towards Federated Learning at Scale: System Design</a>. MLSys 2019 <br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;此文章提出了一种应用于大规模横向联邦学习的框架，基于TensorFlow开发，同时也是现阶段横向联邦学习的主要应用系统。本章将简单描述此框架。<br>

  <h2>Protocol</h2>
  <img src="pic/3.1.png" style="margin: 0 auto;"><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;上图是该系统的成员和流程描述图。在此系统中，参与方被称为devices，所有的参与方在本地通过本地数据计算updates，而后上传给centric server。Centric server使用updates更新全局模型，再下发给各个devices，一次迭代结束。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这里首先介绍下该系统中的术语，在Devices上运行的任务称为FL Tasks，在一次迭代中的所有参与devices的规模称为FL population。Server每次迭代会选择此次迭代的FL population，并下发FL plan（一个TensorFlow Graph，用于指导device如何完成FL Tasks）。一次迭代称为round，一旦round被建立，server会给每个参与方发送当前的global model parameters，和FL checkpoint。在同步了术语之后，本章将描述一个round中会有哪些具体的阶段（Phases）。

  <h3>Phases</h3>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Selection</b>: 对应途中黄色部分。Devices向server汇报自身的存在，而后server根据certain goals like the optimal number of participating devices选择合适的参与方。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Configuration</b>: 根据aggregation mechanism，配置devices，而后发送FL checkpoint和global model。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Reporting</b>: Server等待devices上传updates。当收到了updates，server使用Federated Averaging方法聚合所有的updates，并更新global model。<be>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在上述过程中包含两个问题，一是如何设置每一次的FL populations，而是在report阶段如何设置recieve time window。在该框架中，提供了Pace Steering阶段，用于Server在round开始前，通过某种算法设置这两个变量以达到某种goal。

  <h2>Device</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本章将描述Device的计算框架。<br>
  <img src="pic/3.2.png" style="margin: 0 auto;"><br>
  Device中的软件架构如上图。Device需要维护一个example store，例如SQLite database，用于存储本地用于训练或evaluation的数据。在Phase章节中描述的FL训练过程的实现是FL Runtime。

  <h2>Server</h2>
  &nbsp;&nbsp;&nbsp;&nbsp;本章描述Server的框架。在横向联邦学习场景中，Server接受到的updates的规模可能是KB到几十MB级别的，每次参与训练的devices数量在几十到hundreds of millions，所以在一次训练时，Server接受到的信息是高度可变的。为了应对这种可变性，通常使用Actor-based系统。Actor可以对接受到的message作出反应，允许创建新的actor和task，下面来介绍Server是如何使用actor承载updates并完成training的。<br>
  <p>
  <img src="pic/3.3.png" style="margin: 0 auto;"><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Coordinator</b>是framework中的top-level actor，每一个coordinator与一个FL population相对应。而后Coordinator将该population告知selector，开始一次round。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Selectors</b>负责接收device connections，它们周期性的接受来自Coordinator的FL population信息，而后在本地决策选择哪些devices用于此轮训练并接受updates。当收到updates后，Selector会将updates传递给Aggregators。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Master Aggregators</b>负责管理每个FL task，在runtime中，其动态决定Aggregator的数量。
</div>

<h2><a name="section4">4. ParSync</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://www.usenix.org/conference/atc21/presentation/feng-yihui">Scaling Large Production Clusters with Partitioned Synchronization</a>. 2021. ATC<br>

  <h2>场景和Problems</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;该论文的场景为对大型集群上运行的billions of tasks进行资源分配，scheduler需要对诸如scheduling efficiency, scheduling quality, resource utilization, fairness and priority等做复杂的取舍。这样以来，scheduler会产生很大的延迟，那么对于大部分short-jobs而言是不能容忍的。故需要降低scheduler的latency。<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;传统的集中式资源分配架构如YARN很难应对大规模集群，故在此考虑使用分布式资源分配架构。在distributed scheduler中，例如Omega，最大的问题是如何维护cluster state。在Omega中使用master维护cluster state，其余的local scheduler周期性地拷贝cluster state from master，于是在local scheduler中，会存在stale state问题。第二个问题是，多个scheduler会产生scheduling confilcts，这时需要使用master进行裁决。两个问题都会导致high scheduling latency。<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;故在此文中，将集群划分为N个部分，每个部分的local scheduler对自身部分持有fresh states，对其他部分持有possible stale states。文章证明了这样的架构明显减少了resource contention，不同的parts之间通过<b>partitioned synchronization (ParSync)</b>实现state交换。
</div>

<h2><a name="section4">5. Hoplite</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://dl.acm.org/doi/abs/10.1145/3452296.3472897">Hoplite: efficient and fault-tolerant collective communication for task-based distributed systems</a>. 2021. OSDI<br>

  <h2>场景和Problems</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;该论文的应用场景为，task-based runtime distributed framework。与传统的task-based framework或data-based framework不同的是，runtime framework会在runtime中创建新的任务，并schedule某个worker去运行该任务。那么对于communication这一层而言，如何在runtime中运行collective collective communication如all-reduce是一个问题。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;传统的collective communication如OpenMPI, MPICH, Horovod, Gloo, NCCl等都需要在runtime之前先得知communication pattern，即app需要指出参与的workers会在runtime中如何通信。但在runtime framework中，对于新的任务我们是没法提前得知其会被部署在哪个worker上，也就无法得知该如何collective communication。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;所以，Hoplite允许参与者动态的运行collective communication，其计算data transfer schedule于任务或对象产生/到来时，并使用fine-grained pipelining运行low-latency data transfer scheduling。
</div>

