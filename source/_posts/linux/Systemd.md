# Systemd

[TOC]

Systemd是Linux系统工具，用来启动守护进程（daemon）。已成为大多数发行版的标准配置。

Systemd取代了initd，成为系统的第一个进程（PID等于1），其他的进程都是他的子进程。



## Systemd架构图

![img](Images/bg2016030703.png)

Systemd的优点是功能强大，使用方便，缺点是体系庞大，非常复杂。

## 系统管理

Systemd并不是一个命令，而是一组命令，涉及到系统管理的方方面面。

### systemctl

Systemctl是Systemd的主命令，用于管理系统。

```bash
# 重启系统
$ sudo systemctl reboot

# 关闭系统，切断电源
$ sudo systemctl poweroff

# CPU停止工作
$ sudo systemctl halt

# 暂停系统
$ sudo systemctl suspend

# 让系统进入冬眠状态
$ sudo systemctl hibernate

# 让系统进入交互式休眠状态
$ sudo systemctl hybrid-sleep

# 启动进入救援状态（单用户状态）
$ sudo systemctl rescue
```

### systemd-analyze

`systemd-analyze`命令用于查看启动耗时。

> ```bash
> # 查看启动耗时
> $ systemd-analyze                                                                                       
> 
> # 查看每个服务的启动耗时
> $ systemd-analyze blame
> 
> # 显示瀑布状的启动过程流
> $ systemd-analyze critical-chain
> 
> # 显示指定服务的启动流
> $ systemd-analyze critical-chain atd.service
> ```

### hostnamectl

`hostnamectl`命令用于查看当前主机的信息。

> ```bash
> # 显示当前主机的信息
> $ hostnamectl
> 
> # 设置主机名。
> $ sudo hostnamectl set-hostname rhel7
> ```

### journalctl

```bash
journalctl -b
journalctl -b -1
journalctl --list-boots

journalctl --since "1 hour ago"
journalctl --since "2015-06-26 23:15:00" --until "2015-06-26 23:20:00"
journalctl -u nginx.service
journalctl -u nginx.service -u mysql.service
journalctl -f
journalctl -u mysql.service -f
journalctl -n 50 --since "1 hour ago"
journalctl -u sshd.service -r -n 1
journalctl -u apache2.service -r -o json-pretty
journalctl -b -1  -p "emerg".."crit"
journalctl _UID=108
```



### localectl

`localectl`命令用于查看本地化设置。

> ```bash
> # 查看本地化设置
> $ localectl
> 
> # 设置本地化参数。
> $ sudo localectl set-locale LANG=en_GB.utf8
> $ sudo localectl set-keymap en_GB
> ```

### timedatectl

`timedatectl`命令用于查看当前时区设置。

> ```bash
> # 查看当前时区设置
> $ timedatectl
> 
> # 显示所有可用的时区
> $ timedatectl list-timezones                                                                                   
> 
> # 设置当前时区
> $ sudo timedatectl set-timezone America/New_York
> $ sudo timedatectl set-time YYYY-MM-DD
> $ sudo timedatectl set-time HH:MM:SS
> ```

### loginctl

`loginctl`命令用于查看当前登录的用户。

> ```bash
> # 列出当前session
> $ loginctl list-sessions
> 
> # 列出当前登录用户
> $ loginctl list-users
> 
> # 列出显示指定用户的信息
> $ loginctl show-user ruanyf
> ```



## 自定义systemd服务

文件名以.service结尾，放在/etc/systemd/system/, /usr/lib/systemd/system/ 目录下。

文件内容一般包含三个部分：

- Unit: 定义service的一些基本信息， description, after
- Service: 定义服务的启动和停止命令，类型等
- Install: 服务的运行级别

```bash
[Unit]
Description=custom service
after=network.service

[Service]
Type=forking
User=test
Group=test
ExecStart=/bin/app start
Restart=always
SuccessExitStatus=KILL

[Install]
WantedBy=multi-user.target

```



https://www.freedesktop.org/software/systemd/man/systemd.service.html



## type forking, simple

Type=forking
systemd认为当该服务进程fork，且父进程退出后服务启动成功。对于常规的守护进程（daemon），除非你确定此启动方式无法满足需求，使用此类型启动即可。使用此启动类型应同时指定 PIDFile=，以便 systemd 能够跟踪服务的主进程
Type=simple
systemd认为该服务将立即启动。服务进程不会 fork 。如果该服务要启动其他服务，不要使用此类型启动，除非该服务是socket 激活型。

