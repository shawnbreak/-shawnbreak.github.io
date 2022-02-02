---
title: Redhat 开发环境配置
---
# redhat7.4 开发环境配置

[TOC]

## 1. 启动U盘制作

制作工具：rufus

## 2. 安装系统

进入BIOS设置U盘启动。

分区配置

| 挂载点 | 容量     |
| ------ | -------- |
| /boot  | 1024M    |
| swap   | 2048M    |
| /      | 所有剩余 |

账户配置

| 账户   | 密码       |
| ------ | ---------- |
| root   | Root1234   |
| hemign | Heming1234 |

软件选择：Server with GUI

## 3. 网络配置

默认：dhcp

## 4. CentOS yum安装

```sh
rpm -qa | greap yum
# 删除原本的yum文件
sudo rpm -qa | grep yum | xargs rpm -e --nodeps
```
~~去阿里镜像站下载五个文件（对应版本的目录下）~~
~~https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/~~

~~yum-metadata-parser~~
~~yum~~
~~yum-plugin-fastestmirror~~
~~yum-utils~~
~~python-urlgrabber~~

```sh 
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-3.4.3-158.el7.centos.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-45.el7.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/yum-utils-1.1.31-45.el7.noarch.rpm
wget https://mirrors.aliyun.com/centos/7/os/x86_64/Packages/python-urlgrabber-3.10-8.el7.noarch.rpm  
```

~~安装，加上 --force强制安装（解决依赖问题）~~

```sh
sudo rpm -ivh xxx1.rpm xxx2.rpm xxx3.rpm xxx4.rpm xxx5.rpm --force --nodeps
```

> 说明：不需要卸载原有的yum重新安装，添加阿里或者163的cengtos的yum仓库即可。

阿里镜像yum源

```sh
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

修改版本号

```sh
sudo vim /etc/yum.repos.d/CentOS-Base.repo 

:%s/$releaserver/7/g
```

最后

```sh
sudo yum clean all 
sudo yum makecache
sudo yum update
```

## 5. idea安装

下载二进制软件包: ideaC.tar.gz

解压安装

```sh
sudo cp ideaC.tar.gz /opt/
cd /opt
sudo tar -zxvf ideaC.tar.gz
cd ideaC
sudo sh idea.sh # 执行这个脚本开始初始配置
```

## 6. VS code 安装

官网下载vs code的rpm包: code*.rpm

rpm安装

```sh
rpm -ivh code*.rpm
```

如果报错：Failed Deppendencies: libXss.so...

```sh
yum install libXss* -y
rpm -ivh code*.rpm 
code ## 启动vscode
```

如果有报错信息：/lib/libdbus.so...... no version available....

```sh
sudo yum install dbus -y
```



## 7. Typora安装

官网下载二进制文件：Typora-linux-x64.tar.gz （https://www.typora.io/#linux）

解压安装

```sh
cp Typora-linux-x64.tar.gz /opt/
cd /opt
tar -zxvf Typora-linux-x64.tar.gz
cd Typora-linux-x64
./Typora # 执行成功会出现tyora编辑界面
```

报错：

1. /lib/libdbus.so...... no version available

```sh
sudo yum install dbus -y # 安装所缺调用库即可
```

2. ..(省略)make sure the /opt/Typora-linux-x64/chrom-sandbox's ower is root, and has 4755 mode.

```sh
sudo chown -R root /opt/Typora-linux-x64
sudo chgrp -R root /opt/Typora-linux-x64
sudo chmod 4755 /opt/Typora-linux-x64/chrome-sandbox
```

创建快捷方式

```sh
su root
vim /usr/share/applications/typora.desktop
```

```sh
[Desktop Entry]
Name=Typora
Exec=/opt/Typora-linux-x64/Typora %U
Icon=/opt/Typora-linux-x64/resources/app/asserts/icon/icon_128x128.png
Type=Application
Terminal=false
```

保存文件即可

## 8. 安装svn

```sh
sudo yum install svn
svn checkout repository-url # 拉去源码
```

## 9. 安装 oracle jdk

```sh
rpm -qa | grep jdk | xargs rpm -e --nodeps
rpm -ivh jdk*.rpm
java -version
```

## 10. 安装nodejs

官网下载二进制包：node-v14.0.0-linux-x64.tar

```sh
cp node-v14.0.0-linux-x64.tar /opt/
xz -d node-v14.0.0-linux-x64.tar.xz
tar -xf node-v14.0.0-linux-x64.tar
```

部署bin文件

```sh
ln -s /opt/node-v14.0.0-linux-x64/bin/node /usr/bin/node
ln -s /opt/node-v14.0.0-linux-x64/bin/npm /usr/bin/npm
ln -s /opt/node-v14.0.0-linux-x64/bin/npm /usr/bin/npx
```

测试

```sh
node -v
npm -v
npx	
```

设置淘宝的npm仓库

```sh
# 查看源
npm get registry

# 设置淘宝源
npm config set registry https://registry.npm.taobao.org
```





## 11.安装gcc, gcc-c++

npm install 时报错 make: g++: Command not found

```sh
sudo yum install make gcc gcc-c++ -y
```

## 12. 安装ipmitool

```sh
sudo yum install ipmitool
```

## 13. 安装Dell的racadm

解压

安装

```sh
# cd 到解压目录的racadm路径下
sudo sh install_racadm.sh
```

使用时报错：

```sh
[heming@localhost ~]$ racadm -r 192.168.1.196 -u root -p calvin --nocertwarn get -t xml -f test.xml
ERROR: RAC1170: Unable to find the SSL library in the default path.
       If a SSL library is not installed, install one and retry the 
       operation. If a SSL library is installed, create a soft-link of the 
       installed SSL library to "libssl.so" using the linux "ln" command 
       and retry the operation.
```

原因：缺少ssl库文件

解决：

安装openssl

```sh
sudo yum install openssl

# 可以找到ssl库的位置在 /usr/lib64/libssl.so.1.0.2k
sudo find / -name libssl.so*
sudo ln -s /usr/lib64/libssl.so.1.0.2k /opt/dell/srvadmin/lib64/libssl.so
```

## 14. 安装Huawei的urest,umate

解压二进制软件包到项目下制定路径即可。

注意：请将压缩包在linux环境下用tar解压，涉及到链接文件，和文件权限等问题。
