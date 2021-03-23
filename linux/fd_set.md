fd_set：一种固定大小的位图数组。在UNIX系统通常通过头文件<sys/select.h>中的常量FD_SETSIZE设置，其值通常是1024。

```c
typedef struct{
    long int fds_bits[32];
}fd_set;
```


```c
#include <sys/select.h>  
int FD_ZERO(int fd, fd_set *fdset);  // 将set清零
int FD_CLR(int fd, fd_set *fdset); // 将fd从set中清除
int FD_SET(int fd, fd_set *fd_set);  // 将fd加入set
int FD_ISSET(int fd, fd_set *fdset); // 检查fd是否在set中
```
