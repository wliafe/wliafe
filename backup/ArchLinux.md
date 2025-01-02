# 安装ArchLinux

## 文章参考

- [Arch Linux 安装使用教程 - ArchTutorial - Arch Linux Studio](https://archlinuxstudio.github.io/ArchLinuxTutorial/#/rookie/archlinux_pre_install)

- [ArchWiki](https://wiki.archlinux.org/)

## 前期准备

- 虚拟机软件：VMware Workstation Lite15.5.5

    虚拟机的配置：1个处理器+2核+2G内存+20G存储（不安装图形化界面系统部分只用不到5G，包括图形化界面10G左右，虚拟机配置可自由选择。）网络部分是桥接模式。

- archLinux系统：archlinux-2022.03.01-x86_64.iso

    archlinux的U盘启动有点像是一个安全模式，本质上就是一个Linux系统，所以archlinux的安装就是用Linux系统安装Linux系统。

## 系统连网

第一步是系统连网，由于是虚拟机安装所以网络基本不用配置，尝试ping通就可。

```bash
ping baidu.com
```

## 分区

初学者只要分两个区就可以了，分别是swap区和根区（/）。

**展示分区状态**

```bash
lsblk
```

**分区**

```bash
cfdisk /dev/sda
```

dev是Linux的外部设备目录，sda是硬盘。

分区格式选择gpt（Linux的最佳格式），将sda分为2G（swap区）+18G（剩余的存储，给根目录）

**格式化分区**

```bash
mkswap /dev/sda1
```

```bash
mkfs.ext4 /dev/sda2
```

**挂载分区**

swap分区不用挂载，但要启动

```bash
swapon /dev/sda1
```

这里的/mnt就是Linux安装好后的根目录

```bash
mount /dev/sda2 /mnt
```

## 更换镜像源

文档参考：[源文档](https://mirrors.ustc.edu.cn/help/index.html)

编译镜像文件

```bash
vim /etc/pacman.d/mirrorlist
```

在文件中添加镜像源（中国国内镜像源选择一个就可以，这里是清华源）

```bash
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

更新镜像源

```bash
pacman -Syy
```

## 下载arch软件

（具体是干啥我也不太清楚）

base base-devel是Linux基础包，必须安装。linux linux-firmware是Linux驱动和Linux的其他东西。。。dhcpcd 是网络自动分配ip的服务，vim是Linux常用的编辑器。这次安装目的是方便在新装的系统中使用。

安装新系统软件

```bash
pacstrap /mnt base base-devel linux linux -firmware dhcpcd vim
```

生成文件系统信息

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

查看文件系统信息

```bash
cat /mnt/etc/fstab
```

## 进入新系统

这个命令将根目录切换为/mnt，也就是/mnt称为新的根目录(/)

```bash
arch-chroot /mnt
```

## 安装系统引导工具

pacman是archlinux的包管理器

```bash
pacman -S grub
```

```bash
grub-install --froce /dev/sda
```

生成默认配置文件

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

## 为root设置密码

```bash
passwd
```

## 解除U盘挂载，重启虚拟机

```bash
exit
```

```bash
umount -R /mnt
```

```bash
reboot
```

至此，你的archlinux就安装完成了，下面介绍archlinux的配置方式

# 配置ArchLinux

如果要安装图形化界面，可配置中文本地化，不安装则没必要。

查看镜像源，重新配置镜像源

重新配置网络

```bash
systemctl (start/status/enable/stop) dhcpcd
```

## 配置语言区域

编辑/etc/locale.gen文件，删除zh_CN.UTF-8 前面的#号，运行命令locale-gen

```bash
vim /etc/locale.gen
```

```bash
locale-gen
```

## 配置时区

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

时间同步

```bash
sudo timedatectl set-ntp true
```

检查时间同步状态

```bash
timedatectl
```

## 设置主机名

编辑文件/etc/hostname，将名字加入文件

```bash
vim /etc/hostname
```

编辑文件/etc/hosts，将名字加入文件

```bash
vim /etc/hosts
```

文件编辑内容

```text
127.0.0.1   localhost
::1         localhost
127.0.1.1   YourHostName
```

## 中文本地化配置

本文只参考这一个命令，拼音输入法建议ibus，具体就不写了。

```bash
pacman -S wqy-zenhei
```

# gnome图形化界面安装

## 前期准备

- 订阅结点，参考我的文章**上网**

**添加软件源**

文档参考：[源文档](https://mirrors.ustc.edu.cn/help/index.html)

编辑/etc/pacman.conf文件，在文件末尾添加。

```text
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

导入GPG key

```bash
sudo pacman -S archlinuxcn-keyring
```

```bash
sudo pacman -Sy
```

# gnome安装

```bash
sudo pacman -S gnome
```

```bash
systemctl enable gdm
```

## 安装插件

这个插件的目的是和谷歌的扩展交互，使谷歌扩展中的插件能够在gnome桌面上执行

```bash
sudo pacman -S chrome-gnome-shell
```

下载谷歌浏览器，安装[插件](https://extensions.gnome.org)

选择插件推荐：

- Desktop lcons NG(DING)

- Dynamic Panel Transparency

- Tray lcons:Reloaded

- User themes

- Dash To Dock

在Linux本地扩展中将插件打开。

至此gnome的可视化界面安装和插件的安装结束。

# 软件安装

**下载软件包管理器**

```bash
sudo pacman -S yay
```

**软件模糊搜索**

通过软件模糊搜索获得软件安装名称，然后可以有选择的安装软件。

```bash
sudo pacman -Ss xxx
```

```bash
yay xxx
```

**软件安装**

```bash
sudo pacman -S xxx
```

```bash
yay -S xxx
```