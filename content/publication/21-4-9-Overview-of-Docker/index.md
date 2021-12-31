---
title: Overview of Docker
summary: '[工具][Docker]Docker技术总结'
authors:
- Minel Huang
date: “2021-04-09T00:00:00Z”
publishDate: "2021-04-09T00:00:00Z"

publication_types: ["0"]

tags: 
- Tools
- Docker
featured: false
---

更新时间：2021/04/09

参考资料：
1. [Youtube教学视频](https://www.youtube.com/watch?v=fqMOX6JJhGo)

# 1 VM和Container
![](1.1.jpg)
当一台计算机运行多个VM时，各VM共享同一个硬件设备，但在每个VM中使用完全独立的OS，来达到各个VM完全隔离的目的。而在docker中，核心组件为container，各个container共享同一个OS，而Container中的App可以相互隔离。所以Container要比VM更轻量级，部署、转移、销毁也更加迅速。

# 2 Container和Image
Image是一个package，这个包可以移植到任何docker平台中运行，其概念更偏向于环境包。
Container是Image中的实例，每个container中运行不同的app

举个例子：
传统的软件编程做法是：
Developer编写了一个App，同时还需将该App运行的环境需求打包；Operator常常面对配置环境的问题，毕竟App并不是他们实际编写的。
Docker所做的是，Developer将其环境需求打包乘一个docker file，docker通过该file创建image，那么所有的Operators只要安装了docker，并且拥有docker image，developer所编写的app便可以运行。

# 3 basic docker commands
1. docker run
运行container from image，如果image未在本地查询到，则docker会在docker hub中将image拉取到本地。
2. docker ps
显示当前正在运行的container，docker ps -a会显示所有的container，无论是否正在运行
3. docker stop
停止某个container
4. docker rm
将container永久删除
5. docker images
显示所有images
6. docker rmi
删除image，在删除之前需要停止该image的所有container
7. docker pull
下载image
8. docker exec
在container中运行命令

# 4 docker network
参考资料：[docker network create](https://docs.docker.com/engine/reference/commandline/network_create/)
docker container是docker image所启用的一个实例，各个container之间是需要保证相互隔离的，而首要保证隔离的是网络通信。所以docker提供了container和实际计算机（host）之间的网络映射，包括端口、IP等，皆在docker network中配置。
每一个container都要对应上一个docker network，在部署container时，docker会检查各个container之间的网络配置是否冲突，包括端口映射、网络ip等。本章节将给出docker network的配置方法。

创建test_network配置方法如下：
```shell
docker network create \
--driver=bridge \
--subnet=192.168.0.1/24 \
--attachable=true \
test_network
```
这里需要解释的是driver与attachable两个参数：
--driver: 表示docker network与host network之间的关系，bridge表示桥接模式，即对于外网，使用host的dns与网关
--attachable: 表示该docker network对于其他docker network或host network是否是可见的，如果两个不同network中的container需要相互通信，那么该项应被赋值为true

使用docker network list可以查看所有已创建的docker network
使用docker network inspect ${docker network name}可以查看该docker network的详细配置

# docker volume
参考资料：[docker volume create](https://docs.docker.com/engine/reference/commandline/volume_create/)
如何将host中的文件映射到container中？在docker中使用volume解决。
创建一个volume配置方法如下：
```shell
# logs
docker volume create --driver local \
--opt type=none \
--opt o=bind \
--opt device=${BASE_DIR} \
logs
```
该命令创建了一个名为logs的volume，其映射的主机地址为${BASE_DIR}，在创建container时可以直接将该volume映射到container中的某个目录，当然也可以不创建volume直接映射，如下：
```shell
# create mysql
docker run -d \
--name mysql \
-v ${BASE_DIR}:/var/lib/mysql/logs \
-v logs:/var/lib/mysql/logs \
--network=test_network \
mysql:8
```
可以使用docker volume list查看所有已创建的volumes

# docker container
参考资料：[docker run](https://docs.docker.com/engine/reference/commandline/run/)
用法：
 docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
一个例子：
```shell
USER=minel
BASE_DIR=/home/minel/mysql
# create sql
docker run -d \
--name ${USER}_mysql \
--expose=3306 \
-v ${BASE_DIR}//confs/mysql/init:/docker-entrypoint-initdb.d/ \
-v ${BASE_DIR}/shared_dir/data/mysql:/var/lib/mysql \
--ipc=shareable \
--network-alias=mysql \
-e "MYSQL_ALLOW_EMPTY_PASSWORD=yes" \
--restart=always \
--network=${USER}_network \
mysql:8
```
参数解释：
- expose: 在docker network中使用的端口，并不占用host端口
- ipc: 指该container是否能被访问
- network-alias: 该container在network中的名称，用于docker dns解析
若不同host间的container有相互通信的需求，需要将container中的虚拟端口映射到实际端口上，可以添加如下参数
```shell
-p 10086:8080/tcp
```
10086为实际端口，8080为container中的虚拟端口