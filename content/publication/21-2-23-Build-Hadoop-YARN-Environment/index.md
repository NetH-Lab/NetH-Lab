---
title: Configuration of Hadoop YARN Environment
summary: '[工具][Hadoop安装]Hadoop YARN集群环境配置'
authors:
- Minel Huang
date: “2021-02-23T00:00:00Z”
publishDate: "2021-02-23T00:00:00Z"

publication_types: ["0"]

tags: 
- Tools
- Open Source System
- Distributed Systems
- Hadoop
featured: false
---

更新时间：2020/02/23

Ubuntu版本：20.04.1

Hadoop版本：3.2.1

参考资料：

1. 官方文档：[link](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html)

## 01 计算机预配置

一、修改ubuntu名称，以分辨不同的计算机：

打开/etc/hostname，将名称修改为ubuntu01

二、配置ubuntu为静态ip

参考资料：[博客](https://www.linuxidc.com/Linux/2017-02/140137.htm)

1. 使用ifconfig查看本机网卡，笔者为ens33

2. sudo vim /etc/network/interfaces

   auto ens33
   iface ens33 inet static
   address 192.168.18.101  #配置静态ip 与主机ip网段一致
   gateway 192.168.18.1    #与主机网关相同
   netmask 255.255.255.0

3. reboot

若使用ubuntu桌面版，可以使用图形化界面。即点击右上角倒三角，进入Wired Settings，在IPV4中手动配置即可

## 02 单机配置（Standalone Operation）

默认配置为单机运行，故在此首先测试一下hadoop是否可以正常运行

首先cd到hadoop根目录，而后

```
  mkdir input
  cp etc/hadoop/*.xml input
  bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.2.jar grep input output 'dfs[a-z.]+'
  cat output/*
```

此配置下etc/hadoop/core-site.xml中为

<configuration>
		</configuration>

etc/hadoop/hdfs-site.xml同上

## 03 伪分布式配置（Pseudo-Distributed Operation）

参考资料：[博客](https://blog.csdn.net/hliq5399/article/details/78193113)

首先我们先按照官方文档进行配置

配置etc/hadoop/core-site.xml:

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

配置etc/hadoop/hdfs-site.xml:

```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

检查ssh是否可以无密码连接本机：

```
  $ ssh localhost
```

若需要输入密码，则配置ssh为无密码：

```
  $ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
  $ chmod 0600 ~/.ssh/authorized_keys
```

配置Hadoop环境变量：

打开/etc/profile

```
export HADOOP_HOME="/home/minelhuang/Hadoop/hadoop-3.2.1"
export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
```

配置hadoop-env.sh、mapred-env.sh、yarn-env.sh文件的JAVA_HOME参数

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre

而后，格式化HDFS

```
  $ bin/hdfs namenode -format
```

开启Hadoop：

```
  $ sbin/start-dfs.sh
```

如果报错：HDFS_NAMENODE_USER defined，解决方法如下：

打开***`$HADOOP_HOME/etc/hadoop/hadoop-env.sh`***，添加如下代码，这里我们设置hadoop的运行账户为root：

```
export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root
```

而后可能报错：pdsh@ubuntu01: localhost: connect: Connection refused，解决方法参考[link](https://stackoverflow.com/questions/48189954/hadoop-start-dfs-sh-connection-refused)

出现该问题的原因为，pdsh默认使用rsh，而不是ssh

更改$HADOOP_HOME/libexec/hadoop-functions.sh中的PDSH_SSH_ARGS_APPEND="${HADOOP_SSH_OPTS}" pdsh \替换为

```
PDSH_RCMD_TYPE=ssh PDSH_SSH_ARGS_APPEND="${HADOOP_SSH_OPTS}" pdsh \
```

若出现报错：localhost: root@localhost: Permission denied (publickey,password).

这是因为启动的账户我们设置为root，但是ssh免密登录并没有设置为root账户，故通过su root进入root账户后，重新配置ssh免密登录即可。

同理，若想在非root账户上运行hadoop，请修改hadoo-env.sh中的账户

在对应账户输入jps，若namenode, datanode, secondary namenode均存在于java进程中，代表hadoop启动成功

若使用hdfs启动成功，YARN也应该可以启动成功，以上为笔者配置hadoop时遇到的一些问题以及解决办法。



