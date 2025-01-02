# 包管理器

Ubuntu包管理器有：apt, apt-cache, apt-get, dpkg

```bash
#更新包管理器
apt update
#模糊查询软件
apt search 软件包
#安装软件
apt install 软件包
#下载软件源码
apt source 软件包
#卸载软件
apt-get remove 软件包
#查看已安装的软件包
dpkg -l
#查看软件包的依赖项
dpkg --list | grep 软件包
```

# 防火墙

Ubuntu防火墙是ufw

```bash
#安装防火墙
apt install ufw
#防火墙状态
ufw status
#开启防火墙
ufw enable
#关闭防火墙
ufw disable
#查看防火墙版本
ufw version
#默认允许外部主机访问
ufw default allow
#默认拒绝外部主机访问
ufw default deny
#开启${port}端口
ufw allow ${port}/tcp
#关闭${port}端口
ufw deny ${port}/tcp
#展示已有防火墙规则
ufw status
```

# 切换到root用户

```bash
#修改root密码
sudo passwd root
#在当前用户临时进入root，使用当前用户的sudo密码
sudo -s
```