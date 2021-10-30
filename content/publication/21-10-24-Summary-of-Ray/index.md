---
title: Summary of Ray
summary: '[分布式系统][分布式机器学习][论文选读]Ray学习笔记'
authors:
- Minel Huang
date: “2021-10-24T00:00:00Z”
publishDate: "2021-10-24T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Distributed Systems
- Distributed Machine Learning
- Ray
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Aug.04 2021</i></font></h4>
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
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. <a href="https://dl.acm.org/doi/abs/10.1145/3102980.3102998">Real-time machine learning: The missing pieces</a>. 2017. HotOS (workshop)<br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. <a href="https://www.usenix.org/conference/osdi18/presentation/moritz">Ray: A distributed framework for emerging {AI} applications</a>.2018.OSDI
  <div align="center">
    <h2> 目录 </h2>
    <p>
  </div>
  <div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍Ray的应用场景和Problems。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>Programming and Computation Model</b></font></a>：介绍Ray是如何解释，翻译用户的program的，并介绍Ray提供了哪些用户接口，用户如何使用这些接口完成RL任务的编写。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 3. <a href="#section2"><font color="blue"><b>Architecture</b></font></a>：介绍Ray的系统架构，重点在于描述Ray如何将它的task graph分配给各个workers，又是如何动态执行的。
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本章从Ray的workshop开始，解读Ray系统的应用场景以及为什么重要（Problems）。
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Ray的应用场景为强化学习（Reinforcement Learning, RL），这类场景下的重要特点是，需要实时的（Real-time）采集环境中的信息，并进行计算。举一个例子，RL常常用于游戏AI或者机器人AI中。以flappy bird为例，AI需要实时的根据即将飞跃的水管高度（环境）来设计出一系列actions，以获得更高的分数（目标）。于是RL算法的特点可以总结为：需要不断地引入新的环境数据，来对更新policy，使AI根据此policy得出的actions可以获得更高的分数（objective）。<br>

  <img src="pic/1.1.jpg" style="margin: 0 auto;"><br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们把采集数据的过程称为simulation，更新policy的过程拆分为training（如根据环境数据计算梯度）和serving（对policy进行evaluation），依旧以flappy bird为例，各个sub-computation的作用如下：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. <b>simulation</b>：获取水管的高度、bird距离水管的距离、bird高度等；负责对actions进行打分，比如输入点两下屏幕，simulation给出反馈rewards=1，即当前得分为1（第二个水管没法过去），这一过程称为policy evaluation。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. <b>serving</b>：根据环境data(current state of environment)和先前的policy，得到一系列actions（例如等待0.1s后再点击一下屏幕），将actions发送至simulation，获得rewards。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;3. <b>training</b>: 根据环境信息（states）和rewards训练policy，通常使用随机梯度下降(SGD)方法。 <br>
  <img src="pic/1.2.jpg" style="margin: 0 auto;"><br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;由此，我们可以总结出RL对系统的几点要求：<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>系统性能需求</b>：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. <b>Low latency</b>：AI需要快速的得到一个policy以获取接下来一段时间的action，故需求低延迟（应用是real-time，reactive核interactive的）。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. <b>High throughput</b>：simulation产生的环境state信息非常庞大（training或inference需要），故底层ML系统的throughput性能要高。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;3. <b>Support heterogeneity in resource usage</b>：例如使用GPUs执行training，使用CPUs执行simulations<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>模型执行需求</b>：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. <b>Dynamic execution</b>：许多RL原语都需要动态执行，这是因为系统并不能知道simulation任务的完成顺序，以及某个computation的结果会决定未来的computation，故希望在execution过程中，还可以动态的添加新的任务。该特性为RL tasks的核心特性，也是Ray系统设计的核心。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. <b>Heterogeneous tasks</b>：在RL任务中，用户调用的Deep learning原语和RL simulation都会产生execution time和resource requirements不同的（widely different）tasks，故系统需要支持tasks和resources的异构型。该点可以理解为，假设一个机器人，simulation获得的数据有视频和前方环境的距离，那么policy的训练过程可以采用不同的算法（视频使用DNN，距离使用LR），即异构任务。第二类异构任务为stateless和stateful，其中stateless指的是可以在任何worker上运行的任务，stateful指的是需要在特定的worker上运行，以适应parameter server架构或提高GPU使用效率（让GPU进行重复计算）<br>
  &nbsp;&nbsp;&nbsp;&nbsp;3. <b>Arbitrary dataflow dependencies</b>：与上述两点类似，deep learning primitives和RL simulations会生成随机的并且often fine-grained task dependencies。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>模型执行需求</b>：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. Transparent fault tolerance<br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. Debuggability and Profiling<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;于此，我们提取出几点Ray需要重点优化的部分，包括如何动态的创建新任务，如何快速schedule新任务，以及如何兼容现有distributed computing system和异构的计算资源。分布式计算系统包含两大任务，其一为如何描述用户（系统的使用者）的Program，或者说需要向用户提供哪些编程接口来满足任务需求；其二为如何将Program分布式的执行，或者说如何将Task分配给各个worker。在下一章节中，我们先讲述第一个大问题，并同时思考该如何设计才能满足Ray的三点重要的系统需求。
</div>

<h2><a name="section2">2. Programming and Computation Model</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本章将解决，如何提供一些编程API，来同时满足用户的编程需求和系统动态执行任务的需求。我们使用Programming model来向用户提供接口口，使用Computation model来实现动态任务执行。

  <h3>Programming Model</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Task</b>：Task代表在一个stateless worker上的一个remote function的execution，当一个remote function被调用，即创建了一个Task，并立刻返回a <b>future</b>。Future代表Task的结果，但并不意味着需要Task真正被执行。用户可以立即将future作为另一个remote function的输入，而不需要等待Task执行结束。Task用于执行stateless和side-effect free的function，即function的输出仅由inputs决定 - 从而简化fault-tolerance中re-execution过程。<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Actor</b>：actor代表stateful function，和Task相同的是actor可以被远程调用执行，并立即返回future。但与Task不同的是，Actor只能在stateful worker上运行。<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;可以看到Ray编程模型中，function的输入输出大多是future，故还需要提供对future的操作，如下图：<br>
  <img src="pic/2.1.jpg" style="margin: 0 auto;"><br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;由此我们发现，Ray通过提供Task，Actor和Future API，使用户可以完成任意的RL任务，这是因为任何RL都可描述为stateless function和stateful function的组合，而future使得用户不需要关注底层分布式系统的运行状态即可完成编程。于是下面的问题为，如何构建Task，Actor之间的依赖关系，又如何将其分配到各个workers上（placement）？Future具体又在何时被真正的执行呢？

  <h3>Computation Model</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Ray使用一个动态的task graph来描述用户程序，当remote functions和actor methods的输入available时便开始自动执行。本节重点描述Ray如何通过Actor、Task来构建computation graph。<br>

  <b>Dynamic task graph</b><br>
  <img src="pic/2.2.png" style="margin: 0 auto;"><br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Ray中的动态task graph如上图所示，其中包含两类node：data object和remote function；和两类边：data edges和control edges。于是，用户的程序可以使用task graph来描述，其中使用data edges描述function的输入输出，使用control edges描述function之间的调用关系（computation dependencies）。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当引入actor后，actor method同样也是function，故不需要在task graph中加入新的节点；然而actor method还需要state信息作为输入，而state是仅存在stateful worker上的，对于这样一种特性，Ray中使用state edges来描述state间的依赖关系。如此，若一个actor method调用了另一个actor method，则在这两个actor间加入一条state edge，代表两个actor method共用一部分state信息。终于，我们在一副task graph中同时描述了stateless function和stateful function，这意味着该task graph能解释任意的User program。
</div>

<h2><a name="section3">3. Architecture</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;首先，我们先简单分析一下上一节的task graph包含了哪些内容。第一是node，graph中包含了两类节点，分别是data node和remote function，故在系统中需要想办法承载这两类node，需要关注两类node可否置于同一个worker上。当node的执行位置固定后，显然function的输入可以根据edge来进行数据拷贝，然而对于actor而言是比较头疼的，因为state信息被多个nodes共用，所以如何在多个node中share states是一个问题。最后一点，该task graph是被动态创建并执行的，那么如何快速的schedule新的node和edge同样是一个问题。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;在本章中，笔者将带着这三个问题来介绍Ray的architecture。

  <h3>Application layer</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Ray中引入三个逻辑节点来向cluster描述task graph，分别是Driver，Worker和Actor。显然，Driver是真正编译程序的地方；worker是处理stateless function的地方；actor是处理stateful function的地方。每一个实际的machine（或者是VMs或者是container）都是这三种逻辑节点中的一种，Driver仅有一个。于是，一个task graph便可以分布式的在一个集群中执行。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;至此，一个集群便可以承载所有的task graph。当然，集群管理者首先需要配置好每个machine的角色。于是问题只剩下了如何将function分配给各个机器，该问题由Ray中的System layer解决。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;Ray architecture如下图所示：<br>
  <img src="pic/2.3.png" style="margin: 0 auto;"><br>

  <h3>System layer</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;首先，我们要先解决function会被动态创建的问题。在执行过程中，remote function node会动态的创建新的node。那么我们可以认为，remote function node还需要维护它创建的function的一些meta信息，例如输入输出数据的位置和程序的执行状态，这意味着该function不是stateless的了。举一个例子，如果f1创建了f2，那么f1还需要维护f2的运行状态和meta数据，这样的话执行f1的worker或actor便维护了一部分state，并且当错误发生时很难恢复f2。，所以，我们希望所以function在被执行的时候都是stateless的，这意味着每个计算节点仅需要执行完当前function后便可以结束，这大大简化了系统的设计。<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;所以，Ray引入了Global Control Store (GCS)来存储、维护function间的依赖关系。于是，上述f1创建f2的过程可以被描述为，f1执行，向GCS报告创建了f2，f1执行完毕，schedule再为f2分配新的执行节点（根据GCS上存储的f2 meta信息）。GCS的设计使得每个function都是stateless的，这里的stateless指的是执行节点仅需要获取输入，然后执行，执行结束后便可以释放掉所有的资源并等待下一次的schedule即可。<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当所有的function都是stateless的了，我们仅需要关注如何快速的schedule所有的function即可。在架构图中我们可以看到两类scheduler，分别是local scheduler和global scheduler。在这里的matric是如何低延迟的schedule，故不能像Spark那样仅使用一个centralized scheduler
</div>

