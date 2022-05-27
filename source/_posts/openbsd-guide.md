---
title: 从图形桌面到中文——OpenBSD调教教程
date: 2022-05-01 12:45:54
tags:
 - BSD
 - OpenBSD
categories: 教程
---

# 什么是OpenBSD

<div class="info">


>**OpenBSD**是一个[类Unix](https://zh.wikipedia.org/wiki/类Unix)计算机[操作系统](https://zh.wikipedia.org/wiki/操作系统)，是[加州大学伯克利分校](https://zh.wikipedia.org/wiki/加州大学伯克利分校)所开发的[Unix](https://zh.wikipedia.org/wiki/Unix)派生系统[伯克利软件套件](https://zh.wikipedia.org/wiki/BSD)（BSD）的一个后继者。它是在1995年尾由[荷裔加拿大籍](https://zh.wikipedia.org/wiki/荷裔加拿大人)项目领导者[西奥·德·若特](https://zh.wikipedia.org/wiki/西奧·德·若特)（Theo de Raadt）从[NetBSD](https://zh.wikipedia.org/wiki/NetBSD)[分支](https://zh.wikipedia.org/wiki/复刻_(软件工程))而出。

</div>

*来自维基百科[OpenBSD](https://zh.wikipedia.org/wiki/OpenBSD)条目*

# 开始安装

<div class="warning">

>由于OpenBSD的安装并不复杂，有完整的安装脚本，所以本文将不再详细赘述如何安装OpenBSD本体

</div>

<div class="warning">

>使用OpenBSD之前确保您有一定的Unix命令使用基础，否则您可能会非常的不幸

</div>

<div class="warning">

>OpenBSD和大多数Linux发行版一样是不能使用`root`账户登入桌面环境的，因此在安装过程中请务必另外新建一个普通账户

</div>

在安装过程中开启 X Window System

![](https://pic.lanta.cyou/img/2022-05-21_00-03.png)

第二项开机启动则为no，我们稍后安装好XFCE4后再开启

![](https://pic.lanta.cyou/img/2022-05-21_00-04.png)

如果安装过程中遇到了验证文件签名的问题可以直接跳过

![](https://pic.lanta.cyou/img/2022-05-21_00-05.png)

# 开始前的准备

首先安装`nano`作为我们的文本编辑器（当然你使用`vim`也是可以的）

```bash
pkg_add nano # or vim
```

# 设置软件源

我们可以将软件源设置到清华软件镜像源中以加快我们的软件下载速度

直接在终端运行以下命令

```bash
export PKG_PATH="https://mirrors.tuna.tsinghua.edu.cn/OpenBSD/$(uname -r)/packages/$(arch -s)/"
```

# 安装桌面环境

<div class="warning">

>执行该章节的安装步骤时请使用`root`账户

</div>

## 安装XFCE

安装完成之后就继续安装XFCE本体以及XFCE套件中的其他软件

```bash
pkg_add xfce xfce-extras consolekit2
```

<div class="info">

> 由于OpenBSD各种原因，这里的安装所花费的时间会非常漫长，所以请耐心等待

</div>

### 新建Xorg配置文件

使用`nano`进行编辑

```bash
nano .xsession
```

输入以下文段

```bash
exec ck-launch-session startxfce4
```

![](https://pic.lanta.cyou/img/2022-05-01_13-15.png)

然后按Ctrl+O保存，Ctrl+X推出`nano`

再在你的用户文件夹新建一份一模一样的

```bash
nano /home/<username>/.xsession
```

## 设置`xenodm`开机自启动

```bash
rcctl enable xenodm
```

<div class="success">

>至此，XFCE安装完成，重启后登录账号即可进入

</div>

## 注意事项

<div class="warning">

>执行下面的步骤时仍然需要使用`root`账户
>
>所以你需要在XFCE终端中输入`su`以切换到root账户

</div>

# 中文适配

## 安装字体

OpenBSD默认不自带中文字体和一些特殊字符的字体，所以我们需要安装字体以解决方块字的问题

我推荐你安装文泉驿和Noto Sans字体，基本上安装这两个字体就能很好的解决方块字的问题

```bash
pkg_add zh-wqy-zenhei-ttf # 安装文泉驿
```

```bash
pkg_add noto-fonts # 安装Noto Sans
```

## 安装Fcitx输入法

```bash
pkg_add fcitx fcitx-configtool fcitx-gtk3 fcitx-qt5 fcitx-pinyin fcitx-table
```

安装完之后我们需要设置一下环境变量，直接在终端中输入即可

```bash
export XMODIFIERS='@im=fcitx'
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
```

再在终端中输入

```bash
/usr/local/bin/fcitx -d
```

然后重新启动

<div class="success">

>至此，中文适配就完成了，重启后Fcitx就可以打出中文

</div>