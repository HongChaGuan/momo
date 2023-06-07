---
title: NextCloud反向代理
excerpt: NextCloud反向代理
categories:
	- 学习笔记
	- 应用
index_img: /img/banner/NextCloud.jpg
tags:
    - NextCloud
    - 反向代理
abbrlink: aae55609
date: 2023-01-25 10:23:44
sticky:
---




```
<?php
$CONFIG = array (
  'memcache.local' => '\\OC\\Memcache\\APCu',
  'memcache.locking' => '\\OC\\Memcache\\Redis',
  'redis' =>
  array (
    'host' => 'Redis地址',
    'port' => Redis端口,
  ),
  'datadirectory' => '/data',
  'instanceid' => 'c1r4ss',
  'passwordsalt' => 'Y465JyqS3RiDl',
  'secret' => '6AaD8x0dyNTwX03RsGpVZMjkEau',
  'trusted_domains' =>
  array (
    0 => '本机IP',
        1 => '信任域名1',
        2 => '信任域名2',
  ),
  'dbtype' => 'mysql',
  'version' => '22.2.3.0',
  'overwrite.cli.url' => 'https://域名',
  'dbname' => '数据库名',
  'dbhost' => '数据库地址:3306',
  'dbport' => '',
  'dbtableprefix' => 'oc_',
  'mysql.utf8mb4' => true,
  'dbuser' => '数据库用户',
  'dbpassword' => '数据库密码',
  'installed' => true,
#  'auth.bruteforce.protection.enabled' => false,
  'overwritehost' => '代理域名或域名加端口',
  'overwriteprotocol' => 'https',
  'app_install_overwrite' =>
  array (
    0 => 'hancomoffice',
  ),
);
```


