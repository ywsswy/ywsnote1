linux内核编程使用C语言，但是跟常规C语言编程的区别在于：
C语言编程中通常包含 main 函数作为程序的入口，而Linux内核编程中不需要 main 函数；
C语言编程生成的是二进制程序文件；
linux内核编程生成的是模块，需要使用insmod来加载，使用rmmod来卸载；linux内核编程示例：
```
#include <linux/init.h>
#include <linux/module.h>

/* 模块初始化函数 */
static int __init my_module_init(void)
{
    printk(KERN_INFO "My module is loaded!\n");
    return 0;
}

/* 模块清理函数 */
static void __exit my_module_exit(void)
{
    printk(KERN_INFO "My module is unloaded!\n");
}

/* 注册初始化和清理函数 */
module_init(my_module_init);
module_exit(my_module_exit);

/* 模块许可证声明 */
MODULE_LICENSE("GPL");

```