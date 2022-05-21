---
title: 如何在Heroku上部署Alist？
date: 2022-05-21 13:46:34
tags: 
 - Heroku
categories: 教程
---

# 前言

因为某些原因，我需要用到[Alist](https://github.com/Xhofe/alist)，可是我不想花钱买服务器

那么Heroku是可以部署Docker服务的，Alist有Docker镜像，所以我开始打起了在Heroku部署Alist的主意

# 部署

目前Github上已经有了大佬做的项目[sbwml/alist-heroku](https://github.com/sbwml/alist-heroku)，直接点击部署按钮即可

默认的管理密码为`alist`

![](https://pic.lanta.cyou/img/2022-05-21_14-22.png)

部署完成后就可以直接打开了

![](https://pic.lanta.cyou/img/2022-05-21_14-23.png)

# 进阶设置

## 添加账号

具体请查看[Alist Doc](https://alist-doc.nn.ci/docs/intro)

## 让 Heroku 应用保持在线

在你的Alist一阵子没有流量访问的时候Heroku会休眠你的应用，因为Heroku的数据是临时存储的，所以就会导致应用数据丢失

当然，我们也可以通过自己制造流量来解决这个问题

方法一：通过 UptimeRobot 对应用地址进行状态监控，UptimeRobot 会对地址每 5 分钟请求一次使其产生访问流量，Heroku 应用将一直保持活跃状态。

方法二：使用 curl 命令配合定时任务每10分钟获取一次 Alist 文件目录列表同样可以达到应用不睡眠效果，环境可以是你家里的路由器或其他有 curl 命令的 linux。

```bash
curl -d '{"path":"/","password":"","page_num":1,"page_size":30}' \
    -H "Content-Type: application/json" \
    -X POST https://应用名称.herokuapp.com/api/public/path
```

## 使用免费 MySQL 远程数据库

使用 MySQL 远程数据库可以避免应用程序休眠导致的应用数据丢失问题

注册免费 MySQL 远程数据库：https://www.db4free.net/

完成注册后，你的邮箱会收到 MySQL 连接地址、端口、数据库名称、用户信息

在部署的时候将数据库存储方式改为 `mysql` 填上 MySQL 连接地址、端口、数据库名称、用户信息 即可

# 最后

目前Alist已经替代了我之前使用的OneManager（因为OneManager的体验是不怎么好，也不如Alist）

这次用了Alist终于知道有多香了
