---
title: Summary of Raft Algorithm

summary: Raft算法学习笔记与总结
authors:
- Minel Huang

date: “2020-11-15T00:00:00Z”
publishDate: "2020-11-15T00:00:00Z"

publication_types: ["0"]

tags: 
- Raft
- Distributed Consensus Algorithms
featured: false
---

参考资料：

1. 《分布式一致性算法开发实践》. 赵辰. 北京大学出版社
2.  Ongaro D, Ousterhout J. In search of an understandable consensus algorithm[C]//2014 {USENIX} Annual Technical Conference ({USENIX}{ATC} 14). 2014: 305-319.
3.  https://www.cnblogs.com/davidwang456/articles/9001331.html
4.  https://www.cnblogs.com/xybaby/p/10124083.html
5. http://thesecretlivesofdata.com/raft/

## **学习笔记**

分布式一致性算法在哪里应用：replicated state machines

一致性的目的是，让所有的机器处于同一个状态。而机器是根据指令进行操作的，换句话说，如果我们能让机器按照同样的指令进行运作，它们就可以达到同样的状态。所以，一致性问题在本论文中被简化成各个计算机的指令集相同。在数据库中，指令的最小单元为Log，所以后文将以数据库为应用场景，叙述Raft算法。

### 一致性所应用的场景？

在开始Raft算法之前，我想先考虑一个问题，一致性算法到底应用在哪里？

首先，我们先从实际的应用场景出发。在很多数据中心中，重要数据需要实时进行备份，比如淘宝中的交易数据。所以在这一场景中，通常使用三个数据库进行描述：A为master，B和C为Backup。

![](./01.jpg)

对于一组request而言，为了保证每一条信息不会丢失，A, B, C数据库需要时刻保持强一致性，故一条request将在ABC都被写入后，再处理下一条request。显然，等待A, B, C都写入成功是需要时间开销的，对于淘宝、支付宝而言，时间开销过高是不能容忍的，所以引入一致性算法，在算法层面上降低备份的时间，我将带着这个问题去探讨Raft算法。

所以在Raft算法中，一致性问题被定位replicated log问题。在这里经过探讨我们发现，log一致并不能完全决定machine state，还需要A，B，C三台机器**存储的内容是一致**的。所以在目前看来，Raft算法仅仅能适用于备份问题，而不是一个通用的一致性算法。我也同样会带着这个问题来阅读论文。

### 一致性算法需要考虑什么

replicated log使各个机器状态一致，为主要问题。同时，算法还需要考虑“容错性”。对于一个分布式系统而言，“出错”是时刻可能发生的。由于使用了更多的机器来提高系统处理速度，整个系统中会出现错误的概率也随之提高。所以在一致性算法中，一定要考虑某个节点出错后的应对措施。在Raft中，作者考虑到了时延、网络分割、丢包、duplication（重复）和重排序问题。

### Raft Consensus Algorithm

Raft是一种leader-based算法，假设集群中存在一个leader，那么replicated log任务可以交给leader去做，这样大大简化了算法的理解难度。Raft可以被分为三类过程：normal operation，leader election和log replication。

### Raft basics

集群中包含三种状态的机器：*leader*, *follower*, *candidate*。在normal operation中，只有一个leader，其他的servers都是followers。followers都是被动的，即不会发布requests，仅会被动产生responds。在leader election中，candidates用于产生leader。Raft中使用term来表示时间长度，以leader election开始。

term的更新：每个节点都会维护本地的current，在通信过程中将捎带传输term。如果节点的term小于其他的，那么更新term至较大的值；如果candidate或leader发现它的term is out of date，它会立刻转变成follower state；如果节点收到一个stale term，则拒绝此次request

### Leader election

Raft使用heartbeat机制，heartbeat由leader发出，当节点在一定时间内没有接受到heartbeat，则开启leader election。