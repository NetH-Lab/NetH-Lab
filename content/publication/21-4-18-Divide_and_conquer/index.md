---
title: Divide and Conquer
summary: '[算法导论][分治法][开放课程学习]'
authors:
- Zobin Huang
tags: 
- Algorithm Introduction
- Divide and Conquer
- Open Lecture
date: “2021-04-18T00:00:00Z”
publishDate: "2021-04-18T00:00:00Z"
featured: false
mathjax: true
---

<head>
<style>
    img{margin-left: 20px; margin-right: 20px;}
    #table th{text-align:center;}
    #table td{text-align:center;}
    p{margin-left: 15px; margin-right: 15px;}
    .div_learning_post{font-size: 16px; word-spacing:0px;}
    .div_learning_post_boder{font-size: 16px; word-spacing:0px;  border:1px solid black;}
    .div_indicate_source{font-size: 18px; word-spacing:0px; background-color: #E0E0E0;}
</style>

<!--支持网页公式显示-->    
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>
</head>

<body>

<div align="center" class="div_indicate_source">
  <h4>⚠ 转载请注明出处：<font color="red"><i>作者：ZobinHuang，更新日期：Mar.13 2021</i></font></h4>
</div>

<!--表格-->
<!--
<table border="1" align="center">
  <caption>表格</caption>
  <tr>
    <th>A</th>
    <th>B</th>
    <th>C</th>
  </tr>
  <tr>
    <td>xxx</td>
    <td>xxx</td>
    <td>xxx</td>
  </tr>
</table>
-->

<!--图片-->
<!--
<div align="center">
  <img src="./pic/xxx.png" width=30%>
</div>
-->

<!--正文-->
<!--
<p>
&nbsp;&nbsp;&nbsp;&nbsp;公式：<span>`\overline{A}\overline{B}`</span>
</p>
-->
<div class="div_learning_post_boder">
<p>
&nbsp;&nbsp;&nbsp;&nbsp;<b>附录：主定理</b>
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;对于 <span>`T(n) = aT(n/b) + f(n)`</span>：
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>CASE 1</b>：<span>`f(n) = \O(n^(log_ba-\epsilon))`</span>，常数 <span>`\epsilon > 0`</span>，<span>`T(n) = \Theta(n^(log_ba))`</span>。
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>CASE 2</b>：<span>`f(n) = \Theta(n^(log_ba)\lg^kn)`</span>，常数 <span>`k \geq 0`</span>，<span>`T(n) = \Theta(n^(log_ba)\lg^(k+1)n)`</span>。
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>CASE 3</b>：<span>`f(n) = \Omega(n^(log_ba+\epsilon))`</span>，常数 <span>`\epsilon > 0`</span>，<span>`T(n) = \Theta(f(n))`</span>。
</p>
</div>

<br>

<div class="div_learning_post">
<p>
&nbsp;&nbsp;&nbsp;&nbsp;在上一文中我们花费了大量的篇幅用于描述分析算法时所需用到的数学工具。在这篇文章中，我们将逐步探索如何设计算法，即从 <b>算法分析</b> 到 <b>算法设计</b> 的过程。今天我们关注的是 <b>分治法</b>。
</p>
</div>

<!--标题-->
<h2>1. 分治法 (Divide and Conquer)设计范式</h2>
<div class="div_learning_post">
<p>
&nbsp;&nbsp;&nbsp;&nbsp;首先我们给出分治法的<b>设计范式 (Design Paradigm)</b>，然后我们结合具体的算法来对这个设计范式做更深入的理解。
<br>&nbsp;&nbsp;&nbsp;&nbsp;具体来说，分治法的设计思路可以分为三步：
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. <b>分解 (Divide)</b>：将问题分解为子问题
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. <b>“治” (Conquer)</b>：递归地解决每一个子问题
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. <b>合并 (Combine)</b>：将子问题的解合并
</p>
</div>

<h2>2. 基于分治法的算法</h2>
<div class="div_learning_post">
<h3>(1) 归并排序</h3>
<p>
&nbsp;&nbsp;&nbsp;&nbsp;首先我们回顾一下我们在第一篇文章已经阐述过的归并排序的步骤：
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. <b>分解 (Divide)</b>：将数组拆分为两个部分
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. <b>“治” (Conquer)</b>：递归地将两个子数组进行排序
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. <b>合并 (Combine)</b>：合并两个排序数组（线性时间）
<br>&nbsp;&nbsp;&nbsp;&nbsp;我们可以得出归并排序的时间复杂度为：
</p>
<div align="center">
  <span>`T(n)=\underbrace{2}_{子问题数目}\underbrace{T(n/2)}_{子问题规模} + \underbrace{\Theta(n)}_{分解和合并开销}`</span>
</div>
<p>
&nbsp;&nbsp;&nbsp;&nbsp;
</p>
</div>
<!--ref-->
<!--
<h2>附录：参考源</h2>
<div class="div_learning_post">
<p>
1. David Money Harris, Sarah L, Harris, 机械工业出版社, <b>数字设计和计算机体系结构</b>
</p>
</div>-->

</body>