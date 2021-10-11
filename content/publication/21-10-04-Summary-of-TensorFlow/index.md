---
title: Summary of TensorFlow
summary: '[分布式系统][分布式机器学习][论文选读]TensorFlow学习笔记'
authors:
- Minel Huang
date: “2021-10-04T00:00:00Z”
publishDate: "2021-10-04T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Distributed Systems
- Distributed Machine Learning
- TensorFlow
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
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：Martin Abadi.<a href="https://www.usenix.org/conference/osdi16/technical-sessions/presentation/abadi">TensorFlow: A System for Large-Scale Machine Learning</a>.OSDI.2016<br>
  &nbsp;&nbsp;&nbsp;&nbsp;Last Update: 10/10/2021
  <div align="center">
    <h2> 目录 </h2>
    <p>
  </div>
  <div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>场景和Problems</b></font></a>：介绍DNN场景以及为何PS/Batch-System不适用

  </div>
</div>

<h2><a name="section1">1. 场景和Problems</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;场景：Neural Network Training，其中以image classification and language modeling为代表性应用。<br>
  &nbsp;&nbsp;&nbsp;&nbsp;在叙述Problems之前，您可以先参考<a href="https://neth-lab.netlify.app/publication/21-09-01-machine-learning-and-federated-learning/">Machine Learning & Federated Learning</a>来了解DNN的训练原理。<br>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;DNN的训练过程包含前向传播和反向传播两个过程，其function特性是包含大量的state信息。例如更新第l层的W时，需要第l+1层的W和第l层的z；在计算l层的z时，需要l-1层的a。故function涉及两类依赖关系，一次迭代中，前向传播和反向传播包含层与层的依赖，并且包含对W的依赖；更新W又依赖前向传播的结果，与l+1层W的更新结果。故每个sub-computation几乎都要依赖一组state，例如W，z。在DNN训练这种复杂的场景下，我们再来审视batch-processing system(Spark)和parameter server system(DistBelief)，会出现哪些问题？<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;首先，Spark调度在data reuse和stateless function时是最高效的，DNN中大量的state信息导致Spark必须多次shuffle，来保证其sub-computation是stateless的（因为其系统中都是stateless worker），从而提高效率。然而我们发现，DNN的训练过程中每个function都需要state信息
</div>