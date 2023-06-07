---
title: Typecho配置Rewrite规则
excerpt: Rewrite主要实现url地址重写，以及重定向，就是把传入web的请求重定向到其他url的过程。
categories:
	- 学习笔记
  - 应用
index_img: /img/banner/Typecho.jpg
tags:
  - Typecho
  - Rewrite
abbrlink: b82be445
date: 2023-01-25 10:18:00
sticky:
---

## **Nginx服务器配置rewrite规则实现静态化**

首先在你的Nginx服务器上配置了 `yourdomain.conf` 文件，注意不是 `nginx.conf` 文件，文件存储路径一般为

```
/usr/local/nginx/conf/vhost/yourdomain.conf
```

在 `yourdomain.conf` 配置文件中找到 `server` 配置段，在其中加入下面一段代码

```nginx
if (!-e $request_filename)
{
rewrite ^(.*)$ /index.php$1 last;
}
```

而其他不需要修改，保存重启服务器。

例如我的配置文件为 `www.conimi.com.conf` ,配置完成如下，

![typecho伪静态-yourdomain.conf](https://raw.githubusercontent.com/HongChaGuan/momo/master/Hexo/typecho-wjt-domain.png)

## **后台配置Typecho实现静态化**

第一步完成后，登录博客后台，进入 设置→永久链接

![typecho伪静态-后台](https://image.tnxw.com/images/2023/01/03/typecho-wjt-h.png)

然后，那个index.php就不见了。

## **可能会出现重写功能检测失败的错误**

![typecho伪静态-error](https://raw.githubusercontent.com/HongChaGuan/momo/master/Hexo/nginx-permalink-d.png)

勾选启用此功能

保存
