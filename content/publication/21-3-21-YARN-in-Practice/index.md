---
title: YARN in Practice
summary: '[分布式系统][Hadoop YARN]YARN实践'
authors:
- Minel Huang
date: “2021-03-21T00:00:00Z”
publishDate: "2021-03-21T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Tools
- Distributed Systems
- Hadoop
featured: false
---

更新时间：2021/03/21

参考资料：
1. [书籍：Hadoop in Practice](https://livebook.manning.com/book/hadoop-in-practice-second-edition)

前置内容：
1. [Summary of Apache Hadoop YARN](https://neth-lab.netlify.app/publication/21-3-19-summary-of-apache-hadoop-yarn/)
# 1 Overview
YARN与其他Hadoop组件的关系图，也为Hadoop 2结构框图：
![](./1.1.jpg)
YARN内部结构图：
![](./1.2.jpg)
YARN application与YARN的关系：
![](./1.3.jpg)
可以看到，YARN app需要三个组件，YARN client，ApplicationMaster，Container
YARN client：与RM交互，创建AM
AM：YARN app的master process，并且还需要管理运行app的containers，向RM请求containers，在NM上真正部署containers

# 2 YARN Configuration
参考资料：[https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-common/yarn-default.xml](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-common/yarn-default.xml)
如何配置？
通过.xml文件配置YARN，编写property，具体参考官方文档
如何查看？
在Hadoop页面的Configuration中可以查看
![](./2.1.jpg)
如果您不明白配置文件中的值，您可以：
1. 测试yarn-site.xml中的property values，如果某个entry没有配置，则运行默认配置
2. 在RM UI中的Configuration里观察正在运行的配置，对比官方文档的介绍来查明其意义

# 3 Distributed Shell
官方的YARN已经绑定了两个应用程序，分别是MapReduce 2和DistributedShell，现在我们尝试运行起DistributedShell。
在命令行中输入：
```bash
hadoop org.apache.hadoop.yarn.applications.distributedshell.Client \
-debug \
-shell_command find \
-shell_args '`pwd`' \
-jar ${HADOOP_HOME}/share/hadoop/yarn/*-distributedshell-*.jar \
-container_memory 350 \
-master_memory 350
```
如果在最后出现
INFO distributedshell.Client: Application completed successfully
代表运行成功。
这一串代码的意义是，使用DistributedShell运行一个find命令，但显然在输出中，我们并没有看到任何携带'Find'的语句。这是因为AM将find命令实际运行在分里的containers中，标准输出被重定向到container的log output目录下。所以如果想看到find命令的output，我们需要访问那个directory。

## 3.1 访问container logs
方法：使用CLI和UI访问
首先我们需要知道app ID，在命令行输出中查找：
![](./3.1.jpg)  
在YARN中，可以使用CLI或者UI获知logs，其中，使用CLI需在yarn-site.xml中配置yarn.log-aggregation-enable，而后通过：
yarn logs -applicationId application_1400286711208_0001
访问Logs

UI方式，直接在浏览器中输入http://192.168.137.101:8088/cluster，进入UI界面
![](./3.2.jpg)

# 4 在YARN上运行MapReduce
## 4.1 剖析YARN MapReduce
![](./4.1.jpg)
- step 1: clients将input分离开并写入HDFS
- step 2: RM create AM for MapReduce job
- step 3, 4: RM为AM分配container，并通知NM创建AM container。注意，AM也是一个container，所以是需要被创建的。
- step 5: MapReduce AM(MRAM)从HDFS上获取input文件
- step 6: MRAM向RM请求map containers，并要求containers的位置靠近input files存储空间
- step 7, 8: RM向MARM分配containers，map和reduce分别开始工作