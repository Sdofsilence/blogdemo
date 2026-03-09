# exit系列比较

## return 与 exit

* 一个是函数的结束，一个是进程的结束。（只有在main函数中效果一致，如果是其他函数中，return结束函数，exit结束进程）

## exit 与 _exit

* exit退出前进行清理，刷新缓冲区，结束文件描述符，关闭流，关闭tmpfile()创建的临时文件，调用atexit()注册的函数。// 1. fork() 后的子进程// 2. 紧急错误处理// 3. 信号处理程序
* _exit/_Exit不清理直接结束进程。
* C程序的终止分为两种：正常终止和异常终止。
  * 正常终止分为：return，exit，_exit，_Exit，pthreade_exit。
  * 异常中指分为：abort，SIGNAL，线程响应取消。