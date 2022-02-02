# shellinabox



说明：通过web访问服务器命令行。



## Install

```sh
yum install shellinabox
```



## Start

```sh
systemctl start shellinaboxd.service
```



访问 https://ip:4200 登录使用

注意，默认不能直接使用root登录。



## Nginx Proxy

1. shellinabox启动参数，禁用ssl，方便nginx代理，改成只监听localhost

vim /etc/sysconfig/shellinabox

修改OPTS为如下， 加上 --localhost-only -t

```sh
OPTS="--disable-ssl-menu --localhost-only -t -s /:LOGIN"
```

2. nginx配置修改

```nginx
 location ^~/shellinabox/ {
        proxy_pass http://127.0.0.1:4200/;
        proxy_redirect default;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        client_max_body_size 10m;
        client_body_buffer_size 128k;

        proxy_connect_timeout 90;
        proxy_send_timeout 90;
        proxy_read_timeout 90;

        proxy_buffer_size 4k;
        proxy_buffers 4 32k;
        proxy_busy_buffers_size 64k;
        proxy_temp_file_write_size 64k;
    }
```

