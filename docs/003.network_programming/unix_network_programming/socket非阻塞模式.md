# socket 设置非阻塞模式

## io模型

* posix标准描述了五种io模型
  * 阻塞I/O
  * 非阻塞I/O
  * IO多路复用
  * 信号驱动IO
  * 异步IO
* 详细介绍见: posix五种io模型.md

## 博客

* linux select 与 阻塞( blocking ) 及非阻塞 (non blocking)实现io多路复用的示例[https://www.cnblogs.com/welhzh/p/4950341.html](https://www.cnblogs.com/welhzh/p/4950341.html)
  * select：<http://www.cnblogs.com/Anker/archive/2013/08/14/3258674.html>
  * poll：<http://www.cnblogs.com/Anker/archive/2013/08/15/3261006.html>
  * epoll：<http://www.cnblogs.com/Anker/archive/2013/08/17/3263780.html>
