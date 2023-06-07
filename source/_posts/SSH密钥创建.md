---
title: SSH密钥创建
excerpt: SSH 密钥是安全外壳 (SSH) 协议中使用的安全访问凭证。 SSH 密钥使用基于公钥基础设施 (PKI) 技术（数字身份认证和加密的黄金标准）的密钥对来提供安全且可扩展的认证方法。由于 SSH 协议广泛用于云服务、网络环境、文件传输工具、配置管理工具和其他依赖计算机的服务中的通信，大多数组织使用 SSH 密钥来验证身份并保护这些服务免受意外使用或恶意攻击。
categories:
	- 学习笔记
	- Linux
index_img: /img/banner/SSH.png
tags:
    - 密钥
    - SSH
abbrlink: 6d2a3284
date: 2023-01-25 10:42:15
sticky:
---


### 1.创建密钥

```shell
ssh-keygen -t rsa -b 4096
#执行创建命令，-b参数为指定密钥长度
Generating public/private rsa key pair.
Enter file in which to save the key (/home/pi/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
#设置密钥密码，可以直接回车跳过
Enter same passphrase again: 
#确认密码，可以直接回车跳过
Your identification has been saved in /home/pi/.ssh/id_rsa.
#公钥
Your public key has been saved in /home/pi/.ssh/id_rsa.pub.
#私钥
The key fingerprint is:
SHA256:B25KhIK5xNfR3j7SBi52+e2Z+t0K7Ouhg3IzRqxyuEE pi@Debian
The key's randomart image is:
+---[RSA 4096]----+
|     ..          |
|.o  ....         |
|oo....o o        |
|.... . + o       |
|.  E  + S .      |
|  .  + X *.      |
|   .o * = o+     |
|   o.+ * oo.* .  |
|   .+ + oo*O.o.. |
+----[SHA256]-----+
```

### 2.检查.ssh下`authorized_keys`文件

```shell
root@Debian:~# cd .ssh/
root@Debian:~# ls .ssh/
id_rsa  id_rsa.pub
root@Debian:~/.ssh# cat id_rsa.pub >> authorized_keys
```

### 	3.修改权限

#### 	修改`.ssh的权限为700`,

```shell
root@Debian:~# cd
root@Debian:~# chmod 700 .ssh
root@Debian:~# cd .ssh/
```

#### 	`authorized_keys`的权限为600或者更严格的400

```shell
root@Debian:~# cd
root@Debian:~# chmod 600 .ssh/authorized_keys
```

### 4.修改ssh的配置

```shell
vi /etc/ssh/sshd_config
```

```
# 允许root登录，但是禁止root密码登录
PermitRootLogin prohibit-password
 
# 通过RSA认证
RSAAuthentication yes
 
# 允许pubKey（id_rsa.pub）登录
PubkeyAuthentication yes

#使用密码  no为不使用密码
PasswordAuthentication yes                        
```


