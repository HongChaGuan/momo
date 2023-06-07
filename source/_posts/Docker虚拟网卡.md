---
title: Docker虚拟网卡
excerpt: 添加删除Docker虚拟网卡
tags:
  - Docker
  - 网卡
categories:
	- 学习笔记
  - 应用
index_img: /img/banner/NetWork.png
abbrlink: 3f443da1
date: 2023-01-25 10:09:41
sticky:
---



#### 创建Docker网卡

```
[root@i ~]# brctl addbr docker0
[root@i ~]# ip addr add 192.168.42.1/24 dev docker0  
# 这里的ip是给docker内部用的, 随意配置一个即可
[root@i ~]# ip link set dev docker0 up
[root@i ~]# ip addr show docker0
# 查看docker
[root@i ~]# systemctl restart docker
[root@i ~]# systemctl restart docker
# 启动docker服务
```

#### 添加子网

```
[root@i ~]# docker network create backend
# 这样我们就创建了backend子网，docker-compose就可以直接使用这个network
# 如果无法常见子网，则使用下面的命令，跳过安全问题
 
[root@i ~]# docker network create backend --subnet 172.24.24.0/24
 
[root@i ~]# docker network ls
NETWORK ID     NAME        DRIVER       SCOPE
6afff4d90f05    backend       bridge       local
57de7f32064e    bridge       bridge       local
4b44a5340d6e    host        host        local
ac8e8ffe243f    none        null        local
```

这里可以看到有backend

#### 删除网卡


用 ifconfig 发现有个叫br开头的` br-59ec53121ef6` 的网桥，使用如下命令会删除全部桥接网卡。

```
docker network rm $(docker network ls)
```

单一网卡删除

```
docker network rm 网卡名
```

brctl命令安装：

```
Centos系统
$ yum install -y bridge-utils

Ubuntu系统   
$ apt-get  install -y bridge-utils
```


```
[root@i ~]# brctl show
#查看网桥状态
bridge name   bridge id        STP enabled   interfaces
br-5db3fa0c465f     8000.02424cfb3937    no       veth038d483
                            veth2950f5c
                            veth669dc5e
                            veth715203f
                            veth9f31643
                            vethd0f5330
docker0     8000.3a4803cd6298    no       veth9d3badb
                            vethd7530fd
[root@i ~]# brctl delif <网桥名> <端口名>
#卸载网桥上的端口
[root@i ~]# ifconfig
#查看是否有网桥网卡名
[root@i ~]# ifconfig <网桥名> down
#关闭此网卡
[root@i ~]# brctl delbr <网桥名>
#删除网桥
```

#### 解决Docker网桥与局域网冲突

##### 问题描述

​	    在使用 docker-compose 部署应用时，docker 默认的网络模式是 bridge，默认网段是 172.17.0.1/16。十分不巧的是我们自己物理机的局域网也使用的是 172.18.0.1/16 的网段。在执行 docker-compose -f docker-compose.yml up -d 部署服务后，自动生成的网桥会依次使用 172.18.x.x，然而悲催的事情发生了。docker 生成的网桥与局域网冲突了。于是乎，面向百度编程的漫漫长路开始了，但是，通过一次次的百度和谷歌，找到好多好多复制来复制去的博客，最终一遍一遍的试错，最终也没有解决我的问题，于是乎，这篇含金量达到 99.9999999999999999999999999% 的文章出炉了。

##### 解决方案

停止 docker-compose 创建的容器

```
docker-compose -f docker-compose.yml down
```

brctl命令安装：

```
Centos系统
$ yum install -y bridge-utils

Ubuntu系统   
$ apt-get  install -y bridge-utils
```

操作 Docker 容器

```
# 停止 Docker 容器
sudo systemctl stop docker
# 停止 docker0 网桥
sudo ip link set dev docker0 down
# 删除 docker0 网桥
sudo brctl delbr docker0
# 重置 iptables
sudo iptables -t nat -F POSTROUTING
```

编辑 daemn.json 文件

```
vi /etc/docker/daemon.json
```

添加如下内容，包含设置 Docker 容器的 IP 网段

```
"default-address-pools" : [
  {
   "base" : "192.168.0.0/16",
   "size" : 24
  }
]
```

注意：如果 daemon.json 中包含了其它的内容，请切记语法格式的正确性，比如用逗号隔开。

重启 Docker 容器

```
sudo systemctl daemon-reload
sudo systemctl start docker
```

重新安装和启动 docker-compose 的命令

```
docker-compose -f docker-compose.yml up -d
```

