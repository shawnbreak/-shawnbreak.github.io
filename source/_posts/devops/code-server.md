# Code-server



## 启动

```sh
code-server --bind-addr localhost:8088
```



## nginx反向代理



```nginx
http {
    
    
    location ^~/code-server/ {
        rewrite ^/code-server/(.*)$ $1 break;
        proxy_pass http://localhost:8088;
        proxy_set_header Host $host;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection upgrade;
        proxy_set_header Accept-Encoding gzip;
    }
    
}
```

