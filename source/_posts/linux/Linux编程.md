# Linux编程



## 简单的模块

```c
// filename: ex01_simple_module.c

#include <linux/init.h>
#include <linux/module.h>

int ex01_simple_module_init(void) {
    printk(KERN_ALERT "Inside the function %s\n", __FUNCTION__);
    return 0;
}


void ex01_simple_module_exit(void) {
    printk(KERN_ALERT "Inside the function %s\n", __FUNCTION__);
}

module_init(ex01_simple_module_init);
module_exit(ex01_simple_module_exit);
```

```makefile
obj-m := ex01_simple_module.o
```



```sh
make -C /lib/modules/$(uname -r)/build M=$PWD modules
insmod ex01_simple_module.ko
lsmod
rmmod ex01_simple_module.ko

dmesg
```







