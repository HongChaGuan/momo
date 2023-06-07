---
title: 华为AC+AP基础配置
tags:
  - 华为
  - AC+AP
index_img: /img/banner/HuaWei-AC.jpg
categories:
  - 交换路由
  - 路由
abbrlink: ac2c0052
date: 2022-07-10 22:30:00
sticky:
---




# 华为AC+AP配置





### 一.AP上线设置

##### 1.创建域模板，国家码设置为CN

```
wlan
regulatory-domain-profile name 名字
country-code CN
```

#### 2.创建AP组

```
wlan
ap-group name 组名
regulatory-domain-profile 域名
```

#### 3.配置AC与AP通信方式，配置AC源接口

在系统视图下

```
capwap source interface VlanifID
```

#### 4.AC上离线导入AP

有三种ap认证方式：mac地址认证、sn序列号认证、不认证，这里我们采用MAC地址认证

```
wlan
ap auth-mode mac-auth	 //设置认证模式
ap-id 0 ap-mac mac地址   //0为AP的ID编号，可依次排序
ap-name AP				//设置AP名字
ap-group apgroup		//设置AP所属组
```







### 二.AC控制器上wlan射频业务配置

#### 1.配置WLAN安全模板

加密WIFI

```
wlan
security-profile name 模板名
security wpa-wpa2 psk pass-phrase 密码 aes //设置无线接入密码
```

开放WIFI

```
wlan
security-profile name 模板名
security open //Guest用户不设密码
```



#### 2.配置SSID模板

```
wlan
ssid-profile name Wlan名字-2.4G
ssid Wlan名字 //修改默认的无线名称
```



```
wlan
ssid-profile name Wlan名字-5G
ssid Wlan名字 //修改默认的无线名称
```



#### 3.配置ap组引用的vap模板

AC控制器上的转发模式有直接转发（本地转发）、隧道转到（集中转发），这里我们采用本地转发，就是流量不经过AC控制器直接转发出去

```
wlan
vap-profile name WLAN-2.4G		//名称
forward-mode direct-forward 	//直接转发模式
service-vlan vlan-id ID			//服务VLAN为ID
security-profile 安全模板名 		//引用安全模板
ssid-profile Wlan名字 		  //引用SSID模板
```



#### 4.ap组引用vap模板

```
wlan
ap-group name 组名
vap-profile Wlan名字-2.4G wlan 1 radio 0 //2.4G射频信号引用Wlan名字-2.4G的vap模板
vap-profile Wlan名字-5G wlan 1 radio 1 //5G射频信号引用Wlan名字-5G的vap模板
vap-profile WLAN-Guest wlan 2 radio 0
vap-profile WLAN-Guest wlan 2 radio 1
```



