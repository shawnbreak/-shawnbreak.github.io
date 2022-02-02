## shell脚本加密



http://www.datsi.fi.upm.es/~frosal/

### 安装

```bash
wget http://www.datsi.fi.upm.es/~frosal/sources/shc-3.8.7.tgz
tar xvfz shc-3.8.7.tgz
cd shc-3.8.7
make
```

### 静态编译脚本

```bash
CFLAGS=-static shc -r -f test.sh
```

说明：

- CFLAGS=-static： 静态编译，生成的可执行文件可在别的设备上执行
- -r: 是生成的可执行文件可在别的系统上执行
- -f： 指定脚本

