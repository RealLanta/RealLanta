---
title: 类CDN？利用Vercel AGA网络加速网站国内访问速度
date: 2021-12-11 11:17:04
categories: 教程
tags: 
 - Vercel
 - 开发
---

## 介绍

Vercel你可以理解为跟Heroku差不多的东西，总之就是可以搭建动态/静态网站（不过动态网站需要一点特殊手段）

此外Vercel用起来应该算是比较麻烦。。。？

## 安装NPM

首先你要安装Node.js

如果你正在使用Windows的话，NPM可能不太友好

当然你也可以去用[CloudStudio](https://cloudstudio.net/)或者[goorm](https://ide.goorm.io/)这类的IDE服务，有小鸡的也行，这样子就不用在你自己电脑装NPM了

### 设置CNPM

如果你自己的网络连接NPM不友好可以尝试使用CNPM

~~~bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
~~~

## 安装Vercel CLI

>Vercel CLI的安装需要使用root权限，如果你是在普通用户的环境下安装记得加sudo

使用NPM安装

~~~bash
npm i -g vercel
~~~

使用CNPM安装

~~~
cnpm i -g vercel
~~~

登录Vercel

~~~bash
vercel login
~~~

![](https://pic.lanta.cyou/img/2021-12-11_11-25.png)

**如果你Vercel帐号是使用Github登录的直接Github就可以了**

## 建立脚本

在你自己喜欢的地方新建一个文件夹，文件夹名字随你自己取一个

然后在里面新建一个json文件，比如*proxy.json*

输入以下内容

~~~json
{
    "name": "这个名字可以随便填",
    "version": 2,
    "routes": [
      {"src": "/(.*)","dest": "网址/$1"}
    ]
  }
  
~~~

网址后面的*/$1*要**保留**，记得带**协议头**（http或https）

例如我自己博客的：

~~~json
{
    "name": "BlogProxy",
    "version": 2,
    "routes": [
      {"src": "/(.*)","dest": "https://blog.lantacn.xyz/$1"}
    ]
  }
  
~~~

>**注意**
>
>如果你想要你自己的主域名使用Vercel反代，请给你的网站另外安排一个子域名
>
>网站里面如果有以前的资源引用不用改域名，直接用就可以了
>
>因为你的最终访问地址依旧是你的主域名

## 上传至Vercel

~~~bash
vercel -A 文件名.json --prod
~~~

首先第一个他会问你是不是要把这个文件夹里面的文件上传至Vercel，这里肯定要Yes

~~~bash
Set up and deploy “~/Documents/vercel-proxy”? [Y/n] y
~~~

第二个问你上传至哪个帐号的Vercel，我们就绑定了一个Vercel的账号，如果你绑定了多个请你自行选择

~~~bash
Which scope do you want to deploy to? 帐号
~~~

第三个问你连接到哪个现成的Vercel项目，直接输入项目名

~~~
What’s the name of your existing project? 项目名
~~~

>**知识点**
>
>如果你没有创建好的项目，请输入N，他就可以帮你创建项目

然后等他跑完就可以了

## 绑定域名

到Vercel的项目设置添加域名

![](https://pic.lanta.cyou/img/2021-12-11_11-42.png)

同时，你的域名要解析到Vercel

| 类型  | 名称     | 内容                 |
| ----- | -------- | -------------------- |
| CNAME | 你的域名 | cname.vercel-dns.com |

设置好之后点击**Refresh**即可

>域名的DNS管理我建议用Cloudflare，秒解析挺好的
>
>但是记得添加域名的时候把**黄色的小云**给**关掉**，也就是**灰色小云**
>
>否则Vercel的加速等于没有，就用了Cloudflare的CDN了

## 检查是否生效

直接ping然后查IP

![](https://pic.lanta.cyou/img/2021-12-11_12-01_1.png)

![](https://pic.lanta.cyou/img/2021-12-11_12-03.png)

甚至你还可以直接访问他的IP

![](https://pic.lanta.cyou/img/2021-12-11_12-03_1.png)

这样就搞定了

## 速度演示

拿我自己的联通卡开流量做演示（清了缓存的）

背景图加载慢是Cloudflare的原因（拿的Workers反代）

<link rel="stylesheet" href="https://g.alicdn.com/de/prismplayer/2.9.16/skins/default/aliplayer-min.css" />

<script type="text/javascript" charset="utf-8" src="https://g.alicdn.com/de/prismplayer/2.9.16/aliplayer-min.js"></script>

<div class="prism-player" id="player-con"></div><script>var player = new Aliplayer({  "id": "player-con",  "source": "https://cdn.lanta.cyou/ScreenRecord-2021-12-11-12-18-53.mp4",  "width": "100%",  "height": "500px",  "autoplay": false,  "isLive": false,  "cover": "",  "rePlay": false,  "playsinline": true,  "preload": true,  "controlBarVisibility": "hover",  "useH5Prism": true}, function (player) {    console.log("The player is created");  });</script>

## 疑难解答

### 子域名？主域名？

如果你想要你的主域名可以访问到博客，就要给你的博客先分配一个子域名

这里用一张图来表示

![](https://pic.lanta.cyou/img/2021-12-11_11-41.png)

如图所示，反代过去之后的网址会从原先的*blog.lantacn.xyz*变成*lantacn.xyz*

所以通过这种方式就可以继续给你的博客使用主域名

### 为什么我这边这么慢？

地区问题，我的朋友[iVampireSP](https://ivampiresp.com/)他那边江苏乱墙
