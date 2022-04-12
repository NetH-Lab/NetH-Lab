---
title: Summary of EVA
summary: '[联邦学习][同态加密优化]EVA学习笔记'
authors:
- Minel Huang
date: “2022-04-12T00:00:00Z”
publishDate: "2022-04-12T00:00:00Z"

publication_types: ["0"]

tags: 
- Federated Learning
- Homomorphic Encryption
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Apr.12 2022</i></font></h4>
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
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍全同态算法和Problems。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>Architecture and details</b></font></a>：介绍VF2Boost的具体优化方案。
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本文探讨的场景为全同态加密（FHE），以state-of-art的CKKS为主要讨论对象。在本节中，我们先从介绍CKKS算法开始。<br>
  <h4>CKKS算法流程</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;CKKS算法的安全性基于RLWE问题，全称为Ring Learning With Error。本文希望通过容易理解的话，来阐述CKKS算法，具体的数学定义请参考其他文章。首先，RLWE问题需要构造`(-a·s+e, a)`对，其中`a`, `s`, `e`均为一个最高项次数不超过`N-1`的多项式。但显然，输入的明文数据`m`显然不是一个多项式，那么第一个问题是，如何将明文变成一个符合RLWE条件的多项式？<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面我们来描述CKKS是怎么做的。首先，定义明文m是一个复数向量，即该向量中的每一个元素都是一个复数。定义`\pi^{-1}`变换和`\delta^{-1}`变换，其中`\delta^{-1}`变换将一个复向量映射到了一个复数多项式上，但是我们在实际应用中很难实现复数，故在`\delta^{-1}`变换前引入`\pi^{-1}`变换，使得最终可以得到一个正整数多项式，即多项式中的每一项系数都为正整数。该多项式记为`m(X)`。于是，我们可以通过两个变化将明文`m`转换为一个符合RLEW问题的多项式`m(X)`。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;
</div>
