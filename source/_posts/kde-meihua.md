---
title: 试试让KDE更mac？——如何让你的KDE更加好看（仿mac）
date: 2021-12-24 22:27:04
tags: Linux
categories: 教程
---

## 关于美化

那么我自己之前就已经用了KDE Plasma+Latte的方案

但是我觉得其实也没好看到哪里去

所以我决定进行进一步的美化

## 安装Latte

没有Latte的就安装一下

~~~bash
yay -S latte-dock
~~~

安装完就出现了一个Dock栏了

![](https://pic.lanta.cyou/img/20211224222943.png)

打开高级编辑

![](https://pic.lanta.cyou/img/20211224223222.png)

接着你就可以调整图标的间距和大小或者Dock栏的大小了

![](https://pic.lanta.cyou/img/20211224223443.png)

## 让Latte代替KDE的任务栏

Latte同样拥有管理（切换）窗口的能力，所以你可以完全抛弃KDE原本的任务栏去使用Latte

### 添加KRunner

KRunner你可以理解为Windows上的“开始”菜单，而你可以把KRunner添加至Latte上

![](https://pic.lanta.cyou/img/20211224223752.png)

![](https://pic.lanta.cyou/img/20211224223855.png)

### 删除任务栏

![](https://pic.lanta.cyou/img/20211224230345.png)

一个一个删除就行了

## 使用Latte仿mac顶栏

Latte可以直接通过添加面板的方式来仿mac的顶栏

在Dock栏右键设置添加空面板

![](https://pic.lanta.cyou/img/20211224230643.png)

添加出来的空面板有亿点粗，你可以适当调整

![](https://pic.lanta.cyou/img/20211224231018.png)

### 添加组件

那么仿mac就要仿彻底一点，你可以在顶栏添加一些组件

- Application title（应用标题，KDE不自带，请前往KDE商店下载）

- 系统托盘 （KDE自带）
- 数字时钟 （KDE自带）
- 查找 （KDE自带）
- 通知 （KDE自带）

>如果你的通知出现了位置错误的情况你可以试着注销重登或直接重启

## 图标包&窗口美化

图标包我这里建议使用Papirus，足够的清爽好用

~~~bash
sudo pacman -S community/papirus-icon-theme
~~~

![](https://pic.lanta.cyou/img/20211224231629.png)

窗口美化你可以去下一个WhiteSur，我这里用的是WhiteSur的WhiteSur-Sharp样式（无底栏）

![](https://pic.lanta.cyou/img/20211224231813.png)

## 尾巴

至此，美化结束，祝你能在“崭新的”KDE Plasma环境下拥有着更舒服的视觉感受:D

![](https://pic.lanta.cyou/img/20211224231918.png)
