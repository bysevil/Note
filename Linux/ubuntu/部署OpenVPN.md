部分选项只做参考，可根据实际情况修改

## 获取本机外网IP地址

```shell
wget -qO - icanhazip.com
```

返回本机外网ip号

```shell
xxx.xxx.xxx.xxx
```

## 下载自动部署脚本

```shell
curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
```

## 运行自动部署脚本

授予部署权限

```shell
chmod +x openvpn-install.sh
```

运行自动部署脚本脚本

```shell
sudo bash openvpn-install.sh
```

开始安装

```shell
Welcome to the OpenVPN installer!
The git repository is available at: https://github.com/angristan/openvpn-install

I need to ask you a few questions before starting the setup.
You can leave the default options and just press enter if you are ok with them.

I need to know the IPv4 address of the network interface you want OpenVPN listening to.
Unless your server is behind NAT, it should be your public IPv4 address.
IP address: xxx.xxx.xxx.xxx

Checking for IPv6 connectivity...
```

### 1.是否授予客户端ipv6地址

```shell
Your host does not appear to have IPv6 connectivity.

Do you want to enable IPv6 support (NAT)? [y/n]: n
```

### 2.选择服务所使用的端口

```shell
What port do you want OpenVPN to listen to?
   1) Default: 1194
   2) Custom
   3) Random [49152-65535]
Port choice [1-3]: 1
```

### 3.选择连接模式

```shell
What protocol do you want OpenVPN to use?
UDP is faster. Unless it is not available, you shouldn't use TCP.
   1) UDP
   2) TCP
Protocol [1-2]: 1
```

### 4.选择DNS服务器地址

```shell
What DNS resolvers do you want to use with the VPN?
   1) Current system resolvers (from /etc/resolv.conf)
   2) Self-hosted DNS Resolver (Unbound)
   3) Cloudflare (Anycast: worldwide)
   4) Quad9 (Anycast: worldwide)
   5) Quad9 uncensored (Anycast: worldwide)
   6) FDN (France)
   7) DNS.WATCH (Germany)
   8) OpenDNS (Anycast: worldwide)
   9) Google (Anycast: worldwide)
   10) Yandex Basic (Russia)
   11) AdGuard DNS (Anycast: worldwide)
   12) NextDNS (Anycast: worldwide)
   13) Custom
DNS [1-12]: 9
```

### 5.是否使用压缩

```shell
Do you want to use compression? It is not recommended since the VORACLE attack makes use of it.
Enable compression? [y/n]: n
```

### 6.是否使用自定义加密

```shell
Do you want to customize encryption settings?
Unless you know what you're doing, you should stick with the default parameters provided by the script.
Note that whatever you choose, all the choices presented in the script are safe. (Unlike OpenVPN's defaults)
See https://github.com/angristan/openvpn-install#security-and-encryption to learn more.

Customize encryption settings? [y/n]: n
```

## 配置单密钥多设备登录

1.修改openvpn服务器端配置文件

```shell
cd /etc/openvpn/server.conf
```

2.添加字段

```shell
duplicate-cn
```

此字段允许多设备同时使用一个密钥连接;

3.重启openvpn，让修改后的配置文件生效

## 启动和关闭openvpn服务

关闭openvpn服务

```shell
pikll openvpn
```

启动openvpn服务

```shell
systemctl restart openvpn@server
```

## 删除openvpn和创建，删除用户

重新运行一下脚本即会提示4个选项

```
Welcome to OpenVPN-install!
The git repository is available at: https://github.com/angristan/openvpn-install

It looks like OpenVPN is already installed.

What do you want to do?
   1) Add a new user
   2) Revoke existing user
   3) Remove OpenVPN
   4) Exit
Select an option [1-4]: 
```

分别表示：

1. 添加新用户
2. 删除用户
3. 卸载Openvpn
4. 退出程序
