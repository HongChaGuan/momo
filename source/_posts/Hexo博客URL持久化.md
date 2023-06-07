---
title: Hexo博客URL持久化
excerpt: 使用Hexo后，默认的链接是http://url/2020/02/10/hello-world这种类型的，这是由年/月/日/标题组成。如果调整过日期会变化，还有就是标题是中文或存在特殊符号的时候这样的链接可能就有问题
tags:
  - Hexo
categories:
  - 学习笔记
  - 应用
index_img: /img/banner/Hexo.jpeg
abbrlink: c57c27f9
date: 2023-01-25 17:42:13
sticky:
---


安装 `hexo-abbrlink` 插件：

```text
npm install hexo-abbrlink --save
```

编辑`_config.yml`中找到`permalink`注释掉，加入如下代码

```text
#permalink: :year/:month/:day/:title/
permalink: article/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
```

效果：

```
http://10.0.0.7:4000/article/4e67a04d.html
```

**注意**：因为你的文章目录结构已经改变，和之前的`年月日`文章路径产生冲突，所以你需要重新生成文章`hexo clean`和`hexo g`，不然会报undefined错
