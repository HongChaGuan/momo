---
title: Linux关闭默认端口
excerpt: Linux关闭默认端口
tags:
  - Linux
  - 端口
categories:
  - 学习笔记
  - Linux
index_img: /img/banner/Linux.png
abbrlink: c42a1fba
date: 2023-06-07 11:29:21
sticky:
---


## Linux关闭默认端口

#### 25端口

```
apt-get autoremove sendmail-bin
```



#### 53端口

```
systemctl stop systemd-resolved.service
systemctl disable systemd-resolved.service
rm /etc/resolv.conf
echo "nameserver 223.5.5.5" >> /etc/resolv.conf
```



#### 111端口

```
systemctl stop rpcbind.socket
systemctl disable rpcbind.socket
```


