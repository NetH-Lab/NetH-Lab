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

更新时间：2021/03/23

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

### 4.1.1 MapReduce流程
![](./4.1.jpg)
- step 1: clients将input分离开并写入HDFS
- step 2: RM create AM for MapReduce job
- step 3, 4: RM为AM分配container，并通知NM创建AM container。注意，AM也是一个container，所以是需要被创建的。
- step 5: MapReduce AM(MRAM)从HDFS上获取input文件
- step 6: MRAM向RM请求map containers，并要求containers的位置靠近input files存储空间
- step 7, 8: RM向MARM分配containers，map和reduce分别开始工作

### 4.1.2 编写MapReduce程序（基于Hadoop库）
首先介绍Hadoop库中几个关键的类

#### 4.1.2.1 Class Mapper<KEYIN,VALUEIN,KEYOUT,VALUEOUT>
参考链接：[Package org.apache.hadoop.mapreduce](https://hadoop.apache.org/docs/stable/api/org/apache/hadoop/mapreduce/package-summary.html)
Function: 输入key/value对，输出中间值key/value对
Hadoop MapReduce框架中，job最初为InputFormat，通过spllit函数将其分割哼InputSplit类，而后每个map函数处理一个InputSplit
其中，InputFormat<K, V>, 通过方法List<InputSplit>在逻辑上把InputFormat分为InputSplit
MapReduce框架中，将首先调用setup(org.apache.hadoop.mapreduce.Mapper.Context), 再对每个InputSplit调用map函数，最终调用cleanup函数
该类包含四个方法，cleanup（任务结束后使用），map，run，setup（任务开始前使用）
例程：
```java
  public static class TokenizerMapper
       extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }
```
该例子中，定义类内全局变量one，word，分别代表1和Text类，context.write(word, one)的意义为在context中写入(word, one)这样一个key/value对
对于map函数的含义也很简单，即遍历Text value，读取其中的每个单词，并转换为(word, one)输出

#### 4.1.2.2 Reducer
参考资料：[Class Reducer<KEYIN,VALUEIN,KEYOUT,VALUEOUT>](https://hadoop.apache.org/docs/stable/api/org/apache/hadoop/mapreduce/Reducer.html)
Reducer包含3个阶段：
1. Shuffle：通过HTTP将Mapper的已经排序好的输出拷贝到本地
2. Sort：不同的Mapper输出的key/value对中，key是相同的，故使用key对所有copy过来的中间key/value进行重排序
3. Reduce：
```java
public void reduce(Text key, Iterable<IntWritable> values,
                    Context context
                    ) throws IOException, InterruptedException {
    int sum = 0;
    for (IntWritable val : values) {
    sum += val.get();
    }
    result.set(sum);
    context.write(key, result);
}
```
作用为对每个key（在wordcount中是指word），将value相加，统计sum即代表word出现的总次数。

#### 4.1.2.3 Job类
参考链接：[Class Job](https://hadoop.apache.org/docs/stable/api/org/apache/hadoop/mapreduce/Job.html)
该类为用户提供配置、提交、控制、查询状态的接口

#### 4.1.2.4 例程：WordCount类 v1.0
详情请查阅官方文档：[Example: WordCount v1.0](https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html)

## 4.2 API Backword Compatibility
本章节主要描述向后兼容问题。
- Code compatibility: 指任何MapReduce code都可以在YARN上运行。这意味着我们不需要修改以前编写好的code
- Binary compatibility: 指MapReduce bytecode不需要更改就可以在YARN上运行，这意味着不需要对Hadoop 1的代码重编译。
## 4.3 编写YARN Application
### 4.3.1 Fundamentals of building a YARN application
![](./4.2.jpg)
YARN application包含5个组件：
- YARN client: 负责launching YARN application。向RM发送creatApplication和submitApplication请求
- ResourceManager: 负责接受container allocation requests，异步通知clients什么时候有资源空闲
- ApplicationMaster: 应用的main coordinator，发起container request请求，并launch到node上
- NodeManager: launch或kill containers
- Container: application-specific process，可以实Linux进程，也可以是map or reduce tasks

YARN application中的interactions

Resource allocation:
当AM向RM请求新的container时，实际上是请求一个Resource object，这一过程AM向RM发送一个ResourceRequest，如下图：
![](./4.3.jpg)
resourceName代表对container的地理位置要求，即指明host和rack的具体名称。RM使用Container Object作为回复。当AM接受到该object，它可以与NM通信，以launch container

Launching a Container
与NM通信使用下图格式;
![](./4.4.jpg)
NM根据localResources，将数据从HDFS下载到本地，而后开始launch container

### 4.3.2 编写一个收集cluster statistics的YARN application
下图显示了我们需要编写哪些程序：
![](./4.5.jpg)
Step 1: YARN client
YARN client有两个功能，1是告知RM AM的系统资源需求，2是监控app的状态
建立Client.class
Step 1.1: Create application
```java
YarnConfiguration conf = new YarnConfiguration();
YarnClient yarnClient = YarnClient.createYarnClient();
yarnClient.init(conf);
yarnClient.start();
YarnClientApplication app = yarnClient.createApplication();
```
其中YarnConfiguration conf指为conf变量分配YarnConfiguration类大小的内存
new YarnConfiguration()代表新建一个YarnConfiguration实例，并赋值给conf
YarnConfiguration是YARN提供的一个配置模板
所以这段代码的含义便是，我们使用一个YARN的配置模板创建了YarnClient类，而后创建了YarnClientApplication类

Step 1.2: Submitting a YARN application
在launch app之前，需要配置一下几样条目：
- app name
- launch AM的命令以及classpath和environment settings
- JARs, configuration files, and other files that app needs
- resource requirements(memory and CPU)
- scheduler queue and priority
- security tokens

上一节我们有谈到，AM需要分别和RM、NM交互，与NM交互所用的格式为Conatiner Launch Context，来指明JARS，环境，文件等等，我们先讲这一部分的配置方法。
```java
//初始化Container Launch Context类
ContainerLaunchContext container =
Records.newRecord(ContainerLaunchContext.class);
//配置stdout和stderr
String amLaunchCmd =
String.format(
"$JAVA_HOME/bin/java -Xmx256M %s 1>%s/stdout 2>%s/stderr",
ApplicationMaster.class.getName(),
ApplicationConstants.LOG_DIR_EXPANSION_VAR,
ApplicationConstants.LOG_DIR_EXPANSION_VAR);

container.setCommands(Lists.newArrayList(amLaunchCmd));
//寻找包含Client.class的JAR路径
String jar = ClassUtil.findContainingJar(Client.class);
FileSystem fs = FileSystem.get(conf);
Path src = new Path(jar);
Path dest = new Path(fs.getHomeDirectory(), src.getName());
//copy到HDFS
fs.copyFromLocalFile(src, dest);

FileStatus jarStat = FileSystem.get(conf).getFileStatus(dest);
//为JAR创建LocalResource
LocalResource appMasterJar = Records.newRecord(LocalResource.class);
appMasterJar.setResource(ConverterUtils.getYarnUrlFromPath(dest));
appMasterJar.setSize(jarStat.getLen());
appMasterJar.setTimestamp(jarStat.getModificationTime());
appMasterJar.setType(LocalResourceType.FILE);
appMasterJar.setVisibility(LocalResourceVisibility.APPLICATION);
//将JAR作为container的local resource
container.setLocalResources(
ImmutableMap.of("AppMaster.jar", appMasterJar));
//将YARN JARs添加值AM的classpath
Map<String, String> appMasterEnv = Maps.newHashMap();
for (String c : conf.getStrings(
YarnConfiguration.YARN_APPLICATION_CLASSPATH,
YarnConfiguration.DEFAULT_YARN_APPLICATION_CLASSPATH)) {
Apps.addToEnvironment(appMasterEnv, Environment.CLASSPATH.name(),
c.trim());
}
//将classpath添加至container的环境中
Apps.addToEnvironment(appMasterEnv,
Environment.CLASSPATH.name(),
Environment.PWD.$() + File.separator + "*");
container.setEnvironment(appMasterEnv);
```

指明对memory和CPU的要求
```java
Resource capability = Records.newRecord(Resource.class);
capability.setMemory(256);
capability.setVirtualCores(1);
```
提交APP至RM，使用SubmissionContext
```java
ApplicationSubmissionContext appContext =
app.getApplicationSubmissionContext();
//配置App名字
appContext.setApplicationName("basic-dshell");
appContext.setAMContainerSpec(container);
appContext.setResource(capability);
appContext.setQueue("default");

ApplicationId appId = appContext.getApplicationId();
yarnClient.submitApplication(appContext);
```

Step 1.3: Waiting for the YARN application to complete
Client需要监控App的状态，并作出相应的调整，App状态如下图
![](./4.6.jpg)
我们可以监控这几种状态值，代码如下
```java
//获取当前状态值
ApplicationReport report = yarnClient.getApplicationReport(appId);
//定义AM终止状态
YarnApplicationState state = report.getYarnApplicationState();
EnumSet<YarnApplicationState> terminalStates =
EnumSet.of(YarnApplicationState.FINISHED,
YarnApplicationState.KILLED,
YarnApplicationState.FAILED);
//循环，直至AM处于终止状态
while (!terminalStates.contains(state)) {
TimeUnit.SECONDS.sleep(1);
report = yarnClient.getApplicationReport(appId);
state = report.getYarnApplicationState();
}
```

Step 2: 编写AM
AM主要需要编写3个模块，如下图
![](./4.7.jpg)
Step 2.1: 在RM上登记AM
由于AM也存在于一个container中，所以要现为自己申请一个container
```java
Configuration conf = new YarnConfiguration();
AMRMClient<ContainerRequest> client = AMRMClient.createAMRMClient();
client.init(conf);
client.start();
client.registerApplicationMaster("", 0, "");
```
Step 2.2: 提交container request并当可用时launch到NM上
```java
//建立于NM通信的client
NMClient nmClient = NMClient.createNMClient();
nmClient.init(conf);
nmClient.start();

//指明优先级
Priority priority = Records.newRecord(Priority.class);
priority.setPriority(0);
//编写需求
Resource capability = Records.newRecord(Resource.class);
capability.setMemory(128);
capability.setVirtualCores(1);
//建立request object以发送给RM
ContainerRequest containerAsk =new ContainerRequest(capability, null, null, priority);
rmClient.addContainerRequest(containerAsk);

//等待收到container
boolean allocatedContainer = false;
while (!allocatedContainer) {
AllocateResponse response = rmClient.allocate(0);
for (Container container : response.getAllocatedContainers()) {
allocatedContainer = true;

//接受到container后，将其launch
ContainerLaunchContext ctx =
Records.newRecord(ContainerLaunchContext.class);
ctx.setCommands(
Collections.singletonList(
String.format("%s 1>%s/stdout 2>%s/stderr",
"/usr/bin/vmstat",
ApplicationConstants.LOG_DIR_EXPANSION_VAR,
ApplicationConstants.LOG_DIR_EXPANSION_VAR)
));
nmClient.startContainer(container, ctx);
}
TimeUnit.SECONDS.sleep(1);
}
```
这里，在接受到container后，AM在container上运行了/usr/bin/vmstat命令，当然我们也可以将其改变为其他任何命令，比如运行jar中的app

Step 2.3: 等待container完成
```java
boolean completedContainer = false;
while (!completedContainer) {
AllocateResponse response = rmClient.allocate(0);
for (ContainerStatus s : response.getCompletedContainersStatuses()) {
    completedContainer = true;
}
TimeUnit.SECONDS.sleep(1);
}
rmClient.unregisterApplicationMaster(
FinalApplicationStatus.SUCCEEDED, "", "");
```

### 4.3.3 编写一个YARN app，打出Hello World
在上一节，我们实现了使用Client完成任务提交，部署AM后申请container，最终在container中运行一条cmd命令。
本节中，笔者将对上一节Client与AM代码进行更改，试图在container中运行Hello.java，打印出Hello World。

