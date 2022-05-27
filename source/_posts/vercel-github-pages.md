---
title: 把网站仓库设置为私人？——如何在Vercel部署Github Pages（将Github Pages迁移至Vercel）
date: 2021-12-31 19:12:38
tags:
 - Vercel
 - Github Pages
 - 开发
categories: 教程
---

# 事情的起因

摆着个网站仓库在那不是很好看，而且还有个PicGo图床仓库在那

如果是我的话我是不愿意把网站仓库公开的

但是私人的话就不能用Github Pages

那我突然想到Waline的Vercel就是私人仓库，既然可以部署到Vercel，那就直接这么干得了

反正现在也是Vercel反代Github Pages加速，倒不如直接让他在Vercel

# 设置仓库为私人

首先我们要跑去Github将仓库可见性改为私人

![](https://pic.lanta.cyou/img/2021-12-31_19-16.png)

# 部署到Vercel

现在我们就要把网站部署到Vercel了，因为Vercel的特殊性，是可以访问私密仓库的

## 新建项目

首先要新建项目

![](https://pic.lanta.cyou/img/2021-12-31_19-18.png)

![](https://pic.lanta.cyou/img/2021-12-31_19-19.png)

然后直接部署就是了

## 设置域名

如果你没有域名的话，你还可以用vercel.app的域名

![](https://pic.lanta.cyou/img/2021-12-31_19-21.png)

接着等着域名生效就完事了

# 尾巴

同理，该方法也是可以用于上次的[PicGo图床](https://www.lanta.cyou/2021/12/17/picgo/)

当然，如果你要更新网站文件直接改Github仓库就可以了，两边是同步的

那么很简单，就这样，没了
