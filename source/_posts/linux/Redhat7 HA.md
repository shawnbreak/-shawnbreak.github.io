# 配置Redhat7高可用、浮动IP

## 准备

两台redhat7服务器: 192.168.10.9, 192.168.10.10

## 配置/etc/hosts

各节点服务器分别执行

```sh
192.168.10.9 node1
192.168.10.10 node2
```

检查

```sh
ping -c 3 node1
ping -c 3 node2
```

## 配置高可用yum源

各节点服务器分别执行

高可用软件在addons文件夹里面

```sh
[HighAvailability]
name=HighAvailability
baseurl=file:///mnt/addons/HighAvailability
gpgcheck=0
enable=1
```

## 安装

各节点服务器分别执行

```sh
yum -y install corosync pacemaker pcs
```

## 启动

配置开机自启动，各节点服务器分别执行

```sh
systemctl enable pcsd
systemctl enable corosync
systemctl enable pacemaker
```

启动，各节点服务器分别执行

```sh
systemctl start pcsd
```

## 配置高可用用户

各节点服务器分别执行

```sh
passwd hacluster
```

## 创建集群

在node1上 执行即可

```sh
# 授权
pcs cluster auth node1 node2
# 配置
pcs cluster setup --name my_cluster node1 node2
# 启动
pcs cluster start --all
pcs cluster enable --all
```

检查状态

```sh
pcs status cluster
```

## 关闭STONITH和Quorum Policy

在node1上 执行即可

> STONITH: Shoot The Other Node In The Head

```sh
pcs property set stonith-enabled=false
pcs property set no-quorum-policy=ignore
pcs property list
```

## 添加Resources

### 查看Resouces

```sh
pcs status resources
```

### 添加浮动IP

```sh
pcs resource create virtual_ip ocf:heartbeat:IPaddr2 ip=192.168.10.11 cidr_netmask=32 op monitor interval=30s
```

## 添加nginx

```sh
pcs resource create webserver ocf:heartbeat:nginx configfile=/etc/nginx/nginx.conf op monitor timeout="5s" interval="5s"
```



## 配置Cluster Constraint

```sh
pcs constraint colocation add webserver virtual_ip INFINITY
pcs constraint order virtual_ip then webserver
```

## 重启集群

```sh
pcs cluster stop --all
pcs cluster start --all
```

## 测试

```sh
pcs cluster stop node1
```

