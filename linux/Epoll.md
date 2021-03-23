epoll于Linux 2.6内核中提出，与poll非常相似的一种I/O多路复用技术。epoll通过监控注册的多个描述字，来进行I/O事件的分发处理。不同于poll的是，epoll不仅提供了默认的level-triggered（条件触发）机制，还提供了性能更为强劲的edge-triggered（边缘触发）机制。

> LT模式：当epoll_wait检测到描述符事件发生并将此事件通知应用程序，==应用程序可以不立即处理该事件==。下次调用epoll_wait时，会再次响应应用程序并通知此事件。 缺省的工作方式，并且同时支持block和no-block socket。

> ET模式：当epoll_wait检测到描述符事件发生并将此事件通知应用程序，==应用程序必须立即处理该事件==。如果不处理，下次调用epoll_wait时，不会再次响应应用程序并通知此事件。只支持no-block socket。ET模式在很大程度上减少了epoll事件被重复触发的次数。


使用步骤如下：

1. 首先**epoll_create** 函数去创建一个epoll实例，epollfd

> epoll实例用来调用epoll_ctl和epoll_wait，如果这个epoll实例不再需要，则需要调用close()方法释放epoll实例，让内核可以回收epoll的资源。

2. 然后通过**epoll_ctl**将需要检测事件的 fd 与 epollfd 绑定

3. 最后调用 **epoll_wait** 检测事件



## 创建epollfd

```c
#include <sys/epoll.h>

int epoll_create(int size);
int epoll_create1(int flags);
```
- 返回值: 若成功返回一个大于0的值，表示epoll实例；若返回-1表示出错

- 参数 **size** 从 Linux 2.6.8 以后就不再使用，但是必须设置一个大于 0 的值。



## 管理epoll实例监控的事件 

```c
int epoll_ctl(int epfd, int op, int fd, struct epoll_event* event);
```
**返回值** 成功 0，调用失败返回 -1，然后通过 **errno** 错误码获取具体的错误原因。

- 参数 **epfd** 使用epoll_create创建的epoll实例， epoll句柄；

- 参数 **op**，操作类型表示新增、删除、修改监控事件，
	EPOLL_CTL_ADD： 向epoll实例注册文件描述符对应的事件；
	
	EPOLL_CTL_DEL：向epoll实例删除文件描述符对应的事件；
	
	EPOLL_CTL_MOD： 修改文件描述符对应的事件。

- 参数 **fd**，需要被操作的 fd，比如socket套接字；

- 参数 **event**，表示的是注册的事件类型。

**epoll_event** 结构体的地址，**epoll_event** 结构体定义如下：

```c
  struct epoll_event
  {
      uint32_t     events;      /* 需要检测的 fd 事件，取值与 poll 函数一样 */
      epoll_data_t data;        /* 用户自定义数据 */
  };

  typedef union epoll_data {
     void        *ptr;
     int          fd;
     uint32_t     u32;
     uint64_t     u64;
 } epoll_data_t;
```



## 检测事件

创建了 epollfd，设置好某个 fd 上需要检测事件并将该 fd 绑定到 epollfd 上去后，就可以调用 **epoll_wait** 检测事件

```c
int epoll_wait(int epfd, struct epoll_event* events, int maxevents, int timeout);
```

返回值: 成功返回的是一个大于0的数，表示事件的个数；返回0表示的是超时时间到；若出错返回-1.

epfd: epoll 实例描述字，epoll句柄

events: 数组，返回给用户空间需要处理的I/O事件，数组中的每个元素都是一个需要待处理的I/O事件，

maxevents: 表示epoll_wait可以返回的最大事件值。

timeout: epoll_wait阻塞调用的超时值，如果这个值设置为-1，表示不超时；如果设置为0则立即返回，即使没有任何I/O事件发生。



## Epoll 的优点

1. 事件集合，
	- 在每次使用poll或select之前，都需要准备一个感兴趣的事件集合，系统内核拿到事件集合，进行分析并在内核空间构建相应的数据结构来完成对事件集合的注册。
	- epoll则不是这样，epoll维护了一个全局的事件集合，通过epoll句柄，可以操纵这个事件集合，增加、删除或修改这个事件集合里的某个元素。
> 绝大多数情况下，事件集合的变化没有那么的大因此不需要每次重新扫描事件集合，构建内核空间数据结构。

2. 就绪列表
	- 每次在使用poll或者select之后，应用程序都需要扫描整个感兴趣的事件集合，从中找出真正活动的事件。
	- epoll则不是这样，epoll返回的直接就是活动的事件列表，应用程序减少了大量的扫描时间。
> 事实上，每次少扫描只能找到几个真正活动的事件。

3. 内存拷贝，利用mmap()文件映射内存加速与内核空间的消息传递；即epoll使用mmap减少复制开销。

>启用或关闭epoll
> lib/event\_loop.c文件的event\_loop\_init\_with\_name函数是关键，可以看到，这里是通过宏EPOLL\_ENABLE来决定是使用epoll还是poll的。


[[epoll的实现流程]]

[[epoll_server]]