---
title: 如何在2022年给你的Mips架构的OpenWrt轻松安装Passwall？（含一键安装脚本）
date: 2022-02-12 17:29:00
tags: OpenWrt
categories: 教程
---

# 前言

不同的OpenWrt的路由器架构是不同的，如果你的SOC是MT76XX型的基本上可以确认是MIPS架构

本教程只适用于MIPS架构的OpenWrt路由器

测试环境为小米CR6608，OpenWrt 7.10内核

## 为什么我会换Passwall

OpenClash有一定的速度损耗，而这会影响到我自己的爬墙体验

所以我决定开始尝试安装Passwall

# 安装

接下来开始我们的安装部分

## 一键安装脚本（Alpha）

如果你真的是~~懒得爆炸~~，那么你可以尝试一下我写的一键安装脚本

Github:https://github.com/RealLanta/mips-pw-install

安装Passwall本体：

```bash
wget --no-check-certificate https://cdn.lanta.cyou/passwall/mips_pw_install.sh && chmod +x mips_pw_install.sh && ./mips_pw_install.sh
```

安装v2ray插件：

```bash
wget --no-check-certificate https://cdn.lanta.cyou/passwall/mips_pw_v2ray.sh && chmod +x mips_pw_v2ray.sh && ./mips_pw_v2ray.sh
```

安装Xray插件：

```bash
wget --no-check-certificate https://cdn.lanta.cyou/passwall/mips_pw_xray.sh && chmod +x mips_pw_xray.sh && ./mips_pw_xray.sh
```

安装Trojan插件：

~~~bash
wget --no-check-certificate https://cdn.lanta.cyou/passwall/mips_pw_trojan.sh && chmod +x mips_pw_trojan.sh && ./mips_pw_trojan.sh
~~~



## 手动安装

如果你乐于折腾，可以尝试手动安装

### 需要的依赖

你需要以下的依赖才可以安装Passwall

- libuv1（ipt2socks需要，这个大部分的固件都有装，直接`opkg install libuv1`即可，如果找不到就更新源）

- [ChinaDNS-Ng](https://cdn.lanta.cyou/passwall/chinadns-ng_1.0-beta.25-20_mipsel_24kc.ipk)
- [ipt2socks](https://cdn.lanta.cyou/passwall/ipt2socks_1.1.3-12_mipsel_24kc.ipk)
- [shadowsocksr-libev-ssr-local](https://cdn.lanta.cyou/passwall/shadowsocksr-libev-ssr-local_2.5.6-34_mipsel_24kc.ipk)
- [shadowsocksr-libev-ssr-redir](https://cdn.lanta.cyou/passwall/shadowsocksr-libev-ssr-redir_2.5.6-34_mipsel_24kc.ipk)
- [xray-core](https://cdn.lanta.cyou/passwall/xray-core_1.5.3-37_mipsel_24kc.ipk)（要Xray还是v2ray按需求选择）
- [v2ray-core](https://cdn.lanta.cyou/passwall/v2ray-core_4.43.0-30_mipsel_24kc.ipk)（要Xray还是v2ray按需求选择）
- [xray-plugin](https://cdn.lanta.cyou/passwall/xray-plugin_1.5.3-32_mipsel_24kc.ipk)（要Xray还是v2ray按需求选择）
- [v2ray-plugin](https://cdn.lanta.cyou/passwall/v2ray-plugin_5.0.2-56_mipsel_24kc.ipk)（要Xray还是v2ray按需求选择）
- [simple-obfs](https://cdn.lanta.cyou/passwall/simple-obfs_0.0.5-12_mipsel_24kc.ipk)
- [trojan-plus](https://cdn.lanta.cyou/passwall/trojan-plus_10.0.3-8_mipsel_24kc.ipk)

以上的软件大部分都不在官方源里面，需要自己找

当然我也有提供下载

### 安装Passwall

[下载](https://cdn.lanta.cyou/passwall/luci-app-passwall_git-22.033.25842-d057a7b_all.ipk)Passwall的ipk文件，然后`opkg install ipk文件名`即可

# 疑难解答

这里会解答一些比较常见的问题

## Kernel太旧？

如果你遇到了类似的错误

` * pkg_hash_check_unresolved: cannot find dependency kernel (= 5.10.96-1-71072b1970d75a18af84e8fe3e8c81e4) for kmod-ip6tables`

就是Kernel太旧了

请到[官方源](https://downloads.openwrt.org/snapshots/targets/ramips/mt7621/packages/)下载一个新的Kernel，然后执行`opkg install 你下载下来的Kernel文件名`