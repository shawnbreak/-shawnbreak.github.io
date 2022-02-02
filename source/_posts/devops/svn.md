# svn 

[TOC]

## 基本使用

### svn import

把现有的项目导入到仓库中

```sh
svn import /myproject https://svn.example.com/svn/repo/project1
```

### svn list

查看仓库文件

```sh
svn list https://svn.example.com/svn/project1
```

### svn checkout

创建一个工作副本

```sh
svn checkout https://svn.example.com/svn/project1/trunk
```

### 基本工作流

#### svn update

拉取项目的最新更行

#### svn add, svn delete, svn copy, svn move

项目编辑，更改等操作

#### svn status, svn diff

查看更改内容

```sh
$ svn status
? scratch.c
A stuff/loot
A stuff/loot/new.c
D stuff/old.c
M bar.c
```

?  item

​	没有加入版本控制的文件

A item

​	加如到仓库的文件，svn add的结果， svn commit后才生效

C item

​	冲突

D item

​	从仓库删除的文件， svn delete的结果，svn commit才生效

M item

​	被改变的文件

#### svn revert

回退错误的更改

```sh
$ svn status README
M README
$ svn revert README
Reverted 'README'
$ svn status README
$
```

#### svn update, svn resolve

#### svn commit



## 分支

### 创建分支

```sh
$ svn copy http://svn.example.com/repos/calc/trunk \
http://svn.example.com/repos/calc/branches/my-calc-branch \
-m "Creating a private branch of /calc/trunk."
```

### 在分支上工作

```sh
svn checkout http://svn.example.com/repos/calc/branches/my-calc-branch
```

### 保持分支同步: 

将主干的修改同步到分支

```sh
$ pwd
/home/user/my-calc-branch
$ svn merge ^/calc/trunk
--- Merging r345 through r356 into '.':
U button.c
U integer.c
--- Recording mergeinfo for merge of r345 through r356 into '.':
U .
$
```

### 将分支修改合并到主干

```sh
$ pwd
/home/user/calc-trunk
$ svn update # (make sure the working copy is up to date)
Updating '.':
At revision 390.
$ svn merge --reintegrate ^/calc/branches/my-calc-branch
--- Merging differences between repository URLs into '.':
U button.c
U integer.c
U Makefile
--- Recording mergeinfo for merge between repository URLs into '.':
U .




$ # build, test, verify, ...
$ svn commit -m "Merge my-calc-branch back into trunk!"
Sending .
Sending button.c
Sending integer.c
Sending Makefile
Transmitting file data ..
Committed revision 391.



$ svn delete ^/calc/branches/my-calc-branch \
-m "Remove my-calc-branch, reintegrated with trunk in r391."
Committed revision 392.
```





## svn仓库管理

### svn 仓库构成

```sh
$ ls repos
conf/	db/  format/  hooks/  locks/  README.txt	
```

conf/

​		各种配置文件

db/

​		版本控制的文件

format

​		描述仓库的结构

hooks/

​		钩子脚本

locks/

​		管理被锁的文件，访问控制

README.txt

​		简单说明

### 推荐的项目结构

/

​	project/

​			trunk/ (主要开发)

​			tags/ （快照，从不改变的）

​			branches/ (主要开发的分支)

### 数据存储

- Berkeley DB
- FSFS（推荐）

### 创建和配置仓库

#### 创建仓库

```sh
svnadmin create /var/svn/repos
```

#### 实现钩子

pre-hooks

post-hooks

推荐实现方式：从现有模板copy一份修改

> 注意：钩子的执行环境没有任何环境变量，所以都需要手动指定

### 仓库维护

#### svnadmin

最基础的管理工具，包括仓库的创建等

#### svnlook

查看仓库的不同版本和事务，不会修改仓库

#### svndumpfilter

仓库清理工具

```sh
svndumpfilter exclude
svndumpfilter include
```



#### svnrdump



#### svnsync

同步一个只读的镜像仓库

### 仓库配置

#### svnserve

最快，最简单

安全性不高，如果只是在局域网内使用够了

```sh
// 服务端开始svn进程
svnserv -d -r /var/svn
```

```sh
// 客户端使用
svn checkout svn://host.example.com/project1
```





#### svnserve over ssh



#### The apache http server

有https加密，有apach日志系统

大项目，需要连接到互联网用这个



前提：httpd, mode_dav_svn



