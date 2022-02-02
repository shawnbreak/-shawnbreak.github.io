# IPMI

>  IPMI(Intelligent Platform Management Interface) 即智能平台管理接口。是使硬件管理具备智能化的新一代通用接口标准。

用户可以利用IPMI监视服务器的物理特征，如温度、电压、电扇工作状态、电源供应以及机箱入侵等。

IPMI最大的优势在于它是独立于CPU BIOS和OS的，所以用户无论在开机状态还是关机状态下，只要接通电源就可以实现对服务器的监控。

IPMI是一种规范的标准，其中最重要的物理部件就是BMC(Baseboard Management Controller)，一种嵌入式管理微控制器，它相当于整个平台的大脑，通过它IPMI可以监控各个传感器的数据并记录各种事件的日志。



### ipmitool 常用命令

1. 查看开关机状态：

```shell
ipmitool -H (BMC的管理IP地址) -I lanplus -U (BMC登录用户名) -P (BMC登录用户名的密码) power status
```

2. 开机：
    Chassis Power Control: Up/On

  ```shell
  ipmitool -H (BMC的管理IP地址) -I lanplus -U (BMC登录用户名) -P (BMC 登录用户名的密码) power on
  ```

3. 关机：
    Chassis Power Control: Down/Off

  ```shell
  ipmitool -H (BMC的管理IP地址) -I lanplus -U (BMC登录用户名) -P (BMC 登录用户名的密码) power off
  ```

4. 重启：
    Chassis Power Control: Reset

  ```shell
  ipmitool -H (BMC的管理IP地址) -I lanplus -U (BMC登录用户名) -P (BMC 登录用户名的密码) power reset
  ```

5. 重启动BMC：

```shell
ipmitool -H (BMC的管理IP地址) -I lanplus -U (BMC登录用户名) -P (BMC 登录用户名的密码) mc reset <warm/cold>
```

6. 查看SEL日志：

```shell
ipmitool -H (BMC的管理IP地址) -I lanplus -U (BMC登录用户名) -P (BMC 登录用户名的密码) sel list
```

