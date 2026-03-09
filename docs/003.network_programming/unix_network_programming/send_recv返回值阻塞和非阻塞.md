# send recv返回值

## recv返回值

1. 阻塞模式下的 recv
返回 n (0 < n < len)：正常。接收缓冲区中有数据，但不足 len 个。函数立即返回当前可用的数据。

返回 0：对端正常关闭连接。

阻塞等待：如果接收缓冲区为空，调用会阻塞，直到有数据到达、对端关闭或发生错误。

2. 非阻塞模式下的 recv
返回 n (0 < n < len)：正常。与阻塞模式相同，返回当前可用的数据。

返回 0：对端正常关闭连接（与阻塞模式相同）。

返回 -1，errno == EAGAIN 或 EWOULDBLOCK：接收缓冲区为空。如果是在阻塞模式下，这会阻塞，但在非阻塞模式下，它立即返回这个错误，告诉你"现在没数据，别等着"。

非阻塞模式下的正确处理：
```
ssize_t n = recv(sockfd, buf, len, 0);
if (n > 0) {
    // 成功接收到数据（可能是部分数据）
    // 需要继续接收直到收满预期数量
} else if (n == 0) {
    // 对端关闭连接
    close(sockfd);
} else if (n < 0) {
    if (errno == EAGAIN || errno == EWOULDBLOCK) {
        // 非阻塞模式下的"正常"情况：没有数据可用
        // 应该返回，等待下次可读事件（如通过epoll）
        return 0; // 表示本次没有读到数据
    } else if (errno == EINTR) {
        // 被信号中断，可以重试
        // ... 
    } else {
        // 真正的错误
        perror("recv error");
        close(sockfd);
    }
}
```

## send返回值

1. 阻塞模式下的 send
返回 n (0 < n < len)：正常。发送缓冲区已满，只能容纳 n 个字节。函数会阻塞直到有空间容纳剩余数据。

返回 -1：真正的错误（如连接断开）。

2. 非阻塞模式下的 send
返回 n (0 < n < len)：正常。发送缓冲区空间不足，只成功写入了 n 个字节。

返回 -1，errno == EAGAIN 或 EWOULDBLOCK：发送缓冲区已满。这是非阻塞模式下的关键情况！如果是在阻塞模式下，这会阻塞，但在非阻塞模式下，它告诉你"现在发不了，别等着"。

非阻塞模式下的正确处理：
```
c
ssize_t n = send(sockfd, buf, len, 0);
if (n >= 0) {
    // 成功发送了n个字节（可能小于len）
    // 需要记录已发送数量，下次继续发送剩余数据
    return n;
} else if (n < 0) {
    if (errno == EAGAIN || errno == EWOULDBLOCK) {
        // 非阻塞模式下的"正常"情况：发送缓冲区满
        // 应该等待可写事件再继续发送
        return 0; // 表示本次没有发送任何数据
    } else if (errno == EINTR) {
        // 被信号中断，可以重试
        // ...
    } else {
        // 真正的错误（如连接断开）
        perror("send error");
        close(sockfd);
        return -1;
    }
}
```