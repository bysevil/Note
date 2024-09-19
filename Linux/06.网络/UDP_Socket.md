## 1.创建socket文件用来通信
```C
#include<sys/socket.h>
#include<sys/types.h>

int socket(int domain, int type, int protocol);
```
返回值：socket文件描述符
参数：
- domain: 指定（网络层）协议域，又称协议族。
	- AF_INET（IPv4协议）
	- AF_INET6（IPv6协议）
	- AF_LOCAL（或称AF_UNIX，Unix域socket）（用一个绝对路径名作为地址）
	- AF_ROUTE
- type: 指定socket类型。
	- SOCK_STREAM
	- SOCK_DGRAM
	- SOCK_RAW
	- SOCK_PACKET
	- SOCK_SEQPACKET等等
- protocol：指定传输层协议。
	- IPPROTO_TCP（TCP传输协议）
	- IPPTOTO_UDP（UDP传输协议）
	- IPPROTO_SCTP（STCP传输协议）
	- IPPROTO_TIPC等（TIPC传输协议）
## 2. 绑定本机地址


在接口上，统一使用`struct sockaddr`类型表示三种地址类型，在底层自行分析具体是那种套接字，通过其sin_fimaily成员分析其具体为哪种套接字。
（sockaddr_in）
创建地址信息对象并填写本机地址信息
```C
#include<netinet/in.h>

sockaddr_in sockaddr;
sockaddr.sin_fimily = AF_INET // 指定协议组，如IPv4协议
sockaddr.sin_port = htons(22) // 指定端口,如22
sockaddr.sin_addr.s_addr = inet_addr("127.0.0.1")//指定本机ip，如127.0.0.1
```
>htons能够将数据转换为网络字节序存储，避免大小端问题
>inetaddr能将形如`127.0.0.1`的点分十进制字符串描述的IP地址转换为4字节整形

绑定主机信息至Socket
```C
#include<sys/socket.h>
#include<sys/types.h>

int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```
返回值：-1绑定失败，0绑定成功
参数：
1. Socket文件描述符
2. 要绑定的本机地址信息对象
3. 本机地址信息对象大小
### 绑定的注意事项
1-1023端口是系统内定的端口，必须要由固定的应用层协议绑定，不允许平台用户绑定
如 http：80 443
 
1023以后的部分端口也由固定协议绑定如：mysql：3306

云服务器禁止直接绑定公网IP，而是需要绑定`0.0.0.0`。这称作任意地址绑定，此时绑定了主机所持有的所有ip，根据端口号向上解包。

客户端端需要绑定本机ip和端口，但是不需要程序员手动绑定。而是由OS在第一次发送信息时自动绑定。
客户端的port不重要，只要保证其在clinet端的唯一性即可
## 3. 读取数据报

```C
int recvfrom(int sockfd, void *buf, int len, unsigned int flags,struct sockaddr *from, int *fromlen);
```
返回值：接收到的数据长度
1. socket文件描述符
2. 用来接收有效信息的C风格字符串
3. 有效信息字符串大小
4. 0
5. 接收到的地址信息对象（发送方的地址信息）
6. 接收到的地址信息对象大小（发生方的地址信息对象大小）

>注意，最后一个参数不是输出型参数，而是要初始化一个值，接收时会判断接收到的socketaddr大小和初始化的值是否相同。相同时才会成功将socketaddr信息写入倒数第二个参数

## 4. 发送数据报
```C
int sendto(int sockfd, const void *msg, int len, unsigned int flags, const struct sockaddr *to, int tolen);
```
1. socket文件描述符
2. 要发送的有效数据的C风格字符串
3. 有效信息字符串大小
4. 0
5. 接收方的地址信息
6. 接收方的地址信息对象大小

>发送时，会将绑定的本机地址信息添加到数据报头，接收方能够解析

## ip地址的转换问题

使用`inet_addr`接口可以将字符串形式的ip地址转换为整型

使用`inet_ntoa`接口可以将整形ip地址转化回字符串

值得注意的是，inet_ntoa接口返回一个C风格字符串的首地址，这个字符串由inet_ntoa管理。这带来的问题是，如果再次调用这个接口，新的字符串会覆盖旧的字符串。那么通过之前的返回值查看字符串时会查看到新的字符串。

要解决这个问题，我们可以在每次调用时，不直接使用其返回值。而是调用结束后复制一份字符串自己进行管理。或者使用`inet_ntop`接口