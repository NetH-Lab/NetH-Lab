---
title: Summary of Apache Hadoop YARN
summary: Apache Hadoop YARN 论文精读
authors:
- Minel Huang
date: “2021-02-05T00:00:00Z”
publishDate: "2021-02-05T00:00:00Z"

publication_types: ["0"]

tags: 
- Distributed Systems
- Datacenters
- Resource efficiency
featured: false
---

更新时间：2021/02/05

参考资料：论文 - Apache Hadoop YARN: Yet Another Resource Negotiator

## 1 Introduction

早期Hadoop的主要问题：

- 编程人员几乎只能使用MapReduce框架，这导致开发者对MapReduce进行滥用，比如仅使用Map函数以达到期望的功能
- Hadoop集中处理jobs' control flow，这导致在扩展规模时出现问题

为解决上述问题，设计了YARN系统，其主要特点如下：

- 将resource management功能与编程模型分离
- 支持多种编程框架
- 引入intra-application, execution flow和dynamic optimizations

## 2 History and rationale

本节描述为什么需要YARN

Requirement1：扩展性。Yahoo! 的工作集群包含800台设备，用于处理WebMap应用，而该应用的规模愈发庞大，这要求Yahoo! 使用更大的集群来处理这些数据。故YARN需要有高的可扩展性。

Requirement2：多租户（Multi-tenancy）。该工作集群不仅能够支持MapReduce业务，还应支持例如spam filtering. content optimization等其他业务

Requirement3：Serviceability。多租户的另一类要求，工作集群需要支持动态给不同租户分配一定数量的空闲机器，即要求运行在集群上的系统需要维护一个共享池（shared pool）。这一方面，Yahoo! 的HoD（Hadoop on Demand）系统实现的非常好。

Requirement4：Locality awareness。在HoD系统中，Torque在分配节点时是不考虑节点的位置的，然而运行map任务的节点，理想状态是和输入数据十分接近的。当未考虑节点位置便直接分配，是不利于MapReduce高效运行的。

Requirement5：High Cluster Utilization。HoD中，工作集群的大小并不会随着任务规模的变化而变化，这导致集群中可能出现大量的休眠机器。例如，假设HoD为一个用户预分配了10台机器，以运行一串任务，其中1项任务仅为1个reduce任务，故只需要1台机器，于是剩下9台机器休眠，最后使得利用率降低。静态分配机器是一种较为低效的资源分配模式，因此Yahoo! 开始逐渐使用shared clusters以解决上述问题。

Requirement6：可靠性。

Requirement7：安全。

Requirement8：支持多种Programming Model。MapReduce并不是所有任务的理想编程模型。比如机器学习任务，如果将对datasets的操作视为一次MapReduce，而后多次迭代以完成整个Machine Learning任务，那么整个的完成时间会大大增加；对于一些图算法也是如此，图问题最好使用BSP模型来解决，而不是用繁重的，all-to-all通信的大规模MapReduce任务。这种对MapReduce的不正当使用使得集群利用率大大降低，故YARN需要支持更多的编程模型。

Requirement9：Flexible Resource Model。MapReduce中包含Map阶段和Reduce阶段，对于每一个上交的任务，用户需要调整这两个阶段间的overlap。

Requirement10：向后兼容。

## 3 Architecture