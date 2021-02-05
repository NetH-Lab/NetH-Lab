---
title: Ubuntu服务器版安装

summary: 关于Ubuntu安装的学习历程的总结。
authors:
- Minel Huang
date: “2020-12-28T00:00:00Z”
publishDate: "2020-12-28T00:00:00Z"

publication_types: ["0"]

tags: ["Ubuntu"]
featured: false
---

更新时间：2020-12-18

参考资料：

1. 官方文档：https://ubuntu.com/tutorials/install-ubuntu-server#1-overview
2. 博客：https://blog.csdn.net/baidu_36602427/article/details/86548203

## 1. 系统盘制作

使用Ubuntu系统制作软件UltraISO制作系统盘。

首先在官网上下载Ubuntu服务器版，博主使用的版本为ubuntu-20.04.1-live-server-amd64，系统盘制作完毕后开始安装ubuntu

## 2. Ubuntu server安装

博主将使用一台独立的计算机进行安装

将系统盘插入计算机后，按f12进入bios界面。选择后ubuntu会自检系统完整性，而后选择语言和键盘布局，都使用English即可。

网络配置：使用IPV4 DHCP配置，不影响最终安装。

Proxy配置：若希望在互联网中显示，可配置一个http地址，本次安装先不配置。

Mirror address：这里我们会在后面修改成清华源

storage configuration：使用Custom进行配置。如果是像博主一样，整个计算机都使用Ubuntu系统，则可以先将磁盘Reformat(清楚原始分区)，再进行手动配置如下：

总空间：931.512G

根目录 10G

/boot 1024M

/home 500G--------主要存储空间

/srv 100G--------作为对外服务用户的数据存储空间

/usr 200G--------存储系统软件资源，如指令集

/var 10G--------系统日志

/var/lib 10G

SWAP 10G--------若内存资源短缺，则临时使用这部分空间

预留40G空间备用

profile setup：配置服务器的用户信息

ssh：服务器需要ssh连接，但不需要安装github上的identify

snap：应用程序快照，先不安装，在应用的时候需要什么再安装什么即可。



