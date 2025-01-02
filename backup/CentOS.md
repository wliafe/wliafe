# centos镜像源更换

**阿里云镜像**

```bash
yum install wget
```

```bash
rm /etc/yum.repos.d/CentOS-Base.repo
```

```bash
sudo wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo 
```

```bash
yum makecache
```

# 包管理器

## yum包管理器

安装软件包

```bash
yum install -y 软件包
```

升级软件包，改变软件设置和系统设置，系统版本内核都升级

```bash
yum update 软件包
```

升级软件包，不改变软件设置和系统设置，系统版本内核都升级

```bash
yum install upgrade
```

模糊查询软件包

```bash
yum search 软件包
```

查询软件包

```bash
yum info 软件包
```

查询命令属于哪一个包

```bash
yum provides /usr/bin/find
```

卸载包

```bash
yum remove -y 软件包
```

按关键字搜索包

```bash
yum search 软件包
```

清除缓存

```bash
yum clan all
```

生成缓存

```bash
yum makecache
```

查看可用的yum源

```bash
yum repolist
```

列出可用组

```bash
yum grouplist
```

## rpm包管理器

命令格式：

```bash
rpm 参数 软件包
```

参数：

```text
安装：
-i 是install的意思，安装软件包
-v 显示附加信息，提供更多详细信息
-V 校验，对已安装的软件进行校验
-h --hash 安装时输出###标记
查询
-q 查询，一般跟下面的参数配合使用
-a 查询所有已安装的软件包
-f 系统文件名（查询系统文件属于哪个安装包）
-i 显示已安装的rpm软件包信息
-l 查询软件包文件的安装位置
-p 查询未安装软件包的相关信息
-R 查询软件包的依赖性
卸载
-e erase
--nodeps 忽略依赖
升级
-U 一般配合vh使用
```

# MySQL

## MySQL安装

下载mysql数据包

```bash
wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
```

安装mysql

```bash
yum -y install mysql57-community-release-el7-10.noarch.rpm
```

```bash
yum -y install mysql-community-server
```

如果报错说公钥尚未安装则用如下命令，跳过公钥检查

```bash
yum -y install mysql-community-server --nogpgcheck
```

启动mysql服务

```bash
systemctl start  mysqld
```

查看初始密码

```bash
grep 'password' /var/log/mysqld.log
```

进入数据库

```bash
mysql -h (主机ip，可省略) -u root -p 
```

修改root密码，xxxx就是新密码，大小写加符号

```bash
ALTER USER USER() IDENTIFIED BY 'XXXX';
```

退出

```bash
exit
```

## MySQL连接c++

### Mysql++简介

这里采用Mysql++这个库

mysql连接c++主要的包有mysql-libs mysql-devel mysql++

Mysql++是官方发布的、一个为MySQL设计的C++语言的API。Mysql++为Mysql的C-Api的再次封装，它用STL(Standard Template Language)开发并编写，并为C++开发者提供像操作STL容器一样方便的操作数据库的一套机制。

### 安装步骤

下载源码包：http://tangentsoft.net/mysql++/

解压

```bash
tar zxvf mysql3.2.1.tar.gz
```

```bash
cd mysql++-3.2.1/
```

```bash
./configure
```

```bash
make
```

```bash
make install
```

修改/etc/ld.so.conf文件，添加
   /usr/local/lib
   /sbin/ldconfig
   /bin/ln -s /usr/local/lib/libmysqlpp.so /usr/lib/libmysqlpp.so

用 echo $LD_LIBRARY_PATH看下LD_LIBRARY_PATH环境变量中是否包含/usr/local/lib，如果没有的话就export下
   
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib

## mysql远程连接

进入数据库

```bash
use mysql;
```

```bash
update user set user.Host='%' where user.User='root';
```

```bash
flush privileges;
```

# 防火墙firewall

## 安装firewall

```bash
yum install -y firewalld
```

## 使用firewall

开放端口

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

重新启动防火墙

```bash
firewall-cmd --reload
```

查看端口是否开启

```bash
firewall-cmd --zone=public --query-port=80/tcp
```

删除端口

```bash
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```

列出开放端口

```bash
firewall-cmd --zone=public --list-ports
```

# 网络配置文件

查看你的网络信息，网卡名，mac地址，IP地址

```bash
ip address
```

```bash
cd /etc/sysconfig/network-scripts
```

你的网卡名

```bash
vim xxx
```

**dhcp自动获取ip**

重要设置：mac地址，dhcp，onboot

```bash
HWADDR=00:0c:29:58:27:57
TYPE=Ethernet
BOOTPROTO=DHCP
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=eno16777736
UUID=71557f7c-446c-4145-8151-1f52f07b8b12
ONBOOT=yes                  
#开启自动启用网络连接
#这里增加了第一行的mac地址，
#最后一行修改成了yes开启网络连接
```

**设置静态ip**

最重要的四个设置，ip、网关、子网掩码、dns

```text
代码示例:
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static  #启动的时候的 IP 取得的协议，这里是固定的，如果是动态主机的话，要改成 dhcp 才行#
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33     #设定网卡的名称，要跟文件名称对应 #
UUID=f5e37a10-3da9-47af-8dbb-370b7bf24509 
DEVICE=ens33   #设定网卡的名称，要跟文件名称对应 #
ONBOOT=yes    #是否在开机的的时候启动网卡# 
IPADDR=192.168.0.7        #IP 地址#   必设置
GATEWAY=192.168.0.2       #网关地址#  必须设置
NETWORK=192.168.0.3　　    #该网段的第一个 IP# 可以不设置
BROADCAST=192.168.0.255　　#最后一个同网段的广播地址#  可以不设置
NETMASK=255.255.255.0     #子网掩码#   必设置
DNS1=192.168.0.1   必设置   跟ip地址一样，只需要把最后末尾改成1即可
#GATEWAYDEV=eth0 推荐阅读： linux网络配置文件(redhat、ubuntu系统) centos基本网络配置-网卡eth0、DNS、Host等
linux主机刚安装好时，ONBOOT属性的缺省值为no，需要修改为yes，BOORPROTO缺省值为dhcp，需要修改为static。
然后，设置IP地址，网络掩码，网关等。
```

# systemctl

## systemctl基础命令

新加载配置

```bash
systemctl daemon-reload
```

列出当前系统服务的状态

```bash
systemctl list-units
```

列出服务的开机状态

```bash
systemctl list-unit-files
```

设定指定服务开机开启

```bash
systemctl enable 服务
```

设定指定服务开机关闭

```bash
systemctl disable 服务
```

使指定服务从新加载配置

```bash
systemctl reload 服务
```

查看指定服务的倚赖关系

```bash
systemctl list-dependencies 服务
```

冻结指定服务

```bash
systemctl mask 服务
```

启用服务

```bash
systemctl unmask 服务
```

## systemctl控制服务配置文件存放位置

/etc/systemd/system/

/usr/lib/systemd/system

/lib/systemd/system

## 配置文件格式及含义

**文件内容**

```text
# sshd.service
[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.service
Wants=sshd-keygen.service
[Service]
Type=forking
PIDFile=/var/run/sshd.pid
EnvironmentFile=/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartSec=42s
[Install]
WantedBy=multi-user.target
```

**文件内容解释**

```text
[Unit] 区块：启动顺序与依赖关系。
Description：当前配置文件的描述信息。
Documentation：帮助信息。
After：表示当前服务是在那个服务后面启动，一般定义为网络服务启动后启动
Wants：表示sshd.service与sshd-keygen.service之间存在”弱依赖”关系，即如果”sshd-keygen.service”启动失败或停止运行，不影响sshd.service继续执行。
[Service] 区块：启动行为
Type：定义启动类型。
PIDFile：服务的pid文件路径。
EnvironmentFile：指定当前服务依赖的环境参数文件。
ExecStart：定义启动进程时执行的命令。
ExecReload：重启服务时执行的命令
KillMode：定义 Systemd 如何停止 sshd 服务。
Restart：定义了 sshd 退出后，Systemd 的重启方式。
RestartSec：表示Systemd重启服务之前，需要等待的秒数。上面的例子设为等待42秒。
[Install] 区块：定义如何安装这个配置文件，即怎样做到开机启动。
WantedBy：表示该服务所在的 Target。multi-user.target表明当系统以多用户方式（默认的运行级别）启动时，这个服务需要被自动运行。
```