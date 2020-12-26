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

## **学习笔记**

Raft集群中，服务器的身份为Leader，Follower和Candidate三者其一，