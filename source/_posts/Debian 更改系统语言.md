---
title: Debian 更改系统语言
tags:
  - Linux
index_img: /img/banner/Linux.png
categories:
	- 学习笔记
	- Linux
abbrlink: 7cc63a46
date: 2023-02-23 21:00:00
sticky:
---

# Debian 更改系统语言

要支持区域设置，首先要安装locales软件包：
```
apt-get install locales
```

首先查看当前语言环境

```bash
env | grep LANG	
```



```bash
export LANG=en_US.UTF-8
#export LANG=zh_cn.UTF-8
#注意  en表示语言，US表示国家，UTF-8表示编码
```

重新配置 locales，然后重启

```bash
dpkg-reconfigure locales
reboot
```