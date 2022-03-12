---
title: 静态博客也能有自己的搜索引擎了？——给Hexo加装Algolia实现增加搜索文章功能！
date: 2022-03-12 05:13:46
tags: 
 - Hexo
 - Github Pages
 - Algolia
categories: 教程
---

# 前言

最近又去看了一下[Yun主题的文档](https://yun.yunyoujun.cn/guide/third-party-support.html#algolia)，发现是有搜索引擎的支持的，在几个搜索引擎的方案中我选择了Algolia

# 配置环境？

因为我个人从ArchLinux回到了Windows，所以没有了原生的Linux环境，不方便操作，索性上CloudIDE了（我用的CloudStudio而不是Github Codespaces或goorm ide的原因是因为我不想~~被延迟搞到高血压~~）

![](https://pic.lanta.cyou/img/20220312051255.png)

# 注册并配置Algolia

无论如何，我们必须要先注册并配置好Algolia，否则我们的网站将没有Algolia的使用权限，即使开启了对Algolia的支持，也只是一片空白

![](https://pic.lanta.cyou/img/20220312230356.png)

[Algolia官网](https://www.algolia.com/)

按道理来说只要填好邮箱和密码，然后再验证一下邮箱就可以注册完成

![](https://pic.lanta.cyou/img/20220312230814.png)

## 新建Indices

在左侧栏的**Products**有个**Search**，我们点进去

![](https://pic.lanta.cyou/img/20220313001226.png)

一打开就是Indices了，我们点击**Create Index**

![](https://pic.lanta.cyou/img/20220313001245.png)

![](https://pic.lanta.cyou/img/20220313001318.png)

这样就可以了

## API Key
进到Algolia的[管理应用程序](https://www.algolia.com/account/applications)

![](https://pic.lanta.cyou/img/20220312232432.png)

点API Keys

![](https://pic.lanta.cyou/img/20220312232519.png)

这里就可以看到我们的appID和apiKey了

![](https://pic.lanta.cyou/img/20220312232628.png)

不过这里我们只需要**Admin API Key**，因为它自带的API Key基本上是不能用的，所以接下来我们要新建一个API Key出来

### 新建API Key

点击All API Keys，然后New API Key

![](https://pic.lanta.cyou/img/20220312235151.png)

**Description**也就是简介，这里看你自己喜欢怎么填

**Indices**中的**Index name**这个就是我们刚刚新建的

![](https://pic.lanta.cyou/img/20220313000048.png)

**ACL**这里，我们把**addObject**和**deleteObject**加上去

![](https://pic.lanta.cyou/img/20220312235608.png)

这样，你就得到了一个新的API Key了

![](https://pic.lanta.cyou/img/20220313000211.png)

复制好appID和**API Key**以及**Admin API Key**备用

# 安装

各类主题的安装方法各不相同，我这里以我自己用的[Yun主题](https://github.com/YunYouJun/hexo-theme-yun)为例子

# 通过npm安装模块

> 您需要先安装 hexo-algolia 或 hexo-algoliasearch，并根据它们的说明文档进行相应的配置。

因为各种原因我推荐你使用**hexo-algolia**那我们就直接`npm install hexo-algolia`即可

![](https://pic.lanta.cyou/img/20220312225250.png)

## 修改Hexo的配置文件

在`_config.yml`加上这一段：

```yaml
algolia:
  applicationID: "" #AppID
  apiKey: "" #刚刚新建的API Key
  adminApiKey: "" #Admin API Key
  indexname: "" #这个就是Indices填的
  chunkSize: 5000
```

## 修改Yun的配置文件

如果你还在使用旧版的Yun，那么你的配置文件应该是在`source/_data/yun.yml`

但是新版Yun已经更换了依赖，配置文件改放到`Hexo根目录/_config.yun.yml`

不过这不是重点，配置文件格式没有变化，是可以通用的，所以影响不大

我们就直接在配置文件加上这一段即可

```yaml
algolia_search:
  enable: true
  hits:
    per_page: 10 # the number of search results per page
```

![](https://pic.lanta.cyou/img/20220312230011.png)

# 参考资料
本文参考了以下资料进行写作

https://yun.yunyoujun.cn/guide/third-party-support.html#algolia

https://blog.csdn.net/qq_35479468/article/details/107335663