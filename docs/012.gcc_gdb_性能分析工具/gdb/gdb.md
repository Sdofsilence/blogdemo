# gdb调试器的使用


**gdb官网**：[Debugging with GDB](https://sourceware.org/gdb/current/onlinedocs/gdb.html/index.html#Top) 字典一样的全部内容可查

## 重要的是知道典型的使用场景（或者说调试的多种方式）

* [GNU GDB Debugger Command Cheat Sheet](https://www.yolinux.com/TUTORIALS/GDB-Commands.html) **适合快速学习**
* [gdb调试教程](https://geekdaxue.co/read/myheros@pkwqzc/pqq09f) **适合快速学习**
* 1.`gdb`
* 2.调试未启动的程序 `gdb -e program -c core_file`
* 3.调试运行中的程序 `gdb program -p <pid>`

下表罗列了一些在启动 GDB 调试器时常用的指令参数，以及它们各自的功能。

| 参 数                 | 功 能                                                          |
| --------------------- | -------------------------------------------------------------- |
| -pid number -p number | 调试进程 ID 为 number 的程序。                                 |
| -symbols file -s file | 仅从指定 file 文件中读取符号表。                               |
| -q -quiet -silent     | 取消启动 GDB 调试器时打印的介绍信息和版权信息                  |
| -cd directory         | 以 directory 作为启动 GDB 调试器的工作目录，而非当前所在目录。 |
| —args 参数1 参数2…  | 向可执行文件传递执行所需要的参数。                             |

除此之外，启动 GDB 调试器时还有其它参数指令可以使用，感兴趣的读者可查阅 [GDB 官网](https://sourceware.org/gdb/current/onlinedocs/gdb/Invoking-GDB.html#Invoking-GDB)做系统了解。

## 视频教程

* b站 码农论坛谈及了1. 基本调试 2. 调试core文件 3. 调试运行中的程序 4. 调试多进程 5. 调试多线程 6. 日志调试[https://b23.tv/v0w7Zkg](https://b23.tv/v0w7Zkg) （快速看）
* b站 [爱编程的大丙](https://b23.tv/iazlgp0) 上面视频中命令的补充（快速看）
