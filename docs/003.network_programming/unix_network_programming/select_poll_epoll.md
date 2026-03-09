# unix网络编程 (io多路复用)

select poll epoll

## select and poll

* 爱编程的大丙 IO多路复用-[多线程网络并发通信 select/poll]
  * [https://www.bilibili.com/video/BV1fg411376j/](https://www.bilibili.com/video/BV1fg411376j/)

## epoll

* 爱编程的大丙 IO多路复用-[多线程网络并发通信 epoll]

  * [https://www.bilibili.com/video/av375913414](https://www.bilibili.com/video/av375913414)
* 里面说到的什么水平触发和边缘触发

  * 水平触发：只要fd可读，就会一直触发
  * 边缘触发：只触发一次（only once）（只在非阻塞模式下可用）每一次读取必须读取所有数据知道触发EAGAIN/EWOULDBLOCK(只在接收缓冲区从空变成可读时触发，而不是数据变化触发，例如2k数据，你读取了1k,又来了2k,此时缓冲区数据有3k,但是不会再次通知你)这种方式会导致饥饿，但是都说它的性能好。

## 博客

* linux select 与 阻塞( blocking ) 及非阻塞 (non blocking)实现io多路复用的示例[https://www.cnblogs.com/welhzh/p/4950341.html](https://www.cnblogs.com/welhzh/p/4950341.html)
  * select：[http://www.cnblogs.com/Anker/archive/2013/08/14/3258674.html](http://www.cnblogs.com/Anker/archive/2013/08/14/3258674.html)
  * poll：[http://www.cnblogs.com/Anker/archive/2013/08/15/3261006.html](http://www.cnblogs.com/Anker/archive/2013/08/15/3261006.html)
  * epoll：[http://www.cnblogs.com/Anker/archive/2013/08/17/3263780.html](http://www.cnblogs.com/Anker/archive/2013/08/17/3263780.html)

## select陷阱

select中的参数 : select(int nfds, fd_set *__restrict readfds, fd_set *__restrict writefds, fd_set *__restrict exceptfds, struct timeval *__restrict timeout)

其中后面四个参数都有可能会在函数执行过程中被修改，三个fds和timeout。

## epoll与多进程多线程如何设计？

* **书籍**相关：workslog_classify_by_category\6.multithreads_and_parallines\资源\大纲-面向模式的软件架构卷2：并发和联网对象模式.pdf，此目录下的其他书籍也是关于并发或网络相关的书籍。（**并发设计模式**）
* 6种Epoll的做法（epoll 封装、 reactor 作用与原理、多线程 多进程 epoll）| linuxC/C++服务器[](https://www.bilibili.com/video/BV1Pf4y1F7V8/) todo：这是一个视频的把他归纳成文档
* 自己的提问ai的回答：提问：基于epoll的io复用是不是只适合io密集型的场景，而多进程线程适合cpu密集和长时间的任务，如果我用一个连接类来管理连接的描述符和描述符相关的接收到的数据，我该如何使用多线程来联系它们，用多进程中多个epoll来处理io还是单进程处理io,然后使用多线程处理连接类合集中的已经接收的数据，这样有个问题，线程处理的可能遇到很多的数据竞争，例如连接关闭，线程和io对连接类的锁竞争；给出一个实际开源软件和商用软件的解决办法和典型epoll和多进程多线程分别负责和结合的实践方案

  你提出的问题非常核心，确实是构建高性能网络服务时会遇到的关键挑战。简单来说，你的思路基本是正确的：一个常见的高效架构确实是“单线程（或少量线程）处理IO，多线程处理业务逻辑”。这个架构的核心在于如何安全、高效地在IO线程和工作线程之间传递数据与连接状态，避免你提到的数据竞争。

  为了让你快速建立一个整体印象，下面这个表格梳理了两种典型架构模式的核心特点：

| 维度               | **Reactor 模式 (事件驱动)**                                                                                                         | **半同步/半异步模式**                                                                                      |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **核心思想** | IO线程（Reactor）负责所有事件监听与分发，工作线程（Handler）负责具体业务处理[](https://www.cnblogs.com/wzzkaifa/p/19139600#commentform)。 | 一个异步IO层（用于处理IO）与一个同步业务处理层（用于处理任务）相结合[](https://www.yisu.com/zixun/566392.html)。 |
| **IO处理**   | **单线程或多线程池** 使用epoll等IO多路复用技术[](https://www.cnblogs.com/wzzkaifa/p/19139600#commentform)。                         | **单线程或固定线程** 进行IO多路复用[](https://www.yisu.com/zixun/566392.html)。                            |
| **任务处理** | IO线程将就绪的事件（如接收到的数据）作为任务分发给**工作线程池**[](https://www.cnblogs.com/wzzkaifa/p/19139600#commentform)。       | IO线程接收请求，将任务放入队列，由**独立的工作线程池**同步处理[](https://www.yisu.com/zixun/566392.html)。 |
| **数据流**   | Reactor → 任务队列 → 工作线程池。                                                                                                       | 异步IO层 → 任务队列 → 同步业务层。                                                                             |
| **优点**     | 资源消耗相对少，延迟低，能够处理高并发连接[](https://cloud.tencent.cn/developer/article/1773769?from=15425)。                             | 职责分离清晰，便于将异步的IO操作与同步的业务逻辑解耦[](https://www.yisu.com/zixun/566392.html)。                 |
| **挑战**     | 需要精细地处理线程间任务分配与状态同步。                                                                                                  | 任务队列可能成为性能瓶颈，需要处理线程间通信。                                                                   |

**如何解决数据竞争与连接管理**

你担心的数据竞争和锁竞争是关键所在。幸运的是，已有一些成熟的开源项目和解决方案提供了最佳实践。

1. 开源实践：muduo网络库的“单IO线程驱动”模型[REF](https://chat.deepseek.com/share/59y5ie201k4nopwao7)

muduo是一个高质量的C++ Reactor网络库，它的设计哲学是 “one loop per thread” ，即每个事件循环（EventLoop）只属于一个线程，所有对某个socket的操作（如发送数据）都必须在其所属的IO线程中执行。

标准 "one loop per thread"：

* 一个监听fd，**只有一个线程**负责accept
* 新连接通过轮询/随机方式分配给其他线程
* 每个线程有自己独立的epoll实例

连接管理：每个连接（TcpConnection对象）都明确归属于一个特定的EventLoop（IO线程）。它的生命周期由IO线程管理。

避免锁竞争：当其他工作线程需要向这个连接发送数据时，它不会直接调用连接的send方法，而是通过调用EventLoop::queueInLoop()方法，将一个发送数据的“任务”异步排队到该连接所属的IO线程中。最终由IO线程来实际执行发送操作。这种方式避免了多线程同时操作同一个socket描述符及其缓冲区，从根本上解决了线程安全问题。

2. 商用级解决方案：惊群处理与负载均衡：见<惊群.md>

### 你的架构方案与优化建议

综合上面的分析，为你提供一个具体的架构方案，它融合了Reactor模式和半同步/半异步模式的优点：

1. **IO线程 (单例或少量)**
   * 由一个或少数几个线程专门负责使用epoll管理所有连接的IO事件（读、写）。
   * 当某个socket可读时，IO线程读取数据，然后将读取到的完整数据包和连接标识一起，封装成一个 **任务对象** 。
   * **关键步骤** ：在将任务交给工作线程之前，IO线程可以**暂时将该socket从epoll监听列表中摘下（使用 `epoll_ctl`的 `EPOLL_CTL_DEL`）**[](https://blog.csdn.net/weixin_47826078/article/details/125248481)。这样可以确保在工作线程处理数据期间，即使该连接上又有新数据到达，也不会被再次触发，从而避免了同一个连接被多个工作线程同时处理的混乱局面。
   * 工作线程处理完数据后，需要通知IO线程，由IO线程再将socket重新加回epoll监听（使用 `epoll_ctl`的 `EPOLL_CTL_ADD`或 `EPOLL_CTL_MOD`）[](https://blog.csdn.net/weixin_47826078/article/details/125248481)。
2. **工作线程池**
   * 由一个或多个线程组成，负责从**共享任务队列**中取出任务，执行具体的业务逻辑（如解析协议、计算、数据库查询等）。
   * 它们不直接操作任何socket，只处理数据。
3. **连接类管理**
   * 维护一个连接映射表（如 `std::unordered_map<int, ConnectionPtr>`）， key为socket fd，value为智能指针包裹的连接对象。
   * **锁的优化** ：不要用一个全局大锁锁住整个映射表。可以考虑使用 **读写锁** ，或者更优的方案是，使用多个锁分段（例如，用socket fd的hash取模）来减小锁的粒度。
   * 连接关闭操作：连接关闭应由IO线程发起。当IO线程检测到连接关闭（如读到 `EOF`）或错误时，它负责从epoll中移除监听，并从连接映射表中安全地清理掉该连接对象。

### ✅ 总结

* **是的，你的方向是对的** 。基于epoll的IO复用确实更擅长处理**IO密集型**的高并发场景，而将计算密集型或耗时长的任务剥离到独立线程池中，能有效避免阻塞IO循环[](https://www.ucloud.cn/yun/ask/76730.html)。
* **核心解决方案是“任务队列 + 异步操作”** 。通过IO线程与工作线程之间的任务队列进行解耦，并遵循“socket操作由其所属的IO线程执行”的原则，可以最大限度地减少锁竞争。
* **实际案例的智慧** ：muduo库的“队列到IO线程”机制[](https://blog.gitcode.com/2f65e568b43109ee848556b39d339d36.html)[](https://www.cnblogs.com/wzzkaifa/p/19139600#commentform)和Nginx的 `SO_REUSEPORT`方案[](https://blog.csdn.net/2302_81250321/article/details/143312444)，都是经过大规模实践检验的宝贵经验，你可以直接借鉴它们的思想。

希望这些分析和方案能帮助你构建出高性能且稳定的网络服务。如果你对特定环节（比如任务队列的具体实现）有更多疑问，我们可以继续深入探讨。

## select、poll、epoll支持的件描述符

* 面试复盘：文件描述符与 select、poll、epoll 的那些事儿[REF](https://juejin.cn/post/7484211071664308233)
* 所有可能的描述符类型：

  * 通信和网络：socket fd,pipe
  * 特殊的内核对象文件描述符（xxx-fd系列）：signalfd,timerfd,eventfd,inotifyfd,pidfd,epollfd,userfaultfd,io_uring
  * 其他：字符块设备，普通文件
