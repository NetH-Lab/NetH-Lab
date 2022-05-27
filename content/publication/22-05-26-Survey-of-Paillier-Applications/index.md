---
title: Survey of Paillier Applications
summary: '[同态加密][Paillier]'
authors:
- Minel Huang
date: “2022-05-26T00:00:00Z”
publishDate: "2022-05-26T00:00:00Z"

publication_types: ["0"]

tags: 
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：May.26 2022</i></font></h4>
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
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#section2"><font color="blue"><b>EVA language</b></font></a>：从EVA提供的接口开始。
  </div>
</div>

<h2><a name="section1">1. From Magazines</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：2020-Queue-Differential Privacy: The Pursuit of Protectionsby Default<br>
  &nbsp;&nbsp;&nbsp;&nbsp;DP方法可以用于<b>federated learning</b>以训练模型；或者和homomorphic encryption一起使用来实现<b>secret sharing</b>。
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：2021-Queue-Toward Confidential Cloud Computing<br>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>Confidential databases and analytics</b>: Database中可能存一些敏感信息，比如personal records, finacial information and government data。通常为了保护这些信息，我们需要一套繁琐的认证系统来确保access是可靠的，有权限的，但这样一套系统并不能防御来自如内部管理人员的攻击（管理者通常有权限访问所有的data）。故，encrypted database的概念被提出。其中一种实现办法（由CryptDB提出）为使用partially Homomorphic Encryption加密数据，如此可以支持数据库中的基本运算。
</div>

<h2><a name="section1">2. From Survey</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：2018-ACM Computing Survey-A Survey on Homomorphic Encryption Schemes: Theory and Implementation<br>
</div>
