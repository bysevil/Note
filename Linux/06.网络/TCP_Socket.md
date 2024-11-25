## 相比UDP

TCP的特性：
1.面向连接
2.面向字节流
3.稳定连接

TCP和UDP都是socket通信，都需要先创建一个数据报：

对于TCP服务器而言，我们需要将socket设置为监听，通过监听来获取客户端连接：
```C
#include <sys/socket.h>
int listen(int sockfd, int backlog);
```
将socket文件设置为监听

开始等待客户端建立连接
```C
include <sys/socket.h>

int accept(int sockfd, struct sockaddr *_Nullable restrict addr,socklen_t *_Nullable restrict addrlen);
```
1.监听socket文件描述符
2.存入客户端socket信息的socket变量地址
3.存入客户端socket大小的变量地址
返回客户端socket文件描述符

对于客户端而言，我们需要连接指定的服务器
```c
#include <sys/socket.h>

int connect(int sockfd, const struct sockaddr *addr,socklen_t addrlen);
```
1.自身绑定的socket文件描述符
2.要连接的服务器的sockaddr
3.要连接的服务器的socketaddr大小
客户端与服务器建立连接，连接时自动绑定


直接对socket文件流进行读写就能实现TCP通信
> 客户端对自身绑定的socket读写，服务器对获取到的客户端socket进行读写

>如果read时，read函数返回值为0，则对方主动断开连接，小于0则对方非正常断开连接，大于0正常通信

### 写失败处理

当服务器向客户端发消息时，如果客户端关闭，则可能回写入失败。此时和管道通信相似，操作系统会向进程发送SIGPIPE信号，导致进程关闭。

服务器显然不能因为一次发送失败就关闭。所以我们需要对SIGPIPE信号进行处理

```C
//忽略SIGPIPE信号
signal(SIGPIPE,SIG_IGN)
```

## 守护进程

### 前后台任务

一个会话只能有一个前台任务，可以有多个后台任务，键盘信号只能发给前台任务。
>区分前后台任务：前台任务持有标准输入，后台任务无法持有标准输入流。

Ctrl+Z将暂停前台任务并移入后台(向进程发送20号(SIGTSTP)信号)

当后台任务放入前台
```shell
fg 任务号
```

查看当前会话后台任务
```shell
jobs
```
jobs返回如下形式
```C
任务号   运行状态   进程名
[1]+     Running    ping -c 3 google.com &
```
>运行状态有两种，Running运行中，Stopping暂停中

使得后台暂停进程开始在后台开始运行
```shell
bg 任务号
```

### 理清关系

进程组是一组进程，PGID为其组ID。
一般来说，同一个程序多次加载到内存后形成的多个进程属于同一个进程组。

任务可以指派给一个或多个进程组，进程组分配给进程执行。

同一个会话(session)内启动的任务SID相同，每个终端的开始和关闭就是一次会话

## 进程守护

当终端退出时，会话结束，其所有子进程退出。也就是通过终端启动的所有进程都会变为孤儿进程被操作系统领养；

我们希望有一个方法，在终端关闭后任务继续运行。这就是守护进程的作用。

守护进程可以使得进程不受任何用户登录和注销的影响。其原理就是使得任务相关的进程自成一组并自成会话。

```C++
#include <unistd.h>
pid_t setsid();
```
此函数实现了：
1. 创建一个新会话
2. 将当前进程移入新会话成为首进程
3. 创建一个进程组
4. 将当前进程设置为进程组组长

>调用此函数的进程不可以是进程组的组长
>成功时返回会话ID，同时也是会话首进程的ID，也就是当前进程的新ID
>失败时返回-1，并设置错误码




