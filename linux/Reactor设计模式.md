## Reactor设计模式

事件驱动模型，也被叫做反应堆模型（reactor），或者是Event loop模型。

第一，存在一个无限循环的事件分发线程，或者叫做reactor线程、Event loop线程。这个事件分发线程背后是poll、epoll等I/O。

第二，所有的I/O操作都抽象成事件，且每个事件必须有回调函数来处理。比如acceptor上有连接建立成功、已连接套接字上发送缓冲区空出可以写、通信管道pipe上有数据可以读。

![](assets/6dcffeb082a412a31c61531a299ce933.png)


[[单一Reactor模式]]

[[主从Reactor模式]]

