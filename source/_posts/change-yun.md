---
title: 我成功毁了整个Yun主题
date: 2021-12-18 07:51:18
categories: 每日秃头
tags: 日常
---

## 事情的起因

我需要在导航栏的左侧加几个自定义链接

按照[Yun Docs](https://yun.yunyoujun.cn/guide/config.html#%E5%9B%BE%E6%A0%87-icon)来看的话，默认使用的是[Remix Icon](https://remixicon.com/)

所以我就尝试着加入Remix Icon的`earth-line`这个图标

按照文档去做的话，直接`icon: icons earth-line`就可以了

结果我成功得到了一片空白

![](https://pic.lanta.cyou/img/2021-12-18_07-55.png)

这个问题我可就没搞明白了，最终也没找到解决办法

## 改用Material Design icons

很明显，Remix Icon肯定是要更适合这个主题的

但我没办法，只好改变策略

首先要在主题配置里面加上这一段

~~~yaml
head:
  css:
    material: https://fonts.googleapis.com/icon?family=Material+Icons
~~~

并且主题也进行了适配

~~~yaml
icon: material-icons public
~~~

~~抱歉我真没找到除了public图标以外的地球图标~~

然后一个Material Design的图标闯了进来必定是要破坏协调感的

所以我干脆全部都换成Material Design了

![](https://pic.lanta.cyou/img/2021-12-18_08-03.png)

~~问题彻底解决~~
