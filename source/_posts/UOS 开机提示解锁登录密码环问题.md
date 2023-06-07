---
title: UOS 开机提示解锁登录密码环问题
excerpt: UOS系统开机或重启时左上角弹窗提示“输入密码以解锁您的登录密码环”，并注明“您登录计算机时，您的登录密码环未被解锁”，同时弹出网络连接密码框。
tags:
  - UOS
categories:
  - 学习笔记
  - Linux
index_img: /img/banner/Uos.jpeg
abbrlink: 48efa1b7
date: 2023-06-07 11:29:56
sticky:
---

1. 文件管理器设置→显示隐藏文件，删除用户数据盘下`.local/share/keyrings/login.keyring`

2. 终端输入命令
	```
	rm /home/用户名/.local/share/keyrings/login.keyring
	```
    重启系统，弹窗消失，根据提示输入网络连接密码，再次重启后恢复正常。