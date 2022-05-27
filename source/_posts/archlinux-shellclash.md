---
title: 在ArchLinux上快捷地使用Clash？——在ArchLinux上使用ShellClash教程
date: 2022-03-26 05:17:10
tags: 
 - Linux
 - ArchLinux
 - Clash
 - ShellClash
categories: 教程
---

# [ShellClash](https://github.com/juewuy/ShellClash)是什么？

是一种以脚本的方式运行的Clash快速使用脚本，大多数用在终端开启但是没有第三方固件的路由器上

ShellClash正好是可以运行在各种奇奇怪怪的Linux系统上的，所以我们可以在ArchLinux上使用

![](https://pic.lanta.cyou/20220326052051.png)

# 使用curl安装

既然他有curl的安装方式那么我们就用curl来安装

```bash
#ghproxy.com加速
export url='https://ghproxy.com/https://raw.githubusercontent.com/juewuy/ShellClash/master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null
#github-CDN源
export url='https://raw.githubusercontent.com/juewuy/ShellClash/master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null
#jsdelivrCDN源
export url='https://cdn.jsdelivr.net/gh/juewuy/ShellClash@master' && sh -c "$(curl -kfsSl $url/install.sh)" && source /etc/profile &> /dev/null
```

<div class="warning">

> 直接运行会提示我们要使用root用户运行，这里注意千万不能使用sudo！
>
> ![](https://pic.lanta.cyou/img/20220326052307.png)

</div>

接下来我们使用`su`或`sudo -i`的命令将终端用户切换至root

这样子就可以正常安装了

![](https://pic.lanta.cyou/img/20220326052457.png)

我们选择测试版

安装目录选择`/usr/share`

![](https://pic.lanta.cyou/img/20220326052527.png)

![](https://pic.lanta.cyou/img/20220326052558.png)

# 进一步的设置

在终端输入`clash`进行进一步的设置

下面选择2

![](https://pic.lanta.cyou/img/20220326052703.png)

使用iptables的方式代理

![](https://pic.lanta.cyou/img/20220326052558.png)

然后确认安装面板，选择Yacd面板

![](https://pic.lanta.cyou/img/20220326052856.png)

最后导入我们的Clash订阅链接运行即可

![](https://pic.lanta.cyou/img/20220326053029.png)

接着它就会下载核心，运行Clash了

![](https://pic.lanta.cyou/img/20220326053121.png)

因为Clash是掌控了整个系统的网络流量，这里建议大家还是开机自启为好

![](https://pic.lanta.cyou/img/20220326053121.png)

这样我们的ShellClash就安装完成了

# 速度测试

我自个的网络是垃圾联通小百兆，仅供参考

![](https://pic.lanta.cyou/img/20220326054032.png)

总的来讲节点不垃圾还是能跑满的