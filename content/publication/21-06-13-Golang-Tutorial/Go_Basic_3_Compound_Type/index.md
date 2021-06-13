---
title: Golang 复合类型
#date: 2021-01-06 01:00:52
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
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#1_array"><font color="blue"><b>数组</b></font></a>：讨论了 Golang 中使用不多的数组的初始化和使用方法，数组是理解 slice 的基础；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 2. <a href="#2_slice"><font color="blue"><b>切片</b></font></a>：介绍了 Golang slice 的底层原理 (与数组的关系)，并且介绍了 slice 的创建、初始化和使用方法；
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.1 <a href="#2_slice_1"><font color="blue">append 函数</font></a>：结合 slice 的底层实现原理，介绍了 slice 在追加元素时候的底层过程；
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2.2 <a href="#2_slice_2"><font color="blue">Slice 内存技巧</font></a>：列举了一些例子展示了 slice 的用法，其中包括如何使用 slice 来实现 Stack；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 3. <a href="#3_map"><font color="blue"><b>Map</b></font></a>：讨论了 Golang 中 map 复合类型的用法和注意事项；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 4. <a href="#4_struct"><font color="blue"><b>结构体</b></font></a>：介绍了结构体的基本初始化和成员访问方法；
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.1 <a href="#4_struct_1"><font color="blue">结构体字面值</font></a>：介绍了结构体的字面值，以及结构体成员的赋值、修改方法；
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.2 <a href="#4_struct_2"><font color="blue">结构体比较</font></a>：介绍了可以进行结构体之间相互比较的前提条件；
    <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4.3 <a href="#4_struct_3"><font color="blue">结构体嵌入和匿名成员</font></a>：介绍了结构体匿名成员的嵌入方法，以及一些注意事项；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 5. <a href="#5_json"><font color="blue"><b>JSON</b></font></a>：介绍了 JSON 基本格式，以及 JSON 和 Go 类型进行编码 (marshal) 和解码 (unmarshal) 的流程和用法；
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 6. <a href="#6_text_and_html"><font color="blue"><b>文本和 HTML</b></font></a>：介绍了 Go 中使用模版生成复杂文本输出或 HTML 文件的方法
  </div>
</div>

<!--标题-->
<h2><a name="1_array">1. 数组</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;数组是一个由固定长度的特定类型元素组成的序列，一个数组可以由零个或多个元素组成。<font color="red">因为数组的长度是固定的，因此在 Go 语言中很少直接使用数组。</font>和数组对应的类型是 <b>Slice（切片）</b>，它是可以增长和收缩的动态序列，slice 功能也更灵活，但是要理解 slice 工作原理的话需要先理解数组。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;数组的每个元素可以通过索引下标来访问，索引下标的范围是从 0 开始到数组长度减 1 的位置。内置的 len 函数将返回数组中元素的个数。

  ```golang
  var a [3]int             // array of 3 integers
  fmt.Println(a[0])        // print the first element
  fmt.Println(a[len(a)-1]) // print the last element, a[2]

  // Print the indices and elements.
  for i, v := range a {
    fmt.Printf("%d %d\n", i, v)
  }

  // Print the elements only.
  for _, v := range a {
    fmt.Printf("%d\n", v)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>默认情况下，数组的每个元素都被初始化为元素类型对应的零值</b>，对于数字类型来说就是 0。我们也可以<b>使用数组字面值语法</b>用一组值来初始化数组：

  ```golang
  var q [3]int = [3]int{1, 2, 3}
  var r [3]int = [3]int{1, 2}
  fmt.Println(r[2]) // "0"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在数组字面值中，如果在数组的长度位置出现的是 “...” 省略号，则表示数组的长度是根据初始化值的个数来计算。因此，上面 q 数组的定义可以简化为

  ```golang
  q := [...]int{1, 2, 3}
  fmt.Printf("%T\n", q) // "[3]int"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">数组的长度是数组类型的一个组成部分，因此 [3]int 和 [4]int 是两种不同的数组类型。</font>数组的长度必须是常量表达式，因为数组的长度需要在编译阶段确定。

  ```golang
  q := [3]int{1, 2, 3}
  q = [4]int{1, 2, 3, 4} // compile error: cannot assign [4]int to [3]int
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们将会发现，数组、slice、map 和结构体字面值的写法都很相似。上面的形式是直接提供顺序初始化值序列，但是也可以指定一个索引和对应值列表的方式初始化，就像下面这样：

  ```golang
  type Currency int

  const (
    USD Currency = iota // 美元
    EUR                 // 欧元
    GBP                 // 英镑
    RMB                 // 人民币
  )

  // initialize a map
  symbol := [...]string{USD: "$", EUR: "€", GBP: "￡", RMB: "￥"}

  fmt.Println(RMB, symbol[RMB]) // "3 ￥"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在这种形式的数组字面值形式中，初始化索引的顺序是无关紧要的，而且没用到的索引可以省略，和前面提到的规则一样，未指定初始值的元素将用零值初始化。例如，

  ```golang
  r := [...]int{99: -1}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;定义了一个含有 100 个元素的数组 r，最后一个元素被初始化为 -1，其它元素都是用 0 初始化。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果一个数组的元素类型是可以相互比较的，那么数组类型也是可以相互比较的，这时候我们可以直接通过 == 比较运算符来比较两个数组，只有当两个数组的所有元素都是相等的时候数组才是相等的。不相等比较运算符 != 遵循同样的规则。

  ```golang
  a := [2]int{1, 2}
  b := [...]int{1, 2}
  c := [2]int{1, 3}
  fmt.Println(a == b, a == c, b == c) // "true false false"
  d := [3]int{1, 2}
  fmt.Println(a == d) // compile error: cannot compare [2]int == [3]int
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;作为一个真实的例子，crypto/sha256 包的 Sum256 函数对一个任意的字节 slice 类型的数据生成一个对应的消息摘要。消息摘要有 256bit 大小，因此对应 [32]byte 数组类型。如果两个消息摘要是相同的，那么可以认为两个消息本身也是相同（译注：理论上有 HASH 码碰撞的情况，但是实际应用可以基本忽略）；如果消息摘要不同，那么消息本身必然也是不同的。下面的例子用 SHA256 算法分别生成 “x” 和 “X” 两个信息的摘要：

  ```golang
  import "crypto/sha256"

  func main() {
    c1 := sha256.Sum256([]byte("x"))
    c2 := sha256.Sum256([]byte("X"))
    fmt.Printf("%x\n%x\n%t\n%T\n", c1, c2, c1 == c2, c1)
    // Output:
    // 2d711642b726b04401627ca9fbac32f5c8530fb1903cc4db02258717921a4881
    // 4b68ab3847feda7d6c62c1fbcbeebfa35eab7351ed5e78f4ddadea5df64b8015
    // false
    // [32]uint8
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;上面例子中，两个消息虽然只有一个字符的差异，但是生成的消息摘要则几乎有一半的 bit 位是不相同的。需要注意 Printf 函数的 %x 副词参数，它用于指定以十六进制的格式打印数组或 slice 全部的元素，%t 副词参数是用于打印布尔型数据，%T 副词参数是用于显示一个值对应的数据类型。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>当调用一个函数的时候，函数的每个调用参数将会被赋值给函数内部的参数变量，所以函数参数变量接收的是一个复制的副本，并不是原始调用的变量。</b>因为函数参数传递的机制导致传递大的数组类型将是低效的，并且对数组参数的任何的修改都是发生在复制的数组上，并不能直接修改调用时原始的数组变量。在这个方面，Go 语言对待数组的方式和其它很多编程语言不同，其它编程语言可能会隐式地将数组作为引用或指针对象传入被调用的函数。当然，我们可以显式地传入一个数组指针，那样的话函数通过指针对数组的任何修改都可以直接反馈到调用者。下面的函数用于给 [32]byte 类型的数组清零：

  ```golang
  func zero(ptr *[32]byte) {
    for i := range ptr {
      ptr[i] = 0
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;其实数组字面值 [32]byte{} 就可以生成一个32字节的数组。而且每个数组的元素都是零值初始化，也就是 0。因此，我们可以将上面的 zero 函数写的更简洁一点：

  ```golang
  func zero(ptr *[32]byte) {
    *ptr = [32]byte{}
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;虽然通过指针来传递数组参数是高效的，而且也允许在函数内部修改数组的值，但是数组依然是僵化的类型，因为数组的类型包含了<b>僵化的长度信息</b>。上面的 zero 函数并不能接收指向 [16]byte 类型数组的指针，而且也没有任何添加或删除数组元素的方法。由于这些原因，除了像 SHA256 这类需要处理特定大小数组的特例外，数组依然很少用作函数参数；相反，我们一般使用 slice 来替代数组。
</div>

<h2><a name="2_slice">2. Slice</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Slice（切片）代表变长的序列，序列中每个元素都有相同的类型。一个 slice 类型一般写作 []T (与数组区别: 数组要在 [] 中指定长度或使用 ... 自动推导长度，切片不用)，其中 T 代表 slice 中元素的类型；slice 的语法和数组很像，只是没有固定长度而已。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;数组和 slice 之间有着紧密的联系。一个 slice 是一个轻量级的数据结构，<b><font color="red">一个 slice 基于一个数组</font>，提供了访问数组子序列（或者全部）元素的功能</b>，而且 slice 的底层确实引用一个数组对象。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们可以基于一个已有数组来创建 slice，如下图展示的例子那样；我们也可以使用内建函数 make 来创建一个 slice (func make(t Type, size ...IntegerType) Type, 用于创建指定类型的 slice, map 或 chan)，此时底层首先会创建相应数组，然后再基于这个数组创建 slice；我们也可以使用 s = []int{} 语句来创建 slice，在这个例子中，我们创建了 slice，但是由于初始化为空值，故底层数据结构并不会被相应创建；若使用 s = []int{3, 4}，则首先会创建数组 [3, 4]，然后再基于这个数组创建 slice。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一个 slice 由三个部分构成：<b>指针、长度和容量</b>。结合下图，我们下面分别作解释：指针指向第一个 slice 元素对应的底层数组元素的地址，要注意的是 slice 的第一个元素并不一定就是数组的第一个元素，如下图的 slice Q2 和 summer 的指针字段 (data) 所示。长度对应 slice 中元素的数目；长度不能超过容量，容量一般是在创建 slice 被分配的一块空间，一般大小是从 slice 的开始位置到底层数据 (i.e. 数组)的结尾位置，如下图的 slice Q2 和 summer 的容量字段 (cap) 所示。内置的len和cap函数分别返回slice的长度和容量。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;对于 slice，可用内置的 len 和 cap 函数来返回 slice 的长度和容量。

  <div align="center">
    <img src="./pic/slice_instance.png" width=500px>
  </div>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;多个 slice 之间可以共享底层的数据，并且引用的数组部分区间可能重叠。上图显示了表示一年中每个月份名字的字符串数组，还有重叠引用了该数组的两个 slice。数组这样定义：
  
  ```golang
  months := [...]string{1: "January", /* ... */, 12: "December"}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;因此一月份是 months[1]，十二月份是 months[12]。通常，数组的第一个元素从索引 0 开始，但是月份一般是从1开始的，因此我们声明数组时直接跳过第 0 个元素，第 0 个元素会被自动初始化为空字符串。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;slice的切片操作 s[i:j]，其中 0 ≤ i ≤ j≤ cap(s)，用于创建一个新的 slice，引用 s 的从第 i 个元素开始到第 j-1 个元素的子序列。新的 slice 将只有 j-i 个元素。如果 i 位置的索引被省略的话将使用 0 代替，如果 j 位置的索引被省略的话将使用 len(s) 代替。因此，months[1:13] 切片操作将引用全部有效的月份，和 months[1:] 操作等价；months[:] 切片操作则是引用整个数组。让我们分别定义表示第二季度和北方夏天月份的 slice，它们有重叠部分：

  ```golang
  Q2 := months[4:7]
  summer := months[6:9]
  fmt.Println(Q2)     // ["April" "May" "June"]
  fmt.Println(summer) // ["June" "July" "August"]
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;两个 slice 都包含了六月份，下面的代码是一个包含相同月份的测试（性能较低）：

  ```golang
  for _, s := range summer {
    for _, q := range Q2 {
      if s == q {
        fmt.Printf("%s appears in both\n", s)
      }
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果切片操作超出 cap(s) 的上限将导致一个 panic 异常，但是超出 len(s) 则是意味着基于底层数据结构 (i.e. 数组) 扩展了 slice，因为新 slice 的长度会变大：

  ```golang
  fmt.Println(summer[:20]) // panic: out of range

  endlessSummer := summer[:5] // extend a slice (within capacity)
  fmt.Println(endlessSummer)  // "[June July August September October]"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;另外，字符串的切片操作和 []byte (字节类型切片) 的切片操作是类似的，都写作 x[m:n]，并且都是返回一个原始字节序列的子序列，底层都是共享之前的底层数组，因此这种操作都是常量时间复杂度。x[m:n] 切片操作对于字符串则生成一个新字符串，如果 x 是 []byte 的话则生成一个新的 []byte。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">因为 slice 值包含指向第一个 slice 元素的指针，因此向函数传递 slice 将允许在函数内部修改底层数组的元素。</font>换句话说，<b>复制一个 slice 只是对底层的数组创建了一个新的 slice 别名。</b>下面的 reverse 函数在原内存空间将 []int 类型的 slice 反转，而且它可以用于任意长度的 slice。

  ```golang
  // reverse reverses a slice of ints in place.
  func reverse(s []int) {
    for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
      s[i], s[j] = s[j], s[i]
    }
  }

  func main(){
    a := [...]int{0, 1, 2, 3, 4, 5}
    reverse(a[:])
    fmt.Println(a) // "[5 4 3 2 1 0]"
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一种将 slice 元素循环向左旋转 n 个元素的方法是三次调用 reverse 反转函数，第一次是反转开头的 n 个元素，然后是反转剩下的元素，最后是反转整个 slice 的元素。（如果是向右循环旋转，则将第三个函数调用移到第一个调用位置就可以了。）

  ```golang
  s := []int{0, 1, 2, 3, 4, 5}
  // Rotate s left by two positions.
  reverse(s[:2])
  reverse(s[2:])
  reverse(s)
  fmt.Println(s) // "[2 3 4 5 0 1]"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;要注意的是 slice 类型的变量 s 和数组类型的变量 a 的初始化语法的差异。slice和数组的字面值语法很类似，它们都是用花括弧包含一系列的初始化元素，但是<b>对于 slice 并没有指明序列的长度</b>，这会<b>隐式地创建一个合适大小的数组</b>，然后 slice 的指针指向底层的数组。就像数组字面值一样，slice 的字面值也可以按顺序指定初始化值序列，或者是通过索引和元素值指定，或者用两种风格的混合语法初始化。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;和数组不同的是，slice 之间不能比较，因此我们不能使用 == 操作符来判断两个 slice 是否含有全部相等元素。不过标准库提供了高度优化的 bytes.Equal 函数来判断两个<b>字节型 slice ([]byte)</b> 是否相等，但是对于其他类型的 slice，我们必须自己展开每个元素进行比较：
  
  ```golang
  func equal(x, y []string) bool {
    if len(x) != len(y) {
      return false
    }
    for i := range x {
      if x[i] != y[i] {
        return false
      }
    }
    return true
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">(尚且没搞明白这块?)</font>上面关于两个 slice 的深度相等测试，运行的时间并不比支持 == 操作的数组或字符串更多，但是为何 slice 不直接支持比较运算符呢？这方面有两个原因。第一个原因，一个 slice 的元素是<b>间接引用</b>的，一个 slice 甚至可以包含自身（译注：当 slice 声明为 []interface{} 时，slice 的元素可以是自身）。虽然有很多办法处理这种情形，但是没有一个是简单有效的。第二个原因，因为 slice 的元素是<b>间接引用</b>的，一个固定的 slice 值（译注：指 slice 本身的值，不是元素的值）在不同的时刻可能包含不同的元素，因为底层数组的元素可能会被修改。而例如 Go 语言中 map 的 key 只做简单的浅拷贝，它要求 key 在整个生命周期内保持不变性（译注：例如 slice 扩容，就会导致其本身的值/地址变化）。而用深度相等判断的话，显然在 map 的 key 这种场合并不合适。对于像指针或 chan 之类的引用类型，== 相等测试可以判断两个是否是引用相同的对象。一个针对slice的浅相等测试的 == 操作符可能是有一定用处的，也能临时解决 map 类型的 key 问题，但是 slice 和数组不同的相等测试行为会让人困惑。因此，安全的做法是直接禁止 slice 之间的比较操作。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;slice唯一合法的比较操作是和nil比较，例如：

  ```golang
  if summer == nil { /* ... */ }
  ```
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>一个零值的 slice 等于 nil</b>。一个 nil 值的 slice 并没有底层数组。一个 nil 值的 slice 的长度和容量都是0，但是也有非 nil 值的 slice 的长度和容量也是0的，例如 []int{} 或 make([]int, 3)[3:] (这里使用 make 创建完长度和容量均为 3 的 slice 后又使用 [3:] 创建了另一个长度为 0 的 slice)。与任意类型的 nil 值一样，我们可以用 []int(nil) 型转换表达式来生成一个对应类型 slice 的 nil 值。

  ```golang
  var s []int    // len(s) == 0, s == nil，没有初始化，slice 没有被创建
  s = nil        // len(s) == 0, s == nil
  s = []int(nil) // len(s) == 0, s == nil
  s = []int{}    // len(s) == 0, s != nil，slice 已经被创建，只是没有底层数据而已
  ```
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果你需要测试一个 slice 是否是空的，使用 len(s) == 0 来判断，而不应该用 s == nil 来判断，原因见上面第四个例子。除了和 nil 相等比较外，<b><font color="red">一个 nil 值的 slice 的行为和其它任意 0 长度的 slice 一样</font></b>；例如 reverse(nil) 也是安全的。除了文档已经明确说明的地方，所有的 Go 语言函数应该以相同的方式对待 nil 值的 slice 和 0 长度的 slice。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;内置的 make 函数创建一个指定元素类型、长度和容量的 slice。容量部分可以省略，在这种情况下，容量将等于长度。

  ```golang
  make([]T, len)
  make([]T, len, cap) // same as make([]T, cap)[:len]
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">在底层，make 创建了一个匿名的数组变量，然后返回一个 slice；只有通过返回的 slice 才能引用底层匿名的数组变量。在第一种语句中，slice 是整个数组的 view。在第二个语句中，slice 只引用了底层数组的前 len 个元素，但是容量将包含整个的数组。额外的元素是留给未来的增长用的。</font>

  <h3><a name="2_slice_1">2.1 append 函数</a></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;内置的 append 函数用于向 slice 追加元素：

  ```golang
  var runes []rune
  for _, r := range "Hello, 世界" {
    runes = append(runes, r)
  }
  fmt.Printf("%q\n", runes) // "['H' 'e' 'l' 'l' 'o' ',' ' ' '世' '界']"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在循环中使用 append 函数构建一个由九个 rune 字符构成的 slice，当然对应这个特殊的问题我们可以通过 Go 语言内置的 []rune("Hello, 世界") 转换操作完成。但在这里，我们主要想理解 append 函数，理解 append 函数对于理解 slice 底层是如何工作的非常重要，所以让我们仔细查看究竟是发生了什么。下面是第一个版本的 appendInt 函数，专门用于处理 []int 类型的 slice：

  ```golang
  func appendInt(x []int, y int) []int {
    var z []int
    zlen := len(x) + 1
    if zlen <= cap(x) {
      // There is room to grow.  Extend the slice.
      z = x[:zlen]
    } else {
      // There is insufficient space.  Allocate a new array.
      // Grow by doubling, for amortized linear complexity.
      zcap := zlen
      if zcap < 2*len(x) {
        zcap = 2 * len(x)
      }
      // 创建一个新的 slice，两倍于先前 slice 的空间
      z = make([]int, zlen, zcap)
      copy(z, x) // a built-in function; see text
    }
    z[len(x)] = y
    return z
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;每次调用 appendInt 函数，必须先检测 slice 底层数组是否有足够的容量来保存新添加的元素。如果有足够空间的话，直接扩展 slice（依然在原有的底层数组之上），将新添加的 y 元素复制到新扩展的空间，并返回 slice。因此，输入的 x 和输出的 z 共享相同的底层数组。如果没有足够的增长空间的话，appendInt 函数则会先分配一个足够大的 slice 用于保存新的结果，先将输入的 x 复制到新的空间，然后添加 y 元素。结果 z 和输入的 x 引用的将是不同的底层数组。
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;虽然通过循环复制元素更直接，不过内置的 copy 函数可以方便地将一个 slice 复制另一个相同类型的 slice。copy函数的第一个参数是要复制的目标 slice，第二个参数是源 slice，目标和源的位置顺序和 dst = src 赋值语句是一致的。两个 slice 可以共享同一个底层数组，甚至有重叠也没有问题。copy函数将返回成功复制的元素的个数（我们这里没有用到），等于两个 slice 中较小的长度，所以我们不用担心覆盖会超出目标slice的范围。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;为了提高内存使用效率，新分配的数组一般略大于保存 x 和 y 所需要的最低大小。通过在每次扩展数组时<b>直接将长度翻倍</b>从而避免了多次内存分配，也确保了添加单个元素操作的平均时间是一个常数时间。这个程序演示了效果：

  ```golang
  func main() {
    var x, y []int
    for i := 0; i < 10; i++ {
      y = appendInt(x, i)
      fmt.Printf("%d cap=%d\t%v\n", i, cap(y), y)
      x = y
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;每一次容量的变化都会导致重新分配内存和copy操作：

  ```golang
  0  cap=1    [0]
  1  cap=2    [0 1]
  2  cap=4    [0 1 2]
  3  cap=4    [0 1 2 3]
  4  cap=8    [0 1 2 3 4]
  5  cap=8    [0 1 2 3 4 5]
  6  cap=8    [0 1 2 3 4 5 6]
  7  cap=8    [0 1 2 3 4 5 6 7]
  8  cap=16   [0 1 2 3 4 5 6 7 8]
  9  cap=16   [0 1 2 3 4 5 6 7 8 9]
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;让我们仔细查看 i=3 时的迭代。当时 x 包含了 [0 1 2] 三个元素，但是容量是4，因此可以简单将新的元素添加到末尾，不需要新的内存分配。然后新的 y 的长度和容量都是4，并且和 x 引用着相同的底层数组，如下图所示。

  <div align="center">
    <img src="./pic/append_1.png" width=500px>
  </div>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在下一次迭代时 i=4，现在没有新的空余的空间了，因此 appendInt 函数分配一个容量为 8 的底层数组，将 x 的 4 个元素 [0 1 2 3] 复制到新空间的开头，然后添加新的元素 i，新元素的值是 4。新的 y 的长度是 5，容量是 8；后面有 3 个空闲的位置，之后三次迭代都不需要分配新的空间。当前迭代中，y 和 x 是对应不同底层数组的 view。这次操作如下图所示。

  <div align="center">
    <img src="./pic/append_2.png" width=500px>
  </div>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">内置的 append 函数可能使用比 appendInt 更复杂的内存扩展策略。</font>因此，通常我们并不知道 append 调用是否导致了内存的重新分配，因此我们也不能确认新的 slice 和原始的 slice 是否引用的是相同的底层数组空间。同样，我们不能确认在原先的 slice 上的操作是否会影响到新的 slice。因此，<b>通常是将 append 返回的结果直接赋值给输入的 slice 变量</b>：
  
  ```golang
  runes = append(runes, r)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">更新 slice 变量</font> (指上面例子中把 append 返回值重新赋给传入 append 的 slice) <font color="red">不仅对调用 append 函数是必要的，实际上对应任何可能导致长度、容量或底层数组变化的操作都是必要的。</font>要正确地使用 slice，需要记住尽管底层数组的元素是间接访问的，但是 slice 对应结构体本身的指针、长度和容量部分是直接访问的。要更新这些信息需要像上面例子那样一个显式的赋值操作。从这个角度看，slice 并不是一个纯粹的引用类型，它实际上是一个类似下面结构体的聚合类型：

  ```golang
  type IntSlice struct {
    ptr      *int
    len, cap int
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们的 appendInt 函数每次只能向 slice 追加一个元素，但是内置的 append 函数则可以追加多个元素，甚至追加一个 slice。

  ```golang
  var x []int
  x = append(x, 1)
  x = append(x, 2, 3)
  x = append(x, 4, 5, 6)
  x = append(x, x...) // append the slice x
  fmt.Println(x)      // "[1 2 3 4 5 6 1 2 3 4 5 6]"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通过下面的小修改，我们可以达到 append 函数类似的功能。其中在 appendInt 函数参数中的最后的 “...” 省略号表示接收变长的参数为slice。我们将在第 5 章详细解释这个特性。在下面的代码中，为了避免重复，和前面相同的代码并没有显示。

  ```golang
  func appendInt(x []int, y ...int) []int {
    var z []int
    zlen := len(x) + len(y)
    // ...expand z to at least zlen...
    copy(z[len(x):], y)
    return z
  }
  ```

  <h3><a name="2_slice_2">2.2 Slice 内存技巧</a></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;让我们看看更多的例子，比如旋转 slice、反转 slice 或在 slice 原有内存空间修改元素。给定一个字符串列表，下面的 nonempty 函数将在原有 slice 内存空间之上返回不包含空字符串的列表：

  ```golang
  // Nonempty is an example of an in-place slice algorithm.
  package main

  import "fmt"

  // nonempty returns a slice holding only the non-empty strings.
  // The underlying array is modified during the call.
  func nonempty(strings []string) []string {
    i := 0
    for _, s := range strings {
      if s != "" {
        strings[i] = s
        i++
      }
    }
    return strings[:i]
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;比较微妙的地方是，输入的 slice 和输出的 slice 共享一个底层数组。这可以避免分配另一个数组，不过原来的数据将可能会被覆盖，正如下面两个打印语句看到的那样：

  ```golang
  data := []string{"one", "", "three"}
  fmt.Printf("%q\n", nonempty(data)) // `["one" "three"]`
  fmt.Printf("%q\n", data)           // `["one" "three" "three"]`
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;因此我们通常会这样使用nonempty函数：data = nonempty(data)。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;nonempty函数也可以使用append函数实现：

  ```golang
  func nonempty2(strings []string) []string {
    out := strings[:0] // zero-length slice of original
    for _, s := range strings {
      if s != "" {
        out = append(out, s)
      }
    }
    return out
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;无论如何实现，以这种方式重用一个 slice 一般都要求最多为每个输入值产生一个输出值，事实上很多这类算法都是用来过滤或合并序列中相邻的元素。这种 slice 用法是比较复杂的技巧，虽然使用到了 slice 的一些技巧，但是对于某些场合是比较清晰和有效的。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一个 slice 可以用来模拟一个 stack。最初给定的空 slice 对应一个空的 stack，然后可以使用 append 函数将新的值压入 stack：

  ```golang
  stack = append(stack, v) // push v
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;stack 的顶部位置对应 slice 的最后一个元素：

  ```golang
  top := stack[len(stack)-1] // top of stack
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通过收缩 stack 可以弹出栈顶的元素：

  ```golang
  stack = stack[:len(stack)-1] // pop
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;要删除 slice 中间的某个元素并保存原有的元素顺序，可以通过内置的 copy 函数将后面的子 slice 向前依次移动一位完成：

  ```golang
  func remove(slice []int, i int) []int {
    copy(slice[i:], slice[i+1:])
    return slice[:len(slice)-1]
  }

  func main() {
    s := []int{5, 6, 7, 8, 9}
    fmt.Println(remove(s, 2)) // "[5 6 8 9]"
  }
  ```
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果删除元素后不用保持原来顺序的话，我们可以简单的用最后一个元素覆盖被删除的元素：

  ```golang
  func remove(slice []int, i int) []int {
    slice[i] = slice[len(slice)-1]
    return slice[:len(slice)-1]
  }

  func main() {
    s := []int{5, 6, 7, 8, 9}
    fmt.Println(remove(s, 2)) // "[5 6 9 8]
  }
  ```
</div>

<h2><a name="3_map">3. Map</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;哈希表是一种巧妙并且实用的数据结构。它是一个无序的 key/value 对的集合，其中所有的 key 都是不同的，然后通过给定的 key 可以在常数时间复杂度内检索、更新或删除对应的 value。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在 Go 语言中，一个 map 就是一个哈希表的引用，map 类型可以写为 map[K]V，其中 K 和 V 分别对应 key 和 value。map 中所有的 key 都有相同的类型，所有的 value 也有着相同的类型，但是 key 和 value 之间可以是不同的数据类型。其中 <b>K 对应的 key 必须是支持 == 比较运算符的数据类型</b>，所以 map 可以通过测试 key 是否相等来判断是否已经存在。虽然浮点数类型也是支持相等运算符比较的，但是将<font color="red">浮点数用做 key 类型则是一个坏的想法</font>，正如第 2 章提到的，最坏的情况是可能出现的 NaN 和任何浮点数都不相等。对于 V 对应的 value 数据类型则没有任何的限制。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;内置的 make 函数可以创建一个 map：

  ```golang
  ages := make(map[string]int) // mapping from strings to ints
  ```
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们也可以用 map 字面值的语法创建 map，同时还可以指定一些最初的 key/value：

  ```golang
  ages := map[string]int{
    "alice":   31,
    "charlie": 34,
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这相当于：

  ```golang
  ages := make(map[string]int)
  ages["alice"] = 31
  ages["charlie"] = 34
  ```
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;因此，另一种创建空的 map 的表达式是 map[string]int{}。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Map 中的元素通过 key 对应的下标语法访问：

  ```golang
  ages["alice"] = 32
  fmt.Println(ages["alice"]) // "32"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;使用内置的 delete 函数可以删除元素：

  ```golang
  delete(ages, "alice") // remove element ages["alice"]
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;所有这些操作是安全的，即使这些元素不在 map 中也没有关系，<font color="red">如果一个查找失败将返回 value 类型对应的零值</font>，例如，即使 map 中不存在 “bob” 下面的代码也可以正常工作，因为 ages["bob"]失败时将返回0。

  ```golang
  ages["bob"] = ages["bob"] + 1 // happy birthday!
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;而且 x += y 和 x++ 等简短赋值语法也可以用在 map 上，所以上面的代码可以改写成

  ```golang
  ages["bob"] += 1
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;更简单的写法

  ```golang
  ages["bob"]++
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;但是 map 中的元素并不是一个变量，因此我们<font color="red">不能对 map 的元素进行取址操作</font>：

  ```golang
  _ = &ages["bob"] // compile error: cannot take address of map element
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;禁止对 map 元素取址的原因是 map 可能随着元素数量的增长而重新分配更大的内存空间，从而可能导致之前的地址无效。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;要想遍历 map 中全部的 key/value 对的话，可以使用 range 风格的 for 循环实现，和之前的 slice 遍历语法类似。下面的迭代语句将在每次迭代时设置 name 和 age 变量，它们对应下一个键/值对：

  ```golang
  for name, age := range ages {
    fmt.Printf("%s\t%d\n", name, age)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Map 的迭代顺序是不确定的，并且不同的哈希函数实现可能导致不同的遍历顺序。在实践中，遍历的顺序是随机的，每一次遍历的顺序都不相同。这是故意的，每次都使用随机的遍历顺序可以强制要求程序不会依赖具体的哈希函数实现。如果要按顺序遍历 key/value 对，我们必须显式地对 key 进行排序，可以使用 sort 包的 Strings 函数对字符串 slice 进行排序。下面是常见的处理方式：

  ```golang
  import "sort"

  var names []string
  for name := range ages {
    names = append(names, name)
  }
  sort.Strings(names)
  for _, name := range names {
    fmt.Printf("%s\t%d\n", name, ages[name])
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;因为我们一开始就知道 names 的最终大小，因此给 slice 分配一个合适的大小将会更有效。下面的代码创建了一个空的slice，但是slice的容量刚好可以放下map中全部的key：

  ```golang
  names := make([]string, 0, len(ages))
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在上面的第一个 range 循环中，我们只关心 map 中的 key，所以我们忽略了第二个循环变量。在第二个循环中，我们只关心 names 中的名字，所以我们使用 “_” 空白标识符来忽略第一个循环变量，也就是迭代 slice 时的索引。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;map 类型的零值是 nil，也就是没有引用任何哈希表。

  ```golang
  var ages map[string]int
  fmt.Println(ages == nil)    // "true"
  fmt.Println(len(ages) == 0) // "true"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;map 上的大部分操作，包括查找、删除、len 和 range 循环都可以安全工作在 nil 值的 map 上，它们的行为和一个空的 map 类似。但是向一个 nil 值的 map 存入元素将导致一个 panic 异常：

  ```golang
  ages["carol"] = 21 // panic: assignment to entry in nil map
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在向 map 存数据前必须先创建 map。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通过 key 作为索引下标来访问 map 将产生一个 value。如果 key 在 map 中是存在的，那么将得到与 key 对应的 value；如果 key 不存在，那么将得到 value 对应类型的零值，正如我们前面看到的 ages["bob"] 那样。这个规则很实用，但是有时候可能需要知道对应的元素是否真的是在map之中。例如，如果元素类型是一个数字，你可能需要区分一个已经存在的 0，和不存在而返回零值的 0，可以像下面这样测试：

  ```golang
  age, ok := ages["bob"]
  if !ok { /* "bob" is not a key in this map; age == 0. */ }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;你会经常看到将这两个结合起来使用，像这样：

  ```golang
  if age, ok := ages["bob"]; !ok { /* ... */ }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在这种场景下，map 的下标语法将产生两个值；第二个是一个布尔值，用于报告元素是否真的存在。布尔变量一般命名为 ok，特别适合马上用于 if 条件判断部分。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;和 slice 一样，map 之间也不能进行相等比较；唯一的例外是和 nil 进行比较。要判断两个 map 是否包含相同的 key 和 value ，我们必须通过一个循环实现：

  ```golang
  func equal(x, y map[string]int) bool {
    if len(x) != len(y) {
      return false
    }
    for k, xv := range x {
      if yv, ok := y[k]; !ok || yv != xv {
        return false
      }
    }
    return true
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;从例子中可以看到如何用 !ok 来区分元素不存在，与元素存在但为 0 的两种情况。我们不能简单地用 xv != y[k] 判断，那样会导致在判断下面两个 map 时产生错误的结果。

  ```golang
  // True if equal is written incorrectly.
  equal(map[string]int{"A": 0}, map[string]int{"B": 42})
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Go 语言中并没有提供一个 set 类型，但是 map 中的 key 也是不相同的，可以用 map 实现类似 set 的功能。为了说明这一点，下面的 dedup 程序读取多行输入，但是对于相同内容的行，只打印第一次出现的行。dedup 程序通过 map 来表示所有的输入行所对应的 set 集合，以确保已经在集合存在的行不会被重复打印。

  ```golang
  func main() {
    seen := make(map[string]bool) // a set of strings
    input := bufio.NewScanner(os.Stdin)
    for input.Scan() {
      line := input.Text()
      if !seen[line] {
        seen[line] = true
        fmt.Println(line)
      }
    }

    if err := input.Err(); err != nil {
      fmt.Fprintf(os.Stderr, "dedup: %v\n", err)
      os.Exit(1)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Go 程序员将这种忽略 value 的 map 当作一个字符串集合，并非所有 map[string]bool 类型 value 都是无关紧要的；有一些则可能会同时包含 true 和 false 的值。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;有时候我们需要一个 map 或 set 的 key 是 slice 类型，但是 map 的 key 必须是可比较的类型，但是 slice 并不满足这个条件。不过，我们可以通过两个步骤绕过这个限制。第一步，定义一个辅助函数k，将 slice 转为 map 对应的 string 类型的 key，确保只有 x 和 y 相等时 k(x) == k(y) 才成立。然后创建一个 key 为 string 类型的 map，在每次对 map 操作时先用 k 辅助函数将 slice 转化为 string 类型。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的例子演示了如何使用 map 来记录提交相同的字符串列表的次数。它使用了 fmt.Sprintf 函数将字符串列表转换为一个字符串以用于 map 的 key，通过 %q 参数忠实地记录每个字符串元素的信息 (e.g. 把值为 ["str_1", "str_2"] 的 []string 类型变量变成 ["str_1" "str_2"] 的 string 形式)：

  ```golang
  var m = make(map[string]int)

  func k(list []string) string { return fmt.Sprintf("%q", list) }

  func Add(list []string)       { m[k(list)]++ }
  func Count(list []string) int { return m[k(list)] }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;使用同样的技术可以处理任何不可比较的 key 类型，而不仅仅是 slice 类型。这种技术对于想使用自定义 key 比较函数的时候也很有用，例如在比较字符串的时候忽略大小写。同时，辅助函数 k(x) 也不一定是字符串类型，它可以返回任何可比较的类型，例如整数、数组或结构体等。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这是 map 的另一个例子，下面的程序用于统计输入中每个 Unicode 码点出现的次数。虽然 Unicode 全部码点的数量巨大，但是出现在特定文档中的字符种类并没有多少，使用 map 可以用比较自然的方式来跟踪那些出现过的字符的次数。

  ```golang
  // Charcount computes counts of Unicode characters.
  package main

  import (
    "bufio"
    "fmt"
    "io"
    "os"
    "unicode"
    "unicode/utf8"
  )

  func main() {
    counts := make(map[rune]int)    // counts of Unicode characters
    var utflen [utf8.UTFMax + 1]int // count of lengths of UTF-8 encodings
    invalid := 0                    // count of invalid UTF-8 characters

    in := bufio.NewReader(os.Stdin)
    for {
      r, n, err := in.ReadRune() // returns rune, nbytes, error
      if err == io.EOF {
        break
      }
      if err != nil {
        fmt.Fprintf(os.Stderr, "charcount: %v\n", err)
        os.Exit(1)
      }
      // 无效 UTF-8 字符
      if r == unicode.ReplacementChar && n == 1 {
        invalid++
        continue
      }
      counts[r]++
      utflen[n]++
    }
    fmt.Printf("rune\tcount\n")
    for c, n := range counts {
      fmt.Printf("%q\t%d\n", c, n)
    }
    fmt.Print("\nlen\tcount\n")
    for i, n := range utflen {
      if i > 0 {
        fmt.Printf("%d\t%d\n", i, n)
      }
    }
    if invalid > 0 {
      fmt.Printf("\n%d invalid UTF-8 characters\n", invalid)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;ReadRune 方法执行 UTF-8 解码并返回三个值：解码的 rune 字符的值，字符 UTF-8 编码后的长度，和一个错误值。我们可预期的错误值只有对应文件结尾的 io.EOF。如果输入的是无效的 UTF-8 编码的字符，返回的将是 unicode.ReplacementChar 表示无效字符，并且编码长度是 1。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Map 的 value 类型也可以是一个聚合类型，比如是一个 map 或 slice。在下面的代码中，图 graph 的 key 类型是一个字符串，value 类型 map[string]bool 代表一个字符串集合。从概念上讲，graph 将一个字符串类型的 key 映射到一组相关的字符串集合，它们指向新的 graph 的 key。

  ```golang
  // key: string, value: map[string]bool
  var graph = make(map[string]map[string]bool)

  func addEdge(from, to string) {
    edges := graph[from]
    if edges == nil {
      edges = make(map[string]bool)
      graph[from] = edges
    }
    edges[to] = true
  }

  func hasEdge(from, to string) bool {
    return graph[from][to]
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;其中 addEdge 函数惰性初始化 map 是一个惯用方式，也就是说在每个值首次作为 key 时才初始化。addEdge 函数显示了如何让 map 的零值也能正常工作；即使 from 到 to 的边不存在， graph[from][to] 依然可以返回一个有意义的结果。
</div>

<h2><a name="4_struct">4. 结构体</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;结构体是一种聚合的数据类型，是由零个或多个任意类型的值聚合成的实体。每个值称为结构体的成员。用结构体的经典案例是处理公司的员工信息，每个员工信息包含一个唯一的员工编号、员工的名字、家庭住址、出生日期、工作岗位、薪资、上级领导等等。所有的这些信息都需要绑定到一个实体中，可以作为一个整体单元被复制，作为函数的参数或返回值，或者是被存储到数组中，等等。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面两个语句声明了一个叫 Employee 的命名的结构体类型，并且声明了一个 Employee 类型的变量 dilbert：

  ```golang
  type Employee struct {
    ID        int
    Name      string
    Address   string
    DoB       time.Time
    Position  string
    Salary    int
    ManagerID int
  }

  var dilbert Employee
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;dilbert 结构体变量的成员可以通过点操作符访问，比如 dilbert.Name 和 dilbert.DoB。因为 dilbert 是一个变量，它所有的成员也同样是变量，我们可以直接对每个成员赋值：
  
  ```golang
  dilbert.Salary -= 5000 // demoted, for writing too few lines of code
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;或者是对成员取地址，然后通过指针访问：

  ```golang
  position := &dilbert.Position
  *position = "Senior " + *position // promoted, for outsourcing to Elbonia
  ```
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">点操作符也可以和指向结构体的指针一起工作</font>：

  ```golang
  var employeeOfTheMonth *Employee = &dilbert
  employeeOfTheMonth.Position += " (proactive team player)"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;相当于下面语句 <font color="red">(i.e. 通过指针访问结构体成员 (employeeOfTheMonth)，和直接通过结构体访问结构体成员 (*employeeOfTheMonth)，在 Go 中都是支持的)</font>

  ```golang
  (*employeeOfTheMonth).Position += " (proactive team player)"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的 EmployeeByID 函数将根据给定的员工 ID 返回对应的员工信息结构体的指针。我们可以使用点操作符来访问它里面的成员：

  ```golang
  func EmployeeByID(id int) *Employee { /* ... */ }

  fmt.Println(EmployeeByID(dilbert.ManagerID).Position) // "Pointy-haired boss"

  id := dilbert.ID
  EmployeeByID(id).Salary = 0 // fired for... no real reason
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;后面的语句通过 EmployeeByID 返回的结构体指针更新了 Employee 结构体的成员。如果将 EmployeeByID 函数的返回值从 *Employee 指针类型改为 Employee 值类型，那么更新语句将不能编译通过，因为在赋值语句的左边并不确定是一个变量 <font color="red">（译注：调用函数返回的是值，并不是一个可取地址的变量）</font>。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通常一行对应一个结构体成员，成员的名字在前类型在后，不过如果相邻的成员类型如果相同的话可以被合并到一行，就像下面的 Name 和 Address 成员那样：

  ```golang
  type Employee struct {
    ID            int
    Name, Address string      // Same type can be merged to the same line
    DoB           time.Time
    Position      string
    Salary        int
    ManagerID     int
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;结构体成员的输入顺序也有重要的意义。我们也可以将 Position 成员合并（因为也是字符串类型），或者是交换 Name 和 Address 出现的先后顺序，那样的话就是定义了不同的结构体类型。通常，我们只是将相关的成员写到一起。


  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red"><b>如果结构体成员名字是以大写字母开头的，那么该成员就是导出的</b></font>；这是 Go 语言导出规则决定的。一个结构体可能同时包含导出和未导出的成员。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;结构体类型往往是冗长的，因为它的每个成员可能都会占一行。虽然我们每次都可以重写整个结构体成员，但是重复会令人厌烦。因此，完整的结构体写法通常只在类型声明语句的地方出现，就像 Employee 类型声明语句那样。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">一个命名为 S 的结构体类型将不能再包含S类型的成员：因为一个聚合的值不能包含它自身。</font>（该限制同样适用于数组。）但是 <font color="red">S 类型的结构体可以包含 *S 指针类型的成员</font>，这可以让我们创建递归的数据结构，比如链表和树结构等。在下面的代码中，我们使用一个二叉树来实现一个插入排序：

  ```golang
  type tree struct {
    value       int
    left, right *tree
  }

  // Sort sorts values in place.
  func Sort(values []int) {
    var root *tree
    for _, v := range values {
      root = add(root, v)
    }
    appendValues(values[:0], root)
  }

  // appendValues appends the elements of t to values in order
  // and returns the resulting slice.
  func appendValues(values []int, t *tree) []int {
    if t != nil {
      values = appendValues(values, t.left)
      values = append(values, t.value)
      values = appendValues(values, t.right)
    }
    return values
  }

  func add(t *tree, value int) *tree {
    if t == nil {
      // Equivalent to return &tree{value: value}.
      t = new(tree)
      t.value = value
      return t
    }
    if value < t.value {
      t.left = add(t.left, value)
    } else {
      t.right = add(t.right, value)
    }
    return t
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;结构体类型的零值是每个成员都是零值。通常会将零值作为最合理的默认值。例如，对于 bytes.Buffer 类型，结构体初始值就是一个随时可用的空缓存，还有在第 8 章将会讲到的 sync.Mutex 的零值也是有效的未锁定状态。有时候这种零值可用的特性是自然获得的，但是也有些类型需要一些额外的工作。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果结构体没有任何成员的话就是空结构体，写作 struct{} 。它的大小为 0，也不包含任何信息，但是有时候依然是有价值的。有些 Go 语言程序员用 map 来模拟 set 数据结构时，用它来代替 map 中布尔类型的 value，只是强调 key 的重要性，但是因为节约的空间有限，而且语法比较复杂，所以我们通常会避免这样的用法。

  ```golang
  seen := make(map[string]struct{}) // set of strings
  // ...
  if _, ok := seen[s]; !ok {
    seen[s] = struct{}{}
    // ...first time seeing s...
  }
  ```

  <h3><a name="4_struct_1">结构体字面值</a></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;结构体值也可以用结构体字面值表示，结构体字面值可以指定每个成员的值。

  ```golang
  type Point struct{ X, Y int }

  p := Point{1, 2}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这里有两种形式的结构体字面值语法，上面的是第一种写法，要求以结构体成员定义的顺序为每个结构体成员指定一个字面值。它要求写代码和读代码的人要记住结构体的每个成员的类型和顺序，不过结构体成员有细微的调整就可能导致上述代码不能编译。因此，上述的语法一般只在定义结构体的包内部使用，或者是在较小的结构体中使用，这些结构体的成员排列比较规则，比如 image.Point{x, y} 或 color.RGBA{red, green, blue, alpha}。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;其实更常用的是第二种写法，以成员名字和相应的值来初始化，可以包含部分或全部的成员，如：

  ```golang
  anim := gif.GIF{LoopCount: nframes}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在这种形式的结构体字面值写法中，如果成员被忽略的话将默认用零值。因为提供了成员的名字，所以成员出现的顺序并不重要。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;两种不同形式的写法不能混合使用。而且，你不能企图在外部包中用第一种顺序赋值的技巧来偷偷地初始化结构体中未导出的成员 :(
  
  ```golang
  package p
  type T struct{ a, b int } // a and b are not exported
  
  - - -

  package q
  import "p"
  var _ = p.T{a: 1, b: 2} // compile error: can't reference a, b
  var _ = p.T{1, 2}       // compile error: can't reference a, b
  ```
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;虽然上面最后一行代码的编译错误信息中并没有显式提到未导出的成员，但是这样企图隐式使用未导出成员的行为也是不允许的。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;结构体可以作为函数的参数和返回值。例如，这个 Scale 函数将 Point 类型的值缩放后返回：

  ```golang
  func Scale(p Point, factor int) Point {
    return Point{p.X * factor, p.Y * factor}
  }

  fmt.Println(Scale(Point{1, 2}, 5)) // "{5 10}"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果考虑效率的话，较大的结构体通常会用指针的方式传入和返回，

  ```golang
  func Bonus(e *Employee, percent int) int {
    return e.Salary * percent / 100
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;如果要在函数内部修改结构体成员的话，用指针传入是必须的；因为在 Go 语言中，所有的函数参数都是值拷贝传入的，函数参数将不再是函数调用时的原始变量。

  ```golang
  func AwardAnnualRaise(e *Employee) {
    e.Salary = e.Salary * 105 / 100
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;因为结构体通常通过指针处理，可以用下面的写法来创建并初始化一个结构体变量，并返回结构体的地址：

  ```golang
  pp := &Point{1, 2}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;它和下面的语句是等价的：

  ```golang
  pp := new(Point)
  *pp = Point{1, 2}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;不过 &Point{1, 2} 写法可以直接在表达式中使用，比如一个函数调用。

  <h3><a name="4_struct_2">4.2 结构体比较</a></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<b>如果结构体的全部成员都是可以比较的，那么结构体也是可以比较的</b>，那样的话两个结构体将可以使用 == 或 != 运算符进行比较。相等比较运算符 == 将比较两个结构体的每个成员，因此下面两个比较的表达式是等价的：

  ```golang
  type Point struct{ X, Y int }

  p := Point{1, 2}
  q := Point{2, 1}
  fmt.Println(p.X == q.X && p.Y == q.Y) // "false"
  fmt.Println(p == q)                   // "false"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;可比较的结构体类型和其他可比较的类型一样，可以用于 map 的 key 类型。

  ```golang
  type address struct {
    hostname string
    port     int
  }

  hits := make(map[address]int)
  hits[address{"golang.org", 443}]++
  ```

  <h3><a name="4_struct_3">结构体嵌入和匿名成员</a></h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在本节中，我们将看到如何使用 Go 语言提供的不同寻常的结构体嵌入机制让一个命名的结构体包含另一个结构体类型的匿名成员，这样就可以通过简单的点运算符 x.f 来访问匿名成员链中嵌套的 x.d.e.f 成员。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;考虑一个二维的绘图程序，提供了一个各种图形的库，例如矩形、椭圆形、星形和轮形等几何形状。这里是其中两个的定义：

  ```golang
  type Circle struct {
    X, Y, Radius int
  }

  type Wheel struct {
    X, Y, Radius, Spokes int
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一个 Circle 代表的圆形类型包含了标准圆心的 X 和 Y 坐标信息，和一个 Radius 表示的半径信息。一个 Wheel 轮形除了包含 Circle 类型所有的全部成员外，还增加了 Spokes 表示径向辐条的数量。我们可以这样创建一个 wheel 变量：

  ```golang
  var w Wheel
  w.X = 8
  w.Y = 8
  w.Radius = 5
  w.Spokes = 20
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;随着库中几何形状数量的增多，我们一定会注意到它们之间的相似和重复之处，所以我们可能为了便于维护而将相同的属性独立出来：

  ```golang
  type Point struct {
    X, Y int
  }

  type Circle struct {
    Center Point
    Radius int
  }

  type Wheel struct {
    Circle Circle
    Spokes int
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这样改动之后结构体类型变的清晰了，但是这种修改同时也导致了访问每个成员变得繁琐：


  ```golang
  var w Wheel
  w.Circle.Center.X = 8
  w.Circle.Center.Y = 8
  w.Circle.Radius = 5
  w.Spokes = 20
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">Go 语言有一个特性让我们只声明一个成员对应的数据类型而不指名成员的名字，这类成员就叫</font><b>匿名成员</b>。匿名成员的数据类型必须是命名的类型或指向一个命名的类型的指针。下面的代码中， Circle 和 Wheel 各自都有一个匿名成员。我们可以说 Point 类型被<b>嵌入</b>到了 Circle 结构体，同时 Circle 类型被<b>嵌入</b>到了 Wheel 结构体。

  ```golang
  type Circle struct {
    Point
    Radius int
  }

  type Wheel struct {
    Circle
    Spokes int
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;得益于匿名嵌入的特性，我们可以直接访问叶子属性而不需要给出完整的路径：

  ```golang
  var w Wheel
  w.X = 8            // equivalent to w.Circle.Point.X = 8
  w.Y = 8            // equivalent to w.Circle.Point.Y = 8
  w.Radius = 5       // equivalent to w.Circle.Radius = 5
  w.Spokes = 20
  ```
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在右边的注释中给出的显式形式访问这些叶子成员的语法依然有效，因此匿名成员并不是真的无法访问了。其中匿名成员Circle和Point都有自己的名字——就是命名的类型名字——但是这些名字在点操作符中是可选的。我们在访问子成员的时候可以忽略任何匿名成员部分。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;不幸的是，<font color="red">结构体字面值并没有简短表示匿名成员的语法</font>， 因此下面的语句都不能编译通过：

  ```golang
  w = Wheel{8, 8, 5, 20}                       // compile error: unknown fields
  w = Wheel{X: 8, Y: 8, Radius: 5, Spokes: 20} // compile error: unknown fields
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;结构体字面值必须遵循形状类型声明时的结构，所以我们只能用下面的两种语法，它们彼此是等价的：

  ```golang
  w = Wheel{Circle{Point{8, 8}, 5}, 20}

  w = Wheel{
    Circle: Circle{
      Point:  Point{X: 8, Y: 8},
      Radius: 5,
    },
    Spokes: 20, // NOTE: trailing comma necessary here (and at Radius)
  }

  fmt.Printf("%#v\n", w)
  // Output:
  // Wheel{Circle:Circle{Point:Point{X:8, Y:8}, Radius:5}, Spokes:20}

  w.X = 42

  fmt.Printf("%#v\n", w)
  // Output:
  // Wheel{Circle:Circle{Point:Point{X:42, Y:8}, Radius:5}, Spokes:20}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;需要注意的是 Printf 函数中 %v 参数包含的 # 副词，它表示用和 Go 语言类似的语法打印值。对于结构体类型来说，将包含每个成员的名字。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;因为匿名成员也有一个隐式的名字，因此不能同时包含两个类型相同的匿名成员，这会导致名字冲突。同时，因为成员的名字是由其类型隐式地决定的，所以<font color="red">匿名成员也有可见性的规则约束</font>。在上面的例子中，Point 和 Circle 匿名成员都是导出的。即使它们不导出（比如改成小写字母开头的point和circle），我们依然可以用简短形式访问匿名成员嵌套的成员


  ```golang
  w.X = 8 // equivalent to w.circle.point.X = 8
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;但是在包外部，因为 circle 和 point 没有导出，不能访问它们的成员，因此简短的匿名成员访问语法也是禁止的。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;到目前为止，我们看到匿名成员特性只是对访问嵌套成员的点运算符提供了简短的语法糖。稍后，我们将会看到匿名成员并不要求是结构体类型；其实任何命名的类型都可以作为结构体的匿名成员。但是为什么要嵌入一个没有任何子成员类型的匿名成员类型呢？

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;答案是<b>匿名类型的方法集</b>。简短的点运算符语法可以用于选择匿名成员嵌套的成员，也可以用于访问它们的方法。实际上，外层的结构体不仅仅是获得了匿名成员类型的所有成员，而且也获得了该类型导出的全部的方法。这个机制可以用于将一些有简单行为的对象组合成有复杂行为的对象。组合是Go语言中面向对象编程的核心，我们将在第 5 章中专门讨论。
</div>

<h2><a name="5_json">5. JSON</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;JavaScript 对象表示法 (JSON) 是<b>一种用于发送和接收结构化信息的标准协议</b>。在类似的协议中，JSON 并不是唯一的一个标准协议。XML、ASN.1 和 Google 的 Protocol Buffers 都是类似的协议，并且有各自的特色，但是由于简洁性、可读性和流行程度等原因，JSON 是应用最广泛的一个。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Go 语言对于这些标准格式的编码和解码都有良好的支持，由标准库中的 encoding/json、encoding/xml、encoding/asn1 等包提供支持（译注：Protocol Buffers的支持由 github.com/golang/protobuf 包提供），并且这类包都有着相似的API接口。本节，我们将对重要的 encoding/json 包的用法做个概述。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;JSON 是对 JavaScript 中各种类型的值 —— 字符串、数字、布尔值和对象 —— 的 Unicode 编码。它可以用有效可读的方式表示第 2 章的基础数据类型和本章的数组、slice、结构体和 map 等聚合数据类型。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;基本的 JSON 类型有数字（十进制或科学记数法）、布尔值（true或false）、字符串，其中字符串是以双引号包含的 Unicode 字符序列，支持和 Go 语言类似的反斜杠转义特性，不过 JSON 使用的是 \Uhhhh 转义数字来表示一个 UTF-16 编码（译注：UTF-16 和 UTF-8 一样是一种变长的编码，有些 Unicode 码点较大的字符需要用 4 个字节表示；而且 UTF-16 还有大端和小端的问题），而不是 Go 语言的 rune 类型。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这些基础类型可以通过 JSON 的数组和对象类型进行<b>递归组合</b>。一个 JSON 数组是一个有序的值序列，写在一个方括号中并以逗号分隔；一个 JSON 数组可以用于编码 G 语言的数组和 slice。一个 JSON 对象是一个<b>字符串到值的映射</b>，写成一系列的 name:value 对形式，用花括号包含并以逗号分隔。JSON 的对象类型可以用于编码 Go 语言的 map 类型 (key 类型是字符串) 和结构体。例如：

  ```javascript
  boolean         true
  number          -273.15
  string          "She said \"Hello, BF\""
  array           ["gold", "silver", "bronze"]
  object          {"year": 1980,
                  "event": "archery",
                  "medals": ["gold", "silver", "bronze"]}
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;考虑一个应用程序，该程序负责收集各种电影评论并提供反馈功能。它的 Movie 数据类型和一个典型的表示电影的值列表如下所示。（在结构体声明中，Year 和 Color 成员后面的字符串面值是结构体成员 Tag，我们稍后会解释它的作用。）

  ```golang
  type Movie struct {
    Title  string
    Year   int  `json:"released"`
    Color  bool `json:"color,omitempty"`
    Actors []string
  }

  var movies = []Movie{
    {Title: "Casablanca", Year: 1942, Color: false,
      Actors: []string{"Humphrey Bogart", "Ingrid Bergman"}},
    {Title: "Cool Hand Luke", Year: 1967, Color: true,
      Actors: []string{"Paul Newman"}},
    {Title: "Bullitt", Year: 1968, Color: true,
      Actors: []string{"Steve McQueen", "Jacqueline Bisset"}},
    // ...
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这样的数据结构特别适合 JSON 格式，并且在两者之间相互转换也很容易。将一个 Go 语言中类似 movies 的结构体 slice 转为 JSON 的过程叫<b>编组 (marshaling)</b>。编组通过调用 json.Marshal 函数完成：

  ```golang
  data, err := json.Marshal(movies)
  if err != nil {
    log.Fatalf("JSON marshaling failed: %s", err)
  }
  fmt.Printf("%s\n", data)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">Marshal 函数返回一个编码后的字节 slice，包含很长的字符串，并且没有空白缩进。</font>我们将它折行以便于显示：

  ```javascript
  [{"Title":"Casablanca","released":1942,"Actors":["Humphrey Bogart","Ingr
  id Bergman"]},{"Title":"Cool Hand Luke","released":1967,"color":true,"Ac
  tors":["Paul Newman"]},{"Title":"Bullitt","released":1968,"color":true,"
  Actors":["Steve McQueen","Jacqueline Bisset"]}]
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这种紧凑的表示形式虽然包含了全部的信息，但是很难阅读。为了生成便于阅读的格式，另一个 json.MarshalIndent 函数将产生整齐缩进的输出。该函数有两个额外的字符串参数用于表示每一行输出的前缀和每一个层级的缩进：

  ```golang
  data, err := json.MarshalIndent(movies, "", "    ")
  if err != nil {
    log.Fatalf("JSON marshaling failed: %s", err)
  }
  fmt.Printf("%s\n", data)
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;上面的代码将产生这样的输出（译注：在最后一个成员或元素后面并没有逗号分隔符）：

  ```javascript
  [
    {
      "Title": "Casablanca",
      "released": 1942,
      "Actors": [
        "Humphrey Bogart",
        "Ingrid Bergman"
      ]
    },
    {
      "Title": "Cool Hand Luke",
      "released": 1967,
      "color": true,
      "Actors": [
        "Paul Newman"
      ]
    },
    {
      "Title": "Bullitt",
      "released": 1968,
      "color": true,
      "Actors": [
        "Steve McQueen",
        "Jacqueline Bisset"
      ]
    }
  ]
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在编码时，默认使用 Go 语言结构体的成员名字作为 JSON 的对象（通过reflect反射技术，我们将在第 11 章讨论）。只有导出的结构体成员才会被编码，这也就是我们为什么选择用大写字母开头的成员名称。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;细心的读者可能已经注意到，其中 Year 名字的成员在编码后变成了 released，还有 Color 成员编码后变成了小写字母开头的 color。这是因为结构体成员 Tag 所导致的。<font color="red">一个结构体成员 Tag 是和在编译阶段关联到该成员的元信息字符串</font>：

  ```golang
  Year  int  `json:"released"`
  Color bool `json:"color,omitempty"`
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">结构体的成员 Tag 可以是<b>任意的字符串面值</b>，但是通常是一系列<b>用空格分隔的 key:"value" 键值对序列</b>。因为值中含有双引号字符，因此成员 Tag 一般用<b>原生字符串面值</b>的形式书写。</font>json 开头键名对应的值用于控制 encoding/json 包的编码和解码的行为，并且 encoding/... 下面其它的包也遵循这个约定。成员 Tag 中 json 对应值的第一部分用于指定 JSON 对象的名字，比如将 Go 语言中的 TotalCount 成员对应到 JSON 中的 total_count 对象。Color 成员的 Tag 还带了一个额外的 omitempty 选项，表示当 Go 语言结构体成员为空或零值时不生成该 JSON 对象（这里 false 为零值）。果然，Casablanca 是一个黑白电影，并没有输出 Color 成员。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;编码的逆操作是<b>解码</b>，对应将 JSON 数据解码为 Go 语言的数据结构，Go语言中一般叫 <b>unmarshaling</b>，通过 json.Unmarshal 函数完成。下面的代码将 JSON 格式的电影数据解码为一个承载结构体的 slice，结构体中只有 Title 成员。<font color="red">通过定义合适的 Go 语言数据结构，我们可以选择性地解码 JSON 中感兴趣的成员。</font>当 Unmarshal 函数调用返回，slice 将被只含有 Title 信息的值填充，其它 JSON 成员将被忽略。

  ```golang
  var titles []struct{ Title string }
  if err := json.Unmarshal(data, &titles); err != nil {
    log.Fatalf("JSON unmarshaling failed: %s", err)
  }
  fmt.Println(titles) // "[{Casablanca} {Cool Hand Luke} {Bullitt}]"
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;许多 web 服务都提供 JSON 接口，通过 HTTP 接口发送 JSON 格式请求并返回 JSON 格式的信息。为了说明这一点，我们通过 Github 的 issue 查询服务来演示类似的用法。首先，我们要定义合适的类型和常量：

  ```golang
  // Package github provides a Go API for the GitHub issue tracker.
  // See https://developer.github.com/v3/search/#search-issues.
  package github

  import "time"

  const IssuesURL = "https://api.github.com/search/issues"

  type IssuesSearchResult struct {
    TotalCount int `json:"total_count"`
    Items          []*Issue
  }

  type Issue struct {
    Number    int
    HTMLURL   string `json:"html_url"`
    Title     string
    State     string
    User      *User
    CreatedAt time.Time `json:"created_at"`
    Body      string    // in Markdown format
  }

  type User struct {
    Login   string
    HTMLURL string `json:"html_url"`
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;和前面一样，即使对应的 JSON 对象名是小写字母，每个结构体的成员名也是声明为大写字母开头的。因为有些 JSON 成员名字和 Go 结构体成员名字并不相同，因此需要 Go 语言结构体成员 Tag 来指定对应的 JSON 名字。同样，在解码的时候也需要做同样的处理，GitHub 服务返回的信息比我们定义的要多很多。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;SearchIssues 函数发出一个 HTTP 请求，然后解码返回的 JSON 格式的结果。因为用户提供的查询条件可能包含类似 ? 和 & 之类的特殊字符，为了避免对 URL 造成冲突，我们用 url.QueryEscape 来对查询中的特殊字符进行转义操作。

  ```golang
  package github

  import (
    "encoding/json"
    "fmt"
    "net/http"
    "net/url"
    "strings"
  )

  // SearchIssues queries the GitHub issue tracker.
  func SearchIssues(terms []string) (*IssuesSearchResult, error) {
    q := url.QueryEscape(strings.Join(terms, " "))
    resp, err := http.Get(IssuesURL + "?q=" + q)
    if err != nil {
      return nil, err
    }

    // We must close resp.Body on all execution paths.
    // (Chapter 5 presents 'defer', which makes this simpler.)
    if resp.StatusCode != http.StatusOK {
      resp.Body.Close()
      return nil, fmt.Errorf("search query failed: %s", resp.Status)
    }

    var result IssuesSearchResult
    if err := json.NewDecoder(resp.Body).Decode(&result); err != nil {
      resp.Body.Close()
      return nil, err
    }
    resp.Body.Close()
    return &result, nil
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在早些的例子中，我们使用了 json.Unmarshal 函数来将 JSON 格式的字符串解码为字节 slice。但是这个例子中，我们使用了<b>基于流式的解码器 json.Decoder</b>，它可以从一个输入流解码 JSON 数据，尽管这不是必须的。如您所料，还有一个针对输出流的 json.Encoder 编码对象。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们调用 Decode 方法来填充变量。这里有多种方法可以格式化结构。下面是最简单的一种，以一个固定宽度打印每个 issue，但是在下一节我们将看到如何利用模板来输出复杂的格式。

  ```golang
  // Issues prints a table of GitHub issues matching the search terms.
  package main

  import (
    "fmt"
    "log"
    "os"

    "gopl.io/ch4/github"
  )

  func main() {
    result, err := github.SearchIssues(os.Args[1:])
    if err != nil {
      log.Fatal(err)
    }
    fmt.Printf("%d issues:\n", result.TotalCount)
    for _, item := range result.Items {
      fmt.Printf("#%-5d %9.9s %.55s\n",
        item.Number, item.User.Login, item.Title)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;通过命令行参数指定检索条件。下面的命令是查询 Go 语言项目中和 JSON 解码相关的问题，还有查询返回的结果：

  ```bash
  $ go build gopl.io/ch4/issues
  $ ./issues repo:golang/go is:open json decoder
  13 issues:
  #5680    eaigner encoding/json: set key converter on en/decoder
  #6050  gopherbot encoding/json: provide tokenizer
  #8658  gopherbot encoding/json: use bufio
  #8462  kortschak encoding/json: UnmarshalText confuses json.Unmarshal
  #5901        rsc encoding/json: allow override type marshaling
  #9812  klauspost encoding/json: string tag not symmetric
  #7872  extempora encoding/json: Encoder internally buffers full output
  #9650    cespare encoding/json: Decoding gives errPhase when unmarshalin
  #6716  gopherbot encoding/json: include field name in unmarshal error me
  #6901  lukescott encoding/json, encoding/xml: option to treat unknown fi
  #6384    joeshaw encoding/json: encode precise floating point integers u
  #6647    btracey x/tools/cmd/godoc: display type kind of each named type
  #4237  gjemiller encoding/base64: URLEncoding padding is optional
  ```


  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;GitHub 的 Web 服务接口 https://developer.github.com/v3/ 包含了更多的特性。
</div>

<h2><a name="6_text_and_html">6. 文本和 HTML</a></h2>
<div class="div_learning_post_boder">

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;前面的例子，只是最简单的格式化，使用 Printf 是完全足够的。但是有时候会需要复杂的打印格式，这时候一般需要将格式化代码分离出来以便更安全地修改。这些功能是由 text/template 和 html/template 等模板包提供的，它们提供了一个将变量值填充到一个文本或 HTML 格式的<b>模板</b>的机制。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<font color="red">一个模板是一个字符串或一个文件，里面包含了一个或多个由双花括号包含的 <b>{{action}} 对象</b>。</font>大部分的字符串只是按字面值打印，但是对于 actions 部分将触发其它的行为。每个 actions 都包含了一个用模板语言书写的表达式，一个 action 虽然简短但是可以输出复杂的打印值，模板语言包含通过选择结构体的成员、调用函数或方法、表达式控制流 if-else 语句和 range 循环语句，还有其它实例化模板等诸多特性。下面是一个简单的模板字符串：

  ```golang
  const templ = `{{.TotalCount}} issues:
  {{range .Items}}----------------------------------------
  Number: {{.Number}}
  User:   {{.User.Login}}
  Title:  {{.Title | printf "%.64s"}}
  Age:    {{.CreatedAt | daysAgo}} days
  {{end}}`
  ```

  {% raw %}

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;这个模板先打印匹配到的 issue 总数，然后打印每个 issue 的编号、创建用户、标题还有存在的时间。对于每一个 action，都有一个<b>当前值</b>的概念，对应点操作符，写作“.”。当前值 “.” 最初被初始化为调用模板时的参数，在当前例子中对应 github.IssuesSearchResult 类型的变量。模板中 {{.TotalCount}} 对应 action 将展开为结构体中 TotalCount 成员以默认的方式打印的值。<font color="red">模板中 {{range .Items}} 和 {{end}} 对应一个循环 action，因此它们之间的内容可能会被展开多次，</font>循环每次迭代的当前值对应当前的 Items 元素的值。

  {% endraw %}
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;在一个action中，<font color="red">| 操作符表示将前一个表达式的结果作为后一个函数的输入</font>，类似于 UNIX 中管道的概念。在 Title 这一行的 action 中，第二个操作是一个 printf 函数，是一个基于 fmt.Sprintf 实现的内置函数，所有模板都可以直接使用。对于 Age 部分，第二个动作是一个叫 daysAgo 的函数，它通过 time.Since 函数将 CreatedAt 成员转换为过去的时间长度：

  ```golang
  func daysAgo(t time.Time) int {
    return int(time.Since(t).Hours() / 24)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;需要注意的是 CreatedAt 的参数类型是 time.Time，并不是字符串。我们可以通过定义一些方法来控制字符串的格式化，一个类型同样可以定制自己的 JSON 编码和解码行为。time.Time 类型对应的 JSON 值是一个标准时间格式的字符串。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;生成模板的输出需要两个处理步骤。第一步是要分析模板并转为内部表示，第二步是基于指定的输入执行模板。分析模板部分一般只需要执行一次。下面的代码创建并分析上面定义的模板 templ。注意方法调用链的顺序：template.New 先创建并返回一个模板；Funcs 方法将 daysAgo 等自定义函数注册到模板中，并返回模板；最后调用 Parse 函数分析模板。

  ```golang
  report, err := template.New("report").
    Funcs(template.FuncMap{"daysAgo": daysAgo}).
    Parse(templ)
  if err != nil {
    log.Fatal(err)
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;因为模板通常在编译时就测试好了，如果模板解析失败将是一个致命的错误。template.Must 辅助函数可以简化这个致命错误的处理：它接受一个模板和一个error类型的参数，检测 error 是否为nil（如果不是nil则发出panic异常），然后返回传入的模板。我们将在第 4 章再讨论这个话题。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;一旦模板已经创建、注册了 daysAgo 函数、并通过分析和检测，我们就可以使用 github.IssuesSearchResult 作为输入源、os.Stdout 作为输出源来执行模板：

  ```golang
  var report = template.Must(template.New("issuelist").
    Funcs(template.FuncMap{"daysAgo": daysAgo}).
    Parse(templ))

  func main() {
    result, err := github.SearchIssues(os.Args[1:])
    if err != nil {
      log.Fatal(err)
    }
    if err := report.Execute(os.Stdout, result); err != nil {
      log.Fatal(err)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;程序输出一个纯文本报告：

  ```bash
  $ go build gopl.io/ch4/issuesreport
  $ ./issuesreport repo:golang/go is:open json decoder
  13 issues:
  ----------------------------------------
  Number: 5680
  User:      eaigner
  Title:     encoding/json: set key converter on en/decoder
  Age:       750 days
  ----------------------------------------
  Number: 6050
  User:      gopherbot
  Title:     encoding/json: provide tokenizer
  Age:       695 days
  ----------------------------------------
  ...
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;现在让我们转到 html/template 模板包。它使用和 text/template 包相同的API和模板语言，但是增加了一个将字符串自动转义特性，这可以避免输入字符串和HTML、JavaScript、CSS或URL语法产生冲突的问题。这个特性还可以避免一些长期存在的安全问题，比如通过生成 HTML 注入攻击，通过构造一个含有恶意代码的问题标题，这些都可能让模板输出错误的输出，从而让他们控制页面。

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的模板以 HTML 格式输出 issue 列表。注意 import 语句的不同：

  ```golang
  import "html/template"

  var issueList = template.Must(template.New("issuelist").Parse(`
  <h1>{{.TotalCount}} issues</h1>
  <table>
  <tr style='text-align: left'>
    <th>#</th>
    <th>State</th>
    <th>User</th>
    <th>Title</th>
  </tr>
  {{range .Items}}
  <tr>
    <td><a href='{{.HTMLURL}}'>{{.Number}}</a></td>
    <td>{{.State}}</td>
    <td><a href='{{.User.HTMLURL}}'>{{.User.Login}}</a></td>
    <td><a href='{{.HTMLURL}}'>{{.Title}}</a></td>
  </tr>
  {{end}}
  </table>
  `))
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面的命令将在新的模板上执行一个稍微不同的查询：

  ```bash
  $ go build gopl.io/ch4/issueshtml
  $ ./issueshtml repo:golang/go commenter:gopherbot json encoder >issues.html
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下图显示了在 web 浏览器中的效果图。每个 issue 包含到 Github 对应页面的链接。

  <div align="center">
    <img src="./pic/html_template.png" width=500px>
  </div>
  
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;上图中 issue 没有包含会对 HTML 格式产生冲突的特殊字符，但是我们马上将看到标题中含有 & 和 < 字符的issue。下面的命令选择了两个这样的issue：

  ```bash
  $ ./issueshtml repo:golang/go 3133 10535 >issues2.html
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下图显示了该查询的结果。注意，html/template 包已经自动将特殊字符转义，因此我们依然可以看到正确的字面值。如果我们使用 text/template 包的话，这 2 个 issue 将会产生错误，其中 “&lt;” 四个字符将会被当作小于字符 “<” 处理，同时 “<link>” 字符串将会被当作一个链接元素处理，它们都会导致 HTML 文档结构的改变，从而导致有未知的风险。

  <div align="center">
    <img src="./pic/html_problem_template.png" width=500px>
  </div>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们也可以通过对信任的 HTML 字符串使用 template.HTML 类型来抑制这种自动转义的行为。还有很多采用类型命名的字符串类型分别对应信任的 JavaScript、CSS 和 URL。下面的程序演示了两个使用不同类型的相同字符串产生的不同结果：A 是一个普通字符串，B 是一个信任的 template.HTML 字符串类型。

  ```golang
  func main() {
    const templ = `<p>A: {{.A}}</p><p>B: {{.B}}</p>`
    t := template.Must(template.New("escape").Parse(templ))
    var data struct {
      A string        // untrusted plain text
      B template.HTML // trusted HTML
    }
    data.A = "<b>Hello!</b>"
    data.B = "<b>Hello!</b>"
    if err := t.Execute(os.Stdout, data); err != nil {
      log.Fatal(err)
    }
  }
  ```

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下图显示了出现在浏览器中的模板输出。我们看到 A 的黑体标记被转义失效了，但是 B 没有。

  <div align="center">
    <img src="./pic/html_ok_template.png" width=500px>
  </div>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们这里只讲述了模板系统中最基本的特性。一如既往，如果想了解更多的信息，请自己查看包文档：

  ```bash
  $ go doc text/template
  $ go doc html/template
  ```
</div>

<!--ref-->

<h2>附录：参考源</h2>
<div class="div_learning_post">
<p>

1. github.com, <a href="https://github.com/gopl-zh/gopl-zh.github.com/tree/master/ch4">Go语言圣经(中文版) Chapter 4</a>
</p>
</div>

</body>