---
title: ubuntu
---
# ubuntu 安装

###  windows10 + ubuntu18.04双系统，重启关机卡的问题



高亮install ubuntu，然后按 `e`键，进入grub设置，将quiet splash -- 改为 quiet splash nomodeset

安装完后，也可以在引导界面高亮ubuntu然后按`e`键进入grub设置。

### 分区

ubuntu 分区

120G

/boot 200M

/ 25G

/home 用户数据，下载的文件和媒体文件，多多益善

swap 8G

/boot 选择逻辑分区

### 安装完后的一些操作

```sh
# 更新apt包
$ sudo apt update
$ sudo apt upgrade
# 驱动
$ ubuntu-drivers devices
$ ubuntu-drivers autoinstall
$ nvidia-settings
```

安装fcitx，安装搜狗输入法



## Ubuntu18安装zsh

```sh
sudo apt install zsh

vim /etc/pam.d/ppp
# 将required 改为sufficient
auth       sufficient   pam_shells.so

chsh -s /bin/zsh

# 根据官网命令安装
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```




