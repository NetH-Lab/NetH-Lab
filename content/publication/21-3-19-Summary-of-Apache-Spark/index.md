---
title: Summary of Apache Spark
summary: '[分布式系统][Spark][论文选读]Apache Spark 论文精读'
authors:
- Minel Huang
date: “2021-03-19T00:00:00Z”
publishDate: "2021-03-19T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Distributed Systems
- Spark
featured: false
---

更新时间：2021/03/19

参考资料：论文 - Apache Spark: Engine for Big Data Processing

# 1 Introduction
spark是什么？
当今大数据应用中，MapReduce提供了batch processing的分布式编程框架，Dremel提供了交互式SQL queries编程框架，Pregel提供了graph algorithm编程框架等等。但是我们依然期望一个统一的编程框架和系统，用于承载分布式的大数据处理，这就是Spark的目标。
Spark提出Resilient Distributed Datasets，称为RDDs，作为programming model，用于承载不同的工作负载。
# Programming Model
Spark中的关键编程抽象是RDD，它们是在群集中分区的对象的容错集合，可以并行操作。Users通过对数据"transformations"以创建RDDs。
