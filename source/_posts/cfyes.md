---
title: 新域名无法添加CNAME自选节点的曲线救国的方法？——使用CFYes接入Cloudflare优化自选节点
date: 2021-12-18 15:43:21
tags: Cloudflare Workers
categories: 教程
---

## 关于CF不给CNAME接入

CF正式关闭了CNAME接入的API，据小道消息付费版Plesk的依然可以接入

## 正片

前往CFYes注册一个帐号https://cfyes.net/

~~拿WordPress做后台是怎么做到的~~

### 解析

名称这里就是他会分配给你的子域名，随便输一个就行，也不需要固定你自己的域名，套上就能用

![](https://pic.lanta.cyou/img/2021-12-18_13-31.png)

要解析两个域名（**记得关小云朵**）

| 类型  | 名称 | 内容             |
| ----- | ---- | ---------------- |
| CNAME | @    | 分配给你的子域名 |
| A     | ori  | 你的服务器IP     |

![](https://pic.lanta.cyou/img/2021-12-18_13-35.png)

### 设置Cloudflare Workers

新建一个Cloudflare Workers

输入这段进去

~~~json
addEventListener(
  "fetch",event => {
    event.respondWith(
      fetch(event.request, { cf: { resolveOverride: 'ori.你的域名' } })
    )
  }
)
~~~

![](https://pic.lanta.cyou/img/2021-12-18_13-40.png)

#### 示例

~~~json
addEventListener(
  "fetch",event => {
    event.respondWith(
      fetch(event.request, { cf: { resolveOverride: 'ori.mark-indigo.com' } })
    )
  }
)
~~~

### 配置网站主域名

切到你域名的Workers设置，选择添加路由

![](https://pic.lanta.cyou/img/2021-12-18_13-37.png)

然后把你的主域名绑定到你刚刚创建的Cloudflare Workers

![](https://pic.lanta.cyou/img/2021-12-18_13-37_1.png)

### 添加ori域名至服务器

![](https://pic.lanta.cyou/img/2021-12-18_16-01.png)

## 尾巴

至此，CFYes的对接就完成了

实际上工作原理应该跟反代差不多

首先ori域名解析到你的服务器

然后让Cloudflare Workers去反代ori域名

CFYes让你的主域名使用自选节点来反代

从而达到加速的效果
