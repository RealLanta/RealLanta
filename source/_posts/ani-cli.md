---
title: 在命令行里面看番？——Ani-cli体验
date: 2021-12-11 21:23:32
categories: 体验
tags: Linux
---

## 介绍

Ani-cli是一个在Linux下的在线看番软件，可以直接搜索番的名字（当然，是英文名）然后选择剧集观看

Github：https://github.com/pystardust/ani-cli

## 使用

如果你跟我一样使用的是Arch Linux，那么你可以直接在AUR上装

~~~bash
yay -S ani-cli-git
~~~

安装完之后直接在终端运行`ani-cli`

![](https://pic.lanta.cyou/img/2021-12-11_21-28.png)

![](https://pic.lanta.cyou/img/2021-12-11_21-30.png)

### 下载番

Ani-cli也支持下载番，在官方的README.MD已经写的很清楚了

~~~bash
ani-cli -d 番名
~~~

### 其他命令

#### 继续看动漫

~~~bash
ani-cli -H
~~~

#### 删除历史记录

~~~
ani-cli -D
~~~

#### 设置视频质量

~~~bash
ani-cli -q 360 #具体这个360是什么我也不清楚，应该是360p的意思
~~~

## 版权问题

那肯定会有人问了，这个东西难道没有版权问题吗

那么下载的时候是会出现它播放源的链接了，链接是这样的

>https://www04.anicdn.stream/videos/hls/auuvdc-TNZgfGKrC8ChZwQ/1639243843/123729/9ba3f79a88
>a92a2a95c2290a0233ff1d/ep.8.1604572806.1080.m3u8

那我们来看看他这个所谓的CDN的IP

>PING www05.anicdn.stream (185.38.15.115) 56(84) 字节的数据

很好，没套cf那些的，来看看他这个服务器的地区在哪

![](https://pic.lanta.cyou/img/2021-12-11_21-36.png)

好家伙，作者也不是个傻子，**荷兰抗DMCA服务器**

那就没有版权的问题了

（而且抗DMCA的服务器套了CF好像就不能抗DMCA了）

## 尾巴

服务器在荷兰的话觉得有点很正常

反正我自个是OpenWRT开梯子，完全不慌

总之是个好东西
