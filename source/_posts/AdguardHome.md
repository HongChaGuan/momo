---
title: 使用AdGuard Home自建DNS防污染、去广告
excerpt: >-
  本篇教程来详细讲解如何正确的设置 Ad­Guard Home ，来更有效的防止 DNS 污染以及去广告。与其它 Ad­Guard Home
  教程的只讲方法、不讲逻辑的胡乱设置不同，认真看完这篇教程你会收获大量的知识和启发
categories:
  - 学习笔记
  - 应用
index_img: /img/banner/Docker.png
tags:
  - AdguardHome
abbrlink: a4b7c8a7
date: 2023-01-25 17:03:26
sticky:
---

#### 安装

`docker-compose.yml`

```
version: "2"
services:
  adguardhome:
    network_mode: host
    image: adguard/adguardhome
    container_name: AdguardHome
    environment:
      - TZ=Asia/Shanghai
    volumes:
      - /appdata/docker/AdGuardHome/workdir:/opt/adguardhome/work
      - /appdata/docker/AdGuardHome/confdir:/opt/adguardhome/conf
    restart: unless-stopped
```



#### DNS 拦截列表

![dns-ljlb](/img/doc/AdguardHome/dns-ljlb.jpg)

编辑`AdGuardHome.yaml`，找到`filters`一栏，替换为如下规则

```
  - enabled: true
    url: https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt
    name: AdGuard DNS filter
    id: 1
  - enabled: true
    url: https://adaway.org/hosts.txt
    name: AdAway Default Blocklist
    id: 2
  - enabled: true
    url: https://www.malwaredomainlist.com/hostslist/hosts.txt
    name: MalwareDomainList.com Hosts List
    id: 4
  - enabled: true
    url: https://anti-ad.net/adguard.txt
    name: anti-ad-adguard
    id: 1635777687
  - enabled: true
    url: https://anti-ad.net/easylist.txt
    name: anti-ad-easylist
    id: 1635777688
  - enabled: true
    url: https://anti-ad.net/domains.txt
    name: anti-ad-domains
    id: 1635777689
  - enabled: true
    url: https://anti-ad.net/surge.txt
    name: anti-ad-surge
    id: 1635777690
  - enabled: true
    url: https://anti-ad.net/surge2.txt
    name: anti-ad-surge2
    id: 1635777691
  - enabled: true
    url: https://anti-ad.net/anti-ad-for-dnsmasq.conf
    name: adblock-for-dnsmasq
    id: 1635777692
  - enabled: true
    url: https://anti-ad.net/clash.yaml
    name: anti-ad-clash
    id: 1635777693
  - enabled: true
    url: https://anti-ad.net/anti-ad-for-smartdns.conf
    name: anti-ad-smartdns
    id: 1635777695
  - enabled: true
    url: https://raw.githubusercontent.com/neodevpro/neodevhost/master/adblocker
    name: neodevhost
    id: 1655021687
  - enabled: true
    url: https://cats-team.coding.net/p/adguard/d/AdRules/git/raw/main/adblock.txt
    name: AdRules
    id: 1655021688
```



#### DNS

![dns-sz](/img/doc/AdguardHome/dns-sz.jpg)

上游 DNS 服务器

```
119.29.29.29
223.5.5.5
61.128.128.68
61.128.192.68
114.114.114.114
tls://dns.alidns.com
tcp://114.114.114.114
8.8.8.8
tcp://8.8.8.8
```

Bootstrap DNS 服务器

```
114.114.114.114
119.29.29.29
223.6.6.6
117.50.60.30
```

