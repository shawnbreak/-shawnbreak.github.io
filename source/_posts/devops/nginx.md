# Nginx配置https和后端转发

[TOC]

## nginx.conf配置

```sh
sudo vim /etc/nginx/nginx.conf
```

在`http{}`块中追加一句

```sh
include /etc/nginx/heming.conf; # 我们的服务器配置写在这里面
```

## 使用openssl 生成证书

```sh
mkdir /etc/nginx/sslcert && cd /etc/nginx/sslcert # 创建并进入证书目录
openssl genrsa -des3 -out server.key 2048 # 私钥
openssl req -new -key server.key -out server.csr # 证书签名请求
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt # 自签名证书
```

## heming.conf配置（https）

```sh
sudo vim /etc/nginx/heming.conf
```

文件内容如下

```sh
# web前端
server {
    listen              8080 ssl;
    server_name         www.hm.com;
    root /var/heming/html; # 前端发布文件目录
    
    ssl_certificate     "/etc/nginx/sslcert/server.crt"; # 证书
    ssl_certificate_key "/etc/nginx/sslcert/server.key"; # 私钥
    ssl_session_timeout 10m;
    ssl_ciphers         HIGH:!aNULL:!MD5;
   	ssl_prefer_server_ciphers on;
   	
   	location / {
   	
   	}
}

# web后端转发
server {
    listen              8091 ssl;
    server_name         www.hm.com;
    
    ssl_certificate     "/etc/nginx/sslcert/server.crt"; # 证书
    ssl_certificate_key "/etc/nginx/sslcert/server.key"; # 私钥
    ssl_session_timeout 10m;
    ssl_ciphers         HIGH:!aNULL:!MD5;
   	ssl_prefer_server_ciphers on;
   	
   	location / {
   		proxy_pass http://localhost:8090;
   	}
}
```

注意：web后端流量有nginx的`8091`端口转发，所以前端配置文件 `/static/config.js`中的`hmcs_url`的访问端口需要改为`8091`端口才能正常访问。



由nginx转发web后端的理由：

1. web后端默认使用的是http访问，所以前端的https去异步请求web后端的时候浏览器汇报mixed_content错，即不允许https的页面去请求http的内容，因此后端也必须配置https。
2. 由nginx转发web后端的请求可以比较简单的实现web后端的https访问，不需要再去配置web后端。

## http自动跳转到https

rewrite

```sh
rewrite ^(.*)$	https://$host$1	permanent;
```

497

```sh
#让http请求重定向到https请求	
error_page 497	https://$host$uri?$args;
```

index.html刷新网页

## 解决413 Request Entity Too Large nginx/1.14.0

 超过了nginx的默认上传文件大小限制，默认1M 

在nginx.conf的http中添加以下配置：

```sh
client_body_buffer_size  128k;
client_max_body_size   16m;
```

