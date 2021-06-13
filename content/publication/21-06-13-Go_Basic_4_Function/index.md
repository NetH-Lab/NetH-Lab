---
title: Golang 函数
summary: '[Golang][Go语言]'
authors:
- Zobin Huang
tags: 
- Golang
date: “2021-06-13T00:00:00Z”
publishDate: "2021-06-13T00:00:00Z"
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>协作者：ZobinHuang，更新日期：June.6 2021</i></font></h4>
  <div align="left">
  <font size="2px">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;这篇文章内容的一些部分是转载自<a href="https://github.com/gopl-zh/gopl-zh.github.com">Go 语言圣经（中文版）</a>，并加上了本人在使用过程中的一些自己的理解和经验，最终整理成可读性更高的网页形式。在此向原作者和译者表示感谢，他们给社区提供了很棒的 Golang 入门参考。
    <br>&nbsp;&nbsp;&nbsp;&nbsp;原作者：Alan A. A. Donovan · Brian W. Kernighan;
    <br>&nbsp;&nbsp;&nbsp;&nbsp;  译者：柴树杉，Github @chai2010，Twitter @chaishushan；Xargin, https://github.com/cch123；CrazySssst；foreversmart, njutree@gmail.com
  </font>
  </div>
</div>

<div class="div_licence">
  <br>
  <div align="center">
      <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="知识共享许可协议" style="border-width:0; margin-left: 20px; margin-right: 20px;" src="https://i.creativecommons.org/l/by-nc-nd/4.0/88x31.png" /></a>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本<span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Text" rel="dct:type">作品</span>由 <span xmlns:cc="http://creativecommons.org/ns#" property="cc:attributionName"><b>ZobinHuang</b></span> 采用 <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><font color="red">知识共享署名-非商业性使用-禁止演绎 4.0 国际许可协议</font></a> 进行许可，在进行使用或分享前请查看权限要求。若发现侵权行为，会采取法律手段维护作者正当合法权益，谢谢配合。
  </p>
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

<br>

<div class="div_catalogue">
  <div align="center">
    <h2> 目录 </h2>
    <p>
    <font size="2px">此文篇幅较长，故设置目录，有特定需要的内容直接跳转到相关章节查看即可。</font>
  </div>
  <div class="div_learning_post_boder">
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 0. <a href="#0_preface"><font color="blue"><b>前言</b></font></a>：讨论了本章阐述的内容；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#1_decoration"><font color="blue"><b>函数声明</b></font></a>：讨论了 Golang 中在声明函数时的注意事项；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#2_recursive"><font color="blue"><b>递归</b></font></a>：介绍了 Golang 中函数递归的写法，给出了一些例子；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 3. <a href="#3_multi_return"><font color="blue"><b>多返回值</b></font></a>：描述了当一个 Golang 函数返回值有多个时的一些特性
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 4. <a href="#4_error"><font color="blue"><b>错误</b></font></a>：讨论了 Golang 中的函数运行返回错误的问题，区分了错误 (error) 和异常 (exception) 这两个概念；
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1 <a href="#4_error_1"><font color="blue">错误处理</font></a>：讨论了应对函数运行错误的五种策略；
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2 <a href="#4_error_2"><font color="blue">文件结尾错误（EOF）</font></a>：讨论了一种特殊的错误类型：文件结尾错误；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 5. <a href="#5_value"><font color="blue"><b>函数值</b></font></a>：讨论了 Golang 中函数也可以作为值进行使用的特性；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 6. <a href="#6_anonymous"><font color="blue"><b>匿名函数</b></font></a>：展示了 Golang 中匿名函数的用法，且基于匿名函数编写了图的广度优先算法和深度优先算法；
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;6.1 <a href="#6_anonymous_1"><font color="blue">警告：捕获迭代变量</font></a>：警告了一种在 Golang 循环中使用函数值易犯的错误；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 7. <a href="#7_variable_parameter"><font color="blue"><b>可变参数</b></font></a>：解释了一个函数接受可变参数的内容 (即 ... 的用法)；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 8. <a href="#8_deferred"><font color="blue"><b>Deferred函数</b></font></a>：讨论了在函数中使用 deferred 机制的一些技巧和注意事项；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 9. <a href="#9_panic"><font color="blue"><b>Panic</b></font></a>：介绍了 Golang 中的 Panic 机制以及其合适的运用方法；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 10. <a href="#10_recover"><font color="blue"><b>Recover 捕获异常</b></font></a>：介绍了 Golang 中如何在 defer 调用的函数中使用 Recover 来捕获 panic 避免程序崩溃；
  </div>
</div>

<h2><a name="0_preface">0. 前言</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数可以让我们将一个语句序列打包为一个单元，然后可以从程序中其它地方多次调用。函数的机制可以让我们将一个大的工作分解为小的任务，这样的小任务可以让不同程序员在不同时间、不同地方独立完成。一个函数同时对用户隐藏了其实现细节。由于这些因素，对于任何编程语言来说，函数都是一个至关重要的部分。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们已经见过许多函数了。现在，让我们多花一点时间来彻底地讨论函数特性。本章的运行示例是一个网络爬虫，也就是 web 搜索引擎中负责抓取网页部分的组件，它们根据抓取网页中的链接继续抓取链接指向的页面。一个网络爬虫的例子给我们足够的机会去探索递归函数、匿名函数、错误处理和函数其它的很多特性。
</div>

<!--标题-->
<h2><a name="1_decoration">1. 函数声明</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数声明包括函数名、形式参数列表、返回值列表（可省略）以及函数体。
  
  ```golang
  func name(parameter-list) (result-list) {
    body
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;形式参数列表描述了函数的参数名以及参数类型。这些参数作为局部变量，其值由参数调用者提供。返回值列表描述了函数返回值的变量名以及类型。如果函数返回一个无名变量或者没有返回值，返回值列表的括号是可以省略的。<font color="red">如果一个函数声明不包括返回值列表，那么函数体执行完毕后，不会返回任何值。</font>在 hypot 函数中：

  ```golang
  func hypot(x, y float64) float64 {
    return math.Sqrt(x*x + y*y) 
  }
  fmt.Println(hypot(3,4)) // "5"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;x 和 y 是形参名，3 和 4 是调用时的传入的实参，函数返回了一个 float64 类型的值。 <font color="red">返回值也可以像形式参数一样被命名。在这种情况下，每个返回值被声明成一个局部变量，并根据该返回值的类型，将其初始化为该类型的零值</font> (注意，如果没有这样声明返回值的名称的话，也就是只简单描述了函数返回值的类型的话，函数只是返回一个值，并不会返回一个新变量。事实上后者才是常态)。如果一个函数在声明时，包含返回值列表，该函数必须以 return 语句结尾，除非函数明显无法运行到结尾处。例如函数在结尾时调用了 panic 异常或函数中存在无限循环。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;正如 hypot 一样，如果一组形参或返回值有相同的类型，我们不必为每个形参都写出参数类型。下面 2 个声明是等价的：

  ```golang
  func f(i, j, k int, s, t string)                 { /* ... */ }
  func f(i int, j int, k int,  s string, t string) { /* ... */ }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面，我们给出 4 种方法声明拥有 2 个 int 型参数和 1 个 int 型返回值的函数。blank identifier(译者注：即下文的_符号) 可以强调某个参数未被使用。

  ```golang
  func add(x int, y int) int   {return x + y}
  func sub(x, y int) (z int)   { z = x - y; return}
  func first(x int, _ int) int { return x }
  func zero(int, int) int      { return 0 }

  fmt.Printf("%T\n", add)   // "func(int, int) int"
  fmt.Printf("%T\n", sub)   // "func(int, int) int"
  fmt.Printf("%T\n", first) // "func(int, int) int"
  fmt.Printf("%T\n", zero)  // "func(int, int) int"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数的类型被称为<b>函数的签名</b>。如果两个函数形式 <b><font color="red">参数列表</font></b> 和 <b><font color="red">返回值列表</font></b> 中的变量类型一一对应，那么这两个函数被认为有相同的类型或签名。形参和返回值的变量名不影响函数签名，也不影响它们是否可以以省略参数类型的形式表示。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;每一次函数调用都必须按照声明顺序为所有参数提供实参（参数值）。在函数调用时，Go 语言没有默认参数值，也没有任何方法可以通过参数名指定形参，因此形参和返回值的变量名对于函数调用者而言没有意义。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在函数体中，函数的形参作为局部变量，被初始化为调用者提供的值。函数的形参和有名返回值作为函数最外层的局部变量，被存储在相同的词法块中。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>实参通过值的方式传递，因此函数的形参是实参的拷贝。</b>对形参进行修改不会影响实参。但是，如果实参包括引用类型，如指针，slice(切片)、map、function、channel 等类型，实参可能会由于函数的间接引用被修改。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;你可能会偶尔遇到<font color="red">没有函数体的函数声明，这表示该函数不是以 Go 实现的。</font>这样的声明定义了函数签名。

  ```golang
  package math

  func Sin(x float64) float //implemented in assembly language
  ```
</div>

<h2><a name="2_recursive">2. 递归</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数可以是递归的，这意味着函数可以直接或间接的调用自身。对许多问题而言，递归是一种强有力的技术，例如处理递归的数据结构。在之前的文章中，我们通过遍历二叉树来实现简单的插入排序，在本章节，我们再次使用它来处理 HTML 文件。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下文的示例代码使用了非标准包 golang.org/x/net/html ，解析 HTML。golang.org/x/... 目录下存储了一些由Go团队设计、维护，对网络编程、国际化文件处理、移动平台、图像处理、加密解密、开发者工具提供支持的扩展包。未将这些扩展包加入到标准库原因有二，一是部分包仍在开发中，二是对大多数 Go 语言的开发者而言，扩展包提供的功能很少被使用。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;例子中调用 golang.org/x/net/html 的部分 api 如下所示。html.Parse 函数读入一组 bytes 解析后，返回 html.Node 类型的 HTML 页面树状结构根节点。HTML 拥有很多类型的结点如 text（文本）、commnets（注释）类型，在下面的例子中，我们只关注 < name key='value' > 形式的结点。

  ```golang
  package html

  type Node struct {
    Type                    NodeType
    Data                    string
    Attr                    []Attribute
    FirstChild, NextSibling *Node
  }

  type NodeType int32

  const (
    ErrorNode NodeType = iota
    TextNode
    DocumentNode
    ElementNode
    CommentNode
    DoctypeNode
  )

  type Attribute struct {
    Key, Val string
  }

  func Parse(r io.Reader) (*Node, error)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;main 函数解析 HTML 标准输入，通过递归函数 visit 获得 links（链接），并打印出这些 links：

  ```golang
  // Findlinks1 prints the links in an HTML document read from standard input.
  package main

  import (
    "fmt"
    "os"

    "golang.org/x/net/html"
  )

  func main() {
    doc, err := html.Parse(os.Stdin)
    if err != nil {
      fmt.Fprintf(os.Stderr, "findlinks1: %v\n", err)
      os.Exit(1)
    }
    for _, link := range visit(nil, doc) {
      fmt.Println(link)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;visit 函数遍历 HTML 的节点树，从每一个 anchor 元素 (i.e. HTML 中的 a) 的 href 属性获得 link，将这些 links 存入字符串数组中，并返回这个字符串数组。为了遍历结点 n 的所有后代结点，每次遇到 n 的孩子结点时，visit 递归的调用自身。这些孩子结点存放在 FirstChild 链表中。

  ```golang
  // visit appends to links each link found in n and returns the result.
  func visit(links []string, n *html.Node) []string {
    // 先分析自己
    if n.Type == html.ElementNode && n.Data == "a" {
      for _, a := range n.Attr {
        if a.Key == "href" {
          links = append(links, a.Val)
        }
      }
    }

    // 递归调用 visit 分析自己下面的元素 (child)
    for c := n.FirstChild; c != nil; c = c.NextSibling {
      links = visit(links, c)
    }
    return links
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;让我们以 Go 的主页（golang.org）作为目标，运行上面的程序。我们以 fetch 的输出作为我们程序的输入，fetch 是一个拉取指定 url 静态资源的程序。下面的输出做了简化处理。

  ```bash
  $ go build gopl.io/ch1/fetch
  $ go build gopl.io/ch5/findlinks1
  $ ./fetch https://golang.org | ./findlinks1
  #
  /doc/
  /pkg/
  /help/
  /blog/
  http://play.golang.org/
  //tour.golang.org/
  https://golang.org/dl/
  //blog.golang.org/
  /LICENSE
  /doc/tos.html
  http://www.google.com/intl/en/policies/privacy/
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;注意在页面中出现的链接格式，在之后我们会介绍如何将这些链接，根据根路径（ https://golang.org ）生成可以直接访问的 url。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在函数 outline 中，我们通过递归的方式遍历整个 HTML 结点树，并输出树的结构。在 outline 内部，每遇到一个 HTML 元素标签，就将其入栈，并输出。

  ```golang
  func main() {
    doc, err := html.Parse(os.Stdin)
    if err != nil {
      fmt.Fprintf(os.Stderr, "outline: %v\n", err)
      os.Exit(1)
    }
    // 这里把 stack ([]string) 赋值为了 nil，
    // 记住形参是实参的拷贝，这里把 nil 赋值给了 stack []string，
    // 即创建了一个值为 nil 的 string slice 的临时变量
    // 一个 nil 值的 slice 的行为和其它任意 0 长度的 slice 一样的 (Chapter 3)
    outline(nil, doc)
  }

  func outline(stack []string, n *html.Node) {
    if n.Type == html.ElementNode {
      stack = append(stack, n.Data) // push tag
      fmt.Println(stack)
    }
    for c := n.FirstChild; c != nil; c = c.NextSibling {
      outline(stack, c)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;有一点值得注意：outline 有入栈操作，但没有相对应的出栈操作。当 outline 调用自身时，被调用者接收的是 stack 的拷贝。被调用者对 stack 的元素追加操作，修改的是 stack 的拷贝，其可能会修改 slice 底层的数组甚至是申请一块新的内存空间进行扩容；但这个过程并不会修改调用方的 stack。因此当函数返回时，调用方的 stack 与其调用自身之前完全一致。


  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面是 https://golang.org 页面的简要结构:

  ```bash
  $ go build gopl.io/ch5/outline
  $ ./fetch https://golang.org | ./outline
  [html]
  [html head]
  [html head meta]
  [html head title]
  [html head link]
  [html body]
  [html body div]
  [html body div]
  [html body div div]
  [html body div div form]
  [html body div div form div]
  [html body div div form div a]
  ...
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;正如你在上面实验中所见，大部分 HTML 页面只需几层递归就能被处理，但仍然有些页面需要深层次的递归。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;大部分编程语言使用固定大小的函数调用栈，常见的大小从 64KB 到 2MB 不等。固定大小栈会限制递归的深度，当你用递归处理大量数据时，需要避免栈溢出；除此之外，还会导致安全性问题。与此相反，<b>Go语言使用可变栈，栈的大小按需增加（初始时很小）。这使得我们使用递归时不必考虑溢出和安全问题。</b>
</div>

<h2><a name="3_multi_return">3. 多返回值</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 Go 中，一个函数可以返回多个值。我们已经在之前例子中看到，许多标准库中的函数返回 2 个值，一个是期望得到的返回值，另一个是函数出错时的错误信息。下面的例子会展示如何编写多返回值的函数。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的程序是上面用于发现 HTML 网页中包含的 url 的程序 findlinks 的改进版本。修改后的 findlinks 可以自己发起 HTTP 请求，这样我们就不必再运行 fetch。因为 HTTP 请求和解析操作可能会失败，因此 findlinks 声明了2个返回值：链接列表和错误信息。一般而言，HTML 的解析器可以处理 HTML 页面的错误结点，构造出 HTML 页面结构，所以解析 HTML 很少失败。这意味着如果 findlinks 函数失败了，很可能是由于I/O的错误导致的。
  
  ```golang
  func main() {
    for _, url := range os.Args[1:] {
      links, err := findLinks(url)
      if err != nil {
        fmt.Fprintf(os.Stderr, "findlinks2: %v\n", err)
        continue
      }
      for _, link := range links {
        fmt.Println(link)
      }
    }
  }

  // findLinks performs an HTTP GET request for url, parses the
  // response as HTML, and extracts and returns the links.
  func findLinks(url string) ([]string, error) {
    resp, err := http.Get(url)
    if err != nil {
      return nil, err
    }
    if resp.StatusCode != http.StatusOK {
      resp.Body.Close()
      return nil, fmt.Errorf("getting %s: %s", url, resp.Status)
    }
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
      return nil, fmt.Errorf("parsing %s as HTML: %v", url, err)
    }
    return visit(nil, doc), nil
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 findlinks 中，有4处 return 语句，每一处 return 都返回了一组值。前三处 return，将 http 和 html 包中的错误信息传递给 findlinks 的调用者。第一处 return 直接返回错误信息，其他两处通过 fmt.Errorf 输出详细的错误信息。如果 findlinks 成功结束，最后的 return 语句将一组解析获得的连接返回给用户。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 findlinks 中，我们必须确保 resp.Body 被关闭，释放网络资源。<font color="red">虽然 Go 的垃圾回收机制会回收不被使用的内存，但是这不包括操作系统层面的资源</font>，比如打开的文件、网络连接。因此我们必须显式的释放这些资源。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;调用多返回值函数时，返回给调用者的是一组值，调用者必须显式的将这些值分配给变量:
  
  ```golang
  links, err := findLinks(url)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果某个值不被使用，可以将其分配给 blank identifier:

  ```golang
  links, _ := findLinks(url) // errors ignored
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一个函数内部可以将另一个有多返回值的函数调用作为返回值，下面的例子展示了与 findLinks 有相同功能的函数，两者的区别在于下面的例子先输出参数：

  ```golang
  func findLinksLog(url string) ([]string, error) {
    log.Printf("findLinks %s", url)
    return findLinks(url)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当你调用接受多参数的函数时，可以将一个返回多参数的函数调用作为该函数的参数。虽然这很少出现在实际生产代码中，但这个特性在 debug 时很方便，我们只需要一条语句就可以输出所有的返回值。下面的代码是等价的：

  ```golang
  // version 1
  log.Println(findLinks(url))

  // version 2
  links, err := findLinks(url)
  log.Println(links, err)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;准确的变量名可以传达函数返回值的含义。尤其在返回值的类型都相同时，就像下面这样：

  ```golang
  func Size(rect image.Rectangle) (width, height int)
  func Split(path string) (dir, file string)
  func HourMinSec(t time.Time) (hour, minute, second int)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;虽然良好的命名很重要，但你也不必为每一个返回值都取一个适当的名字。比如，按照惯例，函数的最后一个 bool 类型的返回值表示函数是否运行成功，error 类型的返回值代表函数的错误信息，对于这些类似的惯例，我们不必思考合适的命名，它们都无需解释。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">如果一个函数所有的返回值都有显式的变量名，那么该函数的 return 语句可以省略操作数。这称之为 bare return。</font>

  ```golang
  // CountWordsAndImages does an HTTP GET request for the HTML
  // document url and returns the number of words and images in it.
  func CountWordsAndImages(url string) (words, images int, err error) {
    resp, err := http.Get(url)
    if err != nil {
      return
    }
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
      err = fmt.Errorf("parsing HTML: %s", err)
      return
    }
    words, images = countWordsAndImages(doc)
    return
  }
  func countWordsAndImages(n *html.Node) (words, images int) { /* ... */ }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;按照返回值列表的次序，返回所有的返回值，在上面的例子中，每一个 return 语句等价于：
  
  ```golang
  return words, images, err
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当一个函数有多处 return 语句以及许多返回值时，bare return 可以减少代码的重复，但是使得代码难以被理解。举个例子，如果你没有仔细的审查代码，很难发现前 2 处 return 等价于  return 0,0,err（ Go 会将返回值 words 和 images在函数体的开始处，根据它们的类型，将其初始化为0），最后一处 return 等价于 return words, image, nil。基于以上原因，不宜过度使用 bare return。
</div>

<h2><a name="4_error">4. 错误</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 Go 中有一部分函数总是能成功的运行。比如 strings.Contains 和 strconv.FormatBool 函数，对各种可能的输入都做了良好的处理，使得运行时几乎不会失败，除非遇到灾难性的、不可预料的情况，比如运行时的内存溢出。导致这种错误的原因很复杂，难以处理，从错误中恢复的可能性也很低。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;还有一部分函数只要输入的参数满足一定条件，也能保证运行成功。比如 time.Date 函数，该函数将年月日等参数构造成 time.Time 对象，除非最后一个参数（时区）是 nil。这种情况下会引发 panic 异常。<font color="red">panic 是来自被调用函数的信号，表示发生了某个已知的bug。</font>一个良好的程序永远不应该发生panic异常。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;对于大部分函数而言，永远无法确保能否成功运行。这是因为错误的原因超出了程序员的控制。举个例子，任何进行 I/O 操作的函数都会面临出现错误的可能，只有没有经验的程序员才会相信读写操作不会失败，即使是简单的读写。因此，当本该可信的操作出乎意料的失败后，我们必须弄清楚导致失败的原因。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在Go的错误处理中，错误是软件包 API 和应用程序用户界面的一个重要组成部分，程序运行失败仅被认为是几个预期的结果之一。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;对于那些将运行失败看作是预期结果的函数，它们会返回一个额外的返回值，通常是最后一个，来传递错误信息。如果导致失败的原因只有一个，额外的返回值可以是一个布尔值，通常被命名为 ok。比如，cache.Lookup 失败的唯一原因是 key 不存在，那么代码可以按照下面的方式组织：

  ```golang
  value, ok := cache.Lookup(key)
  if !ok {
    // ...cache[key] does not exist…
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通常，导致失败的原因不止一种，尤其是对 I/O 操作而言，用户需要了解更多的错误信息。因此，额外的返回值不再是简单的布尔类型，而是 <b>error 类型</b>。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>内置的 error 是接口类型</b>。我们将在第 6 章了解接口类型的含义，以及它对错误处理的影响。现在我们只需要明白 error 类型可能是 nil 或者 non-nil。nil意味着函数运行成功，non-nil表示失败。对于non-nil的error类型，我们可以通过调用error的Error函数或者输出函数获得字符串类型的错误信息。

  ```golang
  fmt.Println(err)
  fmt.Printf("%v", err)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通常，当函数返回 non-nil 的 error 时，其他的返回值是未定义的（undefined），这些未定义的返回值应该被忽略。然而，有少部分函数在发生错误时，仍然会返回一些有用的返回值。比如，当读取文件发生错误时，Read 函数会返回可以读取的字节数以及错误信息。对于这种情况，正确的处理方式应该是先处理这些不完整的数据，再处理错误。因此对函数的返回值要有清晰的说明，以便于其他人使用。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 Go 中，函数运行失败时会返回错误信息，<b>这些<font color="red">错误信息</font>被认为是一种预期的值而非<font color="red">异常（exception）</font></b>，这使得 Go 有别于那些将函数运行失败看作是异常的语言。虽然 Go 有各种异常机制，但这些机制仅被使用在处理那些未被预料到的错误，即 bug，而不是那些在健壮程序中应该被避免的程序错误。对于 Go 的异常机制我们将在后面进行介绍。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Go 这样设计的原因是由于对于某个应该在控制流程中处理的错误而言，将这个错误以异常的形式抛出会混乱对错误的描述，这通常会导致一些糟糕的后果。当某个程序错误被当作异常处理后，这个错误会将堆栈跟踪信息返回给终端用户，这些信息复杂且无用，无法帮助定位错误。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;正因此，Go 使用控制流机制（如 if 和 return）处理错误，这使得编码人员能更多的关注错误处理。

  <h3><a name="4_error_1">4.1 错误处理</a></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当一次函数调用返回错误时，调用者应该选择合适的方式处理错误。根据情况的不同，有很多处理方式，让我们来看看常用的五种方式。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;首先，也是最常用的方式是<b>传播错误</b>。这意味着函数中某个子程序的失败，会变成该函数的失败。下面，我们以前文所编写的 findLinks 函数作为例子。如果 findLinks 对 http.Get 的调用失败，findLinks 会直接将这个 HTTP 错误返回给调用者：

  ```golang
  resp, err := http.Get(url)
  if err != nil{
    return nil, err
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当对 html.Parse 的调用失败时，findLinks 不会直接返回 html.Parse 的错误，因为缺少两条重要信息：(1) 发生错误时的解析器（html parser）；(2) 发生错误的 url。因此，findLinks 构造了一个新的错误信息，既包含了这两项，也包括了底层的解析出错的信息。

  ```golang
  doc, err := html.Parse(resp.Body)
  resp.Body.Close()
  if err != nil {
    return nil, fmt.Errorf("parsing %s as HTML: %v", url, err)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;fmt.Errorf 函数使用 fmt.Sprintf 格式化错误信息并返回。我们使用该函数添加额外的前缀上下文信息到原始错误信息。当错误最终由 main 函数处理时，错误信息应提供清晰的从原因到后果的因果链，就像美国宇航局事故调查时做的那样：

  ```bash
  genesis: crashed: no parachute: G-switch failed: bad relay orientation
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;由于错误信息经常是以链式组合在一起的，所以错误信息中应避免大写和换行符。最终的错误信息可能很长，我们可以通过类似 grep 的工具处理错误信息（译者注：grep 是一种文本搜索工具）。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;编写错误信息时，我们要确保错误信息对问题细节的描述是详尽的。尤其是要注意错误信息表达的一致性，即相同的函数或同包内的同一组函数返回的错误在构成和处理方式上是相似的。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;以 os 包为例，os 包确保文件操作（如 os.Open、Read、Write、Close）返回的每个错误的描述不仅仅包含错误的原因（如无权限，文件目录不存在）也包含文件名，这样调用者在构造新的错误信息时无需再添加这些信息。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一般而言，被调用函数 f(x) 会将调用信息和参数信息作为发生错误时的上下文放在错误信息中并返回给调用者，调用者需要添加一些错误信息中不包含的信息，比如添加 url 到 html.Parse 返回的错误中。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;让我们来看看处理错误的第二种策略。如果错误的发生是偶然性的，或由不可预知的问题导致的。一个明智的选择是<b>重新尝试失败的操作</b>。在重试时，我们需要限制重试的时间间隔或重试的次数，防止无限制的重试。

  ```golang
  func WaitForServer(url string) error {
    const timeout = 1 * time.Minute
    deadline := time.Now().Add(timeout)
    for tries := 0; time.Now().Before(deadline); tries++ {
      _, err := http.Head(url)
      if err == nil {
        return nil // success
      }
      log.Printf("server not responding (%s);retrying…", err)
      time.Sleep(time.Second << uint(tries)) // exponential back-off
    }
    return fmt.Errorf("server %s failed to respond after %s", url, timeout)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果错误发生后，程序无法继续运行，我们就可以采用第三种策略：<b>输出错误信息并结束程序</b>。需要注意的是，<font color="red">这种策略只应在 main 中执行</font>。对库函数而言，应仅向上传播错误，除非该错误意味着程序内部包含不一致性，即遇到了 bug，才能在库函数中结束程序。

  ```golang
  if err := WaitForServer(url); err != nil {
    fmt.Fprintf(os.Stderr, "Site is down: %v\n", err)
    os.Exit(1)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;调用 log.Fatalf 可以实现用更简洁的代码达到与上文相同的效果。log 中的所有函数，都默认会在错误信息之前输出时间信息。

  ```golang
  if err := WaitForServer(url); err != nil {
    log.Fatalf("Site is down: %v\n", err)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;长时间运行的服务器常采用默认的时间格式，而交互式工具很少采用包含如此多信息的格式。

  ```bash
  2006/01/02 15:04:05 Site is down: no such domain:
  bad.gopl.io
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们可以设置log的前缀信息屏蔽时间信息，一般而言，前缀信息会被设置成命令名。

  ```golang
  log.SetPrefix("wait: ")
  log.SetFlags(0)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;第四种策略：有时，我们只需要<b>输出错误信息就足够了，不需要中断程序的运行</b>。我们可以通过 log 包提供函数

  ```golang
  if err := Ping(); err != nil {
    log.Printf("ping failed: %v; networking disabled",err)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;或者标准错误流输出错误信息。

  ```golang
  if err := Ping(); err != nil {
    fmt.Fprintf(os.Stderr, "ping failed: %v; networking disabled\n", err)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;log 包中的所有函数会为没有换行符的字符串增加换行符。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;第五种，也是最后一种策略：我们可以直接忽略掉错误。

  ```golang
  dir, err := ioutil.TempDir("", "scratch")
  if err != nil {
    return fmt.Errorf("failed to create temp dir: %v",err)
  }
  // ...use temp dir…
  os.RemoveAll(dir) // ignore errors; $TMPDIR is cleaned periodically
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;尽管 os.RemoveAll 会失败，但上面的例子并没有做错误处理。这是因为操作系统会定期的清理临时目录。正因如此，虽然程序没有处理错误，但程序的逻辑不会因此受到影响。我们应该在每次函数调用后，都养成考虑错误处理的习惯，当你决定忽略某个错误时，你应该清晰地写下你的意图。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 Go 中，错误处理有一套独特的编码风格。检查某个子函数是否失败后，我们通常将处理失败的逻辑代码放在处理成功的代码之前。如果某个错误会导致函数返回，那么成功时的逻辑代码不应放在else 语句块中，而应直接放在函数体中。Go 中大部分函数的代码结构几乎相同，首先是一系列的初始检查，防止错误发生，之后是函数的实际逻辑。

  <h3><a name="4_error_2">4.2 文件结尾错误（EOF）</a></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数经常会返回多种错误，这对终端用户来说可能会很有趣，但对程序而言，这使得情况变得复杂。很多时候，程序必须根据错误类型，作出不同的响应。让我们考虑这样一个例子：从文件中读取 n 个字节。如果 n 等于文件的长度，读取过程的任何错误都表示失败。如果 n 小于文件的长度，调用者会重复的读取固定大小的数据直到文件结束。这会导致调用者必须分别处理由文件结束引起的各种错误。基于这样的原因，io 包保证任何由文件结束引起的读取失败都返回同一个错误 —— io.EOF，该错误在 io 包中定义：

  ```golang
  package io

  import "errors"

  // EOF is the error returned by Read when no more input is available.
  var EOF = errors.New("EOF")
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;调用者只需通过简单的比较，就可以检测出这个错误。下面的例子展示了如何从标准输入中读取字符，以及判断文件结束。

  ```golang
  in := bufio.NewReader(os.Stdin)
  for {
    r, _, err := in.ReadRune()
    if err == io.EOF {
      break // finished reading
    }
    if err != nil {
      return fmt.Errorf("read failed:%v", err)
    }
    // ...use r…
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;因为文件结束这种错误不需要更多的描述，所以 io.EOF 有固定的错误信息 —— “EOF”。对于其他错误，我们可能需要在错误信息中描述错误的类型和数量，这使得我们不能像 io.EOF 一样采用固定的错误信息。在第 6 章中，我们会提出更系统的方法区分某些固定的错误值。
</div>

<h2><a name="5_value">5. 函数值</a></h2>
<div class="div_learning_post_boder">
  <h3><a name="5_value"></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 Go 中，函数被看作<b>第一类值（first-class values）</b>：<font color="red">函数像其他值一样，拥有类型，可以被赋值给其他变量，传递给函数，从函数返回。</font>对函数值（function value）的调用类似函数调用。例子如下：
  
  ```golang
  func square(n int) int { return n * n }
	func negative(n int) int { return -n }
	func product(m, n int) int { return m * n }

	f := square
	fmt.Println(f(3)) // "9"

	f = negative
	fmt.Println(f(3))     // "-3"
	fmt.Printf("%T\n", f) // "func(int) int"

	f = product // compile error: can't assign func(int, int) int to func(int) int
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数类型的零值是 nil。调用值为 nil 的函数值会引起 panic 错误：

  ```golang
  var f func(int) int
	f(3) // 此处f的值为nil, 会引起panic错误
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数值可以与 nil 比较：

  ```golang
  var f func(int) int
	if f != nil {
		f(3)
	}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;但是函数值之间是不可比较的，也不能用函数值作为 map 的 key。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数值使得我们不仅仅可以通过数据来参数化函数，亦可通过行为。标准库中包含许多这样的例子。下面的代码展示了如何使用这个技巧，strings.Map 对字符串中的每个字符调用 add1 函数，并将每个 add1 函数的返回值组成一个新的字符串返回给调用者。

  ```golang
  func add1(r rune) rune { return r + 1 }

	fmt.Println(strings.Map(add1, "HAL-9000")) // "IBM.:111"
	fmt.Println(strings.Map(add1, "VMS"))      // "WNT"
	fmt.Println(strings.Map(add1, "Admix"))    // "Benjy"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;上一章中我们使用了辅助函数 visit，遍历和操作了 HTML 页面的所有结点。使用函数值，我们可以将遍历结点的逻辑和操作结点的逻辑分离，使得我们可以复用遍历的逻辑，从而对结点进行不同的操作。

  ```golang
  // forEachNode针对每个结点x，都会调用pre(x)和post(x)。
  // pre和post都是可选的。
  // 遍历孩子结点之前，pre被调用
  // 遍历孩子结点之后，post被调用
  func forEachNode(n *html.Node, pre, post func(n *html.Node)) {
    if pre != nil {
      pre(n)
    }
    for c := n.FirstChild; c != nil; c = c.NextSibling {
      forEachNode(c, pre, post)
    }
    if post != nil {
      post(n)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;该函数接收 2 个函数作为参数，分别在结点的孩子被访问前和访问后调用。这样的设计给调用者更大的灵活性。举个例子，现在我们有 startElement 和 endElement 两个函数用于输出 HTML 元素的开始标签和结束标签：
  <div align="center">
    <xmp><b>...</b></xmp>
  </div>

  ```golang
  var depth int
  func startElement(n *html.Node) {
    if n.Type == html.ElementNode {
      fmt.Printf("%*s<%s>\n", depth*2, "", n.Data)
      depth++
    }
  }
  func endElement(n *html.Node) {
    if n.Type == html.ElementNode {
      depth--
      fmt.Printf("%*s</%s>\n", depth*2, "", n.Data)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;上面的代码利用 fmt.Printf 的一个小技巧控制输出的缩进。%*s 中的 * 会在字符串之前填充一些空格。在例子中，每次输出会先填充 depth*2 数量的空格，再输出 ""，最后再输出 HTML 标签。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果我们像下面这样调用 forEachNode：

  ```golang
  forEachNode(doc, startElement, endElement)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;与之前的 outline 程序相比，我们得到了更加详细的页面结构：

  ```bash
  $ go build gopl.io/ch5/outline2
  $ ./outline2 http://gopl.io
  <html>
    <head>
      <meta>
      </meta>
      <title>
      </title>
      <style>
      </style>
    </head>
    <body>
      <table>
        <tbody>
          <tr>
            <td>
              <a>
                <img>
                </img>
  ...
  ```
</div>

<h2><a name="6_anonymous">6. 匿名函数</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;拥有函数名的函数只能在包级语法块中被声明，通过函数字面量（function literal），我们可绕过这一限制，<font color="red">在任何表达式中表示一个函数值</font>。函数字面量的语法和函数声明相似，区别在于 func 关键字后没有函数名。函数值字面量是一种表达式，它的值被称为<b>匿名函数（anonymous function）</b>。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数字面量允许我们在使用函数时再定义它。通过这种技巧，我们可以改写之前对 strings.Map 的调用：

  ```golang
  strings.Map(func(r rune) rune { return r + 1 }, "HAL-9000")
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;更为重要的是，通过这种方式定义的函数可以访问完整的词法环境（lexical environment），<font color="red">这意味着在函数中定义的内部函数可以引用该函数的变量</font>，如下面这个神奇的例子所示：

  ```golang
  // squares返回一个匿名函数。
  // 该匿名函数每次被调用时都会返回下一个数的平方。
  func squares() func() int {
    var x int
    return func() int {
      x++
      return x * x
    }
  }
  func main() {
    f := squares()
    fmt.Println(f()) // "1"
    fmt.Println(f()) // "4"
    fmt.Println(f()) // "9"
    fmt.Println(f()) // "16"
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;函数 squares 返回另一个类型为 func() int 的函数。对 squares 的一次调用会生成一个局部变量x并返回一个匿名函数。每次调用匿名函数时，该函数都会先使 x 的值加 1，再返回 x 的平方。第二次调用 squares 时，会生成第二个 x 变量，并返回一个新的匿名函数。新匿名函数操作的是第二个 x 变量。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;squares 的例子证明，<b>函数值</b><font color="red">不仅仅是一串代码，还记录了状态。</font>在 squares 中定义的匿名内部函数可以访问和更新 squares 中的局部变量，这意味着匿名函数和 squares 中，<b>存在变量引用</b>。这就是函数值属于引用类型和函数值不可比较的原因。Go 使用<b>闭包（closures）</b>技术实现函数值，Go 程序员也把函数值叫做闭包。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通过这个例子，我们看到<b>变量的生命周期不由它的作用域决定</b>：squares 返回后，变量 x 仍然隐式的存在于 f 中。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;接下来，我们讨论一个有点学术性的例子，考虑这样一个问题：给定一些计算机课程，每个课程都有前置课程，只有完成了前置课程才可以开始当前课程的学习；我们的目标是选择出一组课程，这组课程必须确保按顺序学习时，能全部被完成。每个课程的前置课程如下：

  ```golang
  // prereqs记录了每个课程的前置课程
  var prereqs = map[string][]string{
    "algorithms": {"data structures"},
    "calculus": {"linear algebra"},
    "compilers": {
      "data structures",
      "formal languages",
      "computer organization",
    },
    "data structures":       {"discrete math"},
    "databases":             {"data structures"},
    "discrete math":         {"intro to programming"},
    "formal languages":      {"discrete math"},
    "networks":              {"operating systems"},
    "operating systems":     {"data structures", "computer organization"},
    "programming languages": {"data structures", "computer organization"},
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这类问题被称作拓扑排序。从概念上说，前置条件可以构成有向图。图中的顶点表示课程，边表示课程间的依赖关系。显然，图中应该无环，这也就是说从某点出发的边，最终不会回到该点。下面的代码用深度优先搜索了整张图，获得了符合要求的课程序列。

  ```golang
  func main() {
    for i, course := range topoSort(prereqs) {
      fmt.Printf("%d:\t%s\n", i+1, course)
    }
  }

  // DFS
  func topoSort(m map[string][]string) []string {
    var order []string
    // 是否访问过
    seen := make(map[string]bool)
    var visitAll func(items []string)
    visitAll = func(items []string) {
      for _, item := range items {
        if !seen[item] {
          seen[item] = true
          visitAll(m[item])
          order = append(order, item)
        }
      }
    }
    var keys []string
    for key := range m {
      keys = append(keys, key)
    }
    sort.Strings(keys)
    visitAll(keys)
    return order
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当匿名函数需要被递归调用时，我们必须首先声明一个变量（在上面的例子中，我们首先声明了 visitAll），再将匿名函数赋值给这个变量。如果不分成两步，函数字面量无法与 visitAll 绑定，我们也无法递归调用该匿名函数，如下所示：

  ```golang
  visitAll := func(items []string) {
    // ...
    visitAll(m[item]) // compile error: undefined: visitAll
    // ...
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 toposort 程序的输出如下所示，它的输出顺序是大多人想看到的固定顺序输出，但是这需要我们多花点心思才能做到。哈希表 prepreqs 的 value 是遍历顺序固定的切片，而不再是遍历顺序随机的 map，所以我们对 prereqs 的 key 值进行排序，保证每次运行 toposort 程序，都以相同的遍历顺序遍历 prereqs。

  ```golang
  1: intro to programming
  2: discrete math
  3: data structures
  4: algorithms
  5: linear algebra
  6: calculus
  7: formal languages
  8: computer organization
  9: compilers
  10: databases
  11: operating systems
  12: networks
  13: programming languages
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;让我们回到遍历 HTML 页面找到其下所有 url 的 findLinks 这个例子。我们将函数重命名为 Extract，在第 7 章我们会再次用到这个函数。新的匿名函数被引入，用于替换原来的 visit 函数。该匿名函数负责将新连接添加到切片中。在 Extract 中，使用 forEachNode 遍历 HTML 页面，由于 Extract 只需要在遍历结点前操作结点，所以 forEachNode 的 post 参数被传入 nil。

  ```golang
  // Package links provides a link-extraction function.
  package links
  import (
    "fmt"
    "net/http"
    "golang.org/x/net/html"
  )
  // Extract makes an HTTP GET request to the specified URL, parses
  // the response as HTML, and returns the links in the HTML document.
  func Extract(url string) ([]string, error) {
    resp, err := http.Get(url)
    if err != nil {
      return nil, err
    }
    if resp.StatusCode != http.StatusOK {
    resp.Body.Close()
      return nil, fmt.Errorf("getting %s: %s", url, resp.Status)
    }
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
      return nil, fmt.Errorf("parsing %s as HTML: %v", url, err)
    }
    var links []string
    visitNode := func(n *html.Node) {
      if n.Type == html.ElementNode && n.Data == "a" {
        for _, a := range n.Attr {
          if a.Key != "href" {
            continue
          }
          link, err := resp.Request.URL.Parse(a.Val)
          if err != nil {
            continue // ignore bad URLs
          }
          links = append(links, link.String())
        }
      }
    }
    forEachNode(doc, visitNode, nil)
    return links, nil
  }

  // forEachNode针对每个结点x，都会调用pre(x)和post(x)。
  // pre和post都是可选的。
  // 遍历孩子结点之前，pre被调用
  // 遍历孩子结点之后，post被调用
  func forEachNode(n *html.Node, pre, post func(n *html.Node)) {
    if pre != nil {
      pre(n)
    }
    for c := n.FirstChild; c != nil; c = c.NextSibling {
      forEachNode(c, pre, post)
    }
    if post != nil {
      post(n)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;上面的代码对之前的版本做了改进，现在 links 中存储的不是 href 属性的原始值，而是通过 resp.Request.URL 解析后的值。解析后，这些连接以绝对路径的形式存在，可以直接被 http.Get 访问。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;网页抓取的核心问题就是如何遍历图。在基于各科先修课程找到课程列表的 topoSort 的例子中，已经展示了深度优先遍历，在下面的网页抓取中，我们将展示如何用广度优先遍历图。在第 7 章，我们会介绍如何将深度优先和广度优先结合使用。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的函数实现了广度优先算法。调用者需要输入一个初始的待访问列表和一个函数 f。待访问列表中的每个元素被定义为 string 类型。广度优先算法会为每个元素调用一次 f。每次 f 执行完毕后，会返回一组待访问元素。这些元素会被加入到待访问列表中。当待访问列表中的所有元素都被访问后，breadthFirst 函数运行结束。为了避免同一个元素被访问两次，代码中维护了一个 map。

  ```golang
  // breadthFirst calls f for each item in the worklist.
  // Any items returned by f are added to the worklist.
  // f is called at most once for each item.
  func breadthFirst(f func(item string) []string, worklist []string) {
    seen := make(map[string]bool)
    for len(worklist) > 0 {
      items := worklist
      worklist = nil
      for _, item := range items {
        if !seen[item] {
          seen[item] = true
          worklist = append(worklist, f(item)...)
        }
      }
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;就像我们在第 2 章解释的那样，append 的参数 “f(item)...”，会将 f 返回的一组元素一个个添加到 worklist 中。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在我们网页抓取器中，元素的类型是 url。crawl函数会将 URL 输出，提取其中的新链接，并将这些新链接返回。我们会将 crawl 作为参数传递给 breadthFirst。

  ```golang
  func crawl(url string) []string {
    fmt.Println(url)
    list, err := links.Extract(url)
    if err != nil {
      log.Print(err)
    }
    return list
  }

  // 把 Extract 继续贴过来方便查看
  // Extract makes an HTTP GET request to the specified URL, parses
  // the response as HTML, and returns the links in the HTML document.
  func Extract(url string) ([]string, error) {
    resp, err := http.Get(url)
    if err != nil {
      return nil, err
    }
    if resp.StatusCode != http.StatusOK {
    resp.Body.Close()
      return nil, fmt.Errorf("getting %s: %s", url, resp.Status)
    }
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
      return nil, fmt.Errorf("parsing %s as HTML: %v", url, err)
    }
    var links []string
    visitNode := func(n *html.Node) {
      if n.Type == html.ElementNode && n.Data == "a" {
        for _, a := range n.Attr {
          if a.Key != "href" {
            continue
          }
          link, err := resp.Request.URL.Parse(a.Val)
          if err != nil {
            continue // ignore bad URLs
          }
          links = append(links, link.String())
        }
      }
    }
    forEachNode(doc, visitNode, nil)
    return links, nil
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;为了使抓取器开始运行，我们用命令行输入的参数作为初始的待访问 url。

  ```golang
  func main() {
    // Crawl the web breadth-first,
    // starting from the command-line arguments.
    breadthFirst(crawl, os.Args[1:])
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;让我们从 https://golang.org 开始，下面是程序的输出结果：

  ```bash
  $ go build gopl.io/ch5/findlinks3
  $ ./findlinks3 https://golang.org
  https://golang.org/
  https://golang.org/doc/
  https://golang.org/pkg/
  https://golang.org/project/
  https://code.google.com/p/go-tour/
  https://golang.org/doc/code.html
  https://www.youtube.com/watch?v=XCsL89YtqCs
  http://research.swtch.com/gotour
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当所有发现的链接都已经被访问或电脑的内存耗尽时，程序运行结束。

  <h3><a name="6_anonymous_1">6.1 警告：捕获迭代变量</a></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本节，将介绍 Go 词法作用域的一个陷阱。请务必仔细的阅读，弄清楚发生问题的原因。即使是经验丰富的程序员也会在这个问题上犯错误。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;考虑这样一个问题：你被要求首先创建一些目录，再将目录删除。在下面的例子中我们用函数值来完成删除操作。下面的示例代码需要引入 os 包。为了使代码简单，我们忽略了所有的异常处理。

  ```golang
  var rmdirs []func()
  for _, d := range tempDirs() {
    dir := d // NOTE: necessary!
    os.MkdirAll(dir, 0755) // creates parent directories too
    rmdirs = append(rmdirs, func() {
      os.RemoveAll(dir)
    })
  }
  // ...do some work…
  for _, rmdir := range rmdirs {
    rmdir() // clean up
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;你可能会感到困惑，为什么要在循环体中用循环变量 d 赋值一个新的局部变量，而不是像下面的代码一样直接使用循环变量dir。需要注意，下面的代码是错误的。

  ```golang
  var rmdirs []func()
  for _, dir := range tempDirs() {
    os.MkdirAll(dir, 0755)
    rmdirs = append(rmdirs, func() {
      os.RemoveAll(dir) // NOTE: incorrect!
    })
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;问题的原因在于循环变量的作用域。在上面的程序中，for 循环语句引入了新的词法块，循环变量 dir 在这个词法块中被声明。在该循环中生成的所有函数值都共享相同的循环变量。需要注意，<font color="red">函数值中记录的是循环变量的内存地址，而不是循环变量某一时刻的值。</font>以 dir 为例，后续的迭代会不断更新 dir 的值，当删除操作执行时，for 循环已完成，dir 中存储的值等于最后一次迭代的值。这意味着，每次对 os.RemoveAll 的调用删除的都是相同的目录。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通常，为了解决这个问题，我们会引入一个与循环变量同名的局部变量，作为循环变量的副本。比如下面的变量 dir，虽然这看起来很奇怪，但却很有用。

  ```golang
  for _, dir := range tempDirs() {
    dir := dir // declares inner dir, initialized to outer dir
    // ...
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这个问题不仅存在基于 range 的循环，在下面的例子中，对循环变量 i 的使用也存在同样的问题：

  ```golang
  var rmdirs []func()
  dirs := tempDirs()
  for i := 0; i < len(dirs); i++ {
    os.MkdirAll(dirs[i], 0755) // OK
    rmdirs = append(rmdirs, func() {
      os.RemoveAll(dirs[i]) // NOTE: incorrect!
    })
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果你使用 go 语句（第 7 章）或者defer语句（本章）会经常遇到此类问题。这不是 go 或 defer 本身导致的，而是因为它们都会等待循环结束后，再执行函数值。
</div>



<h2><a name="7_variable_parameter">7. 可变参数</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参数数量可变的函数称为可变参数函数。典型的例子就是 fmt.Printf 和类似函数。Printf 首先接收一个必备的参数，之后接收任意个数的后续参数。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在声明可变参数函数时，需要在参数列表的最后一个参数类型<b>之前</b>加上省略符号 “...”，这表示该函数会接收任意数量的该类型参数。

  ```golang
  func sum(vals ...int) int {
    total := 0
    for _, val := range vals {
      total += val
    }
    return total
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;sum函数返回任意个 int 型参数的和。在函数体中，vals 被看作是类型为 []int 的切片。sum 可以接收任意数量的 int 型参数：

  ```golang
  fmt.Println(sum())           // "0"
  fmt.Println(sum(3))          // "3"
  fmt.Println(sum(1, 2, 3, 4)) // "10"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在上面的代码中，调用者隐式的创建一个数组，并将原始参数复制到数组中，再把数组的一个切片作为参数传给被调用函数。如果原始参数已经是切片类型，我们该如何传递给 sum？只需在最后一个参数<b>后</b>加上省略符。下面的代码功能与上个例子中最后一条语句相同。

  ```golang
  values := []int{1, 2, 3, 4}
  fmt.Println(sum(values...)) // "10"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;虽然在可变参数函数内部，...int 型参数的行为看起来很像切片类型，但实际上，可变参数函数和以切片作为参数的函数是不同的。

  ```golang
  func f(...int) {}
  func g([]int) {}
  fmt.Printf("%T\n", f) // "func(...int)"
  fmt.Printf("%T\n", g) // "func([]int)"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;可变参数函数经常被用于格式化字符串。下面的 errorf 函数构造了一个以行号开头的，经过格式化的错误信息。函数名的后缀 f 是一种通用的命名规范，代表该可变参数函数可以接收 Printf 风格的格式化字符串。

  ```golang
  func errorf(linenum int, format string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, "Line %d: ", linenum)
    fmt.Fprintf(os.Stderr, format, args...)
    fmt.Fprintln(os.Stderr)
  }
  linenum, name := 12, "count"
  errorf(linenum, "undefined: %s", name) // "Line 12: undefined: count"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;interface{} 表示函数的最后一个参数可以接收任意类型，我们会在第 6 章详细介绍。
</div>


<h2><a name="8_deferred">8. Deferred 函数</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在之前在一个 HTML 页面中收集所有的 url 的 findLinks 的例子中，我们用 http.Get 的输出作为 html.Parse 的输入。只有 url 的内容的确是HTML格式的，html.Parse 才可以正常工作，但实际上，url 指向的内容很丰富，可能是图片，纯文本或是其他。将这些格式的内容传递给 html.parse，会产生不良后果。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的例子获取 HTML 页面并输出页面的标题。title 函数会检查服务器返回的 Content-Type 字段，如果发现页面不是 HTML，将终止函数运行，返回错误。
  
  ```golang
  func title(url string) error {
    resp, err := http.Get(url)
    if err != nil {
      return err
    }
    // Check Content-Type is HTML (e.g., "text/html;charset=utf-8").
    ct := resp.Header.Get("Content-Type")
    if ct != "text/html" && !strings.HasPrefix(ct,"text/html;") {
      resp.Body.Close()
      return fmt.Errorf("%s has type %s, not text/html",url, ct)
    }
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
      return fmt.Errorf("parsing %s as HTML: %v", url,err)
    }
    visitNode := func(n *html.Node) {
      if n.Type == html.ElementNode && n.Data == "title"&&n.FirstChild != nil {
        fmt.Println(n.FirstChild.Data)
      }
    }
    forEachNode(doc, visitNode, nil)
    return nil
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面展示了运行效果：

  ```bash
  $ go build gopl.io/ch5/title1
  $ ./title1 http://gopl.io
  The Go Programming Language
  $ ./title1 https://golang.org/doc/effective_go.html
  Effective Go - The Go Programming Language
  $ ./title1 https://golang.org/doc/gopher/frontpage.png
  title1: https://golang.org/doc/gopher/frontpage.png has type image/png, not text/html
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;resp.Body.close 调用了多次，这是为了确保 title 在所有执行路径下（即使函数运行失败）都关闭了网络连接。随着函数变得复杂，需要处理的错误也变多，维护清理逻辑变得越来越困难。而 Go 语言独有的 <b>defer 机制</b>可以让事情变得简单。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;你只需要在调用普通函数或方法前加上关键字 defer，就完成了 defer 所需要的语法。当执行到该条语句时，函数和参数表达式得到计算，但<font color="red">直到包含该 defer 语句的函数执行完毕时，defer后的函数才会被执行，不论包含defer语句的函数是通过return正常结束，还是由于panic导致的异常结束</font>。你可以在一个函数中执行多条defer语句，<b>它们的执行顺序与声明顺序相反。</b>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;defer 语句经常被用于处理成对的操作，如打开、关闭、连接、断开连接、加锁、释放锁。通过 defer 机制，不论函数逻辑多复杂，都能保证在任何执行路径下，资源被释放。释放资源的 defer 应该直接跟在请求资源的语句后。在下面的代码中，一条 defer 语句替代了之前的所有 resp.Body.Close

  ```golang
  func title(url string) error {
    resp, err := http.Get(url)
    if err != nil {
      return err
    }
    defer resp.Body.Close()
    ct := resp.Header.Get("Content-Type")
    if ct != "text/html" && !strings.HasPrefix(ct,"text/html;") {
      return fmt.Errorf("%s has type %s, not text/html",url, ct)
    }
    doc, err := html.Parse(resp.Body)
    if err != nil {
      return fmt.Errorf("parsing %s as HTML: %v", url,err)
    }
    // ...print doc's title element…
    return nil
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在处理其他资源时，也可以采用defer机制，比如对文件的操作：

  ```golang
  package ioutil
  func ReadFile(filename string) ([]byte, error) {
    f, err := os.Open(filename)
    if err != nil {
      return nil, err
    }
    defer f.Close()
    return ReadAll(f)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;或是处理互斥锁（第 8 章）

  ```golang
  var mu sync.Mutex
  var m = make(map[string]int)
  func lookup(key string) int {
    mu.Lock()
    defer mu.Unlock()
    return m[key]
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">【妙阿】</font>调试复杂程序时，defer 机制也常被用于记录何时进入和退出函数。下例中的 bigSlowOperation 函数，直接调用 trace 记录函数的被调情况。bigSlowOperation 被调时，trace 会返回一个函数值，该函数值会在 bigSlowOperation 退出时被调用。通过这种方式， 我们可以只通过一条语句控制函数的入口和所有的出口，甚至可以记录函数的运行时间，如例子中的 start。需要注意一点：不要忘记 defer 语句后的圆括号，否则本该在进入时执行的操作会在退出时执行，而本该在退出时执行的，永远不会被执行。

  ```golang
  func bigSlowOperation() {
    defer trace("bigSlowOperation")() // don't forget the extra parentheses
    // ...lots of work…
    time.Sleep(10 * time.Second) // simulate slow operation by sleeping
  }
  func trace(msg string) func() {
    start := time.Now()
    log.Printf("enter %s", msg)
    return func() { 
      log.Printf("exit %s (%s)", msg,time.Since(start)) 
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;每一次 bigSlowOperation 被调用，程序都会记录函数的进入，退出，持续时间。（我们用 time.Sleep 模拟一个耗时的操作）

  ```bash
  $ go build gopl.io/ch5/trace
  $ ./trace
  2015/11/18 09:53:26 enter bigSlowOperation
  2015/11/18 09:53:36 exit bigSlowOperation (10.000589217s)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们知道，<b><font color="red">defer 语句中的函数会在 return 语句更新返回值变量后再执行</font></b>，又因为在函数中定义的匿名函数可以访问该函数包括返回值变量在内的所有变量，所以，对匿名函数采用 defer 机制，可以使其观察函数的返回值。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;以 double 函数为例：

  ```golang
  func double(x int) int {
    return x + x
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们只需要首先命名 double 的返回值，再增加 defer 语句，我们就可以在 double 每次被调用时，输出参数以及返回值。

  ```golang
  func double(x int) (result int) {
    defer func() { fmt.Printf("double(%d) = %d\n", x, result) }()
    return x + x
  }
  _ = double(4)
  // Output:
  // "double(4) = 8"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;可能 double 函数过于简单，看不出这个小技巧的作用，但对于有许多 return 语句的函数而言，这个技巧很有用。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b><font color="red">被延迟执行的匿名函数甚至可以修改函数返回给调用者的返回值</font></b>：

  ```golang
  func triple(x int) (result int) {
    defer func() { result += x }()
    return double(x)
  }
  fmt.Println(triple(4)) // "12"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在循环体中的 defer 语句需要特别注意，因为只有在函数执行完毕后，这些被延迟的函数才会执行。下面的代码会导致系统的文件描述符耗尽，因为在所有文件都被处理之前，没有文件会被关闭。

  ```golang
  for _, filename := range filenames {
    f, err := os.Open(filename)
    if err != nil {
      return err
    }
    defer f.Close() // NOTE: risky; could run out of file descriptors
    // ...process f…
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一种解决方法是将循环体中的 defer 语句移至另外一个函数。在每次循环时，调用这个函数。

  ```golang
  for _, filename := range filenames {
    if err := doFile(filename); err != nil {
      return err
    }
  }
  func doFile(filename string) error {
    f, err := os.Open(filename)
    if err != nil {
      return err
    }
    defer f.Close()
    // ...process f…
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的代码是用于获取某个 url 下的 HTML 文件的 fetch 程序的改进版，我们将 http 响应信息写入本地文件而不是从标准输出流输出。我们通过 path.Base 提出 url 路径的最后一段作为文件名。

  ```golang
  // Fetch downloads the URL and returns the
  // name and length of the local file.
  func fetch(url string) (filename string, n int64, err error) {
    resp, err := http.Get(url)
    if err != nil {
      return "", 0, err
    }
    defer resp.Body.Close()
    local := path.Base(resp.Request.URL.Path)
    if local == "/" {
      local = "index.html"
    }
    f, err := os.Create(local)
    if err != nil {
      return "", 0, err
    }
    n, err = io.Copy(f, resp.Body)
    // Close file, but prefer error from Copy, if any.
    if closeErr := f.Close(); err == nil {
      err = closeErr
    }
    return local, n, err
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;对 resp.Body.Close 延迟调用我们已经见过了，在此不做解释。上例中，通过 os.Create 打开文件进行写入，在关闭文件时，我们没有对 f.close 采用 defer 机制，因为这会产生一些微妙的错误。<font color="red"><b>许多文件系统，尤其是 NFS，写入文件时发生的错误会被延迟到文件关闭时反馈。</b></font>如果没有检查文件关闭时的反馈信息，可能会导致数据丢失，而我们还误以为写入操作成功。如果 io.Copy 和 f.close 都失败了，我们倾向于将 io.Copy 的错误信息反馈给调用者，因为它先于 f.close 发生，更有可能接近问题的本质。
</div>


<h2><a name="9_panic">9. Panic</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Go 的类型系统会在编译时捕获很多错误，但有些错误只能在运行时检查，如数组访问越界、空指针引用等。这些运行时错误会引起 <b>painc 异常</b>。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一般而言，当 panic 异常发生时，程序会中断运行，并立即执行在该 goroutine（可以先理解成线程，在第 7 章会详细介绍）中被延迟的函数（defer 机制）。随后，程序崩溃并输出日志信息。日志信息包括 panic value 和函数调用的堆栈跟踪信息。panic value 通常是某种错误信息。对于每个 goroutine，日志信息中都会有与之相对的，发生 panic 时的函数调用堆栈跟踪信息。通常，我们不需要再次运行程序去定位问题，日志信息已经提供了足够的诊断依据。因此，在我们填写问题报告时，一般会将 panic 异常和日志信息一并记录。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;不是所有的 panic 异常都来自运行时，<font color="red">直接调用内置的 panic 函数也会引发 panic 异常</font>；panic 函数接受任何值作为参数。当某些不应该发生的场景发生时，我们就应该调用 panic。比如，当程序到达了某条逻辑上不可能到达的路径：
  
  ```golang
  switch s := suit(drawCard()); s {
  case "Spades":                                // ...
  case "Hearts":                                // ...
  case "Diamonds":                              // ...
  case "Clubs":                                 // ...
  default:
    panic(fmt.Sprintf("invalid suit %q", s)) // Joker?
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;断言函数必须满足的前置条件是明智的做法，但这很容易被滥用。除非你能提供更多的错误信息，或者能更快速的发现错误，否则不需要使用断言，编译器在运行时会帮你检查代码。

  ```golang
  func Reset(x *Buffer) {
    if x == nil {
      panic("x is nil") // unnecessary!
    }
    x.elements = nil
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;虽然 Go 的 panic 机制类似于其他语言的异常，但 panic 的适用场景有一些不同。<b>由于 panic 会引起程序的崩溃，因此 panic 一般用于严重错误</b>，如程序内部的逻辑不一致。勤奋的程序员认为任何崩溃都表明代码中存在漏洞，所以对于大部分漏洞，我们应该使用 Go 提供的错误机制，而不是 panic，尽量避免程序的崩溃。在健壮的程序中，任何可以预料到的错误，如不正确的输入、错误的配置或是失败的 I/O 操作都应该被优雅的处理，最好的处理方式，就是使用 Go 的错误机制。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;考虑 regexp.Compile 函数，该函数将正则表达式编译成有效的可匹配格式。当输入的正则表达式不合法时，该函数会返回一个错误。当调用者明确的知道正确的输入不会引起函数错误时，要求调用者检查这个错误是不必要和累赘的。我们应该假设函数的输入一直合法，就如前面的断言一样：当调用者输入了不应该出现的输入时，触发 panic 异常。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在程序源码中，大多数正则表达式是字符串字面值（string literals），因此 regexp 包提供了包装函数 regexp.MustCompile 检查输入的合法性。

  ```golang
  package regexp
  func Compile(expr string) (*Regexp, error) { /* ... */ }
  func MustCompile(expr string) *Regexp {
    re, err := Compile(expr)
    if err != nil {
      panic(err)
    }
    return re
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;包装函数使得调用者可以便捷的用一个编译后的正则表达式为包级别的变量赋值：

  ```golang
  var httpSchemeRE = regexp.MustCompile(`^https?:`) //"http:" or "https:"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;显然，MustCompile 不能接收不合法的输入。<font color="red">函数名中的 Must 前缀是一种针对此类函数的命名约定</font>，比如 template.Must (3.6 节)

  ```golang
  func main() {
    f(3)
  }
  func f(x int) {
    fmt.Printf("f(%d)\n", x+0/x) // panics if x == 0
    defer fmt.Printf("defer %d\n", x)
    f(x - 1)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;上例中的运行输出如下：

  ```golang
  f(3)
  f(2)
  f(1)
  defer 1
  defer 2
  defer 3
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;当 f(0) 被调用时，发生 panic 异常，之前被延迟执行的 3 个 fmt.Printf 被调用。程序中断执行后，panic 信息和堆栈信息会被输出（下面是简化的输出）：

  ```bash
  panic: runtime error: integer divide by zero
  main.f(0)
  src/gopl.io/ch5/defer1/defer.go:14
  main.f(1)
  src/gopl.io/ch5/defer1/defer.go:16
  main.f(2)
  src/gopl.io/ch5/defer1/defer.go:16
  main.f(3)
  src/gopl.io/ch5/defer1/defer.go:16
  main.main()
  src/gopl.io/ch5/defer1/defer.go:10
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们在下一节将看到，如何使程序从 panic 异常中恢复，阻止程序的崩溃。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;为了方便诊断问题，runtime 包允许程序员输出堆栈信息。在下面的例子中，我们通过在 main 函数中延迟调用 printStack 输出堆栈信息。

  ```golang
  func main() {
    defer printStack()
    f(3)
  }
  func printStack() {
    var buf [4096]byte
    n := runtime.Stack(buf[:], false)
    os.Stdout.Write(buf[:n])
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;printStack 的简化输出如下（下面只是 printStack 的输出，不包括 panic 的日志信息）：

  ```bash
  goroutine 1 [running]:
  main.printStack()
  src/gopl.io/ch5/defer2/defer.go:20
  main.f(0)
  src/gopl.io/ch5/defer2/defer.go:27
  main.f(1)
  src/gopl.io/ch5/defer2/defer.go:29
  main.f(2)
  src/gopl.io/ch5/defer2/defer.go:29
  main.f(3)
  src/gopl.io/ch5/defer2/defer.go:29
  main.main()
  src/gopl.io/ch5/defer2/defer.go:15
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;将 panic 机制类比其他语言异常机制的读者可能会惊讶，runtime.Stack 为何能输出已经被释放函数的信息？原因是<b><font color="red">在 Go 的 panic 机制中，延迟函数的调用在释放堆栈信息之前。</font></b>
</div>

<h2><a name="10_recover">10. Recover 捕获异常</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通常来说，不应该对 panic 异常做任何处理，但有时，也许我们可以<b>从异常中恢复</b>，至少我们可以在程序崩溃前，做一些操作。举个例子，<font color="red">当 web 服务器遇到不可预料的严重问题时，在崩溃前应该将所有的连接关闭；如果不做任何处理，会使得客户端一直处于等待状态。如果 web 服务器还在开发阶段，服务器甚至可以将异常信息反馈到客户端，帮助调试。</font>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果<b>在 deferred 函数中调用了内置函数 recover</b>，并且定义该 defer 语句的函数发生了 panic 异常，recover 会使程序从 panic 中恢复，并返回 panic value。导致panic 异常的函数不会继续运行，但能正常返回。在未发生 panic 时调用 recover，recover 会返回 nil。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;让我们以语言解析器为例，说明 recover 的使用场景。考虑到语言解析器的复杂性，即使某个语言解析器目前工作正常，也无法肯定它没有漏洞。因此，当某个异常出现时，我们不会选择让解析器崩溃，而是会将 panic 异常当作普通的解析错误，并附加额外信息提醒用户报告此错误。

  ```golang
  func Parse(input string) (s *Syntax, err error) {
    defer func() {
      if p := recover(); p != nil {
        err = fmt.Errorf("internal error: %v", p)
      }
    }()
    // ...parser...
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;deferred 函数帮助 Parse 从 panic 中恢复。在 deferred 函数内部，panic value 被附加到错误信息中；并用 err 变量接收错误信息，返回给调用者。我们也可以通过调用 runtime.Stack 往错误信息中添加完整的堆栈调用信息。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;不加区分的恢复所有的 panic 异常，不是可取的做法；因为在 panic 之后，无法保证包级变量的状态仍然和我们预期一致。比如，对数据结构的一次重要更新没有被完整完成、文件或者网络连接没有被关闭、获得的锁没有被释放。此外，如果写日志时产生的 panic 被不加区分的恢复，可能会导致漏洞被忽略。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;虽然把对 panic 的处理都集中在一个包下，有助于简化对复杂和不可以预料问题的处理，但作为被广泛遵守的规范，你不应该试图去恢复其他包引起的 panic。公有的 API 应该将函数的运行失败作为 error 返回，而不是 panic。同样的，你也不应该恢复一个由他人开发的函数引起的 panic，比如说调用者传入的回调函数，因为你无法确保这样做是安全的。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;有时我们很难完全遵循规范，举个例子，net/http 包中提供了一个 web 服务器，将收到的请求分发给用户提供的处理函数。很显然，我们不能因为某个处理函数引发的 panic 异常，杀掉整个进程；web 服务器遇到处理函数导致的 panic 时会调用 recover，输出堆栈信息，继续运行。这样的做法在实践中很便捷，但也会引起资源泄漏，或是因为 recover 操作，导致其他问题。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;基于以上原因，安全的做法是<b>有选择性的 recover</b>。换句话说，只恢复应该被恢复的 panic 异常，此外，这些异常所占的比例应该尽可能的低。为了标识某个 panic 是否应该被恢复，<font color="red">我们可以将 panic value 设置成特殊类型</font>。在 recover 时对 panic value 进行检查，如果发现 panic value 是特殊类型，就将这个 panic 作为 error 处理，如果不是，则按照正常的 panic 进行处理（在下面的例子中，我们会看到这种方式）。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的例子是 title 函数的变形，title 函数作用是如果发现 HTML 页面包含多个 title 标签，该函数会给调用者返回一个错误（error）。而在 soleTitle 内部处理时，如果检测到有多个 title 标签，会调用 panic，阻止函数继续递归，并将特殊类型 bailout 作为 panic 的参数。

  ```golang
  // soleTitle returns the text of the first non-empty title element
  // in doc, and an error if there was not exactly one.
  func soleTitle(doc *html.Node) (title string, err error) {
    type bailout struct{}
    defer func() {
      switch p := recover(); p {
      case nil:       // no panic
      case bailout{}: // "expected" panic
        err = fmt.Errorf("multiple title elements")
      default:
        panic(p) // unexpected panic; carry on panicking
      }
    }()
    // Bail out of recursion if we find more than one nonempty title.
    forEachNode(doc, func(n *html.Node) {
      if n.Type == html.ElementNode && n.Data == "title" &&
        n.FirstChild != nil {
        if title != "" {
          panic(bailout{}) // multiple titleelements
        }
        title = n.FirstChild.Data
      }
    }, nil)
    if title == "" {
      return "", fmt.Errorf("no title element")
    }
    return title, nil
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在上例中，deferred 函数调用 recover，并检查 panic value。当 panic value 是 bailout{} 类型时，deferred 函数生成一个 error 返回给调用者。当 panic value 是其他 non-nil 值时，表示发生了未知的 panic 异常，deferred 函数将调用 panic 函数并将当前的 panic value 作为参数传入；此时，等同于 recover 没有做任何操作。（请注意：在例子中，对可预期的错误采用了panic，这违反了之前的建议，我们在此只是想向读者演示这种机制。）

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;有些情况下，我们无法恢复。某些致命错误会导致Go在运行时终止程序，如内存不足。
</div>

<!--ref-->

<h2>附录：参考源</h2>
<div class="div_learning_post">
<p>

1. github.com, <a href="https://github.com/gopl-zh/gopl-zh.github.com/tree/master/ch5">Go语言圣经(中文版) Chapter 5</a>
</p>
</div>

</body>