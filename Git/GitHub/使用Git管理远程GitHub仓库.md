 ## Git克隆远程GitHub仓库
```git
git clone 仓库地址
```
可以使用三种克隆方式，这三种克隆方式提供三种不同类型的仓库地址。
- HTTPS：任何人拿到链接后都能克隆此仓库
- SSH：通过公钥-密钥的方式安全的克隆仓库
- GitHub CLI：github提供的命令行工具
### HTTPS方式
HTTPS方式可以直接克隆GitHub仓库，但是默认情况下只能在本地进行修改，无法推送修改到远程仓库

如果要推送到远程仓库，会提示你输入github的账号和密码。但是这种方式已经被github弃用了。也就是说，即使你正确的输入了账号和密码。也会提示无法推送到远程仓库

正确的方法应该是，在你的github设置中生成一个有推送权限的token，每次推送时，输入你的github账号，密码填入这个token。
### SSH方式
[Github官方的教程](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
默认情况下，无法通过SSH方式克隆仓库。因为SSH必须在公钥和私钥配对成功的时候才会进行克隆。

这种克隆方式的好处是，克隆下来的仓库你拥有相应的权限。无需像https那样每次输入账号和密钥，只需要`git push`即可推送到对应的仓库

#### 首先，你需要在本机创建密钥和私钥：
```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```
老式机型可能需要:
```shell
 ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
 ```
将双引号内的内容换成你自己的邮箱,用这个邮箱来生成公钥和私钥

如果正确的话，会出现
```shell
> Generating public/private ALGORITHM key pair.
```
之后会依次弹出三个问题
```shell
> Enter a file in which to save the key (/c/Users/YOU/.ssh/id_ALGORITHM):
```
此问题要求你设置储存公钥和密钥的文件名，直接回车会使用默认名称
```shell
> Enter passphrase (empty for no passphrase): [Type a passphrase]
```
此问题要求你配置此密钥的密码，可以直接回车，设置为无密码。
```shell
> Enter same passphrase again: [Type passphrase again]
```
此问题要求你重复输入一次密码，如果为无密码则直接回车

> 密码会在你每次使用git修改远程仓库时询问。

 此时在`/root/.ssh/`目录下生成了两个文件
```SHELL
~/.SSH $ ls
ed25519 ed25519.pub
```
如果你在第一问时配置了自己的名称，则文件名是你自己的名称

无后缀的文件为私钥，`pub`后缀的文件是公钥

#### 将SSH密钥添加到SSH代理
```shell
$ eval "$(ssh-agent -s)"
```
如果正确的话,会返回`Agent pid xxxxx`
```shell
> Agent pid 59566
```
#### 在GitHub配置SSH公钥
1. 打开你的GitHub个人设置
2. 进入`SSH and GPG keys`设置项
3. 点击`New SSH key`
	- 在`Title`栏输入你密钥的别名，只是为了方便管理，不输入也无影响
	- `key type`默认即可
	- 在`key`栏输入文件`ed25519.pub`文件中的内容
此时你便可以正常的使用Git来正常的管理GitHub远程仓库了