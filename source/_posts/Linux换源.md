---
title: Linux换源
excerpt: apt-get，是一条linux命令，适用于deb包管理式的操作系统，主要用于自动从互联网的软件仓库中搜索、安装、升级、卸载软件或操作系统。有时由于网络问题，无法安装各种包，尤其是一些国外的包，就需要进行换源操作。
categories:
	- 学习笔记
	- Linux
index_img: /img/banner/Linux.png
tags:
    - 源
abbrlink: 18f26aa6
date: 2023-01-25 10:36:14
sticky:
---


### 1.Debian

####	1.Debian 10（buster）
##### 1.腾讯云

```
deb https://mirrors.cloud.tencent.com/debian/ buster main contrib non-free
deb https://mirrors.cloud.tencent.com/debian/ buster-updates main contrib non-free
deb https://mirrors.cloud.tencent.com/debian/ buster-backports main contrib non-free
deb https://mirrors.cloud.tencent.com/debian-security buster/updates main contrib non-free
deb-src https://mirrors.cloud.tencent.com/debian/ buster main contrib non-free
deb-src https://mirrors.cloud.tencent.com/debian/ buster-updates main contrib non-free
deb-src https://mirrors.cloud.tencent.com/debian/ buster-backports main contrib non-free
deb-src https://mirrors.cloud.tencent.com/debian-security buster/updates main contrib non-free
```
##### 2.清华源

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free

deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
```





#### 2.Debian 11（bullseye）
##### 1.清华源

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free

deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
```
##### 2.中科大

```
deb http://mirrors.ustc.edu.cn/debian stable main contrib non-free
# deb-src http://mirrors.ustc.edu.cn/debian stable main contrib non-free
deb http://mirrors.ustc.edu.cn/debian stable-updates main contrib non-free
# deb-src http://mirrors.ustc.edu.cn/debian stable-updates main contrib non-free

# deb http://mirrors.ustc.edu.cn/debian stable-proposed-updates main contrib non-free
# deb-src http://mirrors.ustc.edu.cn/debian stable-proposed-updates main contrib non-free
```

##### 3.内网

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb http://mirrors.tnxw.com/debian/ bullseye main contrib non-free
# deb-src http://mirrors.tnxw.com/debian/ bullseye main contrib non-free
deb http://mirrors.tnxw.com/debian/ bullseye-updates main contrib non-free
# deb-src http://mirrors.tnxw.com/debian/ bullseye-updates main contrib non-free

deb http://mirrors.tnxw.com/debian/ bullseye-backports main contrib non-free
# deb-src http://mirrors.tnxw.com/debian/ bullseye-backports main contrib non-free

#deb http://mirrors.tnxw.com/debian-security bullseye-security main contrib non-free
# deb-src http://mirrors.tnxw.com/debian-security bullseye-security main contrib non-free
```







### 2.Ubuntu

#### 1.Ubuntu 20.04(focal)

##### 1.清华源

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

##### 2.中科大

```
# 默认注释了源码仓库，如有需要可自行取消注释
deb https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.ustc.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

##### 3.内网

```
deb http://mirrors.tnxw.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.tnxw.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.tnxw.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.tnxw.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.tnxw.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.tnxw.com/ubuntu/ focal-backports main restricted universe multiverse
deb http://mirrors.tnxw.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.tnxw.com/ubuntu/ focal-security main restricted universe multiverse
# 预发布软件源，不建议启用
# deb http://mirrors.tnxw.com/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src http://mirrors.tnxw.com/ubuntu/ focal-proposed main restricted universe multiverse
```





















