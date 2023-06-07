---
title: GitLab-CE和Runner安装
excerpt: Runner就像一个个的工人，而Gitlab-CI就是这些工人的一个管理中心，所有工人都要在Gitlab-CI里面登记注册，并且表明自己是为哪个工程服务的。当相应的工程发生变化时，Gitlab-CI就会通知相应的工人执行软件集成脚本。如下图所示
categories:
  - 学习笔记
  - 应用
index_img: /img/banner/Gitlab.jpeg
tags:
  - GitLab
abbrlink: bd0156ec
date: 2023-01-25 10:45:49
sticky:
---


##	GitLab-CE和Gitlab-Runner安装

## 1.GitLab-CE安装

### 1.GitLab-CE

```
docker run -d \
--name Gitlab-CE \
-p 22:22 \
-p 80:80 \
-p 443:443 \
-v /appdata/docker/Gitlab/Gitlab-CE/config:/etc/gitlab \
-v /appdata/docker/Gitlab/Gitlab-CE/logs:/var/log/gitlab \
-v /appdata/docker/Gitlab/Gitlab-CE/data:/var/opt/gitlab \
--restart always \
gitlab/gitlab-ce:latest
```
### 2.root密码查看

```
docker exec -it Gitlab-CE grep 'Password:' /etc/gitlab/initial_root_password
```


## 2.Gitlab-Runner安装

### 1.安装Runner

```
docker run -d \
--name Gitlab-Runner \
-v /appdata/docker/Gitlab/Gitlab-Runner/config:/etc/gitlab-runner \
-v /var/run/docker.sock:/var/run/docker.sock \
--restart always \
gitlab/gitlab-runner:latest
```

### 2.注册Runner

```
docker run --rm \
-v /mnt/user/appdata/Gitlab/Gitlab-Runner/config:/etc/gitlab-runner gitlab/gitlab-runner register \
--non-interactive \
--executor "docker" \
--docker-image alpine:latest \
--url "http://IP/" \
--registration-token "Token" \
--description "first-register-runner" \
--tag-list "MicroServer,Home" \
--run-untagged="true" \
--locked="false" \
--access-level="not_protected"
```

