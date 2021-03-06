## 请求首部

### 客户端信息性首部

| 首　　部 |   描　　述 |
| -------------------------- | ----------------------------- |
| Client-IP |  客户端的IP地址 |
| From  |   客户端用户的E-mail地址 |
| Host   |  客户端请求（服务端）的主机名和端口号 |
| Referer | 用户是从这个页面发起的请求|
| User-Agent |  客户端发起请求的应用程序 |
| UA-Color  |   客户端显示器的显示颜色 |
| UA-CPU6 | 客户端CPU的类型或制造商 |
| UA-Disp | 客户端显示器（屏幕）能力 |
| UA-OS  |  客户端机器上的操作系统名称及版本 |
| UA-Pixels |   客户端显示器的像素信息 |

> referer : 当前请求URI的文档的URL 

### accept首部
| 首　　部 |   描　　述 |
| -------------------------- | ----------------------------- |
| Accept |  客户端可以接收的媒体类型 |
| Accept-Charset |  客户端可以接收的字符集 |
| Accept-Encoding | 客户端可以接收的编码方式，与响应端的Content-Encoding结合使用 |
| Accept-Language | 客户端可以接收的语言，ch,fr,en |
| TE7 | 客户端可以接收的扩展传输编码 |

### 条件请求首部
客户端期望请求满足条件的响应

| 首　　部 |  验证码 | 描　　述 |
| -------------- | ------------ | ----------------------------- |
| Expect  | - | 客户端通过 Expect 首部告知服务器需求某种行为 |
| If-Match |  ETag| 如果实体标记与文档当前的实体标记相匹配，就获取这份文档 |
| If-None-Match |  ETag | 如果提供的实体标记与当前文档的实体标记不相符，就获取文档 |
| If-Modified-Since | Last-Modified | 除非在某个指定的日期之后资源被修改过，否则就限制这个请求 |
| If-Unmodified-Since | Last-Modified | 除非在某个指定日期之后资源没有被修改过，否则就限制这个请求 |
| If-Range | - |允许对文档的某个范围进行条件请求 |
| Range |  - |如果服务器支持范围请求，就请求资源的指定范围 |


### 安全请求首部

HTTP支持简单的质询 / 响应机制，此机制要求客户端在获取特定的资源之前，先对自身进行认证。

| 首　　部 |   描　　述 |
| -------------------------- | ----------------------------- |
| Authorization   | 客户端提供给服务器的认证数据 |
| Cookie  | 客户端的令牌 |

承载用户相关信息的HTTP首部

| 首部 | 	描　　述 |
| -------------------------- | ----------------------------- |
| From	|用户的 E-mail 地址 |
| User-Agent| 用户的浏览器软件 |
| Referer | 用户是从这个页面上依照链接跳转过来的 |
| Authorization | 用户名和密码 |
| Client-IP| 客户端的 IP 地址 |
| X-Forwarded-For| 客户端的 IP 地址 |
| Cookie | 服务器产生的 ID 标签 |