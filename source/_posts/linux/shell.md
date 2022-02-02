---
title: shell
---

# Shell

## 基础



### shell 变量

- **变量**

注意：等号两边不能有空格

```sh
var=shawn
echo $var
```

单引号：仅一般字符

双引号：可以保有变量内容

反引号：反引号的命令会被先执行，作为外部的输入信息

```sh
ls -l `locate crontab`
```

```sh
# 一样
version=$(uname -r)
version=`uname -r`
```

 字符串可以用单引号，也可以用双引号，也可以不用引号。

- 只读变量

```sh
myUrl="https://www.shawn.com"
readonly myUrl
```

- 删除变量

```sh
unset variable_name
```

- 获取字符串长度

```sh
string="abcd"
echo ${#string} #输出 4
```

- 提取子字符串

以下实例从字符串第 **2** 个字符开始截取 **4** 个字符：

```sh
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

### 变量字符串操作

- **${#var}**: 返回字符串var的长度

- **${var:m}**: 截取字符串，第m个字符串到最后

- **${var:\m:len}**: 截取字符串，第m个字符开始，长度为len

- **${var#patern}**: 删除var开头部分与pattern匹配的最小部分

- **${var##pattern}**: 删除var开头部分与pattern匹配的最大部分

- **${var%pattern}**: 删除var结尾部分与pattern匹配的最小部分

- **${var%%pattern}**: 删除var结尾部分与pattern匹配的最大部分

- **${var/old/new}**: 用new替换var中第一次出现old的地方

- **${var//old/new}**: 用new替换var中所有出现old的地方

- **${var/#old/new}**: 用new替换var中开头与old匹配的地方

- **${var//old/new}**: 用new替换var中结尾与old匹配的地方



- **${var:-word}**: var未定义且为空值时返回word，但不改变var
- **${var:=word}**:var未定义且为空值时返回word，且设置var为word
- **${var:?word}**:var未定义且为空值时，输出word，且终止脚本
- **${var:+word]}**:var存在且非空时，返回word






### 环境变量

```sh
env # 查看环境变量
set # 查看所有变量
```

```sh
# 系统环境变量
echo $HOME
echo $LANG
echo $SHELL
echo $HISTSIZE
echo $MAIL
echo $RANDOM
echo $PATH
echo $PS1
```



`export`



### 算术运算

>  原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。 

```sh
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"
```

 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2。

 乘号(*)前边必须加反斜杠(\)才能实现乘法运算。

### 关系运算

| 运算符 | 说明                                                  | 举例                         |
| :----- | :---------------------------------------------------- | :--------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | `[ $a -eq $b ]` 返回 false。 |
| -ne    | 检测两个数是否不相等，不相等返回 true。               | `[ $a -ne $b ] `返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | `[ $a -gt $b ]` 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | `[ $a -lt $b ]` 返回 true。  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | `[ $a -ge $b ]` 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | `[ $a -le $b ] `返回 true。  |

### 布尔运算

| 运算符 | 说明                                                | 举例                                       |
| :----- | :-------------------------------------------------- | :----------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 | `[ ! false ] `返回 true。                  |
| -o     | 或运算，有一个表达式为 true 则返回 true。           | `[ $a -lt 20 -o $b -gt 100 ]` 返回 true。  |
| -a     | 与运算，两个表达式都为 true 才返回 true。           | `[ $a -lt 20 -a $b -gt 100 ] `返回 false。 |

### 逻辑运算

| 运算符 | 说明       | 举例                                        |
| :----- | :--------- | :------------------------------------------ |
| &&     | 逻辑的 AND | `[[ $a -lt 100 && $b -gt 100 ]]` 返回 false |
| \|\|   | 逻辑的 OR  | `[[ $a -lt 100 || $b -gt 100 ]]` 返回 true  |

### 字符串运算

| 运算符 | 说明                                         | 举例                     |
| :----- | :------------------------------------------- | :----------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。      | [ $a = $b ] 返回 false。 |
| !=     | 检测两个字符串是否相等，不相等返回 true。    | [ $a != $b ] 返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。        | [ -z $a ] 返回 false。   |
| -n     | 检测字符串长度是否不为 0，不为 0 返回 true。 | [ -n "$a" ] 返回 true。  |
| $      | 检测字符串是否为空，不为空返回 true。        | [ $a ] 返回 true。       |

### 文件测试运算

| 操作符  | 说明                                                         | 举例                      |
| :------ | :----------------------------------------------------------- | :------------------------ |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              | [ -b $file ] 返回 false。 |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            | [ -g $file ] 返回 false。 |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  | [ -k $file ] 返回 false。 |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            | [ -u $file ] 返回 false。 |
| -r file | 检测文件是否可读，如果是，则返回 true。                      | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写，如果是，则返回 true。                      | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          | [ -e $file ] 返回 true。  |



### shell 引用外部脚本

```sh
. filename   # 注意点号(.)和文件名中间有一空格

或

source filename
```



### 两种判断方式

- test
- [ ]

### 函数

```sh
# 函数的返回值只能为数字
fun_name () {
    echo "hello"
    return `expr 1+1`
}

# 执行
fun_name
# 执行结果
echo $?
```

### seq与大括号表达式

| 语法      | 举例             | 说明                                             |
| --------- | ---------------- | ------------------------------------------------ |
| seq n     | seq 10           | 生成序列[1..n]，步长1                            |
| seq m n   | seq 0 9          | 生成序列[m..n]，步长1                            |
| seq m s n | seq 10 3 20      | 生成序列[m..n]，步长s                            |
| {m..n}    | {1..10}, {10..1} | 生成序列[m..n]，若m>n，步长为-1，若m<n，步长为-1 |
| {m..n..s} | {1..10..3}       | 生成序列[m..n]，若m>n，步长为s，若m<n，步长为s   |

> 大括号能生成字符序列，seq不能
>
> 大括号能生成逆序序列，seq不能

### for in

```sh
# for i in $(seq 10); do echo $i; done
for i in $(seq 10)
do 
	echo $i
done

# for i in $(cat README.txt); do echo $i; done
for i in $(cat README.txt)
do 
	echo $i 
done
```

###  while do

```sh
# while (( $i < 5 )); do echo $i; let "i++"; done
while (( $i < 5 ))
do 
	echo $i
	let "i++"
done
```

注意：

1. `$((2+1))` 连个圆括号可以做算术运算，也可用`$(expr 2 + 1)`,但这种方式比较严格，操作符两边必须要空格隔开。
2. `let "i++"：let也可以做算数运算



### case esac

```sh
read -p "Enter a number (between 1 - 4)" aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```



### 输入输出重定向

| 命令            | 说明                                               |
| :-------------- | :------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |
| 2>&1            | 将标准输出和标准错误输出合并                       |

注意：

2是标准错误输出的文件描述符（file descriptor）

1是标准输出的文件描述符



## BASH行操作

>  Bash 内置了 Readline 库，具有这个库提供的很多“行操作”功能，比如命令的自动补全，可以大大加快操作速度。
> 这个库默认采用 Emacs 快捷键，也可以改成 Vi 快捷键。

```sh
set -o vi
set -o emacs

# permanent
~/.inpuptrc
set editing-mode vi


# disable the readline
bash --noediting
```

### 光标移动

字符:C-f,C-b
词：M-f, M-b
行首位：C-a,C-e
行：C-p, C-n

### 撤销
C-/

### 查找历史命令
C-r

### 编辑
字符删除：C-d, C-h
单词删除：C-w, M-Backspace, M-d
行删除：C-u， M-k
位置转换：C-t, M-t
大小写转换：M-l, M-u
粘贴： C-y

## 进阶

### 判断文件是否存在

```sh
if [ -e /home/filename ]
then
   echo "file exist"
else 
   eccho "file not exist"
fi
```

### 判断文件是否包含字符串

```sh
if cat /home/test.txt | grep "keyword" > /dev/null
then
	echo "yes, /home/test.txt contains 'keyword'"
fi
```

### 获取PID

```sh
ps aux | grep -v grep | grep java | awk '{pring $2}'
#or
pgrep java
```

### 获取脚本目录

```sh
script_abs=$(readlink -f "$0")
script_dir=$(dirname $script_abs)
cd $script_dir
```

### redhat7 自动运行脚本

```sh
# 需要自动运行的脚本
chmod +x /home/scripts/startup.sh

chmod +x /etc/rc.d/rc.local
echo /home/sripts/startup.sh >> /etc/rc.d/rc.local
```

### 使一个脚本可以全局运行

```sh
# 需要全局运行的脚本
chmod +x /home/scripts/test.sh
ln -s /home/scripts/test.sh /usr/local/bin
```

### 自制systemd服务





### 修改环境变量

1. /etc/profile
2. （推荐）在/etc/profile.d路径下添加自己的脚本
3. ~/.bash_profile（只对 当前用户生效）

```sh
PATH=PATH:/usr/local/jdk
exprot PATH
```



```sh
# 使配置生效
. /etc/profile
```



### sed

sed -i 可以直接修改原文

```sh
sed [-nefri] [动作]
```



sed 修改换行符。sed是按行处理的，替换换行符需要一些高级技巧。

```sh
sed ":a;N;s/\n//g;ta" a.txt
```

 :a和ta是配套使用，实现跳转功能。t是test测试的意思。 

 N是把下一行加入到当前的hold space模式空间里，使之进行后续处理 。





mysqldump导出去掉DEFINER。

```sh
 mysqldump dbname --no-data --routines | sed -e 's/\/\*!50017 DEFINER=`hmcs_developing`@`%`\*\///g; s/DEFINER=`hmcs_developing`@`%`//g' > schema.sql
```



### awk

以行为单位的处理，将一行分成多个字段

```sh
awk [选项参数] 'script'
```



### cut



### 计算md5值

```sh
md5_value=$(echo -n "abc" | md5sum | cut -d ' ' -f1)
```



### find -exec {} \;

```sh
find . -type f -exec cmd {} \;
```

{}：占位符，表示前面find到的东西

\; 分隔符



###  ( echo "test"; exit 1; )

由于圆括号会开一个新的子shell，所以在圆括号中`exit`只是退出了子shell，并不会推出脚本执行

可以使用花括号

```sh
{ echo "test";exit 1;}
```

### && || ； &

shell脚本中的`if ... else...`语法不优雅，所以尽量避免使用

我们可以用 `&&`， `||`, `;`等来进行流程控制

### eval "ls"

将一个字符串转换成命令执行

使用场景：脚本参数对应脚本中的函数名称，可以直接使用 `eval "func"`，避免了冗长的`if ... else .,.`判断

### id -u username

判断一个用户是否存在

### 带颜色PS1

```sh
# 修改文件 ~/.bashrc
#默认的
PS1="[\u@\h \W]\$ "
#带颜色的
PS1="[\[\e[32m\]\u\[\e[m\]@\[\e[36m\]\h\[\e[31m\] \[\e[33m\]\W\[\e[0m\]]\[\e[32m\]\$\[\e[0m\] "
```

### \${var^^}, \${var,,},\ ${var%%*}, \${var##\*}

`^^`: 全大写

`^`: 首字母大写

`,,`:全小写

`,`：首字母小写



`${var%%,*}`:逗号到末尾全部删除， 两个`%%`表示贪婪匹配，一个`%`表示匹配到一个就行

`${var##*,}`:开头到逗号全部删除， 两个`##`表示贪婪匹配，一个`#`表示匹配到一个就行



`${#var}`:计算字符长长度

`${var/target/replcase}`:变量替换



### ASCII字符图标

banner
figlet
toilet

### w 查看当前在线用户



### shc - 将脚本分装成二进制可执行文件

shc主页：https://www.datsi.fi.upm.es/~frosal/

安装

```sh
cd /usr/local/src
wget http://www.datsi.fi.upm.es/%7Efrosal/sources/shc-3.8.7.tgz
tar vxf shc-3.8.7.tgz
cd shc-3.8.7
mkdir -p /usr/local/man/man1/
make test
make strings
make install
```



打包

```sh
shc -v -r -f mysql_backup.sh

# 会生成一下两个文件
# mysql_backup.sh.x 二进制可执行文件
# mysql_backup.sh.x.c c源码

# 查看文件类型
file mysql_backup.sh.x
```

注意：默认是打包成dynamic linked，所以只能在本机运行，在别的机器上运行不起来



打包成static linked

```sh
CFLAGS=-static shc -r -f mysql_backup.sh
```

注意：需要glibc-static的支持，使用` rpm -qa | grep glibc-static`查看是否有，没运行下面的命令手动安装

```sh
wget https://mirrors.163.com/centos/7/os/x86_64/Packages/glibc-static-2.17-307.el7.1.x86_64.rpm
rpm -ivh glibc-static-2.17-307.el7.1.x86_64.rpm
```



### 文件加密

1. tar配置openssl加密



2. zip加密



3. GPG加密



### base64

```sh
echo -e "Hello World" | base64
SGVsbG8gV29ybGQK
echo -e "SGVsbG8gV29ybGQK" | base64 -d
Hello World
```



原理说明：

Base64要求把每三个8Bit的字节转换为四个6Bit的字节（3*8 = 4*6 = 24），然后把6Bit再添两位高位0，组成四个8Bit的字节，也就是说，转换后的字符串理论上将要比原来的长1/3。

应用：

Base64编码可用于在HTTP环境下传递较长的标识信息。

### updatedb && locate， find

查找文件



### xargs 和 exec

```sh
ls *.txt | xargs -n1 -i{} mv {} {}_bak
```



-n1 : 一次一个

-i{} 把{}当作前面找到的对象的占位符



```sh
# 找到修改时间大于10天的并删除

find ./ -mtime +10 -exec rm -rf {} \;
find ./*txt -exec mv {} {}_bak \;
```

### mkfifo -- named pipe

```sh
mkfifo mynamedpipe
ls -l
ls > mynamedpipe

cat mynamedpipe # 在另一个终端
```



###  添加用户

```sh
useradd -m -U -d /var/hmcs -s /sbin/nologin Username
echo "password"| passwd Username --stdin
```



### 设置代理

```sh
export https_proxy="socks5://192.168.1.119:9999"
export http_proxy="socks5://192.168.1.119:9999"
```

### 检查smb配置

```sh
testparm
```

### 恢复文件权限

linux对文件权限控制的比较强，文件权限过高或过低都会导致某些应用不能正常启动或运行。

如果对某个目录的权限误操作了，如何快速恢复呢？Linux提供了getfacl和setfacl这两个命令，可以帮助我们快速恢复目录的权限。

首先需要正确权限的配置文件，可以从另一台服务器或者提前备份（通过getfacl命令）。

然后在你想要恢复的路径下执行 setfacl命令即可

```sh
# 文件权限正确的服务器
getfacl -R /var > perms.facl

# 文件权限错误的服务器
setfacl --restore=perms.facl
```

 

### getopts 处理命令行参数

syntax: getopts <optstring> name [arg]

optstring一串字母，每个字母代表一个选项。如果字母后面跟一个冒号，表示这个选项还应有个参数，没有冒号则不需要。

例一：

```sh
#!/bin/bash

usage()
{
  echo "Usage: $0 [-s|-r] [ -d DB_DUMP ] [ -f TARBALL ]"
  exit 2
}

set_variable()
{
  local varname=$1
  shift
  if [ -z "${!varname}" ]; then
    eval "$varname=\"$@\""
  else
    echo "Error: $varname already set"
    usage
  fi
}

#########################
# Main script starts here

unset DB_DUMP TARBALL ACTION

while getopts 'srd:f:?h' c
do
  case $c in
    s) set_variable ACTION SAVE ;;
    r) set_variable ACTION RESTORE ;;
    d) set_variable DB_DUMP $OPTARG ;;
    f) set_variable TARBALL $OPTARG ;;
    h|?) usage ;; esac
done

[ -z "$ACTION" ] && usage
[ -z "$DB_DUMP" ] && [ -z "$TARBALL" ] && usage

if [ -n "$DB_DUMP" ]; then
  case $ACTION in
    SAVE) save_database $DB_DUMP ;;
    RESTORE) restore_database $DB_DUMP ;;
  esac
fi

if [ -n "$TARBALL" ]; then
  case $ACTION in
    SAVE) save_files $TARBALL ;;
    RESTORE) restore_files $TARBALL ;;
  esac
fi
```

例二：

```sh
#!/bin/bash
unset VERBOSE
while getopts 'smv' c
do
  echo "Processing $c : OPTIND is $OPTIND"
  case $c in
    s) ACTION=sha1sum ;;
    m) ACTION=md5sum ;;
    v) VERBOSE=true ;;
  esac
done

echo "Out of the getopts loop. OPTIND is now $OPTIND"
shift $((OPTIND-1))
if [ $VERBOSE ]; then
  set -x
fi
$ACTION $@
```



### set 命令

```sh
# 使用未绑定的变量时报错，并退出
set -u

# 输出运行结果之前，先输出执行的命令
set -x

# 脚本只要发生错误，就终止执行
set -e
# 可以控制开启关闭
# set +e
# command1
# command2
# set -e

# 解决管道命令， 只要一个子命令失败了，就报错并退出
set -o pipeline

# 设置了set -e，会导致函数内部的错误不会被trap捕获, -E参数可纠正这个行为
set -E
```



### mktemp

创建临时文件

```sh
#!/bin/bash

trap 'rm -f "$TMPFILE"' EXIT

TMPFILE=$(mktemp) || exit 1
echo "Our temp file is $TMPFILE"
```



### trap

响应系统信号

trap -l 列出所有的系统信号

格式： trap [动作] [信号1] [信号2] ...





## 常见问题解决：

### syntax error:unexpected end of file

1. windows下编辑的文件换行符多一个`^M`
2. 花括号里面的语句结束一定要加一个分号

```sh
[ -e /home/test ] && { mkdir -p /home/test; echo "Test Success." }
```



