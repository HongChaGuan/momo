---
title: Harbor密码重置
excerpt: Harbor密码重置
categories:
  - 学习笔记
  - Docker
index_img: /img/banner/Harbor.jpeg
tags:
  - Harbor
abbrlink: eede64bd
date: 2023-01-25 10:44:34
sticky:
---



# Harbor忘记密码，密码重置、密码修改 、admin密码重置

##### 1.进入`harbor-db`的容器

```bash
docker exec -it harbor-db /bin/bash
```

##### 2.进入postgresql命令行

```undefined
psql -h postgresql -d postgres -U postgres 
```

默认密码root123

##### 3.切换到harbor所在的数据库

```swift
\c registry
```

##### 4.查看harbor_user表

```csharp
select user_id,username,password,creation_time,update_time,password_version  from harbor_user;
```

##### 5.重置密码

修改admin的密码，修改为初始化密码Harbor12345 ，修改好了之后登录web ui上再修改

```bash
update harbor_user  set salt='',password='' where  user_id = 1;
```

##### 6.退出 \q 退出postgresql，exit退出容器。
```
\q
exit
```

##### 7.重启Harbor

```shqll
docker-compose down
docker-compose up  -d
docker-compose ps
```