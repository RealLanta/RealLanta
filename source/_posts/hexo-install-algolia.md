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

# 安装

各类主题的安装方法各不相同，我这里以我自己用的[Yun主题](https://github.com/YunYouJun/hexo-theme-yun)为例子

## 通过npm安装模块

> 您需要先安装 hexo-algolia 或 hexo-algoliasearch，并根据它们的说明文档进行相应的配置。

那我们就直接`npm install hexo-algolia`即可

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
