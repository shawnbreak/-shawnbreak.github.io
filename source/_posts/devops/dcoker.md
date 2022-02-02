---
title: docker
---

# Docker

## 常用docker命令

```bash
常用docker命令

docker build -t web .
docker run -dp 80:80 web
docker stop <container-id>
docker rm <container-id>


docker volume create mydb
docker run -dp 80:80 -v mydb:/var/www/html web
docker volume inspect mydb
docker run -dp 80:80 -w /app -v "$(pwd):/app" web


docker network create mynet
docker run -d --network mynet --network-alias mysql -v mydb:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=todos mysql:8.0.23


docker run -dp 3000:3000 \
   -w /app -v "$(pwd):/app" \
   --network todo-app \
   -e MYSQL_HOST=mysql \
   -e MYSQL_USER=root \
   -e MYSQL_PASSWORD=secret \
   -e MYSQL_DB=todos \
   node:12-alpine \
   sh -c "yarn install && yarn run dev"
   
   
docker run -p 33061:3306 --name mysql -e MYSQL_ROOT_PASSWORD=ShanweIasd@zh0u19 -v d:/mysql/data:/var/lib/mysql -v d:/mysql/mysqlconf:/etc/mysql/conf.d -d mysql:8.0.23 --lower_case_table_names=1
```

## docker network

```bash
docker create network mynetwork
```

## docker volume

```bash
docker volume create myvolume
docker volume inspect myvolume
```



## 安装Docker

### Windows 10

1. 安装wsl2

https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-4---download-the-linux-kernel-update-package

要求：

- For x64 systems: **Version 1903** or higher, with **Build 18362** or higher.

```sh
# 1) 开启wsl
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 2) 开启虚拟机功能
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /disable-feature /featurename:VirtualMachinePlatform /all /norestart

# 3) 下载wsl镜像并安装

# 4) 设置默认wsl2
wsl --set-default-version 2
```



2. 安装docker for windows

https://docs.docker.com/docker-for-windows/



> docker启动时遇到报错： Docker Desktop : Failed to set version to docker-desktop: exit code: -1 
> 解决方法，执行如下命令后重启（https://github.com/docker/for-win/issues/9586）
>
> ```sh
> netsh winsock reset
> ```



- 镜像加速

windows 打开docker-engine配置，配置如下内容后，点击apply/restart

```json
{
  "registry-mirrors": [
    "https://hub-mirror.c.163.com",
    "https://mirror.baidubce.com"
  ]
}
```

运行docker info，查看registry mirrors，可看到当前配置



### Centos

- Docker官方

https://download.docker.com/linux/centos/7/x86_64/stable/Packages/

- 清华大学镜像

https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/7/x86_64/stable/Packages/



下载需要的rpm 包：

1)    containerd.io-1.4.3-3.1.el7.x86_64.rpm
2)    docker-ce-19.03.14-3.el7.x86_64.rpm
3)    docker-ce-cli-19.03.14-3.el7.x86_64.rpm



- 从文件安装

```sh
cd docker
rpm -ivh *.rpm --nodeps
```

- 从仓库安装

```sh
sudo yum install -y yum-utils

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    
sudo yum install docker-ce docker-ce-cli containerd.io
```



- 配置镜像加速

```sh
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://1nj0zren.mirror.aliyuncs.com",
        "https://docker.mirrors.ustc.edu.cn",
        "http://f1361db2.m.daocloud.io",
        "https://dockerhub.azk8s.cn"
    ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
docker info
```





## MySQL

```sh
docker pull mysql:8.0.23
docker run --rm -d --name some-mysql -p 33060:3306 -v ./database:/var/lib/mysql  -v C:\Users\Shawn\mysql_conf:/etc/mysql/conf.d mysql:8.0.23 --default-authentication-plugin=mysql_native_password
```

> 在windows上加上--default-authentication-plugin=mysql_native_password后，登录到数据库查看默认授权确实是mysql_native_password，但是root的授权插件还是caching_sha2_password， 需要手动更改
>
> ```sh
> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
> ```



## 删除none images

```bash
docker image prune
```

