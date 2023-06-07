---
title: Chevereto安装
excerpt: Chevereto是一个自建图片托管网站，目前已更新到V4，但也开始收费，但可以选择使用另一个分支：Cheverote-Free，是基于Cheverote V3.16.2版本，目前已停止维护，只提供基本的功能，供个人或小型社区免费试用。
tags:
  - 图床
categories:
	- 学习笔记
  - 应用
index_img: /img/banner/Chevereto.jpg
abbrlink: 5d938d1d
date: 2023-01-25 10:21:01
sticky:
---


# Chevereto安装

**环境要求：Apache/Nginx、PHP 5.5+、MySQL 5.0+**

### 1、搭建web环境

```
我们可以用lnmp、lamp一键包之类的面板来搭建web环境。
lnmp安装方法可参考：https://oneinstack.com
```

### 2、上传chevereto程序

搭建好`web`环境后，添加网站并解析，再上传`chevereto`程序到网站目录，下载地址：[chevereto](https://github.com/rodber/chevereto-free)
这里以`lnmp`为例，执行命令：

```
cd /data/wwwroot/www.yourdomain.com
wget https://github.com/rodber/chevereto-free/releases/download/1.6.2/1.6.2.zip
tar zfvx 1.6.2.zip
chmod -R 775 ./*
```

修改网站配置文件`/usr/local/nginx/conf/vhost/xx.com.conf`，在server中添加以下代码。

```
location / {
try_files $uri $uri/ /index.php?$query_string;
}
```

然后重启`Nginx`，使用命令：

```
/etc/init.d/nginx restart
#或者
lnmp restart
```

最后就可以打开你的网站按要求填入数据库信息进行安装了。

**注意：经测试，使用v1.0.7程序的打开网站后可能会出现Chevereto can’t create the app/settings.php file. You must manually create this file该错误，这时在app目录新建settings.php文件并给予可写入权限即可，也可使用命令，以lnmp为例：**

```
cd /home/wwwroot/xx.com/app
touch settings.php
chmod -R 777 settings.php
```

当然本教程使用的是最新版v1.0.8暂时没遇到过该问题。
