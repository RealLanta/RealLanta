---
title: 没办法绑定域名到Heroku或其他地方？使用Cloudflare Workers反代
date: 2021-12-11 11:49:49
tags: Cloudflare Workers
categories: 教程
---

## 介绍

Cloudflare Workers跟之前说的Vercel差不多，可以使用JSON反代

而Heroku绑定域名需要信用卡验证，所以我们就可以用这个办法解决绑定域名的问题

同样适用于其他平台的反代

~~其实这个方案是原本白嫖Heroku+CF搭V2梯子用的~~

因为可以直接在cf上编辑，所以用不着终端

~~~json
addEventListener(
  "fetch",event => {
     let url=new URL(event.request.url);
     url.hostname="网址";
     let request=new Request(url,event.request);
     event. respondWith(
       fetch(request)
     )
  }
)
~~~

网址**不要带**协议头（http或https），结尾也不要带**斜杠**

例如：

![](https://pic.lanta.cyou/img/2021-12-11_11-53.png)

## 绑定域名

把你的域名添加到Cloudflare

然后来到管理域名

![](https://pic.lanta.cyou/img/2021-12-11_11-56.png)

![](https://pic.lanta.cyou/img/2021-12-11_11-57.png)

然后就搞定了

## 尾巴

其实Wokers的加速效果不大，毕竟CF是什么成分懂的都懂

不过通常一些地区无法访问的问题只要套个CF就能解决

所以这也是一个办法吧（
