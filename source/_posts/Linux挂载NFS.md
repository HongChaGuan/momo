---
title: Linux挂载NFS
excerpt: NFS网络文件系统是一种分布式文件系统协议，使您可以通过网络共享远程目录。
tags:
	- NFS
categories:
	- 学习笔记
	- Linux
index_img: /img/banner/NFS.png
abbrlink: '19355343'
date: 2023-02-06 20:48:13
sticky:
---

##### 安装客户端

基于Debian的Linux发行版，请运行以下命令安装NFS文件系统挂载软件。

```
apt update && apt install -y nfs-common
```

基于RedHat的Linux发行版，请运行以下命令安装NFS文件系统挂载软件。

```
yum install -y nfs-utils
```



##### 挂载文件系统

先查服务端的共享目录，这里的IP地址是nfs服务器的地址。

```
showmount -e 10.0.0.10
```

将服务端的目录挂载到本地

```bash
mount -t nfs 10.0.0.10:/test /data
```

##### 自动挂载文件系统

要在Linux系统启动时自动挂载NFS共享，请在`/etc/fstab`文件中添加一行。该行必须包含NFS服务器的主机名或IP地址，NFS共享目录以及本地计算机的挂载点。

```
vi /etc/fstab
```

按照如下格式，更改IP地址和挂载路径。

```
10.0.0.10:/test /data  nfs      defaults    0       0
```

##### 卸载共享文件系统

`umount`命令从目录树中卸载已挂载的文件系统，要卸载已挂载的NFS共享，请使用`umount`命令，后跟已挂载的目录或NFS共享目录。

```
umount 10.0.0.10:/test
umount /data
```

命令`fuser -m MOUNT_POINT`可帮助找我们到正在访问NFS共享目录的进程，`MOUNT_POINT`是挂载点。

```
fuser -m /data
```
