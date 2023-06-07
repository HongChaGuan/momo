---
title: Hexo安装
excerpt: >-
  Hexo是一款基于Node.js的静态博客框架，依赖少易于安装使用，可以方便的生成静态网页托管在GitHub和Coding上，是搭建博客的首选框架。大家可以进入hexo官网进行详细查看
tags:
  - Hexo
categories:
  - 学习笔记
  - 应用
index_img: /img/banner/Hexo.jpeg
abbrlink: dc9e160a
date: 2023-02-05 14:50:01
sticky:
---





### Hexo安装

#### 安装Git

windows：到git官网上下载,[Download git](https://git-scm.com/download/win),下载后会有一个Git Bash的命令行工具，以后就用这个工具来使用git。

linux：对linux来说实在是太简单了，因为最早的git就是在linux上编写的，只需要一行代码。

```text
sudo apt-get install git
```

安装好后，用`git --version` 来查看一下版本



#### 安装nodejs

Hexo是基于nodeJS编写的，所以需要安装一下nodeJs和里面的npm工具。

windows：[nodejs](https://nodejs.org/zh-cn)选择LTS版本就行了

linux：

```text
sudo apt-get install nodejs
sudo apt-get install npm
```

安装完后，打开命令行，检查一下有没有安装成功。

```text
node -v
npm -v
```

windows在git安装完后，就可以直接使用git bash来敲命令行了。

#### 安装hexo

前面git和nodejs安装好后，就可以安装hexo了，你可以先创建一个文件夹`blog`，然后`cd`到这个文件夹下

输入命令

```text
npm install -g hexo-cli
```

依旧用`hexo -v`查看一下版本

至此就全部安装完了。

接下来初始化一下hexo

```text
hexo init myblog
```

这个myblog可以自己取什么名字都行，然后

```bash
cd myblog //进入这个myblog文件夹
npm install
```

新建完成后，指定文件夹目录下有：

- node_modules: 依赖包
- public：存放生成的页面
- scaffolds：生成文章的一些模板
- source：用来存放你的文章
- themes：主题
- ** _config.yml: 博客的配置文件**

```text
hexo g
hexo clean
hexo server
```

打开hexo的服务，在浏览器输入localhost:4000就可以看到你生成的博客了。

#### 生成SSH添加到GitHub

回到你的git bash中，

```text
git config --global user.name "yourname"
git config --global user.email "youremail"
```

这里的yourname输入你的GitHub用户名，youremail输入你GitHub的邮箱。这样GitHub才能知道你是不是对应它的账户。

可以用以下两条，检查一下你有没有输对

```text
git config user.name
git config user.email
```

然后创建SSH,一路回车

```text
ssh-keygen -t rsa -C "youremail"
```

#### 将hexo部署到GitHub

这一步，我们就可以将hexo和GitHub关联起来，也就是将hexo生成的文章部署到GitHub上，打开站点配置文件 `_config.yml`，翻到最后，修改为 YourgithubName就是你的GitHub账户

```text
deploy:
  type: git
  repo: https://github.com/YourgithubName/YourgithubName.github.io.git
  branch: master
```

这个时候需要先安装deploy-git ，也就是部署的命令,这样你才能用命令部署到GitHub。

```text
npm install hexo-deployer-git --save
```

然后

```text
hexo clean
hexo generate
hexo deploy
```

其中 `hexo clean`清除了你之前生成的东西，也可以不加。 `hexo generate` 顾名思义，生成静态文章，可以用 `hexo g`缩写 `hexo deploy` 部署文章，可以用`hexo d`缩写

注意deploy时可能要你输入username和password。





### 更换设备操作

一样的，跟之前的环境搭建一样，

#### 安装git

```text
sudo apt-get install git
```

#### 设置git全局邮箱和用户名

```text
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```

#### 设置ssh key

```text
ssh-keygen -t rsa -C "youremail"
#生成后填到github和coding上（有coding平台的话）
#验证是否成功
ssh -T git@github.com
ssh -T git@git.coding.net #(有coding平台的话)
```

#### 安装nodejs

```text
sudo apt-get install nodejs
sudo apt-get install npm
```

#### 安装hexo

```text
sudo npm install hexo-cli -g
```

但是已经不需要初始化了，

直接在任意文件夹下，

```text
git clone git@………………
```

然后进入克隆到的文件夹：

```text
cd xxx.github.io
npm install
npm install hexo-deployer-git --save
```

#### 生成，部署：

```text
hexo g
hexo d
```

然后就可以开始写你的新博客了

```text
hexo new newpage
```

#### Tips:

不要忘了，每次写完最好都把源文件上传一下

```text
git add .
git commit –m "xxxx"
git push 
```

如果是在已经编辑过的电脑上，已经有clone文件夹了，那么，每次只要和远端同步一下就行了

```text
git pull
```
