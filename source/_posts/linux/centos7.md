# Centos 7



## SELinux

关闭SELinux

```sh
sestatus
set enforce 0 #permissive
/etc/selinux/config
# SELinux=enforcing
# SELinux=permissive
SELinux=disabled
```

## Firewalld

```sh
firewall-cmd --status
systemctl status firewalld
systemctl stop firewalld
systemctl disable firewalld
```



## yum 源

下载源配置文件

```sh
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

修改源配置文件

```sh
vim /etc/yum.repos.d/CentOS-Base.repo 
```

打开文件后将版本号改为7即可

```sh
:%s/$releaserver/7/g
```

更新即可

```sh
yum clean all 
yum makecache
yum update
```



报错：initscripts conflicts with redhat-release-server-7.4-18.el7.x86_64

解决1：删除冲突包

```sh
rpm -e redhat-release-server-7.4-18.el7.x86_64 --nodeps
```



解决2(推荐)：阻止更新内核 initscripts

```sh
vim /etc/yum.conf
```

追加以下内容

```sh
exclude=kernel*
exclude=centos-release*
exclude=initscripts
```





## Centos 7 配置Samba

- 安装

将本地iso配置为yum源

使用yum安装

```sh
yum install samba -y
```

- 配置

在配置文件中` /etc/samba/smb.conf `追加如下内容

```sh
[hmcsdata]
	path=/home/heming/hmcsdata
	writeable=yes
```



- 创建共享目录和共享用户

```sh
mkdir /home/heming/hmcsdata # 创建共享目录
groupadd sambashare # 闯将共享用户组
chgrp sambashare /home/heming/hmcsdata # 将共享目录的用户组改为sambashare
useradd -s /usr/sbin/nologin -G sambashare josh # 创建共享用户
```

> `-M` -do not create the user’s home directory. We’ll manually create this directory.
>
> `-d /samba/josh` - set the user’s home directory to `/samba/josh`.
>
> `-s /usr/sbin/nologin` - disable shell access for this user.
>
> `-G sambashare` - add the user to the `sambashare` group.

- 启用共享用户

```sh
smbpasswd -a josh # 将用户添加（add）到samba配置
smbpasswd -e josh # 启用（enable）用户
```

## mail

安装

```sh
# ubuntu
sudo apt-get install heirloom-mailx

# centos
yum install -y mailx 
```

添加配置

```sh
# ubuntu
vim /etc/nail.rc
# centos
vim /etc/mail.rc


set from=mail@qq.com
set smtp=smtp.exmail.qq.com
set smtp-auth-user=mail@qq.com
set smtp-auth-password=123456
set smtp-auth=login
```

测试

```sh
echo  "MAIL-BODY" | mail -s "Title" 123@qq.com,345@qq.com
```

## GNOME图形化界面

1. root模式
2. 安装 X Windows System

```sh
yum groupinstall "X Window System" -y
yum grouplist # 查看已经安装的软件
```

3. 安装gnome

```sh
yum groupinstall "GNOME Desktop" "Graphical Administration Tools" -y
```

4. 启动图形化界面

```sh
reboot # 先重启
startx
```

5. 系统的默认启动界面还是命令行，更改为启动为图形界面执行下面的命令

```sh
ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
```

## XFCE 图形界面安装

```sh
yum grouplist # 查看可用安装组
yum groupinstall "X Window system"
yum groupinstall xfce

systemctl isolate graphical.target # 启动
systemctl set-default graphical.target # 设置默认启动
```

```sh
yum install cjkuni* -y # 安装中文字体
yum install google-noto-sans-simplified-chinese-fonts.noarch
```



```sh
# 安装输入法，待验证
yum install -y ibus ibus-libpinyin ibus-gtk2 ibus-gtk3 im-chooser gtk2-immodule-xim gtk3-immodule-xim

im-chooser
```



## Redhat 7 远程桌面方案

> 前提：Redhat 7 已经安装了桌面环境 X Window System和Gnome或Xfce或KDE

### VNC



### XRDP

> an open-source Remote Desktop Protocol server

```sh
# 安装
yum install epel-release -y
yum install xrdp -y

# 设置开机自启并启动
systemctl enable xrdp
systemctl enable xrdp-sesman
systemctl start xrdp-sesman
systemctl start xrdp

# 重启防火墙
firewall-cmd --permanent --zone=public --add-port=3389/tcp
firewall-cmd --reload
```

## yum download only

```sh
yum install yum-plugin-downloadonly 
yum install httpd httpd-devel --downloadonly --downloaddir=/home/httpd
```

## 单网卡多IP设置

### 方法1：通过网卡配置文件配置多个IP

vim /etc/sysconfig/network-scripts/ifcfg-ens33

```sh
TYPE=Ethernet
BOOTPROTO=none
NAME=eno16777736
DEVICE=eno16777736
ONBOOT=yes
IPADDR0=10.91.137.66
PREFIX0=27
GATEWAY0=10.91.137.65        
#####################
IPADDR1=192.168.100.2             
PREFIX1=24      
```

> 可通过nmtui运行命令行图形化界面配置

### 方法2： 通过网卡别名配置多个IP

```sh
cp /etc/sysconfig/network-scripts/ifcfg-ens33 /etc/sysconfig/network-scripts/ifcfg-ens33:0
vim /etc/sysconfig/network-scripts/ifcfg-ens33:0
```

```sh
TYPE=Ethernet
BOOTPROTO=none
NAME=eno16777736:0
DEVICE=eno16777736:0
ONBOOT=yes
IPADDR=192.168.100.3
NETMASK=255.255.255.0
```



## 开启telnet服务

- 检查是否安装telnet和xinetd

  ```sh
  rpm -qa | grep telnet
  rpm -qa | grep xinetd
  ```

- 安装`telnet`，`xinetd`服务

  ```sh
  yum install xinetd telnet telnet-server -y
  ```

  

- 启动 `telnet`服务

```sh
# 设置开机自启动
systemctl enable telnet.socket
systemctl enable xinetd
```

```sh
# 启动telnet
systemctl start telnet.socket
systemctl start xinetd
```

```sh
# 查看telnet的服务状态
systemctl status telnet.socket
systemctl status xinetd
```

- 防火墙开放 `telnet`服务端口

```sh
cd /usr/lib/firewalld/services
vim tel.xml
```

写入以下内容

```xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>Telnet</short>
  <description>Telnet is a protocol for logging into remote machines. It is unencrypted, and provides little security from network snooping attacks. Enabling telnet is not recommended. You need the telnet-server package installed for this option to be useful.</description>
  <port port="23" protocol="tcp"/>
</service>
```

在终端键入以下命令

```sh
firewall-cmd --add-service=telnet --zone=public --permanent
firewall-cmd --reload
```

>查看防火墙是否开启`telnet`服务端口的命令
>
>```sh
>firewall-cmd --list-services
>```
>
>

- 关闭`telnet`服务

```sh
systemctl stop telnet.socket
systemctl stop xinetd
firewall-cmd --remove-service=telnet --permanent
firewall-cmd --reload
```





# 常见问题



## 非root用户绑定低端口问题

### 解决1： cap_net_bind_service

setcap 'cap_net_bind_service=+ep' /usr/java/jdk1.8.0_241-amd64/jre/bin/java
setcap -r /usr/java/jdk1.8.0_241-amd64/jre/bin/java
注意：
1、Linux的内核版本必须不低于2.6.24
2、针对脚本不起作用，只能是ELF可执行文件
3、会禁用程序的LD_LIBRARY_PATH变量
java有自己的LD_LIBRARY_PATH变量，所以这招行不通

### 解决2：端口转发

### 解决3：authbind

github:https://github.com/tootedom/authbind-centos-rpm
参考博客： https://beproud.wordpress.com/2019/04/19/install-tomcat-in-centos-port-80-443/




## java非root用户调用InetAddress.isReachable()失败问题

 Linux non-root user call java InetAddress.isReachable()
为什么Linux非root用户调用java的InetAddress.isReachable()会失败？
java中的InetAddress.isReachable()是通调用Linux的系统调用来实现的，然而Linux的系统调用是需要root权限的，所以会失败。
参考回答：https://stackoverflow.com/questions/2777226/isreachable-in-java-doesnt-appear-to-be-working-quite-the-way-its-supposed-to
参考博客：https://bordet.blogspot.com/2006/07/icmp-and-inetaddressisreachable.html

解决1：直接调用系统提供ping程序

```java
Process p1 = java.lang.Runtime.getRuntime().exec("ping -c 1 www.google.com");
int returnVal = p1.waitFor();
boolean reachable = (returnVal==0);
```

说明：ping有suid权限，任何人调用这个程序的暂时有root的权限



## U盘安装启动问题问题

制作Linux启动U盘时，U盘格式为FAT32，U盘名字的长度有限制。

如`RHEL-7.4 Server.x86_64`，grub的安装启动参数中只能读到``RHEL-7_4\20SE`，因此会导致安装的时候找不到安装源。

解决方案有两种（亲测有效）

1. 改U盘名字（LABEL方式启动）

   制作完启动U盘后，将其名字改短，如`RHEL`

   启动界面按<kbd>e</kbd>编辑安装启动参数，将`inst.stage2=hd:LABEL=RHEL-7_4\20SE quiet`改为 `inst.stage2=hd:LABEL=RHEL quiet`

   

2. 直接用`/dev/sdb4`启动

   不改名字直接启动会报错，然后几分钟后会进入dracut。

   ```sh
   ls /dev
   ```

   可以看到有 `sda sda1 sdb sdb1 sdb2`等等

   找到你U盘对应的那一个，一般来说为sd开头的最后一个，不确定可以挂载看看里面的内容。

   然后执行`reboot`命令重启。

   启动界面按<kbd>e</kbd>编辑安装启动参数，将`inst.stage2=hd:LABEL=RHEL-7_4\20SE quiet`改为 `inst.stage2=hd:/dev/sdb4 quiet`（注意不一定是sdb4，根据具体情况）

## 启动报错

 Failed to open \efi\centos\grubx64.efi - not found 

解决方案：用启动盘进入急救模式，将启动盘中的grubx64.efi文件拷到 对应目录下即可



## 重启进入grub

环境：redhat 7.4， minimal install + X Window system + xfce

问题描述：换yum源后yum update，然后重启进入grub模式。启动选项有一个centos和一个redhat。

解决：急救模式下，将 /boot/efi/EFI/redhat/grub.cfg拷到 /boot/efi/EFI/centos/然后重启即可



# 重装yum

> ~~背景：redhat 7 没有redhat账户不能用~~
>
> 只要配置对应的yum源就行，不需要重装yum

```sh
rpm -qa | greap yum
# 删除原本的yum文件
rpm -qa | grep yum | xargs rpm -e --nodeps
```
去阿里镜像站下载五个文件（对应版本的目录下）
https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/

yum-metadata-parser
yum
yum-plugin-fastestmirror
yum-utils
python-urlgrabber

```sh 
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-3.4.3-158.el7.centos.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-45.el7.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-utils-1.1.31-45.el7.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/python-urlgrabber-3.10-8.el7.noarch.rpm  
```

安装，加上 --force强制安装（解决依赖问题）

```sh
rpm -ivh xxx1.rpm xxx2.rpm xxx3.rpm xxx4.rpm xxx5.rpm --force --nodeps
```

阿里镜像yum源

```sh
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget-O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

修改版本号

```sh
vim /etc/yum.repos.d/CentOS-Base.repo 

:%s/$releaserver/7/g
```

最后

```sh
yum clean all 
yum makecache
yum update
```