---
title: Basic Golang
summary: Golang 基础概念，以及日常开发所总结的使用经验
type: project

tags: 
- 基础知识
- Golang
- Go语言
authors:
- Zobin Huang

date: "2021-06-13T00:00:00Z"
lastmod: "2021-06-13T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by ZobinlHuang
  focal_point: Smart

links:
#- icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/georgecushen
# url_code: 
# url_pdf: 
#url_slides: ""
#url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
#slides: example
---
<head>
<style>
    img{margin-left: 20px; margin-right: 20px;}
    p{margin-left: 15px; margin-right: 15px;}
    th{text-align:center;}
    td{text-align:center;}
    .div_learning_post{font-size: 16px; word-spacing:0px;}
    .div_indicate_source{font-size: 18px; word-spacing:0px; background-color: #E0E0E0;}
</style>
<!--支持网页公式显示-->    
<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=AM_HTMLorMML-full"></script>
</head>

<body>

<div class="post_fs14_ws0">

|<div align="center"><h1><b>Go 基础知识 <font color="red">[部分转载]</font></b></h1></div>|
|:-|
|注：这些文章内容的一些部分是转载自 <a href="https://github.com/gopl-zh/gopl-zh.github.com">Go 语言圣经 (中文版)</a>，并加上了本人在使用过程中的一些自己的理解和经验，最终整理成可读性更高的网页形式。在此向原作者和译者表示感谢，他们给社区提供了很棒的 Golang 入门参考。<br>&nbsp;&nbsp;&nbsp;&nbsp;原作者：Alan A. A. Donovan · Brian W. Kernighan<br>&nbsp;&nbsp;&nbsp;&nbsp;译者：柴树杉，Github @chai2010，Twitter @chaishushan<br>&nbsp;&nbsp;&nbsp;&nbsp;译者：Xargin, https://github.com/cch123<br>&nbsp;&nbsp;&nbsp;&nbsp;译者：CrazySssst<br>&nbsp;&nbsp;&nbsp;&nbsp;译者：foreversmart njutree@gmail.com|
|[<b>Chapter 1: Golang 程序结构</b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_1_Basic_Concept/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 描述了 Golang 语言程序的基本元素结构、变量、新类型定义、包和文件、以及作用域等概念|
|[<b>Chapter 2: Golang 基础数据类型</b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_2_Basic_Data/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 讨论了数字、布尔值、字符串和常量，并演示了如何显示和处理 Unicode 字符。|
|[<b>Chapter 3: Golang 复合类型</b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_3_Compound_Type/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 描述了复合类型，从简单的数组、字典、切片到动态列表。|
|[<b>Chapter 4: Golang 函数</b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_4_Function/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 涵盖了函数，并讨论了错误处理、panic 和 recover ，还有 defer 语句|
|[<b>Chapter 5: Golang 方法</b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_5_Method/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 介绍了 Golang 的方法|
|[<b>Chapter 6: Golang 接口</b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_6_Interface/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 介绍了 Golang 的 Interface 机制|
|[<b>Chapter 7: Golang 并发编程 <font color="red">[未完成]</font></b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_7_Go_Routines/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 介绍了 Golang 的基于 goroutines 和 channel 的并发编程机制|
|[<b>Chapter 8: Golang 基于共享变量的并发编程 <font color="red">[未开始]</font></b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_8_Go_Shared_Variables/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 讨论了传统的基于共享变量的并发编程|
|[<b>Chapter 9: Golang Package <font color="red">[未开始]</font></b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_9_Go_Package/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 讨论了 Golang 的包机制和包的组织结构|
|[<b>Chapter 10: Golang 单元测试 <font color="red">[未开始]</font></b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_10_Unit_Test/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 讨论了单元测试，测试库提供了一些基本构件，必要时可以用来构建复杂的测试构件|
|[<b>Chapter 11: Golang 反射机制 <font color="red">[未开始]</font></b>](https://neth-lab.netlify.app/publication/21-06-13-Go_Basic_11_Reflection/index.html) <br> &nbsp;&nbsp;&nbsp;&nbsp; 讨论了 Golang 的反射机制，一种程序在运行期间审视自己的能力。|
</div>

</body>