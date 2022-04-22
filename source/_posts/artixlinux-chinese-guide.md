---
title: 从入门到偷ArchLinuxCN源最后逃回ArchLinux——最详细的Artix Linux中文保姆级安装教程
date: 2022-04-09 08:47:57
tags: 
 - Linux
 - 保姆级教程
 - Artix Linux
categories: 教程
---

<div class="info">

>Thanks a lot for [Shikikan_Neko08](https://twitter.com/Shikikan_Neko08)
>
>He helped me solve a lot of problems during the install Artix Linux process
>
>非常感谢[Shikikan_Neko08](https://twitter.com/Shikikan_Neko08)，他帮我解决了安装 Artix Linux 过程中的很多问题

</div>

<div class="warning">

> Warning
>
> 本文仅为 [Artix Linux](https://artixlinux.org) 的第三方安装教程，不代表 [Artix Linux](https://artixlinux.org) 官方，因此跟[Artix Linux Wiki](https://wiki.artixlinux.org)中的[Installation](https://wiki.artixlinux.org/Main/Installation)有所差异，但这也意味着本教程的内容将会**更为全面**

</div>

<div class="info">

>[Artix Linux](https://artixlinux.org)的特点是[systemd-free](https://nosystemd.org/)(无systemd)，是指没有systemd的Linux
>
>systemd相当于Windows中的注册表，没有它就不能正常运行，但是Linux社区因为某种原因讨厌systemd，出现了无systemd的内核（例如[OpenRC](https://wiki.gentoo.org/wiki/Project:OpenRC)、[runit](http://smarden.org/runit/)、[s6](https://skarnet.org/software/s6/)）
>
>一般情况下，如果你只是普通的日常使用Linux，使用systemd作为内核的Linux就已经足够了
>
>而且因为种种原因，**我不推荐所有Linux新手使用没有systemd的Linux**
>
>一般情况下，我推荐你使用runit，所以本文将会以runit的Artix Linux作为演示

</div>

<div class="info">

>[Artix Linux](https://artixlinux.org)和[Arch Linux](https://archlinux.org) 一样的特点是[Rolling Update](https://zh.wikipedia.org/zh-tw/%E6%BB%BE%E5%8B%95%E7%99%BC%E8%A1%8C)(滚动更新)，是指软件开发中**经常性**将更新发送到软件的概念
>
>也就是说，[Artix Linux](https://artixlinux.org) 的软件库是经常性更新的，你也需要**经常更新软件源**，操作方法很简单，只要`sudo pacman -Syu`即可；如果不经常更新，则可能导致滚动更新后**无法正常使用系统**
>
>如果你并不想~~或者说懒~~经常更新软件源，我建议你使用`linux-lts`内核

</div>

# 准备操作

首先你需要确保你的网络畅通（放心，这不需要你准备梯子，Artix Linux是有南京大学镜像源的），Artix Linux是**在线安装**

# 下载LiveCD

前往[NJU Mirror（南京大学镜像站）](https://mirror.nju.edu.cn/artixlinux-iso/)下载

![](https://pic.lanta.cyou/img/20220409090124.png)

下载完成后备用

## 刻录到U盘

你可以使用以下几种方式来将Artix Linux镜像刻录到U盘中

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

确保您的U盘已经写入了镜像、分区已经确定好没问题之后，就可以重启进入LiveCD

# LiveCD

## 登录

Artix Linux的LiveCD并不会自动登录，LiveCD的用户名为`artix`密码为`artix`

![](https://pic.lanta.cyou/img/20220409090535.png)

登录之后输入`su`进入root账号

![](https://pic.lanta.cyou/img/2022-04-09_09-06.png)

## 确保网络畅通

前面提到，Artix Linux是在线安装方式，所以你必须要确保LiveCD中的系统已经连接到了网络

### 有线网络

如果你是有线网络，大概率不需要担心网络问题，因为LiveCD已经通过DHCP服务自动联网

不过发现网络出现玄学问题，你可以执行`sv restart dhcpcd`来重启DHCP服务让Live CD重新获取IP

### WiFi连接

LiveCD中自带连接WiFi的工具，你可以一步一步让Live CD连接到WiFi

```bash
artixlinux:[artix]:$ iwctl  # 进入 iwd 的交互提示符
[iwd]# device list  # 列出所有 WiFi 设备
[iwd]# station <device> scan  # 扫描 WiFi 网络
[iwd]# station <device> connect <ssid>  # 连接到 WiFi 网络
```

（以上文段来自[Chi_Tang的博客](https://chitang233.github.io/posts/arch-guide/#WiFi-%E8%BF%9E%E6%8E%A5)）

### 测试网络

最简单的办法就是用Ping

```bash
ping -c 3 artixlinux.org
```

没有报错则网络连接正常

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

![](https://pic.lanta.cyou/img/2022-04-09_09-10.png)

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

![](https://pic.lanta.cyou/img/2022-04-09_09-11.png)

### MBR

```bash
mount /dev/sda1 /mnt  # 挂载系统分区到 /mnt( / 分区)
mkdir /mnt/home # 创建 /mnt/home 文件夹
mount /dev/sda2 /mnt/home # 挂载用户资料分区到 /mnt/home( /home 分区，在有用户资料分区的情况下才执行)
```

（以上文段参考了[Chi_Tang的博客](https://chitang.tech/posts/arch-guide/#%E6%8C%82%E8%BD%BD)并进行修改）

## 更换软件源

接下来我们需要更新一下镜像源以加快我们的下载速度

```bash
nano /etc/pacman.d/mirrorlist # 新建软件源列表
```

在软件源列表中第一行写入`Server = 地址`即可，这里给大家列几个镜像源

| 名称                                                         | 地址                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn) | https://mirrors.tuna.tsinghua.edu.cn/artixlinux/$repo/os/$arch |
| [腾讯软件源](https://mirrors.cloud.tencent.com/)             | https://mirrors.cloud.tencent.com/artixlinux/$repo/os/$arch  |
| [阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/)   | https://mirrors.aliyun.com/artixlinux/$repo/os/$arch         |

在上面选择一个镜像源，假如我要用清华的，那就`Server = https://mirrors.tuna.tsinghua.edu.cn/artixlinux/$repo/os/$arch`

当然如果速度还是上不去可以尝试删除其他镜像源

保存并退出即可

## 安装本体到硬盘

### 内核的选择

下面为Artix Linux官方源中可选的部分内核，可以根据你自己的需求选择

| 内核        | 特点                                           |
| ----------- | ---------------------------------------------- |
| `linux`     | 官方的默认内核                                 |
| `linux-lts` | 官方的长期支持内核，版本较低，但相对不容易滚挂 |
| `linux-zen` | 社区制作的更适合日常使用的内核                 |

（以上表格来自[Chi_Tang的博客](https://chitang.tech/posts/arch-guide/#%E5%86%85%E6%A0%B8%E7%9A%84%E9%80%89%E6%8B%A9)）

## 安装命令

首先安装runit

```bash
basestrap /mnt base base-devel runit elogind-runit
```

![](https://pic.lanta.cyou/img/2022-04-09_09-12.png)

![](https://pic.lanta.cyou/img/2022-04-09_09-13.png)

然后安装内核

```
basestrap /mnt <linux-kernel> linux-firmware
```

![](https://pic.lanta.cyou/img/2022-04-09_09-24.png)

## 生成分区表

```bash
fstabgen -U /mnt >> /mnt/etc/fstab
```

可以自己检查一下 `/mnt/etc/fstab` 以确保信息正确无误

## 切换到安装好的系统

接下来我们要通过**chroot**切换到我们安装好的系统的终端进行初步的系统设置

```bash
artix-chroot /mnt
```

<div class="info">


> 要退出返回LiveCD就直接输入`exit`即可

</div>

切换之后安装一下vim作为文本编辑器

```bash
pacman -S vim
```



## 设置时区

```bash
ln -sf /usr/share/zoneinfo/<Region>/<City> /etc/localtime
hwclock --systohc
```

`<Region>`和`<City>`分别是你所在的地区和城市，例如上海时间`Asia/Shanghai`

## 配置本地化

### locale.gen

```bash
vim /etc/locale.gen
```

在文件中找到 `en_US.UTF-8 UTF-8` 及 `zh_CN.UTF-8 UTF-8`，删除前面的 `#` 以取消注释

同样地，在这里也可以设置其他语言的的 `locale`，例如 `ja_JP.UTF-8 UTF-8`

```bash
locale-gen  # 生成本地化文件
```

### locale.conf

```bash
vim /etc/locale.conf
```

在文件中加入以下设置

 ```
 export LANG="en_US.UTF-8"
 export LC_COLLATE="C"
 ```

保存并退出

<div class="warning">


> 请不要在这里设置任何其他的语言，否则可能会导致终端乱码

</div>

## 安装引导

### 安装GRUB本体

```bash
pacman -S grub os-prober efibootmgr
```

如果你需要GRUB能引导到其他系统（例如Windows）我们要先开启GRUB使用**os-prober**才可以让GRUB引导到其他系统

```bash
vim /etc/default/grub
```

在结尾或者其他位置中添加`GRUB_DISABLE_OS_PROBER="false"`保存并退出即可

### UEFI + GPT

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub # 在 /boot 中安装GRUB引导
grub-mkconfig -o /boot/grub/grub.cfg  # 生成 GRUB 配置
```

![](https://pic.lanta.cyou/img/2022-04-09_09-48.png)

### Legacy + MBR

<div class="info">

> 设sdX为你的硬盘号

</div>

```bash
grub-install --target=i386-pc /dev/sdX  # 在 /dev/sdX 安装引导，不要加分区号
grub-mkconfig -o /boot/grub/grub.cfg  # 生成 GRUB 配置
```

## 设置网络

### 计算机名

```bash
vim /etc/hostname
```

在文件中写入你的计算机名，例如 `my-pc-1s-artixlinux`

保存并退出

### hosts 文件

```bash
vim /etc/hosts
```

在文件中写入下面的内容

```none
127.0.0.1 localhost
::1       localhost
127.0.1.1 <hostname>.localdomain  <hostname>
```

将 `<hostname>` 替换为你的计算机名，保存并退出

### 安装Connman

如果进入系统后突然发现系统联网不正常，一般来说装个Connman就能解决

<div class="info">

> 因为种种原因，NetworkManager可能无法在Artix Linux正常使用，所以我不推荐你使用NetworkManager

</div>

```bash
pacman -S connman-runit connman-gtk # 安装Connman
```

将Connman开机自启动

```
ln -s /etc/runit/sv/connmand /etc/runit/runsvdir/default
```

![](https://pic.lanta.cyou/img/2022-04-09_11-14.png)

![](https://pic.lanta.cyou/img/2022-04-09_11-22.png)

![](https://pic.lanta.cyou/img/2022-04-09_11-22_1.png)

(Thanks for [Shikikan_Neko08](https://twitter.com/Shikikan_Neko08))

## 重启前的准备

```bash
passwd root # 设置root的密码
```

## 重启

<div class="success">


> 到这里，我们的LiveCD的安装步骤就完成了，曙光就在眼前了！接下来我们进入系统进行初步设定

</div>

# 系统初步配置

## 创建新用户

很多软件是不能直接使用root用户的，而且直接使用root用户是很危险的，~~比如运行一个`rm -rf /*`都不带`sudo`的~~

```bash
useradd -m -G wheel 用户名
passwd 用户名
```

![](https://pic.lanta.cyou/img/2022-04-09_10-46.png)

### 将新建的用户设置权限

在终端中运行

```bash
EDITOR=vim visudo
```

在这个配置文件中找到`%wheel ALL=(ALL:ALL) ALL`，如果前面带个`#`号就给它删掉，保存并退出

![](https://pic.lanta.cyou/img/2022-04-09_10-46_1.png)

## 设置SWAP（交换文件）

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=<size>  # 在 size 处填写需要的 swap 空间大小(单位 MiB)
sudo mkswap /swapfile
sudo chmod 600 /swapfile
sudo swapon /swapfile
```

![](https://pic.lanta.cyou/img/2022-04-09_10-47.png)

使用vim编辑`/etc/fstab`，在末尾中加入

```bash
/swapfile none swap defaults 0 0 # 注意空格
```

![](https://pic.lanta.cyou/img/2022-04-09_10-48.png)

## 设置pacman.conf

```bash
sudo vim /etc/pacman.conf
```

### pacman美化


<div class="info">

> 这一步是可选的，只是为了让你的pacman更好看

</div>

把`Color`前面的`#`去掉，让pacman能显示出颜色（能帮助你更好地判断内容）

把 `VerbosePkgLists` 前面的`#`去掉，让 pacman 以表格显示更详细的信息

##  额外软件源

#### lib32

`lib32` 软件源中包含一些 32 位的依赖包

滑到文件后面，找到

```none
#[lib32]
#Include = /etc/pacman.d/mirrorlist
```

去掉这两行前面的 `#` 以启用 `lib32` 软件源

![](https://pic.lanta.cyou/img/2022-04-09_10-50.png)

### Arch Linux Source

为了能使用Arch Linux的软件源（如`multilib`）以适配部分Arch Linux软件，你需要安装`artix-archlinux-support`

```bash
sudo pacman -S artix-archlinux-support
```

然后让它信任Arch Linux的密钥

~~~bash
sudo pacman-key --populate archlinux
~~~

这样就可以使用了

### Arch Linux CN

Artix Linux是可以偷`Arch Linux CN` 源的，`Arch Linux CN`中包含许多在国内使用 Linux 常用的软件包，而且软件包下载速度比较快

在文件最后面添加下面这两行

```none
[archlinuxcn]
Server = https://mirrors.bfsu.edu.cn/archlinuxcn/$arch
```

![](https://pic.lanta.cyou/img/2022-04-09_10-50_1.png)

保存并退出后更新软件源

```bash
sudo pacman -Sy
```

![](https://pic.lanta.cyou/img/2022-04-09_11-06.png)

安装 `archlinuxcn-keyring`

```bash
sudo pacman -S archlinuxcn-keyring
```

![](https://pic.lanta.cyou/img/2022-04-09_11-16.png)

如果安装`archlinuxcn-keyring`出现了以下错误

![](https://pic.lanta.cyou/img/2022-04-09_11-16_1.png)

那么往下看本文章末尾的疑难解答

## AUR 

AUR源包含着很多有用的个人开发者软件，所以安装AUR工具可以让我们从AUR源中获取很多个人开发者的优秀软件

```bash
sudo pacman -S yay
```

## 安装中文

### 安装Fcitx5输入法

```bash
sudo pacman -S fcitx5-im # Fcitx5本体组
sudo pacman -S fcitx5-chinese-addons # 中文输入引擎
yay -S fcitx5-pinyin-zhwiki-rime # 中文维基百科词库
yay -S fcitx5-pinyin-moegirl # 萌娘百科词库（二刺螈词库）
```

<div class="warning">

> 因为萌娘百科词库本体在Github，所以你可能需要开梯子才能访问

</div>

如果出现了这个

![](https://pic.lanta.cyou/img/2022-04-09_11-31.png)

那么可以尝试安装`fcitx5-git`

![](https://pic.lanta.cyou/img/2022-04-09_11-32.png)

安装好之后就可以继续了

![](https://pic.lanta.cyou/img/2022-04-09_11-35.png)


### 设置环境变量

```bash
vim ~/.xprofile
```

添加下面这几行:

```bash
export LANG=zh_CN.UTF-8
export LC_ALL=zh_CN.UTF-8
export GTK_IM_MODULE=fcitx # 带有fcitx字样的是为了能让Fcitx5输入法能正常使用
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export SDL_IM_MODULE=fcitx
```

<div class="info">


> Fcitx5输入法需要进入图形界面的“设置”中才能进一步地设置Fcitx5

</div>

### 安装字体

安装下面几个字体以让中文能正常显示

```bash
sudo pacman -S noto-fonts noto-fonts-emoji noto-fonts-extra #Noto Sans
yay -S wqy-microhei-nightly_build # 文泉驿
```

![](https://pic.lanta.cyou/img/2022-04-09_11-39.png)

![](https://pic.lanta.cyou/img/2022-04-09_14-23.png)

# 图形界面

## 显卡驱动

<div class="info">


> 部分显卡驱动需要启用mutilib源（问题不大，我们前面已经启用了）
> 还有一些带有<sup>AUR</sup>的就是来自AUR的软件包，前面我们已经安装了yay所以问题不大

</div>

<div class="info">

> 以下资料整理来自[Chi_Tang的博客](https://chitang.tech/posts/arch-guide/)

</div>

#### Intel 显卡

```bash
sudo pacman -S xf86-video-intel # 驱动本体
sudo pacman -S mesa # OpenGL 支持
sudo pacman -S lib32-mesa # 32 位 OpenGL 支持
sudo pacman -S vulkan-intel # Vulkan 支持
paru -S intel-opencl  # OpenCL 支持
```

#### NVIDIA 显卡

##### 专有驱动

本体:

|           | GeForce 930(NV110 及更新) | GeForce 630-920(NVE0) | GeForce 400/500/600(NVCx & NVDx) | GeForce 8/9(NV5x, NV8x, NV9x, NVAx) 不推荐安装专有驱动 | GeForce 7 及以下 |
| --------- | ------------------------- | --------------------- | -------------------------------- | ------------------------------------------------------ | ---------------- |
| linux     | nvidia                    | nvidia-470xx-dkmsAUR  | nvidia-390xx-dkmsAUR             | nvidia-340xx-dkmsAUR                                   | 不支持           |
| linux-lts | nvidia-lts                | nvidia-470xx-dkmsAUR  | nvidia-390xx-dkmsAUR             | nvidia-340xx-dkmsAUR                                   | 不支持           |
| 其他      | nvidia-dkms               | nvidia-470xx-dkmsAUR  | nvidia-390xx-dkmsAUR             | nvidia-340xx-dkmsAUR                                   | 不支持           |

```bash
sudo pacman -S <driver> # 驱动本体
sudo pacman -S <driver>-utils # OpenGL 支持
sudo pacman -S lib32-<driver>-utils # 32 位 OpenGL 支持
sudo pacman -S opencl-nvidia  # OpenCL 支持
```

##### 开源驱动

```bash
sudo pacman -S xf86-video-nouveau # 驱动本体
sudo pacman -S mesa # OpenGL 支持
sudo pacman -S lib32-mesa # 32 位 OpenGL 支持
```

#### AMD / ATI 显卡

##### 开源驱动

| 架构                                            | 驱动              | OpenGL | 32 位 OpenGL | Vulkan        | 32 位 Vulkan        |
| ----------------------------------------------- | ----------------- | ------ | ------------ | ------------- | ------------------- |
| RDNA, RDNA 2, GCN 1, GCN 2, GCN 3, GCN 4, GCN 5 | xf86-video-amdgpu | mesa   | lib32-mesa   | amdvlk        | lib32-amdvlk        |
| GCN 1, GCN 2, TeraScale 或更老                  | xf86-video-ati    | mesa   | lib32-mesa   | vulkan-radeon | lib32-vulkan-radeon |

```bash
sudo pacman -S opencl-mesa  # OpenCL 支持
```

##### 闭源驱动

AMD 闭源驱动仅支持 `RDNA`, `RDNA 2`, `GCN 3`, `GCN 4`, `GCN 5` 架构的显卡

```bash
sudo pacman -S xf86-video-amdgpu # 驱动本体
paru -S amdgpu-pro-libgl  # OpenGL 支持
paru -S opencl-amd  # OpenCL 支持
paru -S vulkan-amdgpu-pro # Vulkan 支持
```

#### 其他显卡

```bash
sudo pacman -S xorg-drivers
sudo pacman -S mesa # OpenGL 支持
sudo pacman -S lib32-mesa # 32 位 OpenGL 支持
sudo pacman -S pocl # OpenCL 支持
```

## 安装Xorg和Xorg Server

因为某些原因，如果要在Artix Linux使用桌面环境推荐安装Xorg和Xorg Server两个一起安装

```bash
sudo pacman -S xorg
```

```bash
sudo pacman -S xorg-server
```

## 图形桌面

常见的图形桌面且能安装在Artix Linux有以下两种

| 名称                                                         | 官方预览图                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [KDE Plasma](https://chitang.tech/posts/arch-guide/#kde-plasma) | ![KDE Plasma](https://kde.org/content/plasma-desktop/plasma-widgets.png) |
| [Xfce 4](https://chitang.tech/posts/arch-guide/#xfce)        | ![Xfce 4](https://cdn.xfce.org/about/screenshots/4.16-1.png) |

<div class="info">

> GNOME因为需要依赖`systemd`所以无法使用

</div>

我推荐你使用KDE Plasma，比较简洁美观

#### KDE Plasma

```bash
sudo pacman -S plasma kde-applications
```

`kde-applications` 是可选的，其中包含 KDE 的其他应用

#### XFCE

```bash
sudo pacman -S xfce4 xfce4-goodies
```

`xfce4-goodies` 是可选的，其中包含 XFCE 的其他应用

## 图形登陆管理器

图形登录管理器用于更方便地启动桌面环境及登录账户

#### SDDM

推荐与 [KDE Plasma](https://chitang.tech/posts/arch-guide/#kde-plasma) 配合使用

```bash
sudo pacman -S sddm-runit # 给runit使用的sddm
```

然后设置开机启动SDDM

```bash
ln -s /etc/runit/sv/sddm /run/runit/sddm
```

(Thanks for [Shikikan_Neko08](https://twitter.com/Shikikan_Neko08))

# 疑难解答

## 如何设置开机自启动（systemctl command not found）

前面说到，Artix Linux是没有systemd的，所以不能使用常规的方式开机自启动

以下命令适用于runit内核

```bash
ln -s /<service-location/. /etc/runit/sv
```

不行的话试试下面这个

```bash
ln -s /<service-location> /run/runit/<service>
```

<div class="info">

> `<service-location>`为应用路径，不知道的自己搜一下就是

</div>

## Grub没有引导项（Grub only have UEFI Firmware Settings）

如果你出现了安装完重启后Grub没有引导项的情况（如图）

![](https://pic.lanta.cyou/img/2022-04-09_10-27.png)

出现这种情况可以尝试重新安装内核（因为可能是`vmlinuz`和`initrd.img`不见了）

```bash
basestrap /mnt <linux-kernel> linux-firmware
```

重新安装后查看`/mnt/boot`是否有`vmlinuz-linux`文件，如果有，就成功了

![](https://pic.lanta.cyou/img/20220409103355.png)

然后`artix-chroot /mnt`，重新生成Grub配置文件

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

![](https://pic.lanta.cyou/img/20220409103702.png)

重启后应该就能恢复正常了

![](https://pic.lanta.cyou/img/2022-04-09_10-44.png)

## archlinuxcn-keyring: signature from "farseerfc <farseerfc@archlinuxcn.org>" is unknown trust 

如果出现了以下错误就是Artix Linux不信任ArchLinuxCN的密钥

那么你可以尝试让Artix Linux不检查密钥

```
vim /etc/pacman.conf
```

将`SigLevel`改为`Never`

![](https://pic.lanta.cyou/img/2022-04-09_11-21.png)

再次安装就正常了

![](https://pic.lanta.cyou/img/2022-04-09_11-21_1.png)

## SDDM无法启动

如果你遇到了sddm无法启动的问题，可以尝试重装`sddm`、`sddm-runit`、`elogind`和Plasma桌面环境