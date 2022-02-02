---
title: NetworkManager
---
# NetworkManager



## Install 

```sh
yum install NetworkManager
```

## Status

```sh
systemctl status NetworkManager
```



## Tools

- nmcli
- nmtui
- nm-connection-editor
- control-centor
- network connection icon

## nmtui



## nmcli 

### 开始、停止网卡

```sh
nmcli con up id bond0
nmcli con up id port0
nmcli dev disconnect bond0
nmcli dev disconnect ens3
```

### 帮助

```sh
nmcli help
```

### 交互式设置

```sh
nmcli con edit ens33
```

### 添加或修改

```sh
nmcli c add type ethernet ifname enp1s0 con-name "My Connection"
```



```sh
nmcli c modify "My favorite connection" ethernet.mtu 1600
```



```sh
nmcli con mod ens33 ipv4.dns "114.114.114.114 8.8.8.8"
nmcli con mod ens33 ipv4.gateway "192.168.86.2"
nmcli con up ens33 # 使配置生效
```





### wifi

```sh
nmcli dev wifi list

nmcli con add con-name MyCafe ifname wlp61s0 type wifi ssid MyCafe \
ip4 192.168.100.101/24 gw4 192.168.100.1

mcli con modify MyCafe wifi-sec.key-mgmt wpa-psk
nmcli con modify MyCafe wifi-sec.psk caffeine

nmcli radio wifi [on | off ]
```



## 设置路由

```sh
nmcli connection modify enp1s0 +ipv4.routes "192.168.122.0/24 10.10.10.1"
```





## GUI

### nm-connection-editor

在命令行执行

```sh
nm-connection-editor
```

会弹出修改界面



## ip command

```sh
ip link set ifname down
ip address add 10.0.0.3/24 dev enp1s0
ip addr show dev enp1s0

# 一块网卡配置多个ip
ip address add 192.168.2.223/24 dev enp1s0
ip address add 192.168.4.223/24 dev enp1s0
```



## vlan over bond

```bash
nmcli connection add type bond con-name Bond0 ifname bond0 bond.options "mode=active-backup,miimon=100" ipv4.method disabled ipv6.method ignore

nmcli connection add type ethernet con-name Slave1 ifname em1 master bond0 slave-type bond
nmcli connection add type ethernet con-name Slave2 ifname em2 master bond0 slave-type bond

nmcli connection add type bridge con-name Bridge0 ifname br0 ip4 192.0.2.1/24

nmcli connection add type vlan con-name Vlan2 ifname bond0.2 dev bond0 id 2 master br0 slave-type bridge
 
nmcli connection show
```



基线脚本片段

```bash
        if [[ $VLANID != "" ]];then
            if [[ "$BONDING_OPTS" == "" ]];then
                cmd="nmcli connection add type bond con-name ${BONDING_NAME} ifname ${BONDING_NAME} bond.options \"mode=${BONDING_MODE},miimon=100\" ipv4.method disabled ipv6.method ignore"
            else
                cmd="nmcli connection add type bond con-name ${BONDING_NAME} ifname ${BONDING_NAME} bond.options \"${BONDING_OPTS}\" ipv4.method disabled ipv6.method ignore"
            fi
            echo $cmd | bash >> $LOGFILE 2>&1
            nmcli connection add type ethernet con-name Slave1 ifname ${NIC1} master ${BONDING_NAME} slave-type bond
            nmcli connection add type ethernet con-name Slave2 ifname ${NIC2} master ${BONDING_NAME} slave-type bond
            nmcli connection add type bridge con-name Bridge0 ifname br0 ipv4.addresses ${IP}/${PREFIX} ipv4.gateway ${GATEWAY} ipv4.method manual
            cmd="nmcli connection add type vlan con-name Vlan${VLANID} ifname bond0${VLANID} dev bond0 id ${VLANID} master br0 slave-type bridge"
        else 
```



## 配置bond

```bash
nmcli connection add type bond ifname ${BONDING_NAME} con-name ${BONDING_NAME} bond.options mode=${BONDING_MODE} miimon=100
nmcli connection add type ethernet ifname ${NIC1}  master ${BONDING_NAME}
nmcli connection add type ethernet ifname ${NIC2}  master ${BONDING_NAME}
nmcli connection mod ${BONDING_NAME} ipv4.addresses ${IP}/${PREFIX} ipv4.gateway ${GATEWAY} ipv4.method manual
nmcli connection modify ${BONDING_NAME} ipv4.dns \"${DNS}\"
```




## 配置bond和vlan

准备

```bash
modprobe --first-time 8021q
modinfo 8021q
```

配置bond

```bash
nmcli con add type bond ifname bond0 con-name bond0 mode 802.3ad miimon 100 downdelay 0 updelay 0 connection.autoconnect yes ipv4.method disabled ipv6.method ignore

nmcli con add type bond-slave ifname enP48p1s0f0 con-name enP48p1s0f0 master bond0
nmcli con add type bond-slave ifname enp1s0f0 con-name enp1s0f0 master bond0

nmcli con up bond0
```

配置vlan taggin

```bash
nmcli con add type vlan ifname bond0.123 con-name bond0.123 id 123 dev bond0 connection.autoconnect yes ip4 192.168.123.123/24 gw4 192.168.123.1 ipv4.dns 192.168.123.4,192.168.123.5 ipv4.dns-search example.com

nmcli con up bond0.123
```



## 配置bond，vlan和bridge

配置bond

```bash
nmcli connection add type bond con-name Bond0 ifname bond0 bond.options "mode=active-backup,miimon=100" ipv4.method disabled ipv6.method ignore

nmcli connection add type ethernet con-name Slave1 ifname em1 master bond0 slave-type bond
nmcli connection add type ethernet con-name Slave2 ifname em2 master bond0 slave-type bond
```

配置brigde

```bash
nmcli connection add type bridge con-name Bridge0 ifname br0 ip4 192.0.2.1/24
```

配置vlan tagging

```bash
nmcli connection add type vlan con-name Vlan2 ifname bond0.2 dev bond0 id 2 master br0 slave-type bridge
```

