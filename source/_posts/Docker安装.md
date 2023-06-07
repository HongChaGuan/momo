---
title: Docker安装
excerpt: Docker 是一种开源容器化技术，用于构建和容器化应用程序。Docker 使用客户端-服务器架构。Docker客户端与 Docker守护进程对话，后者负责构建、运行和分发 Docker 容器的繁重工作。 Docker 客户端和守护程序可以在同一系统上运行，或者您可以将 Docker 客户端连接到远程 Docker 守护程序。Docker 客户端和守护进程使用 REST API、UNIX 套接字或网络接口进行通信。另一个 Docker 客户端是 Docker Compose，它允许您使用由一组容器组成的应用程序。
tags:
   - Docker
categories:
   - 学习笔记
   - 应用
index_img: /img/banner/Docker.png
abbrlink: 2a40ac23
date: 2023-01-25 10:57:02
sticky:
---


### 清华源


如果你过去安装过 docker，先删掉:
```
apt-get remove docker docker-engine docker.io
```
首先安装依赖:
```
apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```
根据你的发行版，下面的内容有所不同。你使用的发行版： 
##### Debian
信任 Docker 的 GPG 公钥:
```
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
```
对于 amd64 架构的计算机，添加软件仓库:
```
add-apt-repository \
   "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/debian \
   $(lsb_release -cs) \
   stable"
```
如果你是树莓派或其它 ARM 架构计算机，请运行:
```
echo "deb [arch=armhf] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/debian \
     $(lsb_release -cs) stable" | \
     tee /etc/apt/sources.list.d/docker.list
```
##### Ubuntu
信任 Docker 的 GPG 公钥:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg |  apt-key add -
```
对于 amd64 架构的计算机，添加软件仓库:
```
add-apt-repository \
   "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
如果你是树莓派或其它 ARM 架构计算机，请运行:
```
echo "deb [arch=armhf] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
     $(lsb_release -cs) stable" | \
     tee /etc/apt/sources.list.d/docker.list
```
最后安装
```
apt-get update
apt-get install docker-ce
```


##### Fedora/CentOS/RHEL

如果你之前安装过 docker，请先删掉
```
yum remove docker docker-common docker-selinux docker-engine
```
安装一些依赖
```
yum install -y yum-utils device-mapper-persistent-data lvm2 wget
```
根据你的发行版下载repo文件: 
CentOS/RHEL
```
wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
```
把软件仓库地址替换为 TUNA:
```
sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```
最后安装:
```
yum makecache fast
yum install docker-ce
```





### 阿里云

安装必要的一些系统工具

```
apt-get update
apt-get -y install apt-transport-https ca-certificates curl software-properties-common
```
##### Debian
安装GPG证书
```
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/debian/gpg |  apt-key add -
```
写入软件源信息
```
add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/debian $(lsb_release -cs) stable"
```
更新并安装Docker-CE
```
 apt-get -y update
 apt-get -y install docker-ce
```

##### Ubuntu
安装GPG证书
```
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg |  apt-key add -
```
写入软件源信息
```
add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```
更新并安装Docker-CE
```
apt-get -y update
apt-get -y install docker-ce
```


##### CentOS
安装必要的一些系统工具
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```
添加软件源信息
```
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
写入软件源信息
```
sed -i 's+download.docker.com+mirrors.aliyun.com/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```
更新并安装Docker-CE
```
yum makecache fast
yum -y install docker-ce
```
开启Docker服务
```
service docker start
```

##### 注意：

官方软件源默认启用了最新的软件，您可以通过编辑软件源的方式获取各个版本的软件包。例如官方并没有将测试版本的软件源置为可用，您可以通过以下方式开启。同理可以开启各种测试版本等。
```
vim /etc/yum.repos.d/docker-ce.repo
```
将[docker-ce-test]下方的enabled=0修改为enabled=1

##### 安装指定版本的Docker-CE:
1.查找Docker-CE的版本:
```
yum list docker-ce.x86_64 --showduplicates | sort -r
   Loading mirror speeds from cached hostfile
   Loaded plugins: branch, fastestmirror, langpacks
   docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable
   docker-ce.x86_64            17.03.1.ce-1.el7.centos            @docker-ce-stable
   docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable
   Available Packages
```
2.安装指定版本的Docker-CE: (VERSION例如上面的17.03.0.ce.1-1.el7.centos)
```
yum -y install docker-ce-[VERSION]
```





### Docker加速

```
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://0qjk30w5.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```


