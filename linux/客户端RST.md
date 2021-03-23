
## 客户端`RST`

构建一个客户端程序，并这设置SO_LINGER套接字选项，把l_onoff标志设置为1，把l_linger时间设置为0。此时连接被关闭时，TCP套接字上将会发送一个RST。


```c
struct linger ling;
ling.l_onoff = 1; 
ling.l_linger = 0;
setsockopt(socket_fd, SOL_SOCKET, SO_LINGER, &ling, sizeof(ling));
close(socket_fd);
```

服务器端使用select I/O多路复用，但监听套接字仍然是blocking的。如果监听套接字上有事件发生，休眠5秒，以便模拟高并发场景下的情形。

```c
if (FD_ISSET(listen_fd, &readset)) {
    printf("listening socket readable\n");
    sleep(5);
    struct sockaddr_storage ss;
    socklen_t slen = sizeof(ss);
    int fd = accept(listen_fd, (struct sockaddr *) &ss, &slen);
```
