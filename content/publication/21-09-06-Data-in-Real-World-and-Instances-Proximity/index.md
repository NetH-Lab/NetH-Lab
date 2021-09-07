---
title: Data in Real World, Instances Proximity and Feature Embedding
summary: '[机器学习][论文选读]数据，样本相似度和Feature Embedding'
authors:
- Minel Huang
tags: 
- 基础知识
date: “2021-09-06T00:00:00Z”
publishDate: "2021-09-06T00:00:00Z"
featured: false
mathjax: true
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>协作者：MinelHuang，更新日期：June.15 2021</i></font></h4>
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
    <font size="2px"></font>
  </div>
  <div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 0. <a href="#section0"><font color="blue"><b>前言</b></font></a>
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>Data in Real World</b></font></a>
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>Feature Embedding</b></font></a>
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 3. <a href="#section3"><font color="blue"><b>Instance Proximity</b></font></a>
  </div>
</div>
<br>

<h2><a name="section0">前言</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本文为笔者阅读Large-Scale Heterogeneous Feature Embedding和Ensemble manifold regularized sparse low-rank approximation for multiview feature embedding两篇论文后的一些笔记。重点将介绍Data特性、样本相似度和Feature embedding的概念。
</div>

<h2><a name="section1">1. 数据特性</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;以FaceBook为例，其数据由各个用户每天所发的posts组成。posts实际上一组非结构化数据，每个人发送的推文可能包含文字、图片甚至是视频；用户之间发送的posts可能是有联系的，比如说A和B是好友，并且一起去了某餐厅吃饭，所以他们都发了关于美食的推文。所以在真实世界的数据特性可以被概括为：<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;1. multi-tpye或者multi-source：数据类型是多样的，比如例子中的文字、图片等，当然也可能是来自不同的sources。不同source可以理解为，假如一组数据都是在描述一个人的长相，并且都是文字信息，那么不同source指的是instances可能在描述眼睛、鼻子、嘴等等部位，也即从不同的view去看待“长相”。
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;2. Correlated：Instance之间是有联系的，Instance A的结果可能对Instance B有影响。我们可以使用一张关系图G来描述不同Instance之间的relationship。
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;所以，数据in real world可以被称为是heterogeneous的。那么如何利用这类数据来进行机器学习呢？根据数据的multi-type或multi-source的特性，我们可以使用multi-model或multi-view两种方式来描述整个DataSet，以下给出两种方式的解释：
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;1. multi-view：DataSet由一系列X矩阵组成，每个矩阵代表一个View。其中，所有的矩阵都包含N个Instances，在一个矩阵中，feature域是一样的，这就意味着在一个View中的Instance是同构的。举个例子，假设我们有一组描述人脸的数据集，第一个X是描述人眼的，第二个X为描述肤色的。
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;2. multi-model：在multi-view中，一个view的Instance可能由不同的model组成，比如描述人眼的图像，和评论信息（文字）。在multi-model中，关注的是Instance的表现形式是一样的，比如都为图像数据。

</div>

<h2><a name="section2">2. Feature Embedding</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;First question是，如何描述section 1中的data，当data异构性较强时，我们只能使用更多的feature来描述，这会导致dataset中，feature域十分庞大，也即high-dimension（高纬度）。我们希望将feature域缩小，以降低整个模型训练的时间成本，故Feature Embedding可以理解为：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;将high-dimension的数据转换为low-dimension的。注意，这里还需要在降维后，所有的数据可以使用同一组feature来表示。
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在现阶段有哪些挑战：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;1. 现有数据集中的数据过于庞大，且每个instance包含过多的特性（即feature）<br>
  &nbsp;&nbsp;&nbsp;&nbsp;2. data in real world <br>
  &nbsp;&nbsp;&nbsp;&nbsp;3. 数据集为稀疏矩阵
</div>

<h2><a name="section3">3. Instance Proximity</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在此类文章中，常会提到Instance Proximity这个词汇，Large-Scale Heterogeneous Feature Embedding中给出了定义：<br>
  &nbsp;&nbsp;&nbsp;&nbsp;Definition 1. (Instance Proximity) It refers to the similarities between instances defined by the features of instances.
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;即指的在一个数据集X中，不同的行（Instance）之间的差异性，这种差异性可以使用不同的数学定义，比如计算两个行向量之间的欧式距离、余弦距离等。我们可以使用一个S图来表示instances之间的差异性，其中vertexs代表instances，edges代表两个instances间的差异性。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在这里我们需要提出一个疑问，对一个view下的instance proximities可不可以进行运算，也即一个view下的instance proximities是同构的吗。在feature embedding中，其中一个算法为Principal Component Analysis (PCA)，即主成分分析。在该算法中，对样本方差贡献度越大的instance，越包含更多的信息，而其中的一步便是计算instance proximity。故在mutil-view下，我们同样可以认为，若一个instance对该view中的方差贡献度越大，它所含的信息越大，所以对一个view下的instance proximities的运算是有意义的，也即其是同构的。
</div>

<!--ref-->
<h2>附录：参考源</h2>
<div class="div_learning_post">
<p>

1. Zhang L, Zhang Q, Zhang L, et al. Ensemble manifold regularized sparse low-rank approximation for multiview feature embedding[J]. Pattern Recognition, 2015, 48(10): 3102-3112.<br>
2. Huang X, Song Q, Yang F, et al. Large-scale heterogeneous feature embedding[C]//Proceedings of the AAAI conference on artificial intelligence. 2019, 33(01): 3878-3885.
</p>
</div>

</body>