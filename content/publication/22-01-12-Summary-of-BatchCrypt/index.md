---
title: Summary of BatchCrypt
summary: '[分布式机器学习][联邦学习][通信优化]BatchCrypt学习笔记'
authors:
- Minel Huang
date: “2022-01-11T00:00:00Z”
publishDate: "2021-01-12T00:00:00Z"

publication_types: ["0"]

tags: 
- Distributed Systems
- Distributed Machine Learning
- Federated Learning
- Communication Efficiency
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
  <h4>⚠ 转载请注明出处：<font color="red"><i>Maintainer: MinelHuang，更新日期：Jan.12 2022</i></font></h4>
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
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍BatchCrypt的应用场景和Problems。
    <p>
    &nbsp;&nbsp;&nbsp;&nbsp;Section 1. <a href="#section1"><font color="blue"><b>前言</b></font></a>：介绍同态加密、量化压缩等背景知识。
  </div>
</div>

<h2><a name="section1">1. 前言</a></h2>
<div class="div_learning_post_boder">
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考文献：<a href = "https://www.usenix.org/conference/atc20/presentation/zhang-chengliang">BatchCrypt: Efficient Homomorphic Encryption for Cross-Silo Federated Learning</a>. 2020. ATC
  <h2>场景和Problem</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;本文描述的场景为纵向联邦学习（Cross-Silo / Vertical / Heterogenous Federated Learning），即参与方持有相同id的数据，但每条数据的Feature不一定相同。通常，各参与方需要计算出local gradient updates，在加密后上传到一个center server，在该server中进行密态的aggregation，再返回给各参与方。最后参与方在本地解密，并更新本地模型，完成一次迭代。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;显然，在纵向联邦学习中，非常重要的一步是加密。目前state-of-art的加密方式为同态加密（additively homomorphic encryption, HE）。在同态加密后（通常使用Paillier加密技术），每一条数据变成了HE key-pair，其外在表现为数据量膨胀（每个值都会被扩展到至少2048bits）。这样的加密方法显然增加data transfer的开销，与明文相比，数据传递的开销增加了至少150x。故BatchCrypt希望解决的问题是，降低同态加密带来的计算和数据传递开销。<br>

  <h2>Solution</h2>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp; 在传统的方法中，每条数据的每个值都需要进行一次同态加密，那么可不可以使用batch encryption技术来对一个batch的数据进行一次加密呢？那么，原先是一个value对应一个密态数，使用batch encryption后，是一个batch的values对应一个密态数，这显然降低了加密和数据传递的开销（数据量被大大降低了）。故在BatchCrypt中，一个参与方首先quantizes its gradient values into low-bit integer representation，而后encodes a batch of quantized values to a long interger and encrypt it in one go。中文简单来说，是先组装成一组数据，而后编码，最后一同加密。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp; 这一部分的难点分为两种，其一是，如何让两个batch密态数完成密态下的加法运算？这样的运算是aggregation步骤中必须要实现的。BatchCrypt为此设计了一个quantization scheme，使梯度数据被quantized to signed integers uniformly distributed in a symmetric range。并且为了能支持简单的加法格式，BatchCrypt还开发了一个新的batch编码技术，其采用了two's compliment representation with two sign bits。
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp; 其二是在quantization前，需要先对gradients values进行clipped。文章提供了一个分析模型，称为dACIQ，用于分析最佳的clipping thresholds with the minimum cumulative error。<br>

  <h2>Contribution</h2>
  &nbsp;&nbsp;&nbsp;&nbsp;与传统的同态加密相比，BatchCrypt在3-layer全连接神经网络、AlexNet、LSTM model三个模型中分别达到了23x、71x和93x的加速，并分别降低了66x、71x和101x的通信负载。
</div>

<h2><a name="section2">2. Background Knowledge</a></h2>
<div class="div_learning_post_boder">
  <h3>Paillier半同态加密</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://zhuanlan.zhihu.com/p/420417626">Paillier半同态加密：原理、高效实现方法和应用</a>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;Paillier中的半同态加密指的是，支持密态下的加法，以及密文加明文两种计算，不支持密文乘密文。计算推导过程见上述链接。在本节中，将给出实际paillier实现中要注意的问题。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;现假设我们使用的p和q为512bit，则n为1024bit，所以对于任意一个明文<b>整数</b>m，其范围都不能超过<b>1024bit</b>。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;问题一，通常明文是64bit的浮点数，那么如何将其转换成整数以满足Paillier的输入条件呢？常规的做法是转移小数点的位置，将浮点数转换为：<b>base * exponent</b>的形式，例如：输入一个浮点数0.1，将其先转换为`1 * 10^{-1}`。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;现在我们把上述过程用更加标准的语言叙述一遍。设base的位宽必须小于1024bit，输入浮点数位宽无限，第一步是将浮点数编码成base * exponent的形式。其中，base < `2^{10}`。为达到此要求，首先用户要给出对浮点数的精度要求precision，例如`2^{-5}`(32 bit)，而后计算exponent = `log{precision}`。则输入x将转换为：fixpoint = int(round(x * 2^{exponent}))。于是，我们成功将一个浮点数转换成了定点数形式，注意fixpoint需小于1024bit。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;最后我们来说下密态加法，c1 + c2。当明文m1和m2都使用base * exponent表示法后，我们只对base进行加密，即：<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;c1 = [base1] * exp1<br>
  &nbsp;&nbsp;&nbsp;&nbsp;c2 = [base2] * exp2<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;设exp1 > exp2，则c1 + c2 = （[base1] * (exp1 / exp2) + [base2]） * exp2<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;至此，我们完成了密态下的加法。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;问题二，我们看到两个密态数相加，假设exp相差100倍，那么较大的数的base会扩大100倍，base是要小于1024bit的，则连续的加法，也即乘明文的操作有可能造成base溢出。所以在每一次密态运算后，需要进行解密，而后检查base是否溢出。当然，连续的多次密态运算依然是可能造成溢出的，只有在解密后才能检查是否溢出。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;问题三，负数如何表示？我们可以把1024bit编码空间分成3份，高位用于表示负数，低位用于表示整数，中间位用于保留防止溢出。<br>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;以上的实现方式均为FATE开源项目中的encode函数的实现方式，更加详细的代码请参考<a href="https://github.com/FederatedAI/FATE">Federated AI Technology Enabler</a>，具体位置为：python/federatedml/secureprotol/encode.py<br>

  <h3>量化与压缩</h3>
  <p>
  &nbsp;&nbsp;&nbsp;&nbsp;参考资料：<a href="https://blog.csdn.net/jinzhuojun/article/details/106955059">闲话模型压缩之量化（Quantization）篇</a>
</div>

<h2><a name="section2">3. BatchCrypt</a></h2>
<div class="div_learning_post_boder">
  
</div>
