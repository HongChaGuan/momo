---
title: 基于TrinityCore编译魔兽世界服务端
excerpt: 基于TrinityCore构建魔兽世界服务端
tags:
  - 游戏
  - 魔兽世界
categories:
	- 学习笔记
	- 游戏
index_img: /img/banner/wow.jpeg
abbrlink: 752edc47
date: 2023-02-23 21:16:22
sticky:
---







编译环境安装，官方文档[TrinityCore](https://trinitycore.info/install/requirements/linux)

Debian 11.x

```
apt-get update
apt-get install git clang cmake make gcc g++ libmariadb-dev libssl-dev libbz2-dev libreadline-dev libncurses-dev libboost-all-dev mariadb-server p7zip default-libmysqlclient-dev
update-alternatives --install /usr/bin/cc cc /usr/bin/clang 100
update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang 100
```

Ubuntu 22.04 

```
apt-get update
apt-get install git clang cmake make gcc g++ libmysqlclient-dev libssl-dev libbz2-dev libreadline-dev libncurses-dev libboost-all-dev mysql-server p7zip
update-alternatives --install /usr/bin/cc cc /usr/bin/clang 100
update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang 100
```

接下来我们要Git获取`TrinityCore`这个下载的时间比较长

```
git clone -b 3.3.5 git://github.com/TrinityCore/TrinityCore.git
```

内网

```
git clone -b 3.3.5 https://git.tnxw.com/Ming/TrinityCore.git
```

下载完成之后依次输入

```
cd TrinityCore
mkdir build
cd build
```

接下来我们就要准备编译文件，输入

```
cmake ../ -DCMAKE_INSTALL_PREFIX=/root/server -DTOOLS=1
```

接着就是编译服务器服务，有2种方式来编译。

第一种是傻瓜式，直接开始编译，这种速度一般。
```
make&&make install
```

第二种是利用全核心CPU make -j 线程数 install 这里线程数要改成自己CPU的线程数，这样可以全核心进行编译。
```
make -j 8
```

```
make install
```



#### 提取数据

上传客户端，完成之后我们开始提取地图。

注意：以下3步前后顺序不可逆，必须按顺序提取。如一步失败，需要删除这步产生的文件重头提取，否则会进入0%死循环。

进入客户端目录，输入地图提取命令，接着系统就会自动提取客户端地图文件。

```
cd /root/wow	#wow为客户端名称
/root/server/bin/mapextractor
```

完成之后依次输入命令

```
mkdir /root/server/data
cp -r dbc maps /root/server/data
```

接着输入提取命令

```
/root/server/bin/vmap4extractor
```

完成之后依次输入，开始编译地图

```
mkdir vmaps
/roo/server/bin/vmap4assembler Buildings vmaps
```

编译完成之后输入

```
cp -r vmaps /root/server/data
```

然后接着输入命令

```
mkdir mmaps
/root/server/bin/mmaps_generator
```

注意：这次提取是整个大地图提取，时间很长视机器情况30~120分钟不等

提取完成之后输入

```
cp -r mmaps /root/server/data
```

这步结束之后，可以通过`sftp`软件进去把/root/server/data里的文件打包下载到本地备份，不然以后丢失损坏还要再来一次。 



#### 设置服务端

进入 `/root/server/etc`

把 authserver.conf.dist 重命名为 authserver.conf 

把 worldserver.conf.dist 重命名为 worldserver.conf 

然后用编辑器打开修改，将 DataDir 的值改为“../data”，或者改为绝对路径，然后保存。



#### 设置数据库



登录数据库

```
mysql -uroot -p
```

```
CREATE USER 'trinity'@'localhost' IDENTIFIED BY 'trinity' WITH MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0;
GRANT USAGE ON * . * TO 'trinity'@'localhost';
CREATE DATABASE `world` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE DATABASE `characters` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
CREATE DATABASE `auth` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON `world` . * TO 'trinity'@'localhost' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON `characters` . * TO 'trinity'@'localhost' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON `auth` . * TO 'trinity'@'localhost' WITH GRANT OPTION;
```



接着访问 https://github.com/TrinityCore/TrinityCore/releases 下载最新的数据库

压缩包解压后可以得到一个几百兆的sql文件，这就是魔兽世界里的数据了，然后把刚才解压的sql文件上传到`/root/server/bin`。

依次输入

```
cd /root/server/bin
./worldserver
```

当显示 ready.. 时表示服务端已经启动了，数据库也导入完毕了。

输入命令关闭服务端

```
server shutdown 1
```

进入数据库，点击`auth`数据库，点击`realmlist`表，双击修改`address`地址为本机IP。





#### 开启服务端

输入命令启动账户服务

```
cd /root/server/bin
./authserver
```

重新打开一个终端，之前开启的窗口不要关闭

输入命令启动游戏服务端

```
cd /root/server/bin
./worldserver
```

启动成功之后输入

```
account create admin admin
```

创建一个账号 admin，密码 admin

这里服务端就开启完毕了。





#### 客户端配置

打开本地的wow游戏目录，在根目录新建一个 wow.bat 文件，并在里面输入

```
echo y | rd /s "Cache"
echo SET realmlist "192.168.0.201" > Data\zhTW\realmlist.wtf
echo SET realmlist "192.168.0.201" > Data\zhCN\realmlist.wtf
echo SET realmlist "192.168.0.201" > Data\enCN\realmlist.wtf
echo SET realmlist "192.168.0.201" > Data\enUS\realmlist.wtf
echo SET realmlist "192.168.0.201" > realmlist.wtf
start wow.exe
goto end
```

这里的IP就是本机的IP

如果你的wow客户端是大于12340版本的，需要下载替换wow.exe文件将版本降低到12340。



#### 自动运行

给Debian增加一个自动启动来自动运行服务，由于Debian默认没有启动rc.local文件，所以我们要自己来写一下。

输入命令

```
vi /etc/rc.local
```

然后输入内容

```
#!/bin/sh -e
sleep 5
nohup /root/server/bin/authserver > /dev/null 2>wow_auth.log &
nohup /root/server/bin/worldserver > /dev/null 2>wow_world.log &
exit 0
```

然后给脚本设置权限

```
chmod 777 /etc/rc.local
```

然后修改worldserver.conf文件里的DataDir 为 "/root/server/data"的绝对路径，否则启动会失败

```
DataDir = "/root/server/data"
```

然后修改worldserver.conf文件里的 Console.Enable 为 "0" 否则后台运行会占用100%CPU

```
Console.Enable=0
```

最后启动服务

```
systemctl enable rc.local
systemctl start rc.local
```

这样重启之后就会自动打开服务了