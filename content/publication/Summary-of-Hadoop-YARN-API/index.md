---
title: Summary of Hadoop YARN API
summary: '[Hadoop YARN][分布式系统编程]Hadoop YARN api总结'
authors:
- Minel Huang
date: “2021-04-22T00:00:00Z”
publishDate: "2021-04-22T00:00:00Z"

publication_types: ["0"]

tags: 
- Study Notes
- Distributed Systems
- Hadoop YARN
featured: false
---

更新时间：2021/04/22
参考资料：[Hadoop JAVA api documents](https://hadoop.apache.org/docs/stable/api/index.html)
使用方法：在本博客查看各常用函数的概述内容，具体使用方法参考官方文档
博客将API分为Client, AM, container三类，有如下关系：
1. Client包含Client本身操作与创建AM的方法
2. AM包含AM本身操作与创建container的方法
3. container包含本身操作与运行container代码方法

每一类中旨在解释如何创建，如何控制，如何通信三类问题。

# 1 YarnClient类
package: org.apache.hadoop.yarn.client.api.YarnClient
summary：client相关函数

## 1.1 创建client相关函数
函数： createYarnClient()
返回值： YarnClient类
作用：Create a new instance of YarnClient.

## 1.2 application相关函数
client向RM最终提交的为一个app，故需要明确app的参数，并提供控制app的方法

### 1.2.1 创建app
函数： createApplication()
返回值： YarnClientApplication类
作用：Obtain a YarnClientApplication for a new application, which in turn contains the ApplicationSubmissionContext and GetNewApplicationResponse objects.

### 1.2.2 提交app
该函数用于YarnClientApplication类配置完成后，向RM提交app
函数：submitApplication
参数：ApplicationSubmissionContext类
返回值：ApplicationId

### 1.2.3 取消app
app控制函数

该函数用于取消app申请
函数：failApplicationAttempt
参数：ApplicationId

该函数用于取消正在运行的app
函数：killApplication
参数：ApplicationId

### 1.2.4 获取app状态
函数：getApplicationReport
参数：ApplicationId
返回值：ApplicationReport类