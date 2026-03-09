# unix fork函数解释

* 只需要记住一点就可以了，fork函数返回两次，一次是在父进程，一次是在子进程。
* 而为什么会这样，fork() 复制了整个进程的上下文，包括程序计数器（PC）和堆栈指针（SP）。
  * 程序计数器（PC）：指向 fork() 调用后的下一条指令
  * 堆栈指针（SP）：指向当前的堆栈位置
  * 所有寄存器状态
  * 整个用户空间内存（代码段、数据段、堆段、堆栈段）
  * 其他资源（如文件描述符等，会导致数据竞争和错误）
* 不会复制
  1. 进程ID（PID）
  2. 父进程ID（PPID）  
  3. 运行时间统计
  4. 挂起的信号
  5. 文件锁（特殊情况）

## fork() 和 vfork() + exec()

* fork() 创建一个新进程，并复制当前进程的内存。
* vfork() 创建一个新进程，但是不复制当前进程的内存，且在exec(),_exit()之前，父进程会被阻塞，等待子进程执行，在此之前不能直接return或改变父进程的变量。

## exec() 系列函数

* exec 系列函数提供了一种在进程中启动另一个程序执行的方法。它们的主要功能是用新程序替换当前进程的用户空间代码和数据，从新程序的启动例程开始执行。调用 exec 并不创建新进程，所以调用 exec 前后该进程的 ID 并未改变。

* exec 系列函数的分类
  * exec 系列函数共有六种，分别是 execl、execlp、execle、execv、execvp 和 execve。这些函数可以分为两组：
  * execl 系列：包括 execl、execle 和 execlp。这些函数需要将每个命令行参数作为函数的参数进行传递。
  * execv 系列：包括 execv、execve 和 execvp。这些函数将所有参数包装到一个矢量数组中传递。

* exec 系列函数的命名规则如下：
  * l：表示参数以列表形式传递。
  * v：表示参数以矢量数组形式传递。
  * p：表示执行文件部分可以不带路径，函数会在 $PATH 中查找。
  * e：表示可以传递环境变量。

* exec 系列函数的参数 exec 系列函数的参数可以分为三部分：
  * 执行文件部分：要执行的程序路径，可以是绝对路径或相对路径。
  * 命令参数部分：命令行参数列表，最后一个参数必须是 NULL。
  * 环境变量部分：这是一个数组，最后的元素必须是 NULL。

* 例如，execl 函数的原型如下：

```c/c++
int execl(const char *path, const char *arg, ...);
```

* 调用示例：

```c/c++
#include <stdio.h>
#include <unistd.h>

int main() {
execl("/bin/ls", "ls", "-l", NULL);
return 0;
}
```

* exec 系列函数的返回值 : 如果 exec 系列函数执行成功，它们不会返回；如果执行失败，它们会返回 -1，并且进程继续执行后面的代码。

* 以下是 execl 和 execv 的代码示例：

* execl 示例

```c/c++
#include <stdio.h>
#include <unistd.h>

int main() {
execl("/bin/ls", "ls", "-l", NULL);
return 0;
}
```

* execv 示例

```c/c++
#include <stdio.h>
#include <unistd.h>

int main() {
char *argv[] = {"ls", "-l", NULL};
execv("/bin/ls", argv);
return 0;
}
```

* 通过这些示例，可以看到 exec 系列函数如何用新程序替换当前进程，并执行新程序的代码
