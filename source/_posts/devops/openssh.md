# OpenSSH

## ssh

```sh
ssh -i ~/.ssh/id_rsa root@host
```

## sshd



## ssh_config



## sshd_config



## ssh-agent

 OpenSSH authentication agent 

windows

```sh
Set-Service -Name ssh-agent -StartupType Manual
Start-Service ssh-agent
```

用途: 

1.不需要用 -i 指定密钥，ssh-agent会帮我们选择

2.设置了pass phrase的话，不用每次都输入pass phrase, ssh-agent会帮我们记住



## ssh-add

 adds private key identities to the OpenSSH authentication agent 

```sh
ssh-add ~/.ssh/id_rsa # 添加
ssh-add -l # 查看 
ssh-add -d ~/.ssh/id_rsa # 删除
```



## sftp

```sh
sftp root@localhost

get -r folder
put -r folder
```



## scp

```sh
scp -r /home/administrator/news.txt root@192.168.6.129:/etc/squid
```



## ssh-keygen

```sh
ssh-keygen # 默认配置,按提示输入信息
ssh-keygen -t rsa -b 1024 -f filename -P passphrase -C "comment"
```



安装公钥

```sh
cat id_rsa.pub >> ~/.ssh/authorized_keys
```



## ssh-keyscan

 Utility for gathering public host keys from a number of hosts 

```sh
ssh-keyscan hostlist
ssh-keyscan -t rsa hostlist
```



## sftp-server



## ssh-keysign



## 免密登录

1.生成密钥对(在自己的电脑上)

```sh
ssh-keygen
```

2.添加到`~/.ssh/config`

```sh
Host *
	
Host aliubuntu
    HostName 39.108.88.198
    Port 22
    User root
    IdentityFile ~/.ssh/id_rsa
```

3.将公钥写到服务器~/.ssh/authorized_keys

4.服务器端设置sshd

/etc/ssh/sshd_config

```sh
# 取消注释下面这行
AuthorizedKeysFile      .ssh/authorized_keys .ssh/authorized_keys2
```

```sh
systemctl restart sshd
```

5. 现在可在自己的电脑上免密登录

   ```sh
   ssh aliubuntu
   sftp aliubuntu
   scp test.txt aliubuntu:~/
   ```




## ssh-copy-id

solution 1

```sh
choco install ssh-copy-id -y
ssh-copy-id -i .ssh/id_rsa.pub root@host
```

solution 2

```sh
type .ssh/id_rsa.pub | ssh root@host "cat >> .ssh/authorized_keys"
```

## 端口转发

### 动态转发

要访问的目标服务器不确定，端口也不确定。

```sh
ssh -D 2121 tunnel-host -N
# -D 动态转发
# -N 只转发，不登录shell
```

采用socks5协议。

建立本地与ssh服务器的加密连接，将本地特定端口的访问都转发到ssh服务器。可以当作简单的代理服务器。

使用案例：

```sh
$ curl -x socks5://localhost:2121 http://www.example.com
```



### 本地转发

目标服务器的端口确定。

在本地执行转发命令。

```sh
 ssh -L 2121:www.example.com:80 tunnel-host -N
 curl http://localhost:2121
```

访问本地2121端口跟访问www.example.com 80端口效果一样。



### 远程转发

目标服务器的端口确定。

在远程（跳板机）执行转发命令。

```sh
# 再跳板机执行
ssh -R 2121:www.example.com:80 local -N
# 在本地执行
curl http://localhost:2121
```

访问本地2121端口跟访问www.example.com 80端口效果一样。



