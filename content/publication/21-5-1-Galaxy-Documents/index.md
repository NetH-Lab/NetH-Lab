---
title: Galaxy Documents
summary: 'Galaxy系统文档'
authors:
- Minel Huang
date: “2021-05-01T00:00:00Z”
publishDate: "2021-05-01T00:00:00Z"

publication_types: ["0"]

tags: 
- Distributed Systems
- Galaxy
featured: false
---

更新时间：2021/05/01

# Galaxy-Client
功能：规定Job类，而后向Galaxy-Server提交任务

# Galaxy-Server
功能：
1. 承接Client提交的Job，并形成JobQueue。
2. 维护当前使用的Container状态，形成资源池Resource pool
3. 运行GalaxyScheduler-RM，为JobQueue中的Job分配container_num并调整任务处理先后顺序
4. 当Resource pool中的container满足Job需求时，提交该任务至YARN RM
5. 监控每个正在运行的YARN app状态，更新每个Job的在线估计完成时间，当Job完成后归还container并pop出JobQueue

# Scheduler

## GalaxyScheduler-RM
按照Scheduler Model要求，提取Job特性，形成<key, Value>对，value为Job特性。根据配置好的Scheduler Model关系，依次调用Scheduler Model函数，将<Job, Value>输入Scheduler Model。
而后将所有Scheduler Model的输出<key, num>对打包输入进Allocator中。
根据Allocator的输出<key, num>对，更新JobQueue

## GalaxyScheduler-AM
AM运行在被分配的container中，

# GalaxyIO

# Execuer Queue

# Class定义

## Job相关

### Job
Job类由Galaxy-Client生成，提交给Galaxy-Server
Job类囊括了计算任务的所有信息，包括：
- job_name
- job_id
- job_type: 任务类型，包括batch-based， latency-sensitive， machine learning
- client_path: 表明Client.class路径
- priority: 优先级，包含0，1，2
- state: 0-等待被处理，1-待提交，2-正在处理，3-完成
- container_num: 表示被分配的container数量
- predict

### JobQueue
将Job类形成队列

### JobSet

## Container相关

### Container

### ContainerSet
