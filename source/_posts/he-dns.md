---
title: 来自某大型互联网线路供应商的DNS解析服务？——Hurricane Electric DNS管理服务体验
date: 2022-06-25 10:45:41
categories: 体验
tags:
 - DNS
 - 域名
---

# 起因

众所周知，我的域名用的是DNSPOD解析，不用CloudFlare的原因是因为经常出事，但是DNSPOD要实名，属实不太爽

然后有人说HE里面有DNS解析服务，HE嘛，我熟，[我嫖过它的香港iPv6隧道](https://www.lanta.cyou/he-ipv6)，~~至今还拿来当v6梯子~~

<div class="warning">

> 执行更换解析供应商之前请先备份你的解析记录

</div>

# 添加域名

首先前往 [**Hurricane Electric Free DNS Management**](https://dns.he.net/)

注册好账号并登录

然后点击左侧栏的 *Add a new domain*

![](https://pic.lanta.cyou/img/2022-06-25_10-49.png)

填入域名

![](https://pic.lanta.cyou/img/2022-06-25_10-50.png)

![](https://pic.lanta.cyou/img/2022-06-25_10-50_1.png)

添加完域名后点旁边的修改按钮就可以看到要求你添加或修改成他们的DNS服务器了

# 修改DNS服务器

<div class="info">

> Data列的地址（如`ns1.he.net`）是DNS服务器

</div>

![](https://pic.lanta.cyou/img/2022-06-25_10-52.png)

你的域名供应商给你填多少个DNS服务器就多少个，比如我是Dynadot只能填两个，那么就填`ns1.he.net`和`ns1.he.net`

![](https://pic.lanta.cyou/img/2022-06-25_10-53.png)

然后你可以回到HE的后台把多余的DNS服务器删除

<div class="warning">

> 注意
>
> 眼睛看好了不要删错

</div>

![](https://pic.lanta.cyou/img/2022-06-25_10-57.png)

![](https://pic.lanta.cyou/img/2022-06-25_10-59.png)

然后你可以等待几万年之后点一下 *Check Delegation* 按钮检查DNS服务器是否更改，没有就继续等几万年
