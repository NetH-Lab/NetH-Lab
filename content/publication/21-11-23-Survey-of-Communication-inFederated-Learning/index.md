---
title: Survey of Communication in Federated Learning
summary: '[机器学习][联邦学习][论文选读]联邦学习中的通信优化调研与综述'
authors:
- Minel Huang
date: “2021-11-23T00:00:00Z”
publishDate: "2021-11-23T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Machine Learning
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
  <div align="center">
    <h2> 目录 </h2>
    <p>
  </div>
  <div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍近年federtaed learning system的场景分类。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section1"><font color="blue"><b>Mitigating Communication Overhead</b></font></a>：通过减少edge devices和center server通信次数，降低通信开销。
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
</div>

<h2><a name="section1">2. Mitigating Communication Overhead</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考文献：<a href="https://ieeexplore.ieee.org/abstract/document/8885054">CMFL: Mitigating Communication Overhead for Federated Learning</a>. 2019. ICDCS <br>
  <h2>1 场景和Problem</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在横向联邦学习中，training通常是放在边缘设备上的，如个人手机或计算机。其过程通常是，中央服务器维护一个全局模型（global model），在一次迭代中，边缘设备上传其updates，而后中央服务器更新全局模型并下发至各边缘设备，一次迭代结束。在这里我们关注这一过程中的通信瓶颈，其分为两个方面：一是对于通信链路而言，部分边缘设备的network是很昂贵的，且不可靠的，比如使用4G的手机；而是对于数据量而言，在例如DNN训练中，updates的量非常庞大，通常由一个很大的gradient vector组成。这两方面原因使得在横向联邦学习中出现通信瓶颈。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下一个问题是，这种通信瓶颈会对系统造成哪方面的影响，即其系统metric是什么，如何量化？在一次迭代中，显然center server需要收集到足够多的updates后才能更新global model，所以当edge device和center server的通信开销无法被computation overlap，或与training time处于同一数量级时，通信开销便可能成为整个系统的瓶颈，其结果为可能大大增加了训练时间。于是其外在的metric可以是迭代时间，内在metric可以是data transfer的时间于迭代时间中的占比，或者是data transfer的次数。

  <h2>2 Solution</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通常有两类降低通信开销的方法，第一类为减少单次通信中的bit数，比如使用一些紧凑的数据结构来压缩updates；第二类为减少通信的次数，在本文中采用的是第二类方法。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;简单来说，该问题可以转换为，edge device如何判断哪些updates需要上传，哪些不需要上传即可。此文章中定义了<b>irrelevant updates</b>，代表该update对于global model而言是divergent的（分歧的，分离的），那么便可以不传这些updates。具体而言，一次迭代中，client或称为edge device先收到一个feedback information（global update）；而后client进行本地的training过程并铲除local update；通过feedback information检测此次local update和global update的差异性，若差异性过大，则认为此次local update时irrelevant的，于是不上传。通过此方法，该算法（成为CMFL）成功的提高了FL速率，与state-of-art的工作相比，分别提高了13.97x（vanilla FL），1.97x（Gaia）。

  <h2>3 Related work</h2>
  &nbsp;&nbsp;&nbsp;&nbsp;CMFL主要对比的工作为Gaia，Gaia是一种为geo-distributed machine learning设计的communication优化器。其工作原理主要是，在训练开始前设置一个predefined threshold，定义significance来表示一个local update的重要程度，若小于阈值，则不进行传输。在定义significance时，Gaia关注的是模型训练的speed，而忽略了optimization direction。这里笔者仅是总结CMFL文章中的观点，具体Gaia是如何优化的笔者将在后文中具体描述。可以看到，横向联邦学习对比的应用场景是geo-distributed learning。
</div>
