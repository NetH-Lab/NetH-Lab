---
title: Machine Learning & Federated Learning
summary: '学习笔记 - 机器学习&联邦学习'
authors:
- Minel Huang
tags: 
- Machine Learning
- Federated Learning
date: “2021-09-01T00:00:00Z”
publishDate: "2021-09-01T00:00:00Z"
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
    <font size="2px">您可以通过目录直接阅读您感兴趣的部分，各章节间并无太大联系。</font>
  </div>
  <div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 0. <a href="#0_preface"><font color="blue"><b>机器学习引言</b></font></a>：机器学习入门相关内容
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>逻辑回归算法</b></font></a>：Logistic Regression
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>基于树的算法</b></font></a>：Tree-based Machine Learning Algorithm
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 3. <a href="#section3"><font color="blue"><b>联邦学习引言</b></font></a>：联邦学习入门相关内容
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 4. <a href="#section4"><font color="blue"><b>纵向联邦逻辑回归</b></font></a>：Heterologous Logistic Regression
    <p>
  </div>
</div>

<br>

<h2><a name="0_preface">0. 机器学习引言</a></h2>
<div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;该部分您可以参考博客<a href="https://www.cnblogs.com/subconscious/p/4107357.html">博客: 从机器学习谈起</a>，原作者：计算机的潜意识。
</div>

<h2><a name="section1">1. 逻辑回归算法</a></h2>
<div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;参考文章：<a href="https://zhuanlan.zhihu.com/p/74874291">【机器学习】逻辑回归（非常详细）</a><br>  
    &nbsp;&nbsp;&nbsp;&nbsp;逻辑回归（Logistic Regression, 后文简称LR）是一种经典的算法，广泛应用于统计学与机器学习领域，因其简单、可并行化、可解释性强而深受工业界喜爱。下面简要对逻辑回归学习做些许介绍，具体可参考上述链接。<br>
    <img src="pic/1.1.png" style="margin: 0 auto;"><br>
    <img src="pic/1.2.png" style="margin: 0 auto;"><br>
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;关于逻辑回归的Python实现您可以参考此blog: <a href="https://neth-lab.netlify.app/publication/21-4-18-logistic-regression/">Logistic Regression in Practice</a>

</div>

<h2><a name="section2">2. </a>基于树的算法</h2>
<div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;回顾下之前的线性模型，在线性模型中，我们对每个特征附上权重，与输入相乘相加后的到一个新的值，我们使用这个值来对输入进行划分。而在树模型中，我们需要对特征一个一个处理。决策树与逻辑回归的分类区别也如此，逻辑回归是将所有特征变化为概率，大于某一阈值的划分一类；决策树是对每个特征做一个划分。

<h3>2.1 决策树算法</h3>
    &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://towardsdatascience.com/decision-trees-in-machine-learning-641b9c4e8052">Decision Trees in Machine Learning</a>
<h4>2.1.1 从一个例子出发</h4>
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;假设我们想得到一个模型，用于预测在一场事故中，什么样的乘客可以生还。数据使用3个特征来表示：性别、年龄和配偶+孩子的数量。<br>
    &nbsp;&nbsp;&nbsp;&nbsp;我们最终得到如下的一个树结构：<br>
    <img src="pic/2.1.png" style="margin: 0 auto;"><br>
    &nbsp;&nbsp;&nbsp;&nbsp;其中，黑色粗体为节点，代表一种情况，最上方的节点为根节点；每个节点将树分为不同的分支（branches），当节点不再可分时，即在branch的末尾，为叶子，图中以红色和绿色表示。<br>
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;从这么一个决策树来看，乘客是否能生还是非常清晰可见的，并且特征间的关系也清晰可见。那么如何从数据得出这么一个模型呢？一种方法为Classification Tree，其目标为将乘客分类成survived和died两种；另一种为Regression Tree，表示方法与CT相同，只是RT是用来预测连续值得，比如房价。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;构建决策树，我们需要知道每个节点该使用哪个特征，需要知道节点splitting时的条件，需要知道何时结束（得到叶子节点）。在算法中通常树是在随机生长的，故我们还需要知道如何修整它。

<h4>2.1.2 Recursive Binary Splitting</h4>
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;最开始我们仅含一个根节点，并且所有的feature还没有分割开。我们有3个features，所以我们有3种可能使用的splits。一个最简单的方式为，计算每种方式的cost函数，选择最小的情况最为分割方式。在本例种我们选择sex作为根节点，随后的分割方法同样可以以这种方式进行。这种方法也被称为贪心算法。
</div>

<h2><a name="section3">3. </a>联邦学习引言</h2>
<div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;关于此节内容，您可以参考此blog：<a href="https://neth-lab.netlify.app/publication/21-3-2-overview-of-federated-learning/">Overview of Federated Learning</a>
</div>

<h2><a name="section4">4. </a>纵向联邦逻辑回归</h2>
<div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;关于heterogeneous federated logistic regression，在<a href="https://neth-lab.netlify.app/publication/21-4-13-heterologous-logistic-regression/">Heterologous Logistic Regression</a>中笔者对其进行了系统解读。以下内容则是对该博客的补充。<br>
    <p>
    <img src="pic/4.1.png" style="margin: 0 auto;"><br>
    <img src="pic/4.2.png" style="margin: 0 auto;"><br>
    &nbsp;&nbsp;&nbsp;&nbsp;那么一次Hetero-LR过程可以概括为：<br>
    <img src="pic/4.3.jpg" style="margin: 0 auto;"><br>
    &nbsp;&nbsp;&nbsp;&nbsp;其中，[[·]]代表使用同态加密方法加密数据。
    <img src="pic/4.4.png" style="margin: 0 auto;"><br>
    <img src="pic/4.5.png" style="margin: 0 auto;"><br>

</div>