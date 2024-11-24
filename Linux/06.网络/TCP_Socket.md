```C
#include <sys/socket.h>
int listen(int sockfd, int backlog);
```
将socket文件设置为监听

等待客户端建立连接
```C
include <sys/socket.h>

int accept(int sockfd, struct sockaddr *_Nullable restrict addr,socklen_t *_Nullable restrict addrlen);
```
1.监听socket文件描述符
2.存入客户端socket信息的socket变量地址
3.存入客户端socket大小的变量地址
返回客户端socket文件描述符

```c
#include <sys/socket.h>

int connect(int sockfd, const struct sockaddr *addr,socklen_t addrlen);
```
客户端与服务器建立连接，连接时自动绑定

直接对socket文件流进行读写就能实现TCP通信
，如果read时，read函数返回值为0，则对方主动断开连接，小于0则对方非正常断开连接，大于0正常通信

## 写失败处理

当服务器向客户端发消息时，如果客户端关闭，则可能回写入失败。此时和管道通信相似，操作系统会向进程发送SIGPIPE信号，导致进程关闭。

服务器显然不能因为一次发送失败就关闭。所以我们需要对SIGPIPE信号进行处理

```C
//忽略SIGPIPE信号
signal(SIGPIPE,SIG_IGN)
```