---


title: 基于azerothcore-wotlk编译魔兽335服务端
excerpt: 基于 azerothcore-wotlk 构建 wow 335服务端
tags:
  - 游戏
  - 魔兽世界
categories:
	- 学习笔记
	- 游戏
index_img: /img/banner/wow.jpeg
abbrlink: d4751c53
date: 2023-02-10 02:51:22
sticky:
---



#### 安装环境

[官方文档](https://www.azerothcore.org/wiki/linux-requirements)

Debian系

```
apt update && apt full-upgrade -y && apt install git cmake make gcc g++ clang libssl-dev libbz2-dev libreadline-dev libncurses-dev libboost-all-dev mariadb-server mariadb-client libmariadb-dev libmariadb-dev-compat screen
```

Arch系

```
pacman -S base-devel git cmake clang boost mariadb
```

数据库，这一步非必要，可以使用源安装的数据库，也可以使用外置。

```
docker run -d \
  --name=mariadb \
  -e PUID=1000 \
  -e PGID=1000 \
  -e MYSQL_ROOT_PASSWORD=PASSWORD \
  -e TZ=Asia/Shanghai \
  -p 3306:3306 \
  -v path_to_data:/config \
  --restart unless-stopped \
  lscr.io/linuxserver/mariadb:latest
```





#### 源码下载

##### azerothcore-wotlk

内网

```
cd
git clone https://git.tnxw.com/Ming/azerothcore-wotlk.git --branch master --single-branch azerothcore --depth 1
```

外网

```
cd
git clone https://github.com/azerothcore/azerothcore-wotlk.git --branch master --single-branch azerothcore --depth 1
```





##### mod-eluna

内网

```
cd azerothcore-wotlk/modules
git clone https://git.tnxw.com/Ming/mod-eluna.git mod-eluna
```

外网

```
cd azerothcore-wotlk/modules
git clone https://github.com/azerothcore/mod-eluna.git mod-eluna
```





#### 编译安装

进入`azerothcore-wotlk`目录创建一个名为`build`文件夹

```
cd azerothcore-wotlk
mkdir build
```

这一步如果报错缺组件，根据提示安装对应组件。`azeroth-server`在用户`home`目录

```
cmake ../ -DCMAKE_INSTALL_PREFIX=$HOME/azeroth-server/ -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ -DWITH_WARNINGS=1 -DTOOLS_BUILD=all -DSCRIPTS=static -DMODULES=static
```

```
make -j 8
#8是服务器的逻辑CPU数量
```

```
make install
```



#### 导入数据

##### Data 文件

 进入`azeroth-server`导入地图，下载地址[data](https://github.com/wowgaming/client-data/releases)，

```
cd
cd azeroth-server/bin
wget https://github.com/wowgaming/client-data/releases/download/v16/data.zip
unzip data.zip
```

将 data 文件夹上传到 /root/azeroth-server 目录

#####  导入数据库

##### 连接数据库

源安装数据库

```
mysql -uroot -p 
```

docker方式安装的数据库

```
docker exec -it mariadb bash
mysql -uroot -p
```

##### 创建数据库

```
#password，密码可以自行修改
CREATE USER 'acore'@'%' IDENTIFIED BY 'password' WITH MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0;
GRANT ALL PRIVILEGES ON * . * TO 'acore'@'%' WITH GRANT OPTION;
CREATE DATABASE `acore_world` DEFAULT CHARACTER SET UTF8MB4 COLLATE utf8mb4_general_ci;
CREATE DATABASE `acore_characters` DEFAULT CHARACTER SET UTF8MB4 COLLATE utf8mb4_general_ci;
CREATE DATABASE `acore_auth` DEFAULT CHARACTER SET UTF8MB4 COLLATE utf8mb4_general_ci;
GRANT ALL PRIVILEGES ON `acore_world` . * TO 'acore'@'%' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON `acore_characters` . * TO 'acore'@'%' WITH GRANT OPTION;
GRANT ALL PRIVILEGES ON `acore_auth` . * TO 'acore'@'%' WITH GRANT OPTION;
exit;
```

```
#自动创建
mysql -uroot -p
source /root/azerothcore-wotlk/data/sql/create/create_mysql.sql
```



####  启动服务端

##### 修改配置文件

修改`conf`文件，移除目录下文件名中的`.dist`

```
cd /root/azeroth-server/etc
ls
authserver.conf  dbimport.conf  modules  worldserver.conf
```

修改`authserver.conf`  `dbimport.conf`  `worldserver.conf`，中的数据库地址和密码。

```
127.0.0.1;3306;acore;acore;acore_auth
```

- 127.0.0.1：数据库地址
- 3306：端口
- acore：第一个为用户名，第二个为用户密码
- acore_auth：数据库名

##### 修改Data路径

`DataDir = "."`  #此字段仅worldserver.conf有，将.更改为你的data文件夹路径



##### 启动服务

###### 手动启动

```
cd /root/azeroth-server/bin
```

启动`authserver`再新建终端启动`worldserver`

```
./authserver
```

```
./worldserver
```

进入数据库，更改IP为本机地址

```
use acore_auth;
UPDATE realmlist SET address = 'IP' WHERE id = 1;
```

###### 启动脚本

输入 `cd /root/azeroth-server/bin && vi auth.sh`，然后输入以下脚本：

```
#!/bin/sh
while :; do
./authserver
sleep 20
done
```

退出编辑后，输入 `vi world.sh`，然后输入以下脚本：

```
#!/bin/sh
while :; do
./worldserver
sleep 20
done
```

退出编辑后，输入 `vi restarter.sh`，然后输入以下脚本：

```
#!/bin/bash
screen -AmdS auth ./auth.sh
screen -AmdS world ./world.sh
```

退出编辑后，输入 `vi shutdown.sh`，然后输入以下脚本：

```
#!/bin/bash
screen -X -S "world" quit
screen -X -S "auth" quit
```

退出编辑后，输入 chmod 777 /root/azeroth-server/bin/*.sh 即可，常用命令有：

1. 输入 cd /root/azeroth-server/bin && ./restarter.sh 可启动服务端；
2. 输入 screen -r auth 或者 screen -r world 可以查看控制台，同时按 Ctrl+A+D 键可退出窗口，但不会结束进程；
3. 输入 cd /root/azeroth-server/bin && ./shutdown.sh 可关闭服务端。



创建账号

```
account create admin password
account set gmlevel admin 3 -1
```

修改客户端`Data\zhCN`目录下的`realmlist.wtf`文件，将IP地址改为服务端IP

```
set realmlist 127.0.0.1
```







其它记录

地图提取

```
/root/wow/bin/mapextractor
mkdir /root/wow/data
cp -r dbc maps /root/wow/data
/root/wow/bin/vmap4extractor
mkdir vmaps
/roo/wow/bin/vmap4assembler Buildings vmaps
cp -r vmaps /root/wow/data
mkdir mmaps
/root/wow/bin/mmaps_generator
cp -r mmaps /root/wow/data
```

















