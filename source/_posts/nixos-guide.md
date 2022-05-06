---
title: 比Arch更好？一个配置文件就能在各个电脑装一模一样的系统和软件？——保姆级教你轻松掌握NixOS
date: 2022-05-06 09:15:59
tags: 
 - NixOS
 - Linux
categories: 体验
---

<div class="info">

>非常感谢[一穂灯花](https://github.com/YisuiDenghua)在我安装NixOS中提供的帮助，以完成我对NixOS的学习和本文章的创作

</div>

# 认识NixOS

NixOS是建立在Nix之上的Linux发行版，Nix就是它的包管理器

## 软件包的安装方式

NixOS非常不同，如果你想安装软件，有多种安装方式，这里举三个例子

1.编辑NixOS的配置文件`configuration.nix`

<div class="info">

>安装在`configuration.nix`的软件就是构建在系统里面了，而且因为在配置文件里面，下次你要安装NixOS到其他电脑，只要用这个配置文件（可能需要稍微修改一下）就可以轻松安装NixOS顺便安装你需要的软件了
>
>比如说我需要装git，把 git 写进`configuration.nix`然后 `nixos-rebuild boot`重启就可以安装完成
>
>缺点是，需要重启（因为重构了部分系统文件），不过重构的时间并不多，基本上几分钟就可以了

</div>

<div class="warning">

>需要注意的是，有一些软件的开源协议是**不自由**的，所以NixOS会阻止你的安装操作
>
>要解决这个问题也很简单，只要在`configuration.nix`中允许即可，在里面加入`nixpkgs.config.allowUnfree = true;`
>
>例如：
>
>![](https://pic.lanta.cyou/img/2022-05-06_09-45.png)

</div>

2.`nixos-shell`临时安装

<div class="info">

>`nix-shell`的安装是临时使用，比如我要临时用一下git，那就`nix-shell -p git`
>
>重启后就会消失

</div>

3.`nix-env -iA nixos.{包名}`

<div class="info">

> 这种不需要 sudo，也不需要重构，它是把软件安装在用户的目录下
>
> 缺点是，这种方式安装的软件不能被 configuration.nix 记录，也不能被其他用户使用
>
> ——[一穂灯花](https://github.com/YisuiDenghua)

</div>

## One config use anywhere

NixOS跟其他Linux发行版最大的不同就是几乎所有的系统设置都在`configuration.nix`

比如说添加用户、安装的软件、字体、网络、系统引导、桌面环境，全部包含在了这么一个文件里面

也就是说，只要你在`configuration.nix`配置好之后，下次你需要安装到其他电脑只要再用这个配置文件然后`nixos-install`就可以得到一模一样的NixOS

同理，如果你去Github下载别人的`configuration.nix`，你可以得到跟他一模一样的NixOS

如果你NixOS被你玩死了，只要你的配置文件还在，可以进Live CD，那么只要`nixos-rebuild`就可以回到之前的样子

>  我的[`configuration.nix`](https://github.com/RealLanta/nixos-config)

# 准备操作

首先你需要确保你的网络畅通，NixOS是**在线安装**

## 下载LiveCD

你可以前往[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/nixos-images/)中下载NixOS的Live CD

![](https://pic.lanta.cyou/img/2022-05-06_09-47.png)

<div class="warning">

>请尽量下载较新的版本

</div>

里面有很多个镜像，这里做一下解释

![](https://pic.lanta.cyou/img/2022-05-06_09-50.png)

## 刻录到U盘

你可以使用以下几种方式来将NixOS镜像刻录到U盘中

<div class="danger">


> 刻录到U盘会导致数据丢失，请在操作之前备份好你的U盘数据

</div>

<div class="info">


>推荐使用Ventoy，因为Ventoy只需要一次安装到U盘之后就不再需要刻录的操作，还可以跟资料共存

</div>


| 名称                                          | 使用方式                                        | 备注               |
| --------------------------------------------- | ----------------------------------------------- | ------------------ |
| [Ventoy](https://ventoy.net/)                 | 将Ventoy安装到U盘后直接把ISO文件复制到U盘中即可 | 一次安装，一盘多用 |
| [Rufus](https://rufus.ie/)                    | 选择ISO文件然后选择U盘直接刻录即可              | 注意选择引导方式   |
| [balenaEtcher](https://www.balena.io/etcher/) | 选择ISO文件然后选择U盘点击Flash开始刻录         | 最简单的方法       |

## 确定分区（必要）

在纯文字的环境中你可能**很难分辨**分区，如果**操作失误**（删错分区）~~则删库跑路~~

所以我建议你**事先**在Windows环境中**确定**要安装的系统分区，以确保不会出现失误导致的~~数据灰飞烟灭~~

## 引导设置（UEFI用户）

进入BIOS中，如果你是有UEFI的电脑那么你应该能看到带有**CSM**字样的选项，将他**Disable**掉

还有一些电脑带有**Secure Boot（安全启动）**，找到**Secure Boot**的字样**Disable**即可

## 重启开始安装

确保您的U盘已经写入了镜像、分区已经确定好没问题之后就可以重启进入LiveCD

# LiveCD

## 进入NixOS的root

进入终端后你会发现NixOS默认登录的不是root账户，首先你需要登录到root账户才能继续

但是默认root账户是没有密码的，所以我们需要设置一个

![](https://pic.lanta.cyou/img/2022-05-06_09-59.png)

## 确保网络畅通

前面提到，NixOS是在线安装方式，所以你必须要确保LiveCD中的系统已经连接到了网络

### 有线网络

一般来说如果你使用了带桌面环境的ISO就已经可以自动联网

不过发现网络出现玄学问题，你可以执行`systemctl restart dhcpcd`来重启DHCP服务让Live CD重新获取IP

### WiFi连接

LiveCD中自带NetworkManager，你可以一步一步让Live CD连接到WiFi

```bash
[root@nixos ~]$ nmcli device wifi connect <WIFI名> password <密码>
```

（以上文段来自[Chi_Tang的博客](https://chitang233.github.io/posts/arch-guide/#WiFi-%E8%BF%9E%E6%8E%A5)）

## 分区

先通过`lsblk`或`fdisk -l`的方式确定硬盘，出现`/dev/某某某`的就是这个硬盘的设备名，记住硬盘的设备名，等会要用到

### 分区的工具

LiveCD中有`parted`、`fdisk`、`cfdisk`等工具，前两个比较麻烦，`cfdisk`拥有半图形界面，更适合小白使用

首先我们要打开cfdisk

<div class="info">


> 设sdX为你的硬盘号

</div>

```bash
cfdisk /dev/sdX # 后面这个就是硬盘设备名
```

然后就会出现一个界面，这里以一个中文化的界面作为对照

![](https://pic.lanta.cyou/img/20220326033131.png)

### 分区格式

<div class="info">


>  用户资料分区是可选的，可以将用户资料存放在系统盘中

</div>

#### MBR（传统引导）推荐分区格式：

| 用途                 | 文件系统 | 分区推荐大小 | 挂载点 | 文章中的分区号                   |
| -------------------- | -------- | ------------ | ------ | -------------------------------- |
| 系统分区             | ext4     | 50GiB        | /      | /dev/sda2                        |
| 用户资料分区（可选） | ext4     | 50GiB        | /home  | （本文章中没有设置用户资料分区） |

#### GPT（UEFI引导）推荐分区格式：

<div class="danger">


> 如果你之前已经在硬盘中另外装了其他使用UEFI引导的系统（比如Windows）请务必不要把引导分区删掉重建，否则会导致原来的系统无法进入！

</div>

<div class="danger">


> 因为某些机制的原因，请记得在`cfdisk`中把引导分区的“类型”(Type)改为"EFI System"

</div>

| 用途                 | 文件系统 | 分区推荐大小 | 挂载点 | 文章中的分区号                   |
| -------------------- | -------- | ------------ | ------ | -------------------------------- |
| 引导分区             | FAT32    | 300MiB       | /boot  | /dev/sda1                        |
| 系统分区             | ext4     | 50GiB        | /      | /dev/sda2                        |
| 用户资料分区（可选） | ext4     | 50GiB        | /home  | （本文章中没有设置用户资料分区） |

## 格式化分区

<div class="danger">


> 如果你之前已经在硬盘中另外装了其他使用UEFI引导的系统（比如Windows）请务必不要把引导分区格式化，否则会导致原来的系统无法进入！

</div>

<div class="info">


> 设引导分区为 /dev/sda1，系统分区为 /dev/sda2

</div>

```bash
mkfs.ext4 /dev/sda2 # 格式化 /dev/sda2 为 ext4 文件系统（系统分区或用户资料分区）
mkfs.vfat /dev/sda1 # 格式化 /dev/sda1 为 FAT32 文件系统（引导分区）
```

## 挂载分区

### GPT

<div class="info">


> 设引导分区为 /dev/sda1，系统分区为 /dev/sda2，用户资料分区为/dev/sda3

</div>

```bash
mount /dev/sda2 /mnt # 挂载系统分区到 /mnt( / 分区)
mkdir /mnt/boot /mnt/home # 新建 /mnt/boot 文件夹和 /mnt/home 文件夹
mount /dev/sda1 /mnt/boot # 挂载引导分区到 /mnt/boot
mount /dev/sda3 /mnt/home # 挂载用户资料分区到 /mnt/home （ /home 分区，在有用户资料分区的情况下才执行）
```

### MBR

```bash
mount /dev/sda1 /mnt  # 挂载系统分区到 /mnt( / 分区)
mkdir /mnt/home # 创建 /mnt/home 文件夹
mount /dev/sda2 /mnt/home # 挂载用户资料分区到 /mnt/home( /home 分区，在有用户资料分区的情况下才执行)
```

（以上文段参考了[Chi_Tang的博客](https://chitang.tech/posts/arch-guide/#%E6%8C%82%E8%BD%BD)并进行修改）

## 安装本体到硬盘

### 生成配置文件

```bash
nixos-config /mnt
```

该命令会生成配置文件到`/etc/nixos/`目录中，里面包括了硬件配置文件(`hardware-configuration.nix`)和`configuration.nix`

如果你有自己配置好的`configuration.nix`只要替换掉即可

### 更改配置文件

使用vim开始编辑

```bash
vim /mnt/etc/nixos/configuration.nix
```

#### 允许安装不自由的软件

![](https://pic.lanta.cyou/img/2022-05-06_10-15.png)

#### 安装NotoFonts字体以显示中文和其他特殊字符

![](https://pic.lanta.cyou/img/2022-05-06_10-16.png)

#### 设置时区

![](https://pic.lanta.cyou/img/2022-05-06_10-16_1.png)

#### 使用NetworkManager联网

![](https://pic.lanta.cyou/img/屏幕截图 2022-05-06 104115.png)

#### 设置中文

![](https://pic.lanta.cyou/img/2022-05-06_10-19.png)

#### 设置桌面环境

如果你使用的是Plasma，那么生成配置文件的时候会自动帮你设置好Plasma，Gnome用户同理

![](https://pic.lanta.cyou/img/2022-05-06_10-20.png)

#### 设置普通用户

![](https://pic.lanta.cyou/img/2022-05-06_10-22.png)

#### 设置安装的软件

![](https://pic.lanta.cyou/img/2022-05-06_10-25.png)

#### 开启SSH（可选）

![](https://pic.lanta.cyou/img/2022-05-06_10-26.png)

### 安装命令

```bash
nixos-install --root /mnt
```

## 重启

<div class="success">


> 到这里，我们的LiveCD的安装步骤就完成了，曙光就在眼前了！接下来我们就进入NixOS了！

</div>
