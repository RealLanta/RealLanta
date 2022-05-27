---
title: 论如何正确注册Yandex的免费域名邮箱？
date: 2022-02-25 16:51:04
tags: 
- 域名邮箱
- Yandex
categories: 教程
---

# 前言
Yandex，是一家毛子的公司，很多人都很熟悉
他们的大部分产品都是很简约好用的，包括他们的邮箱
今天我会教大家怎么去注册他们的Yandex免费域名邮箱
# Yandex 360
Yandex 360你可以理解为是抄巨硬的Office365和谷歌的Workspace的产物，也就是一整套的办公商务在线服务系统
首先你得事先准备一个Yandex的普通帐号，或者直接在360的套餐选择中注册也行
如果你看不懂俄文的话，别急，进了后台就有英文可以选了
![](https://pic.lanta.cyou/img/2022-01-21_20-07.png#vwid=299&vhei=141)
## 注册套餐
[直达套餐页面](https://360.yandex.ru/business/tariff?from=landing)
![](https://pic.lanta.cyou/img/2022-01-21_20-02.png#vwid=1904&vhei=902)
这里有些人看到套餐可能就怕了，别急
往下拉，拉到最底部，最下面会有一个**Try limited version**，这是免费的套餐
![](https://pic.lanta.cyou/img/2022-01-21_20-03.png#vwid=1016&vhei=582)
具体免费套餐的区别嘛。。。
![](https://pic.lanta.cyou/img/2022-01-21_20-04.png#vwid=918&vhei=658)
问题不大，我们只用到它的邮箱服务，直接点Try limited version完事
## 绑定域名
从左侧栏点**Domains**来添加域名
![](https://pic.lanta.cyou/img/2022-01-21_20-05.png#vwid=1278&vhei=683)
### 添加域名记录
接下来我们需要添加域名记录以完成绑定域名的操作，基本上添加完点Check即可
首先添加一个TXT记录来做域名验证
![](https://pic.lanta.cyou/img/2022-01-21_20-10.png#vwid=765&vhei=684)
![](https://pic.lanta.cyou/img/2022-01-21_20-10_1.png#vwid=1417&vhei=295)
验证完之后他会回去主页面，这里我们点**Configure manually**
![](https://pic.lanta.cyou/img/2022-01-21_20-25.png#vwid=776&vhei=232)
添加一个MX记录以指向Yandex的邮箱服务器
![](https://pic.lanta.cyou/img/2022-01-21_20-26.png#vwid=952&vhei=302)
![](https://pic.lanta.cyou/img/2022-01-21_20-27.png#vwid=1112&vhei=325)
接着点Check Again（Again是因为它刚刚尝试检查你有没有提前添加MX记录）
添加DKIM验证，依旧是TXT记录
![](https://pic.lanta.cyou/img/2022-01-21_20-29.png#vwid=774&vhei=257)
![](https://pic.lanta.cyou/img/2022-01-21_20-30.png#vwid=1085&vhei=498)
添加SPF记录，还是TXT记录
![](https://pic.lanta.cyou/img/2022-01-21_20-31.png#vwid=1426&vhei=565)
![](https://pic.lanta.cyou/img/2022-01-21_20-32.png#vwid=1092&vhei=357)
最后恭喜你，添加成功了
![](https://pic.lanta.cyou/img/2022-01-21_20-33.png#vwid=674&vhei=230)
## 添加用户
添加一个用户以使用这个域名
点击左侧栏的**Users**
![](https://pic.lanta.cyou/img/2022-01-21_20-34.png#vwid=1074&vhei=645)
![](https://pic.lanta.cyou/img/2022-01-21_20-36.png#vwid=500&vhei=870)
添加完之后就可以直接从[Yandex Mail](https://mail.yandex.com)登录了
登录的时候会询问你绑定手机号，绑定一个+86什么的就行