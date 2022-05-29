---
title: Hexo也能有网页后台了？——Qexo部署教程
date: 2022-05-29 11:53:14
tags: 
 - Qexo
 - Hexo
 - Vercel
categories: 教程
---

# 什么是Qexo？

Qexo 是一个快速、强大、漂亮的在线 Hexo 编辑器。使用 GPL 开源协议。支持包括且不限于在 Vercel 等平台部署

# 事前准备

**准备好一个已经可以使用Github Actions部署Hexo的仓库**

**如果不知道怎么搞可以看看这篇**[知乎的图文](https://zhuanlan.zhihu.com/p/170563000)

![](https://pic.lanta.cyou/img/2022-05-29_11-33.png)

# 注册MongoDB

**首先我们要去**[注册](https://www.mongodb.com/cloud/atlas/register)MongoDB来给Qexo提供数据库

![](https://pic.lanta.cyou/img/2022-05-29_11-17.png)

![](https://pic.lanta.cyou/img/2022-05-29_11-18.png)

**新建成功后会自动跳到"Security"的"Quickstart"**

![](https://pic.lanta.cyou/img/2022-05-29_11-18_1.png)

**等待几分钟数据库新建完成后**

**点击Connect**

![](https://pic.lanta.cyou/img/2022-05-29_11-21.png)

**允许所有IP段访问（0.0.0.0）**

**然后连接方式选择MangoDB Shell**

![](https://pic.lanta.cyou/img/2022-05-29_11-22.png)

![](https://pic.lanta.cyou/img/2022-05-29_11-23.png)

# 部署到Vercel

**点击下面的按钮部署**

[![部署到 Vercel](https://camo.githubusercontent.com/5e471e99e8e022cf454693e38ec843036ec6301e27ee1e1fa10325b1cb720584/68747470733a2f2f76657263656c2e636f6d2f627574746f6e)](https://vercel.com/new/clone?repository-url=https://github.com/am-abudu/Qexo)

**第一次部署会直接爆炸，问题不大，这是因为我们还没有设置数据库**

![](https://pic.lanta.cyou/img/photo_2022-05-29_11-06-41.jpg)

**回到项目首页，点击上面的"Settings"**

![](https://pic.lanta.cyou/img/2022-05-29_11-26.png)

**然后点左侧栏的"Environment Variables"**

![](https://pic.lanta.cyou/img/2022-05-29_11-27.png)

**照着下列表格来添加**

| **名称**         | **意义**                                                              | **示例**                                    |
| ------------------ | ----------------------------------------------------------------------- | --------------------------------------------- |
| **DOMAINS**      | **你所允许通信的安全域名，可以添加多个（ 注意双引号而且是英文半角）** | **["XXX.vercel.app", "XXX.com"]**           |
| **MONGODB_HOST** | **MongoDB 数据库连接地址**                                            | **mongodb+srv://cluster0.xxxx.mongodb.net** |
| **MONGODB_PORT** | **MongoDB 数据库通信端口 默认应填写 27017**                           | **27017**                                   |
| **MONGODB_USER** | **MongoDB 数据库用户名**                                              | **chenrui**                                 |
| **MONGODB_DB**   | **MongoDB 数据库名**                                                  | **Cluster0**                                |
| **MONGODB_PASS** | **MongoDB 数据库密码**                                                | **JWo0xxxxxxxx**                            |

**添加完之后到顶部的"Deployments"然后"Redeploy"**

![](https://pic.lanta.cyou/img/2022-05-29_11-30.png)

**然后就能顺利部署成功了**

# 初始化Qexo

**设置一下用户配置，API密钥看你自己配置**

![](https://pic.lanta.cyou/img/2022-05-29_11-32.png)

## 配置Github

**你使用了Github Actions部署Hexo的仓库**

```
username/repo
```

**仓库的分支**

```
master
```

### Github 密钥

​

​

> **注意，请保留好该密钥，密钥生成后出于安全原因不会再出现，也不要泄漏给别人**

**</div>**

**于 **[Github 设置](https://github.com/settings/tokens) 生成的 Token 需要 Repo 的读取和写入权限

```
wrq_P8sYPlYA9fjMlOPEYSKA84xxxxxxxxxxxxxx
```

![image-20220529113805881](file:///home/lanta/.config/Typora/typora-user-images/image-20220529113805881.png?lastModify=1653796403)

### 仓库路径

**仓库的路径 若为根目录留空**

```
path/
```

## 图床配置

**参照官方文档来设置，如果你要使用别的图床程序（比如PicGO）也可以直接跳过**

[Qexo文档 - 自定义图床配置](https://github.com/Qexo/Qexo/wiki/%E8%87%AA%E5%AE%9A%E4%B9%89%E5%9B%BE%E5%BA%8A%E9%85%8D%E7%BD%AE)

[Qexo文档 - S3 图床配置](https://github.com/Qexo/Qexo/wiki/S3-%E5%9B%BE%E5%BA%8A%E9%85%8D%E7%BD%AE)

[Qexo文档 - FTP 图床配置](https://github.com/Qexo/Qexo/wiki/FTP-%E5%9B%BE%E5%BA%8A%E9%85%8D%E7%BD%AE)

## Vercel配置

### Vercel密钥

​

​

> **注意，请保留好该密钥，密钥生成后出于安全原因不会再出现，也不要泄漏给别人**

**</div>**

**首先前往**[Vercel后台](https://vercel.com/account/tokens)生成密钥

![](https://pic.lanta.cyou/img/2022-05-29_11-42.png)

![](https://pic.lanta.cyou/img/2022-05-29_11-43.png)

### 项目ID

**前往你的Qexo项目**

**Settings -> General**

**往下滑就可以看到Project ID**

![](https://pic.lanta.cyou/img/2022-05-29_11-45.png)

## 完成

![](https://pic.lanta.cyou/img/2022-05-29_11-50.png)

​

​

> **至此，Qexo的安装就已经完成了**

**</div>**

