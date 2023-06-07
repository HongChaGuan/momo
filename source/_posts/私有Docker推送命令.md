---
title: 私有Docker推送命令
excerpt: 私有Docker,Harbor推送命令
index_img: /img/banner/Harbor.jpeg
categories:
	- 学习笔记
  - 应用
tags:
  - Docker
  - Harbor
abbrlink: d32525a3
date: 2023-01-25 09:56:50
sticky:
---


Docker 推送命令
在项目中标记镜像：
```
docker tag oldiy/dosgame-web-docker:latest hub.tnxw.com/momo/dosgame-web-docker:latest
```
推送镜像到当前项目：
```
docker push hub.tnxw.com/momo/dosgame-web-docker:latest
```
Helm 推送命令
在项目中打包 chart
```
helm package CHART_PATH
```
推送 chart 到当前项目
```
helm push CHART_PACKAGE oci://hub.tnxw.com/momo
```
CNAB 推送命令
推送 CNAB 到当前项目
```
cnab-to-oci push CNAB_PATH --target hub.tnxw.com/momo/REPOSITORY[:TAG] --auto-update-bundle
```
