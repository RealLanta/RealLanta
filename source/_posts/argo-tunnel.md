---
title: 如何使用Cloudflare的Argo Tunnel实现内网穿透？
date: 2022-01-23 21:23:32
categories: 教程
tags: Cloudflare
---



# 前言

前阵子给自己的小米 5 上了 De­bian AR­M64，这次打算实际操作一下，并且使用 Argo Tun­nel 作为内网穿透的解决方案

当然，如果你不嫌弃 Argo Tun­nel 在国内的速度那就没什么问题了（他妈的给你开 80 和 443 了还要求这么多）



[![img](https://pic.lanta.cyou/img/2022-01-23_19-33.png#vwid=282&vhei=219)](https://pic.lanta.cyou/img/2022-01-23_19-33.png#vwid=282&vhei=219)



# 安装

这里用到是 Linux AR­M64 版的 [cloudflared](https://github.com/cloudflare/cloudflared/releases/tag/2022.1.2)，其他平台的使用方法基本相同



[![img](https://pic.lanta.cyou/img/2022-01-23_19-37.png#vwid=1420&vhei=485)](https://pic.lanta.cyou/img/2022-01-23_19-37.png#vwid=1420&vhei=485)



`wget` 下载完文件之后建议用 `mv` 重命名一下名字，这样就不用输很长一串文件名了

```bash
mv 原来的文件名 新文件名
```

下载完之后记得配置好文件权限，否则会出现 Per­mis­sion de­nied 的情况

```bash
chmod +x 文件名
```

## 登录

```bash
./cloudflared tunnel login
```

输入完之后会出现一个链接，在你电脑上打开它，他将用于登录你的账号并授权域名



[![img](https://pic.lanta.cyou/img/2022-01-23_19-41.png#vwid=1059&vhei=237)](https://pic.lanta.cyou/img/2022-01-23_19-41.png#vwid=1059&vhei=237)





[![img](https://pic.lanta.cyou/img/2022-01-23_19-43.png#vwid=1223&vhei=805)](https://pic.lanta.cyou/img/2022-01-23_19-43.png#vwid=1223&vhei=805)





[![img](https://pic.lanta.cyou/img/2022-01-23_19-44.png#vwid=1040&vhei=286)](https://pic.lanta.cyou/img/2022-01-23_19-44.png#vwid=1040&vhei=286)





[![img](https://pic.lanta.cyou/img/2022-01-23_19-44_1.png#vwid=915&vhei=252)](https://pic.lanta.cyou/img/2022-01-23_19-44_1.png#vwid=915&vhei=252)



你的 `/home或root/` 目录中会出现` ~/.cloudflared/cert.pem`。在我们创建隧道和设置 DNS 解析的时候，我们会用到这个文件。

## 创建隧道

```bash
./cloudflared tunnel create <隧道名称>
```

运行这条命令创建一个隧道，创建隧道会需要之前出现的 `cert.pem` 来**验证身份**



[![img](https://pic.lanta.cyou/img/2022-01-23_19-47.png#vwid=1433&vhei=140)](https://pic.lanta.cyou/img/2022-01-23_19-47.png#vwid=1433&vhei=140)



你的 `/home或root/` 目录中会出现 `~/.cloudflared/[一长串UUID].json`，里面保存着运行这条隧道所需要的**授权信息**

## 配置路由

创建隧道之后，我们还需要让它可以被访问（让 我 访 问），Cloud­flare 支持将其部署到负载均衡器后端或者通过 DNS 直接访问

这里介绍 DNS 直接访问的使用办法

```none
./cloudflared tunnel route dns [名字或者UUID] [想要绑定的域名或二级域名]
```

你会发现这个域名被设置了一个指向 [UUID].cfar­go­tun­nel.com 的 CNAME 记录，并且通过 Cloud­flare 进行代理

## 设置配置文件

在 `/home或root/.cloudflared` 目录中并在里面创建一个 `config.yml`，内容如下

```yml
tunnel: <你的隧道名称>
credentials-file: /root/.cloudflared/<隧道UUID>.json(这里看情况，可以自己修改json路径)

ingress:
  - hostname: <域名>
    service: https://127.0.0.1<此处自己视情况修改>
  - hostname: <域名>
    service: https://127.0.0.1<此处自己视情况需改>
  - service: http_status:404
```

# 运行

一切配置就绪之后就可以运行了（默认会使用 `/home或root/.cloudflared/config.yml` 作为配置文件）

```bash
./cloudflared tunnel run
```

因为他自己是不会在后台运行的，所以关掉 SSH 之后就停止了运行，这里可以用 screen 解决

```bash
apt install screen #安装screen的方式看你自己的系统
```

使用 screen 运行

```bash
screen ./cloudflared tunnel run
```

至于 screen 的用法不在本文的范畴，请自行百度或[参考这篇文章](https://www.myfreax.com/how-to-use-linux-screen/)

# 疑难解答

## 修复无法创建链接

如果你出现了下面提示

```none
ERR Unable to establish connection with Cloudflare edge error="TLS handshake with edge error: EOF" connIndex=0
```

参考[开启全局透明代理后，cloudflared（argo tunnel）无法使用 – V2rayA/V2rayA](https://issueexplorer.com/issue/v2rayA/v2rayA/188)

解决办法：不使用代理

## 修复SSL问题

如果你出现了下面提示

```none
ERR  error="Unable to reach the origin service. The service may be down or it may not be responding to traffic from cloudflared: x509: cannot validate certificate for 127.0.0.1 because it doesn't contain any IP SANs"
```

解决办法：

在 `config.yml` 中加入以下内容

```bash
originRequest:
 noTLSVerify: true
```

也就是关闭证书验证