---
title: cockpit
---
# cockpit

[toc]

[Running Cockpit — Cockpit Project (cockpit-project.org)](https://cockpit-project.org/running.html)

## Install

```sh
yum install cockpit -y
```

## Firewall

```sh
firewall-cmd --add-service=cockpit --permanent
firewall-cmd --reload
```



## Start

```sh
systemctl start cockpit.socket
```



## Nginx Proxy

vim /etc/cockpit/cockpit.conf

允许跨域，关闭ssl，设置根路径

```sh
[WebService]
Origins = https://vm2 https://vm2:9090 wss://vm wss://vm:9090
ProtocolHeader = X-Forwarded-Proto
AllowUnencrypted = true
UrlRoot = /cockpit0/
```



vim  vim /etc/systemd/system/cockpit.socket

因为关闭了ssl，所以为了安全，改成只监听本地端口

```sh
[Socket]
ListenStream=127.0.0.1:9090
```
修改配置后重启cockpit
```sh
systemctl daemon-reload
systemctl restart cockpit
```



修改nginx配置，可通过 https://ip/cockpi0/ 访问 cockpit

```nginx
location ^~/cockpit0/ {
    proxy_pass http://localhost:9090/cockpit0/;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header Host $host;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection upgrade;
    proxy_set_header Accept-Encoding gzip;
    proxy_http_version 1.1;
    proxy_buffering off;
    gzip off;
}
```

