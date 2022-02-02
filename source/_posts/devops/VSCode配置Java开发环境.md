---
title: vscode 配置java开发环境
---
# VSCode 配置Java开发环境



## 安装jdk8

自行下载安装。

配置环境变量。

### Linux

在 .bash_profile 里追加如下内容

```sh
JAVA_HOME=/opt/usr/jdkxxxx/
export JAVA_HOME
PATH=$PATH:$JAVA_HOME/bin
export PATH
```

运行 java -version 查看版本，查看成功则配置成功。

### windows

win + R 运行 sysdm.cpl， 弹出框选择高级=>环境变量。

在系统环境变量添加 JAVA_HOME

在Path里面添加 %JAVA_HOME%/bin

## maven安装配置

下载解压到安装路径。

配置环境变量。

### Linux

在 .bash_profile 里面追加如下内容

```sh
MAVEN_HOME=/opt/maven
export MAVEN_HOME
PATH=$PATH:$MAVEN_HOME
export $PATH
```

运行mvn --version查看maven版本，查看成功则配置成成功。

### Windwos

win + R 运行 sysdm.cpl，弹出框选择高级=>环境变量。

添加系统环境变量MAVEN_HOME。在Path追加%MAVEN_HOME%/bin



## 安装VSCode

自行下载安装

https://code.visualstudio.com/



## 插件安装

- Debugger for Java

- Language Support for Java(TM) by Red Hat

> 注意：Language Support for Java(TM) by Red Hat: 0.64及以下版本可以用jdk8， 0.64以上需要安装jdk11。
>
> 离线安装： Ctrl+Shift+p 调出执行命令框。输入Install  Extension from vsix 回车。选择需要安装的插件即可。



## 配置

### vscode 

打开settings.json

```json
{
    //"java.home": "/usr/java/jdk1.8.0_241-amd64",
    "java.requirements.JDK11Warning":false,
    "java.import.maven.enabled":true,
    "java.configuration.maven.userSettings": "/root/code/.m2/settings",
    "java.project.importOnFirstTimeStartup":"automatic"
}
```

### maven

打开.m2/settings.xml

```xml
<localRepository>/root/code/.m2/repositoryt</localRepository>

<offline>true</offline>
```

### debug

打开项目 .vscode/launche.json

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "java",
            "name": "Debug (Launch)-HmcsWebApplication<hmcs-web>",
            "request": "launch",
            "mainClass": "com.hm.hmcs.web.HmcsWebApplication",
            "projectName": "hmcs-web"
        }
    ]
}
```



## 调试

Ctrl + Shift + D调出调试栏，选择对应的debug条目启动即可。



## 打包

```sh
mvn clean package
```

