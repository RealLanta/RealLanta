---
title: 把OneDrive作为文件床或者图床？——使用Vercel部署OneManager
date: 2022-01-14 21:42:29
categories: 
 - 教程
tags:
 - 开发
 - Vercel
 - OneDrive
---

# 前言

Heroku+Cloudflare的方案属实难用，速度慢的要死

虽然有直接使用Cloudflare Workers的版本，但我还是建议直接在Vercel部署

首先Vercel它时不时可能会发神经，导致出现很多玄学错误，通常来讲多试几次就没什么问题了

~~实在不行去[TG群](https://t.me/joinchat/I_RVc0bqxuxlT-d0cO7ozw)锤作者~~

# 安装

## 生成密钥

去Vercel的设置里[生成密钥](https://vercel.com/account/tokens)

![](https://pic.lanta.cyou/img/2022-01-14_21-58.png)

名字随便取一个就行了

<font size="+3" color="red">密钥不会二次显示，请务必要保管好</font>

## 部署

去[Github](https://github.com/qkqpttgf/OneManager-php)直接下载Zip，不要去[Releases](https://github.com/qkqpttgf/OneManager-php/releases)下

> Releases只是当存档在用的，并不是最新代码。

下载完之后到作者给的[部署页面](https://scfonedrive.github.io/Vercel/Deploy.html)
![](https://pic.lanta.cyou/img/MD6NkQb.png)

部署完之后直接进设置向导

## 设置向导

![](https://pic.lanta.cyou/img/2022-01-14_21-55.png)

![](https://pic.lanta.cyou/img/2022-01-14_21-57.png)

然后你成功得到了一个错误

![](https://pic.lanta.cyou/img/2022-01-14_22-00.png)

对于该错误，幸运的啥事都不会发生，不幸运的...删库重开

![](https://pic.lanta.cyou/img/2022-01-14_22-01.png)

点击“返回首页”然后登录后台继续设置

![](https://pic.lanta.cyou/img/2022-01-14_22-02.png)

## 进一步的设置

![](https://pic.lanta.cyou/img/2022-01-14_22-03.png)

接着添加盘就可以了，如果你在添加盘的过程中遇到了玄学问题，删库重开总能解决

# 删库重开以解决问题

![](https://pic.lanta.cyou/img/photo_2022-01-14_20-00-23.jpg)

![](https://pic.lanta.cyou/img/2022-01-14_22-04.png)

![](https://pic.lanta.cyou/img/2022-01-14_22-05.png)

# 主题

他自带一个主题，我觉得挺好看的（但是登录按钮不会显示，要进入后台得要你的域名`https://你的域名/?admin`这样才可以进去）

![](https://pic.lanta.cyou/img/2022-01-14_22-14.png)

![](https://pic.lanta.cyou/img/2022-01-14_22-15.png)

![](https://pic.lanta.cyou/img/2022-01-14_22-15_1.png)

但是登录管理员账号之后主题就没用了（悲）

# 尾巴

OneManager确实是个好东西

~~当然Vercel部署的任何问题到底是作者的错还是Vercel的错我们就不知道了~~

有问题可以在评论区问我
