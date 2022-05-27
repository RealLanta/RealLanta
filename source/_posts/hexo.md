---
title: 从0开始部署Hexo博客？——如何在Github Pages部署一个Hexo博客
date: 2021-12-25 11:47:04
tags: 
 - Hexo
 - 开发
 - 日常
categories: 教程
---

# 前言

在这篇文章中，我将会教你如何从0开始部署一个属于你自己的Hexo博客

首先你需要一台PC~~（DO NOT USE NODE.JS ON WINDOWS）~~

一个注册好的Github帐号和一个Github仓库（仓库的名字`取名为你的用户名.github.io`）

以及梯子

本文测试环境为Arch Linux，其他Linux的部署过程基本一致，记得给你自己的系统装上Node.js

## Github SSH配置

> 本文并不涉及该方面的配置，但是为了照顾各位小白我搬运了一段，以下文段摘自[这里](https://xiongaoxr.work/2019/11/10/Linux%E9%83%A8%E7%BD%B2Hexo%E5%8D%9A%E5%AE%A2/)
>

### 创建邮箱key

```
ssh-keygen -t rsa -C “你注册github的邮箱”
```

然后回车三次，其实这个命令就是让你选择`ssh密钥`生成的位置，默认就好，这样待会也好找

[![ssh-keygen.png](https://file.moetu.org/images/2019/11/12/ssh-keygen2ccb992b7d74073c.png)](https://moetu.org/image/VQ7Zh)

### 复制本地ssh密钥

执行以下命令，并复制ssh密钥

```
  
cd /root/.ssh
cat id_rsa.pub
```

[![密钥复制.png](https://file.moetu.org/images/2019/11/12/keygen45ad544b41418525.png)](https://moetu.org/image/VQ6hE)

### 创建密钥容器

打开`github`，新建一个`ssh key`，名字自拟，把刚才复制的密钥粘贴进去即可

[![打开ssh.png](https://file.moetu.org/images/2019/11/12/sshc3c42c4ab84fd9c0.png)](https://moetu.org/image/VQYWI)

### 粘贴本地ssh密钥

[![密钥粘贴.png](https://file.moetu.org/images/2019/11/12/ssh320f6f5a89ba8605.png)](https://moetu.org/image/VQaa8)

### 复制github ssh密钥

打开仓库主页面，复制仓库`ssh地址`

[![复制仓库地址.png](https://file.moetu.org/images/2019/11/12/use-sshd0f090e57c60d6da.png)](https://moetu.org/image/VQPYY)

## 安装Hexo部署环境

首先从Node.js安装Hexo

~~~bash
npm install hexo-cli -g
~~~

创建软链接（创建软链接后可以直接在终端输入hexo而不用拖着整个目录运行）

~~~bash
ln -s /usr/node/lib/node_modules/hexo-cli/bin/hexo /usr/local/bin/hexo
~~~

新建一个文件夹给你的博客，并让Hexo在你的文件夹开始部署软件，例如：

~~~bash
mkdir Hexo
cd Hexo
hexo init
~~~

### 本地预览

部署完之后你可以先看看效果如何，输入`hexo s`

接着在浏览器访问`localhost:4000`即可

如果你想更换预览的端口，你可以输入`hexo s -p 端口`

### 安装Git Push插件

Hexo本身并没有通过Git Push到Github的能力，需要安装插件来实现

~~~bash
npm install hexo-deployer-git –save
~~~

### 将Github仓库连接到本地

配置好你的Git

~~~bash
git config --global user.name "用户名"
git config --global user.email 邮箱
~~~

## 选择一个适合你的编辑器

如果你是万能Vim~~主义者~~那么你可以直接使用Vim来编辑Hexo的配置文件，当然，我喜欢VSCode

Hexo的文章格式是Markdown，所以你需要一个支持Markdown的编辑器

文章编辑器你可以用Typora，但是他已经开始收费了，你可以安装旧版Typora以解决这个问题（操，我滚动更新的时候也把Typora更上去了）

VSCode也支持Markdown写作（但是没有预览效果）

## 配置Hexo

当你的Hexo已经部署完毕之后我们就可以进行进一步的配置了

以下配置都在目录里的`_config.yml`里面设置

### 设置网站信息

在`Site`进行设置

~~~yaml
# Site
title: Lanta Blog ## 博客标题
subtitle: 'Lanta的博客' ## 副标题
description: '' ## 简介（此处可不设置）
keywords: ## 关键词（此处可不设置）
author: Lanta ## 作者名称
language: zh-CN ## 语言
timezone: '' ## 时区（此处可不设置）
~~~

### 设置域名

在`URL`设置访问域名

例如：

~~~yaml
# URL
url: https://lanta.cyou/ ## 此处写入访问的地址，比如你的用户名.github.io
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true 
  trailing_html: true
~~~

#### 自定义域名

如果你要使用自定义域名，请到你Hexo目录下的`source`文件夹添加一个`CNAME`文件（不用加后缀，直接CNAME）

CNAME文件里写入你的域名，比如我是*blog.lanta.cyou*，那么我直接写`blog.lanta.cyou`就可以了

![](https://pic.lanta.cyou/img/20211225121618.png)

### 设置仓库

在`Deployment`修改仓库地址，增加`master`分支（当然你也可以直接用main分支）

例如：

~~~yaml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repo: git@github.com:PhyllisJohnson/phyllisjohnson.github.io.git
  branch: main ## 这个是分支名
~~~

### 安装标签和分类以及RSS插件

Hexo本身不具有标签和分类的功能，也是需要自行安装

### 安装标签插件

~~~bash
npm install hexo-generator-tag
~~~

#### 创建标签列表页面

~~~bash
hexo new pages tags
~~~

成功后显示：

~~~bash
INFO  Created: ~/Documents/blog/source/tags/index.md
~~~

上面的路径（指`~/Documents/blog/source/tags/index.md`也就是这个页面的文件）

打开后的默认内容：

~~~yaml
---
title: 标签
date: 20XX-XX-XX XX:XX:XX
---
~~~

将`type: "tags"`添加进去，添加后如下：

~~~yaml
---
title: 标签
date: 20XX-XX-XX XX:XX:XX
type: "tags"
---
~~~

随后**不必再书写其他内容**保存并关闭文件即可

### 安装分类插件

~~~bash
npm install hexo-generator-category
~~~

#### 创建分类列表页面

~~~bash
hexo new pages categories
~~~

成功后显示：

~~~bash
INFO  Created: ~/Documents/blog/source/categories/index.md
~~~

上面的路径（指`~/Documents/blog/source/categories/index.md`也就是这个页面的文件）

打开后的默认内容：

~~~yaml
---
title: 分类
date: 20XX-XX-XX XX:XX:XX
---
~~~

将`type: "categories"`添加进去，添加后如下：

~~~yaml
解答---
title: 标签
date: 20XX-XX-XX XX:XX:XX
type: "categories"
---
~~~

随后**不必再书写其他内容**保存并关闭文件即可

### 安装RSS插件

~~~bash
npm install hexo-generator-feed
~~~

安装好之后就可以使用了，RSS地址就是`你的域名/atom.xml`

## 部署到Github Pages

终于，正片

如果你已经设置好了Git，那么你可以直接部署到Github Pages了

~~~bash
hexo clean（删除缓存）
hexo g （生成）
hexo d （部署）
~~~

如果你遇到更改主题没有效果的问题通常清除缓存即可

当然你如果等不了那么久或者觉得太麻烦你可以直接一条命令搞定

~~~bash
hexo clean && hexo g && hexo d
~~~

# 疑难解答

该文段将会记录一些常见的问题以便给大家解决

## Node.js

### 为什么我的NPM安装插件的时候遇到了audit报错？

audit是安全报错，通常来说不会有什么安全问题，但是也不是不可以直接绕过安全检查

例如：

~~~bash
npm install --no-audit 软件包
~~~

