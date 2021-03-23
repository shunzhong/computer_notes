##### send() with MSG_ZEROCOPY

用户进程就能够把用户缓冲区的数据通过零拷贝的方式经过内核空间发送到网络套接字中去。

使用模式，首先给要发送数据的 socket 设置一个 SOCK_ZEROCOPY option，然后在调用 `send()` 发送数据时再设置一个 MSG_ZEROCOPY option。

```c
if (setsockopt(socket_fd, SOL_SOCKET, SO_ZEROCOPY, &one, sizeof(one)))
        error(1, errno, "setsockopt zerocopy");

ret = send(socket_fd, buffer, sizeof(buffer), MSG_ZEROCOPY);
```

即通过 `send()` 把数据在用户缓冲区中的分段指针发送到 socket 中去

首先利用 page pinning 页锁定机制锁住用户缓冲区的内存页，然后利用 DMA 直接在用户缓冲区通过内存地址指针进行数据读取

目前来说，这种技术的主要缺陷有：

1. 只适用于大文件 (10KB 左右) 的场景，小文件场景因为 page pinning 页锁定和等待缓冲区释放的通知消息这些机制，甚至可能比直接 CPU 拷贝更耗时；
2. 因为可能异步发送数据，需要额外调用 `poll()` 和 `recvmsg()` 系统调用等待 buffer 被释放的通知消息，增加代码复杂度，以及会导致多次用户态和内核态的上下文切换；
3. MSG_ZEROCOPY 目前只支持发送端，接收端暂不支持。
