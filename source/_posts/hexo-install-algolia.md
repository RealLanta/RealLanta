---
title: 静态博客也能有自己的搜索引擎了？——给Hexo加装Algolia实现增加搜索文章功能！
date: 2022-03-12 05:13:46
tags:
---

# 前言

最近又去看了一下[Yun主题的文档](https://yun.yunyoujun.cn/guide/third-party-support.html#algolia)，发现是有搜索引擎的支持的，在几个搜索引擎的方案中我选择了Algolia

# 配置环境？

因为我个人从ArchLinux回到了Windows，所以没有了原生的Linux环境，不方便操作，索性上CloudIDE了（我用的CloudStudio而不是Github Codespaces或goorm ide的原因是因为我不想~~被延迟搞到高血压~~）

![](https://pic.lanta.cyou/img/20220312051255.png)

# 注册Algolia

无论如何，我们必须要先注册Algolia，否则我们的网站将没有Algolia的使用权限，即使开启了对Algolia的支持，也只是一片空白

![](https://pic.lanta.cyou/img/20220312230356.png)

[Algolia官网](https://www.algolia.com/)

按道理来说只要填好邮箱和密码，然后再验证一下邮箱就可以注册完成

![](https://pic.lanta.cyou/img/20220312230814.png)

# 安装

各类主题的安装方法各不相同，我这里以我自己用的[Yun主题](https://github.com/YunYouJun/hexo-theme-yun)为例子

## 修改Hexo的配置文件

接下来，我们需要把Algolia的`appID`和`apiKey`填入至Hexo的配置文件

进到Algolia的[管理应用程序](https://www.algolia.com/account/applications)

![](https://pic.lanta.cyou/img/20220312232432.png)

点API Keys

![](https://pic.lanta.cyou/img/20220312232519.png)

这里就可以看到我们的appID和apiKey了

![](https://pic.lanta.cyou/img/20220312232628.png)

复制好appID和**Usage API Key**以及**Admin API Key**

然后在`_config.yml`加上这一段：

```yaml
algolia:
  appId: "就你刚刚复制的"
  apiKey: "就你刚刚复制的"
  adminApiKey: "就你刚刚复制的"
  chunkSize: 5000
  indexName: "my-hexo-blog"
  fields:
    - content:strip:truncate,0,500
    - excerpt:strip
    - gallery
    - permalink
    - photos
    - slug
    - tags
    - title
```

### 您正在使用Github Actions部署您的Hexo博客？

这里注意一下，如果你使用的是Github Actions部署Hexo的方案，我建议你使用Github自带的`Actions secrets`以保护自己的密钥不被泄露

![](https://pic.lanta.cyou/img/20220312232331.png)

配置文件的密钥区只需要填`${{ secrets.你取的名字 }}`，Github在部署的时候会自动帮你补上去

例如我自己的：

![](https://pic.lanta.cyou/img/20220312233516.png)

## 通过npm安装模块

> 您需要先安装 hexo-algolia 或 hexo-algoliasearch，并根据它们的说明文档进行相应的配置。

那我们可以两个都装，就直接`npm install hexo-algolia hexo-algoliasearch`即可

![](https://pic.lanta.cyou/img/20220312225250.png)

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
