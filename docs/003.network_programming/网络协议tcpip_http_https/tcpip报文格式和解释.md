# tcp报文和相关概念

## tcp/ip/udp报文格式

**链路层数据包格式**

| 源mac地址 6 | 目标mac地址 6 | 数据帧类型 2 |  数据 | fcs校验和 4 | 链路层最小有效包长度为64，内容为64-18=46字节最小；但是ip头20-60字节，tcp头20字节,udp头8字节，所以会有填充满足64字节，这也是为什么会有nagle （delay）算法的原因。

**ip数据包格式**

| 版本 4 | 首部长度 4 | 服务类型 8 | 总长度 16 | \
| id 16 | 分片标志 3 | 片偏移 13 | \
| TTL 8 | 协议 8 | 校验和 16 | \
| 源端口 16 | 目标端口 16 | \
| options 0-40 |

由于首部长度为4字节（5-15），单位为4字节，所以ip头长度为20-60字节

**tcp数据包格式**

| 源端口 16 | 目标端口 16 | 序列号 32 | 确认号 32 | \
| 序列号seq 32 |\
| 确认号ack 32 |\
| 首部长度 4 | 保留 6 |  Urgent 1 | ACK 1 | PSH 1 | RST 1 | SYN 1 | FIN 1 |  窗口wind 16 | \
| 校验和 16 |  紧急指针 16 | \

由于首部长度为4字节（5-15），单位为4字节，所以tcp头长度为20-60字节

**udp数据包格式**
| 源端口 16 | 目标端口 16 | 总长度 16 | 校验和 16 |

由于总长度为16字节，所以最大的udp数据包大小为65535字节

**实际的包最大值依赖于链路层的MTU**比如以太网的MTU为1500字节，拨号上网 pppoe(ppp on erthnet) 的MTU为1492字节(ppp8的包头和尾部)\
所以局域网中的tcp包最大1500-20-20=1460字节，tcp最大为1500-20-8=1472字节\
而而internet推荐的MTU为576字节(所以udp编程中的udp数据包最大为576-8-20=548字节),\

## tcp 协议


* TCP协议详解之TCP Flag标志位来判断TCP会话的开始和结束<https://blog.csdn.net/answer3lin/article/details/84780514>
* TCP/IP 数据包报文格式（IP包、TCP报头、UDP报头）(转）<https://www.cnblogs.com/limanjihe/p/10134385.html>
* TCP协议特点及报文格式详解[https://blog.csdn.net/ling_zu_qi/article/details/139048263](https://blog.csdn.net/ling_zu_qi/article/details/139048263)

* 【网络协议】万文长篇，带你深入理解 TCP；场景复现，掌握鲜为人知的细节（上）<https://cloud.tencent.com/developer/article/2320329>
  * 概述；
  * 状态机；
  * 三握四挥；
  * 粘包拆包；（TCP 是基于流的，其实没这表述的）

* 【网络协议】万文长篇，带你深入理解 TCP；场景复现，掌握鲜为人知的细节（中）<https://cloud.tencent.com/developer/article/2320331?from_column=20421&from=20421>
  * 重传机制；
  * 滑动窗口；
  * 流量控制；
  * 拥塞控制；

* 【网络协议】万文长篇，带你深入理解 TCP；场景复现，掌握鲜为人知的细节（下）<https://cloud.tencent.com/developer/article/2320334?from_column=20421&from=20421>
  * 握手失败；
  * 挥手失败；
  * 为什么是三次握手？
  * 如何避免 SYN 攻击？
  * MTU 与 MSS 那些事儿；
  * TIME_WAIT 的巧妙设计；
  * 初始序列号 ISN 为什么不同？
  * 知道 TCP 的最大连接数吗？
