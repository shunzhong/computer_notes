请求URL

统一资源标识符(Uniform Resource Identiter，URI) : Web 服务器资源的唯一标志。URI 的形式包括： URL 和 URN。
统一资源定位符（URL）：描述一台特定服务器上某资源的特定位置。URL地址包含三部分内容：
方案，说明了访问资源所使用的协议类型。比如HTTP://
服务器的地址，比如：www.joes-hardware.com
资源路径，比如：/specials/saw-blade.gif

网络上的资源可以通过不同的方案（访问协议）进行访问，因此URL语法会随着方案的改变而不同。通用URL语法规则：

```
<scheme>://<user>:<password>@<host>:<port>/<path>;<params>?<query>#<frag>
```

统一资源名（URN）：指明特定内容的唯一名称（试验阶段）