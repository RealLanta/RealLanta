---
title: 在Github搭图床？套个Vercel反代速度上天？——PicGo+Github+Vercel部署教程
date: 2021-12-17 23:31:41
categories: 教程
tags: 
 - 开发
 - Vercel
---

## 事情的起因

我目前使用的图床方案是Office E5的Sharepoint对接了Heroku上的OneManager

后来部分地区有访问问题套了个cf

不知道是cf还是M$的问题，速度拉的一批

刚好最近接触了PicGo这个东西，所以我们来研究一下

**另外，如果你要大量迁移图片的话我建议直接从Github上传，因为PicGo他的进度显示并不明确，有时候卡住了都不知道**

## 安装PicGo

Github:https://github.com/Molunerfinn/PicGo

PicGo是一个比较好用的图床上传软件，可以快捷拖动，也可以从剪贴板导入（剪贴板在Linux下需要安装`xclip`）

PicGo有Windows、Mac、Linux版

如果你跟我一样使用的是**ArchLinux**

那么你可以直接在AUR上安装

~~~bash
yay -S picgo-appimage
~~~

## 对接Github

首先你要在Github创建一个仓库，怎么创建不需要我说

创建完之后，你要去新建一个Token

![](https://pic.lanta.cyou/img/2021-12-17_23-48.png)

![](https://pic.lanta.cyou/img/2021-12-17_23-50.png)

![](https://pic.lanta.cyou/img/2021-12-17_23-51.png)

![](https://pic.lanta.cyou/img/2021-12-17_23-52.png)

![](https://pic.lanta.cyou/img/2021-12-17_23-54.png)

如果你有自定义域名就设置一下，这样就不用手动把他Github下载链接替换成你的域名了

至于那个**存储路径**，比如说我设置了img/，仓库就是这样的

![](https://pic.lanta.cyou/img/2021-12-17_23-56.png)

这样子就能设置图片了

## Github Pages+Vercel

其实这个非必要，但是考虑到PicGo的默认获取方式都是直接从Github的**Raw**获取，一样都有地区问题，所以还是得搞一下的

其实也很简单

### 设置Github Pages

![](https://pic.lanta.cyou/img/2021-12-17_23-58.png)

![](https://pic.lanta.cyou/img/2021-12-17_23-59.png)

（另外，如果你刚刚上传完打开Github Pages链接发现是404是很正常的，因为上传到Github的时候需要等一会儿部署到Github Pages）

### 设置Vercel

具体如何部署Vercel的反代我已经写过一篇[文章](https://www.lantacn.xyz/2021/12/11/vercel-proxy/)了，直接把Github Pages链接复制进去就行

## 图片测试

![](https://pic.lanta.cyou/img/836727.jpg)

## 尾巴

目前博客都是已经把图片那些的改为这个方案了

至于那些什么视频啊，文件啊

还是照样存Sharepoint

就这样吧
