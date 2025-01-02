# 简介

为了将一台电脑分为多个服务器，我尝试安装esxi，尝试过许多教程，下面是对教程的整理。

# ESXI下载

[官网下载地址](https://customerconnect.vmware.com/cn/downloads/info/slug/datacenter_cloud_infrastructure/vmware_vsphere/7_0)

[脚本之家下载地址](https://www.jb51.net/softjc/717737_all.html)

建议在脚本之家下载，因为有免费的许可证。

这里下载的esxi没有网卡驱动因此在安装时会显示No Network Adapters的错误，但你如果只是在VMware虚拟机上体验完全够用了。

如果你是在实体虚拟机上安装，请往下看。

# No Network Adapters问题

## 根据网卡型号选择驱动

设备管理器里找到自己的网卡型号。

![查看网卡型号](https://github.com/user-attachments/assets/e5532ab9-8f27-4d8e-a05a-382ce4433952)

根据网卡型号选择合适的驱动：[网站链接](https://vibsdepot.v-front.de/wiki/index.php/List_of_currently_available_ESXi_packages)

![网卡型号与驱动选择](https://github.com/user-attachments/assets/a7272129-2b96-4284-9670-18305895fb86)

net55-r8168驱动支持网卡型号（Realtek RTL8111B / RTL8168B / RTL8111/RTL8168 / RTL8111C / RTL8111CP / RTL8111D(L) / RTL8168C / RTL8111DP / RTL8111E / RTL8168E / RTL8111F / RTL8411 / RTL8111G / RTL8111GUS / RTL8411B(N) / RTL8118AS / D-Link DGE-528T）

## 根据下面的教程将网卡驱动封装到系统中

教程太多了，就不在这里描述了，在这里附[链接](https://blog.whsir.com/post-4462.html)，希望不会失效吧。

如果你所需要的驱动恰好是net55-r8168，可直接在下面的百度网盘资源中下载已经封装好的ESXI系统。

[百度网盘资源](https://pan.baidu.com/s/1XuOWRG-kNes3gi2lzyX_CA?pwd=91bb)

## 制作系统启动盘，安装系统

制作启动盘装，这个略过不提。

安装系统可根据脚本之家网站教程进行安装。

# 内存过小，无法安装

## 问题描述

在安装过程中我又遇到了这个问题，我的内存是3.9G的，但他需要的最小内存为4G。

此文使用的方法在很早的版本ESXi安装时就有人使用过，可以绕过检查，但并不推荐在小内存机器上安装ESXi。

有些机器本身是4G内存，也许因为部分内存被核显使用，或者需要被BMC/BIOS/UEFI预留，或者其它ESXi的计算方法，会在启动是显示为3.xGB内存，这样因为差一点点内存不能使用ESXi还是有点可惜的，就可以想办法绕过内存检查脚本。

## 解决方法

U盘启动安装ESXi，在黄色背景界面看到内存为3.7GiB(我使用物理ESXi嵌套安装，分配的3.75GiB内存)。

![3](https://github.com/user-attachments/assets/c2d8e133-787d-4137-82ba-ceb14389973e)

识别到3.7 GiB内存

同意协议，选择安装介质，设置密码等一系列操作后，遇到内存检查错误，提示需要4.00 GiB。

![4](https://github.com/user-attachments/assets/7498715c-3f0a-49cc-a825-bee039e3c063)

内存检查错误

在上图界面键盘按下ALT+F1，进入下图界面，然后用户名root，密码为空(之前设置的密码还未生效)。

![5](https://github.com/user-attachments/assets/315b97fe-a476-4a85-8e20-3bb9c40e6971)

ALT+F1进入Shell

执行如下命令切换到检查脚本所在目录。

```bash
cd /usr/lib/vmware/weasel/util
```

备份原有脚本，设置权限, 然后vi修改。

```bash
mv upgrade_precheck.py upgrade_precheck.py.bak
cp upgrade_precheck.py.bak upgrade_precheck.py
chmod 666 upgrade_precheck.py
vi upgrade_precheck.py
```

vi编辑器中搜索 “MEM_MIN_SIZE"，在vi中输入/MEM_MIN_SIZE。

![6](https://github.com/user-attachments/assets/4d267bb7-f868-4b9d-adbc-f3e97ad6787c)

vi搜索MEM_MIN_SIZE

第一次搜索命中结果应该定位到如下图光标位置。

![7](https://github.com/user-attachments/assets/b3dbed6c-b78c-438f-8041-82f44f222c64)

第一次命中结果

我们要修改的就在下方，使用光标定位到下面或者在搜索时按下n键去找第二次命中应该就是了。

![8](https://github.com/user-attachments/assets/371510a8-b60b-447f-8e08-48a21d8531b0)

需要修改的部分

定位光标到第一个4后面，按i进入编辑模式，退格删除4，修改为2。

![9](https://github.com/user-attachments/assets/f8c9140f-a38b-46e9-8691-64145c59316d)

修改最小内存为2GiB

按ESC进入命令模式，输入:wq,回车保存退出。

执行下面的命令(杀掉当前的python进程)。

```bash
kill -9 $(ps -c | grep weasel | grep -v grep | awk '{print $1}')
```

然后就又回到熟悉的安装界面了。

![10](https://github.com/user-attachments/assets/edfa5586-6076-48ac-901c-c4e94bd9a95d)

回到安装界面

一系列操作后，原来的内存检查错误界面不会出现了，出现了擦除安装介质的对话框，F11继续。

![11](https://github.com/user-attachments/assets/635685aa-5d95-45d1-b6f8-7ec01fa96bd8)

没有再次出现内存检查错误

然后就是等进度条啦。

![12](https://github.com/user-attachments/assets/449f84be-e25c-47de-845f-b3e6d055bc70)

开始等进度条

安装结束，提示拔下ESXi安装盘，比如U盘，然后回车重启。

![13](https://github.com/user-attachments/assets/b97984db-3c4c-4759-b94b-d56c86b32a4d)

安装完成

ESXi重启加载完毕后。

![14](https://github.com/user-attachments/assets/7a6fc542-e515-4944-9e90-c4db2e4408e4)

重启完毕

访问ESXi网页控制台，3.75 GB内存正常使用。

![15](https://github.com/user-attachments/assets/26225b9c-058e-4fab-ab32-3add128e6103)

3.75GiB正确识别并启动成功

# 如果你的存储太小，那么你有可能遇到新的问题

## 问题描述

我当时在虚拟机安装时，只给它分了40G的空间，安装好后完全没有存储空间可用，放心不是系统本身所占空间过大。如果你遇到同样的问题，不论是虚拟机还是实体机，都可参考下面的教程。

## 解决方法

在启动显示以下界面时在3秒内按下 SHIFT+O 组合键（字母O不是数字0）即可修改引导选项。

![16](https://github.com/user-attachments/assets/c6cba0e2-0f2a-41cc-bb94-3f055520354a)

在最后加上 autoPartitionOSDataSize 参数即可定制虚拟闪存大小。

![17](https://github.com/user-attachments/assets/0583c47c-8b7f-4d82-bd9f-6a612465357f)

>注意：默认已有 cdromBoot runweasel 段，务必需要在后面加上空格分隔参数！！默认单位为 MB ，例子为分配 8G，毕竟磁盘只有可怜的 80GB 容量。

然后一路安装即可，在部分主机上使用 DP 接口可能无法看到此界面，可以在按电源后一直按着组合键或者尝试使用 VGA 接口显示器（如果有的话）。

重新安装后即可看到虚拟闪存已经变成 8 GiB ，存储空间已经恢复正常。

![18](https://github.com/user-attachments/assets/74dc8274-58c1-4366-ba41-3df14f0e85a2)