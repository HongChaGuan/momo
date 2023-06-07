---
title: Linux设置静态IP
excerpt: 新安装的Linux系统，默认一般使用DHCP获取IP地址，除非在安装过程中，使用了指定的IP地址。本文将介绍如何在各Linux发行版中，配置使用静态IP地址，配置网关，以及设置DNS服务器。
categories:
	- 学习笔记
	- Linux
index_img: /img/banner/Linux.png
tags:
    - IP
abbrlink: ef9375d1
date: 2023-01-25 10:24:54
sticky:
---



##### **ArchLinux**

​	1.编辑配置文件

​			网卡

````javascript
/etc/dhcpcd.conf
````


​	2.添加下列内容

````javascript
interface eth0						#设置网卡名
static ip_address=10.0.0.10/24				#设置IP和掩码位
static routers=10.0.0.1					#设置网关
static domain_name_servers=10.0.0.1			#设置DNS
````

​	3.常用命令

````javascript
systemctl status systemd-networkd			#查看网络状态
systemctl restart dhcpcd				#重启DHCP
````



------



##### **CentOS**

​	1.编辑配置文件

````javascript
/etc/sysconfig/network-scripts
````

​	2.清空内容并添加以下内容

````javascript
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="none"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="eth0"										#网卡名称
UUID="5ee00e18-1161-4c0b-b874-1931dbd7ea33"		#ID
DEVICE="eth0"
ONBOOT="yes"
IPADDR="10.0.0.132"								#静态IP地址
PREFIX="28"										#掩码位
GATEWAY="10.0.0.128"							#子网掩码
DNS1="10.0.0.128"								#DNS

````





------



##### **Debian**

​	1.编辑配置文件

````javascript
/etc/network/interfaces
````

​	2.清空内容并添加以下内容

````javascript
auto eth0						#设置网卡名

iface eth0 inet static 				
address 10.0.0.9					#设置IP和掩码位
netmask 255.255.255.0 					#设置子网掩码
gateway 10.0.0.1					#设置网关


auto eth0 eth1

iface eth0 inet static                 
address 10.0.0.10
netmask 255.255.255.0
gateway 10.0.0.1
dns-nameservers 10.0.0.1

iface eth1 inet static                 
address 10.0.1.10
netmask 255.255.255.0
gateway 10.0.1.1

````

##### **armbian**

```
source /etc/network/interfaces.d/*

# Network is managed by Network manager
# You can choose one of the following two IP setting methods:
# Use # to disable another setting method


# 01. Enable dynamic DHCP to assign IP
#auto eth0
#iface eth0 inet dhcp
        hwaddress ether mac


# 02. Enable static IP settings(IP is modified according to the actual)
auto eth0
allow-hotplug eth0
iface eth0 inet static
address 10.0.0.1
netmask 255.255.255.0
gateway 10.0.0.1
dns-nameservers 119.29.29.29
dns-nameservers 10.0.0.1


# 03. Docker install OpenWrt and communicate with each other
#allow-hotplug eth0
#no-auto-down eth0
#auto eth0
#iface eth0 inet manual
#
#auto macvlan
#iface macvlan inet dhcp
#        hwaddress ether mac
#        pre-up ip link add macvlan link eth0 type macvlan mode bridge
#        post-down ip link del macvlan link eth0 type macvlan mode bridge
#
#auto lo
#iface lo inet loopback

```

------

##### **Ubuntu**

​	1.编辑配置文件

````javas
/etc/netplan/********.yaml				#*填写对应的名称
````

​	2.修改内容为如下,本设置为双网卡

````javas
network:
    ethernets:
        eth0:						#网卡名称
            dhcp4: false							
            addresses: [10.0.0.11/24]			#IP和掩码位
            routes:
                   - to: 0.0.0.0/0
                     via: 10.0.0.1					
                     metric: 30				#优先级
            optional: true
            nameservers:
　　　　    addresses: [10.0.0.1,119.29.29.29]		#DNS

        eth1:
            dhcp4: false
            addresses: [10.0.0.131/28]
            routes:
                    - to: 0.0.0.0/0
                      via: 10.0.0.128
                      metric: 20
            optional: true
            nameservers:
　　　　    addresses: [10.0.0.128]
    version: 2
````

​	3.重启网卡并

````javas
netplan apply
````




