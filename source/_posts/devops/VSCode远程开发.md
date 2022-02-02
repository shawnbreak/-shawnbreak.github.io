# VS Code 远程开发

[TOC]

## 配置步骤

1、先配置客户端

2、服务器端安装vs coe server

3、安装服务器端开发环境和插件



## 客户端

1、vscode最新版

2.、插件：

- remote ssh
- remote ssh ： edit configuration files

3、open ssh客户端（windows 10自带）



## 服务器端 vs code server 的安装

### 服务器有网络

服务器端有网络的情况下会自动去网上下载安装

### 服务器无网络

```sh
    commit_id=a0479759d6e9ea56afa657e454193f72aef85bd0
```

1. First get commit id
2. Download vscode server from url: `https://update.code.visualstudio.com/commit:${commit_id}/server-linux-x64/stable`
3. Upload the `vscode-server-linux-x64.tar.gz` to server
4. Unzip the downloaded `vscode-server-linux-x64.tar.gz` to `~/.vscode-server/bin/${commit_id}` without vscode-server-linux-x64 dir
5. Create `0` file under `~/.vscode-server/bin/${commit_id}`

ref: https://stackoverflow.com/questions/56671520/how-can-i-install-vscode-server-in-linux-offline

关于commit_id：

1.连接失败的返回结果中包含commit_id



## 无网络vscode插件安装

1、获取插件， 如：octref.vetur-0.27.1.vsix

2、按<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>p</kbd>，执行 install from vsix，然后选择插件路径



## 服务器端

1、open ssh服务器端（Linux自带）

2、插件：

	-  Java Language support for Java 0.64 (Redhat) (注意：自0.65起需要最低JAVA11的支持)
	-  Debugger For Java
	-  Vetur

3、开发环境：

- jdk 8

- node 14

  



