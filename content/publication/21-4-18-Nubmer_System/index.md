---
title: Number System
summary: '[数字电路基础][数值系统]'
authors:
- Zobin Huang
tags: 
- Digital Circuit
- Computer Architecture
- Combinational Circuit
date: “2021-04-18T00:00:00Z”
publishDate: "2021-04-18T00:00:00Z"
featured: false
mathjax: true
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

<div align="center" class="div_indicate_source">
  <h4>⚠ 转载请注明出处：<font color="red"><i>作者：ZobinHuang，更新日期：Feb.6 2021</i></font></h4>
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
<!--<img src="./pic/xxx.png" width=30% style="float:left">-->
<!--正文-->
<!--
<p>
&nbsp;&nbsp;&nbsp;&nbsp;公式：<span>`\overline{A}\overline{B}`</span>
</p>
-->

<div class="div_learning_post">
<p>
&nbsp;&nbsp;&nbsp;&nbsp;本文将描述计算机内部是如何表示数据并且进行各种计算操作的。
</p>
</div>

<h2>1. 二进制基本原理</h2>
<div class="div_learning_post">
  <h3>1.1 二进制</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;对于计算机，使用的是我们熟知的二进制表示方法：
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`D=(\underbrace{d_(p1)}_{MSB}d_(p2)...d_(1)d_(0)\underbrace{.}_{小数点}d_(-1)d_(-2)...\underbrace{d_(-n)}_{LSB})_2=\sum_{i=-n}^{p1}d_i*2^i`<span>
  </p>

  <h3>1.2 二进制与十进制的相互转化</h3>
  <h4>1.2.1 二进制转十进制</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;<span>对于任意的数值系统来说，其转换为十进制的方法都是（尤其注意小数部分）：
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`D=D_(I n t e g e r)+D_(Fraction)=\underbrace{(d_(n-1)*r^(n-1)+d_(n-2)*r^(n-2)+...+d_(1)*r^(1)+d_(0)*r^(0))}_{I n t e g e r}+\underbrace{(d_(-1)*r^(-1)+d_(-2)*r^(-2)+...+d_(-m)*r^(-m))}_{Fraction}`</span>
  </p>

  <h4>1.2.2 十进制转二进制</h4>
  <h5>(1) <b>整数部分</b>(以十进制数179为例)</h5>
  <div align="center">
    <span>`2\lfloor\underline{179} ..... 1`</span>
    <br><span>`2\lfloor\underline{89} ...... 1`</span>
    <br><span>`2\lfloor\underline{44} ...... 0`</span>
    <br><span>`2\lfloor\underline{22} ...... 0`</span>
    <br><span>`2\lfloor\underline{11} ...... 1`</span>
    <br><span>`2\lfloor\underline{5} ....... 1`</span>
    <br><span>`2\lfloor\underline{2} ....... 0`</span>
    <br><span>`2\lfloor\underline{1} ....... 1`</span>
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;如上图所示，我们不断地将179除以2，将每一次除法得到的余数记录下来，最终二进制的结果就是从底到顶的01序列，即10110011。这种计算方法的原理是：<b>第n次除以二时，得到的余数就是所需要的<span>`2^(n-1)`</span>的个数</b>（在二进制中只有0个或者1个）。具体演算如下：
    </p>
  </div>
  <div align="left">
    <span>`179=2*89+\underbrace{1*1}_{2^0}`</span>
    <br><span>`2*89=2*(2*44+1)=4*44+\underbrace{1*2}_{2^1}`</span>
    <br><span>`4*44=4*(2*22+0)=8*22+\underbrace{0*4}_{2^2}`</span>
    <br><span>`8*22=8*(2*11+0)=16*11+\underbrace{0*8}_{2^3}`</span>
    <br><span>`16*11=16*(2*5+1)=32*5+\underbrace{1*16}_{2^4}`</span>
    <br><span>`32*5=32*(2*2+1)=64*2+\underbrace{1*32}_{2^5}`</span>
    <br><span>`64*2=64*(2*1+0)=128*1+\underbrace{0*64}_{2^6}`</span>
    <br><span>`128*1=\underbrace{1*128}_{2^7}`</span>
    <br>&nbsp;&nbsp;&nbsp;&nbsp;因此，<span>`179=1*2^0+1*2^1+0*2^2+0*2^3+1*2^4+1*2^5+0*2^6+1*2^7=(10110011)_2`</span>
  </div>

  <h5>(2) <b>小数部分</b>(以十进制数0.17为例)</h5>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;下面讲述一下整个演算的过程和原理。首先我们将0.17连续乘以2直到积大于1为止。
  </p>
  <div align="center">
    <span>`0.17*2=0.34 ...... 0`</span>
    <br><span>`0.34*2=0.68 ...... 0`</span>
    <br><span>`0.68*2=1.36 ...... 1`</span>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们发现乘了3次之后积是1.36，因此组成0.17的最大的2的整数次幂是<span>`2^(-3)=1/(2^3)=0.125`</span>。然后我们就得开始找组成<span>`(0.17-0.125)=0.045`</span>这个部分的2的整数次幂。其实我们不用独立去计算下一个要找哪个数的最大2的整数次幂，更方便的方法是我们取出积1.36的小数部分，即0.36，因为0.36是<span>`(0.17-0.125)=0.045`</span>被放大<span>`2^3=8`</span>倍的结果。
  </p>
  <div align="center">
  <span>`0.36*2=0.72 ...... 0`</span>
  <br><span>`0.72*2=1.44 ...... 1`</span>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;我们使用同样的连续乘2来处理0.36直到积大于1，乘了2次2之后我们得到积1.44，因此组成0.36的最大的2的整数次幂是<span>`2^(-2)=1/(2^2)=0.25`</span>，除去前面被放大的<span>`2^3=8`</span>倍，组成0.045的的最大的2的整数次幂是<span>`0.25/8=2^(-5)=1/(2^5)=0.03125`</span>。如上道理，我们取出0.44并继续找组成0.44以及后续数的最大2的整数次幂。根据精度要求找到在精度范围内所有2的整数次幂，就能完成小数部分的二进制转化工作。
  </p>
  <div align="center">
  <span>`0.44*2=0.88 ...... 0`</span>
  <br><span>`0.88*2=1.76 ...... 1`</span>
  <br><span>`0.76*2=1.52 ...... 1`</span>
  <br><span>`......`</span>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;综上，我们得到<span>`0.17`</span>的二进制表示方法是<span>`2^(-3)+2^(-5)+2^(-7)+2^(-8)=(0.00101011)_2`</span>。
  </p>
</div>

<h2>2. 二进制四则运算</h2>
<div class="div_learning_post">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;二进制四则运算的运算规则和十进制相似，在脑中过一遍计算过程有助于后序讨论如何用电路实现二进制运算电路。
  </p>

  <h4>1.3.1 二进制加法</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;二进制加法规则与十进制相似，不再赘述。
  </p>
  <div align="center">
    <img src="./pic/bin_add.png" width=500px>
  </div>

  <h4>1.3.2 二进制减法</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;二进制减法规则与十进制相似，区别就是每一次不够减借位是借2。
  </p>
  <div align="center">
    <img src="./pic/bin_sub.png" width=500px>
  </div>

  <h4>1.3.3 二进制乘法</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;二进制乘法规则与十进制相似。通常使用移位和加法的操作来实现乘法。
  </p>
  <div align="center">
    <img src="./pic/bin_mul.png" width=500px>
  </div>

  <h4>1.3.4 二进制除法</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;二进制除法规则与十进制相似，不再赘述。
  </p>
  <div align="center">
    <img src="./pic/bin_div.png" width=500px>
  </div>
</div>

<h2>3. 二进制负数的表示</h2>
<div class="div_learning_post">
  <h3>3.1 我们为什么要二进制负数表示？</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;后面的文章将会看到，对于加法器可以很简单地用数字电路搭建出来，因此可以使用电路很简单地实现二进制数的加法操作。其它的运算都需要转化为加法操作。对于减法，最直接的办法就是想办法使用二进制数来表示负数，然后就能将 <span>`A-B`</span> 转化为 <span>`A+(-B)`</span>。因此我们先明确一点：讨论二进制负数表示的意义在于可以找到一种负数表示方法，使得加法电路可以用来做减法操作。在本节我们会介绍几种二进制表示负数的方法，其中我们重点关注补码。
  </p>

  <h3>3.2 <b>模(Modulo)</b>的概念</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;模指的是一个有限计数系统的计数范围。比如时钟，模是12，计数范围是0~11；四位寄存器，模是<span>`2^4=16`</span>，计数范围是0~15。有限计数系统的最大特点在于其有<b>补数</b>的性质。举个例子，假设当前时针指向11点，而准确时间是8点，调整时间可有以下两种拨法：一种是倒拨3小时，即<span>`11-3=8`</span>，另一种是顺拨9小时即<span>`11+9=12+8=8`</span>，可以看出，在以模为12的系统中，加9和减3效果是一样的，因此凡是减3运算，都可以用加9来代替。对“模”12而言，9和3互为补数（二者相加等于模）。所以我们可以得出一个结论：<b>在有限计数系统中，减一个数等于加上它的补数，从而实现将减法运算转化为加法运算的目的</b>。
  </p>

  <h3>3.3 Twos'-Complement(补码)表示法</h3>
  <div align="center">
    <img src="./pic/twos_comp.png" width=500px>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;基于上面时钟系统的讨论，可以发现，我们能将补数用于我们的二进制编码系统，从而实现将减法操作转化为加法操作，以使用我们的加法电路来实现减法。我们将这套系统称为<b>补码</b>系统，如上图所示。对于正数和0，我们保持和正常计数方法一样不变。<b>对于负数，我们使它们的值等于它们绝对值所对应补数的值</b>。例如上图所示的四位系统，-1的值为 <span>`f_(补数)(1)=15=(1111)_2`</span> ，-3的值为 <span>`f_(补数)(3)=13=(1101)_2`</span>。你可能会有疑问，那这些补数(i.e. 前面例子中的15，13)怎么表示呢？由于n位补码系统的表示范围是 <span>`2^(-(n-1)) ~ 2^(n-1)-1`</span>，如上面的例子中是-8~7，因此负数们所对应的补数根本就无法出现在计算系统中，因此就没有了这些补数该如何表示的顾虑。下面，我们考虑一个四位的计算机系统，其模是16，对计算式<span>`5-3`</span>的计算过程进行分析。
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`5-3`</span>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`=5+(16-3)`</span>               <i>【套用补数的概念】</i>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`=5+((10000)_2-(0011)_2)`</span>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`=5+(1+(1111)_2-(0011)_2)`</span>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`=5+(1+(1100)_2)`</span>        <i>【此处我们可以发现负数的补数的简单求法：该负数的反码+1】</i>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`=(0101)_2+(1101)_2`</span>     <i>【-3的补码即1101】</i>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`=(10010)_2`</span>             <i>【由于是4位计算机系统，因此最高位1溢出】</i>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`=(0010)_2`</span>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;<span>`=2`</span>
  <br>&nbsp;&nbsp;&nbsp;&nbsp;综上，我们找到了补码这一能够让我们方便地使用加法电路完成减法操作的计数系统。负数的补码值其实有另一种简单的求法，上面的过程只是为了更好的理解补码的意义所在，这种简单的求法就是众所周知的：负数反码值加一。下面简单介绍其它两种计数方法：原码和反码。
  </p>

  <h3>3.4 Signed-Magnitude(原码)表示法</h3>
  <div align="center">
    <img src="./pic/sm.png" width=500px>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;原码表示法采用最高一位比特作为符号位，0代表正数，1代表负数。原码的优势在于取反操作十分简单，但是其缺点在于：(1)有两种0的表示方法；(2)无法对原码进行计算操作。因此显然原码并不适用于我们的需求。
  </p>

  <h3>3.5 Ones'-Complement(反码)表示法</h3>
  <div align="center">
    <img src="./pic/ones_comp.png" width=500px>
    <img src="./pic/ones_comp_add.png" width=100%>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;反码表示法表示负数的方法是反转每一位比特，如上第一幅图所示。在上面第二副图中，我们发现对于MSB位的carry out为0的加法计算，是可以通过直接对反码进行计算得到正确结果的，但是如果MSB位的carry out为1，则结果会出问题，必须在结尾在加上一个1，才能得到正确的结果，这种现象会导致计算电路变得十分复杂。
  </p>

  <h3>3.6 总结</h3>
  <div align="center">
    <img src="./pic/convert.png" width=100%>
  </div>
</div>

<h2>4. 二进制小数的表示</h2>
<div class="div_learning_post">
  <h3>4.1 定点数</h3>
  <h4>4.1.1 定点数表示方法</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;定点数表示法指的是使用某些特定比特位作为整数部分，剩余特定比特位作为小数部分。以数6.75为例，若我们使用四个比特表示整数部分，另四个比特表示小数部分，则可以得到<span>`(01101100)_2`</span>，我们在其中其实省略了一个小数点，即<span>`(0110.1100)_2`</span>。
  </p>
  <h4>4.1.2 使用补码来表示有符号定点数</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;值得一提的是带符号的定点数，我们同样可以使用补码来表示定点数。以数-6.75为例，根据我们上面讨论的补码的定义：<b>对于负数，我们使它们的补码值等于它们的绝对值所对应补数的值</b>，我们考虑6.75所对应的补数的值，<span>`6.75=(00100110)_2`</span>，其补数为<span>`2^9-6.75=(100000000)_2-(00100110)_2=(11011010)_2`</span>，因此我们得到-6.75的补码为<span>`(11011010)_2`</span>。我们可以发现其实这正是-6.75的反码值<span>`(11011001)_2`</span>加1的结果，因此对于有符号的浮点数我们同样可以使用反码低位加一的方法来求得补码值，只不过对于浮点数加一的位置不在<span>`2^0`</span>位上，而在最低位上，在上面的-6.75(<span>`(11011010)_2`</span>)的例子中，加一的位置在<span>`2^(-4)`</span>位上。
  </p>
  <h3>4.2 浮点数</h3>
  <h4>4.2.1 浮点数的组成</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;与科学计数法类似，浮点数解决了整数和小数位长度固定的限制，允许表示一个非常大或非常小的数。浮点数的组成如下所示：
  </p>
  <div align="center">
    <span>`\pmM*B^E`</span>
  </div>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;其中<span>`M`</span>是尾数(mantissa)，<span>`B`</span>是基数(base)，<span>`E`</span>是阶码(exponent)。在二进制中，基数<span>`B`</span>是2。
  </p>
  <h4>4.2.2 计算机浮点数的表示方法</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本节将以32位浮点数为例，阐述浮点数在实际计算机中的表示方法。
  </p>
  <h5>(1) 基本版本</h5>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;以十进制数228为例，<span>`228=(11100100)_2=(1.11001)_2*2^7`</span>。在基本版本的浮点表示法中，228的表示方法如下所示，使用高1位作为符号位，随后8位作为阶码，低23位作为尾码。
  </p>

  <table align="center">
    <caption>基本版本32浮点数表示方法</caption>
    <tr>
      <th>符号</th>
      <th>阶码</th>
      <th>尾数</th>
    </tr>
    <tr>
      <td>1 bit</td>
      <td>8 bits</td>
      <td>23 bits</td>
    </tr>
    <tr>
      <td>0</td>
      <td>0000 0111</td>
      <td>111 0010 0000 0000 0000 0000</td>
    </tr>
  </table>

  <h5>(2) 省略前导1版本</h5>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;细心的话可以发现，如果采用科学计数法表示二进制，尾码的最高位（即小数点前1位）永远为1，因此在浮点数的表示中我们可以将其省略，低23位用于存储小数点后23 bits的数据。相比于第一种版本，尾码将多出一比特的精度。下面给出十进制数228的省略前导1版本32浮点数表示方法。
  </p>
  <table align="center">
    <caption>省略前导1版本32浮点数表示方法</caption>
    <tr>
      <th>符号</th>
      <th>阶码</th>
      <th>尾数</th>
    </tr>
    <tr>
      <td>1 bit</td>
      <td>8 bits</td>
      <td>23 bits</td>
    </tr>
    <tr>
      <td>0</td>
      <td>0000 0111</td>
      <td>110 0100 0000 0000 0000 0000</td>
    </tr>
  </table>

  <h5>(3) IEEE 754版本</h5>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;再细心的话还可以发现，我们目前的阶码只能表示正数。因此在IEEE 754标准中，给阶码段加了一个偏置数127。例如，对于阶码7，阶码字段的值应是<span>`7+127=134=(10000110)_2`</span>。对于阶码-4，阶码字段的值应是<span>`-4+127=123=(01111011)_2`</span>。下面给出十进制数228的IEEE 754版本32浮点数表示方法。
  </p>
  <table align="center">
    <caption>IEEE 754版本32浮点数表示方法</caption>
    <tr>
      <th>符号</th>
      <th>阶码</th>
      <th>尾数</th>
    </tr>
    <tr>
      <td>1 bit</td>
      <td>8 bits</td>
      <td>23 bits</td>
    </tr>
    <tr>
      <td>0</td>
      <td>1000 0110</td>
      <td>110 0100 0000 0000 0000 0000</td>
    </tr>
  </table>

  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;对于一些特殊的数，如<span>`0`</span>，<span>`\pm\infty`</span>，<span>`NaN`</span>（非法值），IEEE754采用下面的方法进行表示。
  </p>
  <table align="center">
    <caption>IEEE 754对<span>`0`</span>，<span>`\pm\infty`</span>，<span>`NaN`</span>的表示方法</caption>
    <tr>
      <th>数字</th>
      <th>符号</th>
      <th>阶码</th>
      <th>尾数</th>
    </tr>
    <tr>
      <td>0</td>
      <td>X</td>
      <td>0000 0000</td>
      <td>000 0000 0000 0000 0000 0000</td>
    </tr>
    <tr>
      <td><span>`\infty`</span></td>
      <td>0</td>
      <td>1111 1111</td>
      <td>000 0000 0000 0000 0000 0000</td>
    </tr>
    <tr>
      <td><span>`-\infty`</span></td>
      <td>1</td>
      <td>1111 1111</td>
      <td>000 0000 0000 0000 0000 0000</td>
    </tr>
    <tr>
      <td><span>`NaN`</span></td>
      <td>X</td>
      <td>1111 1111</td>
      <td>非零值</td>
    </tr>
  </table>

  <h4>4.2.3 单、双精度浮点数</h4>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;IEEE 754中同时规定了32位（单精度浮点数，signle-precision，signle或float）和64位（双精度浮点数，double-precision，double）浮点数表示法，具体如下所示。
  </p>
  <table align="center">
    <caption>单、双精度浮点数</caption>
    <tr>
      <th>格式</th>
      <th>总位数</th>
      <th>符号位数</th>
      <th>阶码位数</th>
      <th>小数位数</th>
    </tr>
    <tr>
      <td>单精度浮点数</td>
      <td>32 bits</td>
      <td>1 bit</td>
      <td>8 bits</td>
      <td>23 bits</td>
    </tr>
    <tr>
      <td>双精度浮点数</td>
      <td>64 bits</td>
      <td>1 bit</td>
      <td>11 bits</td>
      <td>52 bits</td>
    </tr>
  </table>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;单精度浮点数的表示范围是<span>`\pm1.175 494*10^(-38) ~ \pm3.402 824*10^(38)`</span>，具有7位有效的十进制数字（<span>`2^(-24)=10^(-7)`</span>，24是因为采用省略前导1后，一共有23位表示小数部分，加上前导1一共有24位精度）。双精度浮点数的表示范围是<span>`\pm2.225 073 858 507 20*10^(-308) ~ \pm1.797 693 134 862 32*10^(308)`</span>，具有15位有效的十进制数字。
  </p>
</div>

<!--ref-->

<h2>附录：参考</h2>
<div class="div_learning_post">
  <p>

  1. David Money Harris, Sarah L, Harris, 机械工业出版社, <b>数字设计和计算机体系结构</b>

  2. 电子科技大学, <b>Digtal Logic Circuit and System 课程课件（2018秋季）</b>

  3. CSDN博主leonliu06，[补码原理——负数为什么要用补码表示](https://blog.csdn.net/leonliu06/article/details/78685197?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.control)

  </p>
</div>

</body>