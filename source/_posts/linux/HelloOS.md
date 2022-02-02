---
title: HelloOS
---
# 操作系统实战 HelloOS

实验环境vmware ubunt20



## HelloOS下载与编译

```sh
git clone
make
cp HellOS.bin /boot/
```



## grub配置修改

针对ubuntu20

首先，ubuntu20默认不显示grub界面，修改配置/etc/default/grub

```sh
# GRUB_TIMEOUT-_STYLE=hidden
GRUB_DEFAULT_TIMEOUT=30
GRUB_CMDLINE_LINUX_DEFAULT="text"
```

添加menuentry, 在配置文件/etc/grub.d/40_custom后追加

```sh
menuentry 'HelloOS' {
     insmod part_msdos #GRUB加载分区模块识别分区
     insmod ext2 #GRUB加载ext文件系统模块识别ext文件系统
     set root='hd0,msdos4' #注意boot目录挂载的分区，这是我机器上的情况
     multiboot2 /boot/HelloOS.bin #GRUB以multiboot2协议加载HelloOS.bin
     boot #GRUB启动HelloOS.bin
}
```

使配置生效

```sh
sudo update-grub
```



> 如果一开始忘记修改grub显示配置，会出现开机就是HelloOS，没有grub的引导界面。
>
> 此时可以用光盘启动，然后选择try ubuntu进入live cd模式。
>
> 进去后执行mount /dev/sda5 /mnt，将系统挂载/mnt目录，/dev/sda5是我安装目录
>
> chroort /mnt 切换到之前的系统工作，然后i需改grub配置文件即可
