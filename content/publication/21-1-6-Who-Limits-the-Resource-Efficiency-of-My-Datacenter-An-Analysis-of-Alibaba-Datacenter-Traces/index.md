---
title: Summary of Who Limits the Resource Efficiency of My Datacenter-An Analysis of Alibaba Datacenter Traces
summary: '[数据中心][论文选读]了解现阶段数据中心的瓶颈为何处，以开展后续研究'
authors:
- Minel Huang
date: “2021-01-06T00:00:00Z”
publishDate: "2021-01-06T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Distributed Systems
- Datacenters
- Resource Efficiency
featured: false
---

更新时间：2020/01/06

参考资料：[Paper](https://dl.acm.org/doi/10.1145/3326285.3329074)

## 01 Introduction

2012年，Google统计了其数据中心下，CPU和内存的利用率仅有20%和40%；同年Amazon的数据中心中CPU的利用率仅为7%-17%，于是，工业和学术界开始着重研究如何提高resource efficiency in datacenters。

为什么资源效率如此低下？主要的障碍是co-located workloads。当我们将工作负载同时置于同一个硬件上运行时，不同的工作负载会互相造成干扰。这种影响严重干扰到了QoS服务，故如何在提供QoS服务的同时提高resource efficiency成为了难题。

当前的resource allocation strategy分静态与动态两种。其中广为使用的静态分配方法为将latency-critical (LC) applications和co-locating batch-processing applications分到不同的CPU cycles进行处理，如果机器出现性能瓶颈，批处理应用会被延后或者取消以保证LC应用的正常运行。在这个过程中，资源被重新调度，意味着更多的时间开销。另外的一些研究着重于动态schedule，静态和动态schedule是可以同时运用的。

在这篇paper中，我们想搞清楚在经过多年的研究后，目前的resource efficiency是怎样的，其瓶颈是否发生了改变，原因是什么？

本篇中我们发现了三个原因：

1. **内存成为数据中心的新的瓶颈**：正常运行中，我们观察到CPU和memory的使用率分别为38.2%和88.2%。为保护LC applications，在resource allocation时为其预定了大量的内存；同时，在计算机本地，memory被分为几个独立的区域，供LC应用与批处理应用分别使用。此外，缺乏有效的内存reclaim策略，使得被LC预定的内存不能reallocate给其他应用。这三个原因综合导致了内存使用率过高。
2. **批处理应用只能在很有限的资源中运行**：在阿里巴巴数据中心中，批处理应用可能对LC应用做出三种妥协：1）以较低的优先级利用有限的资源。2）会受到rescheduling的影响。3）在白天，供其利用的资源很有限，即使有大量的空闲资源；大多数批处理应用被安排到夜晚执行，此时需处理的任务总量又远大于工作负载。
3. **大量的JVMs使资源management变得复杂**：

