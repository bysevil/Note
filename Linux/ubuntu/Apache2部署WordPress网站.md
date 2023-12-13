## 安装相关软件

1.更新储存库

```shell
sudo apt-get update
```

2.安装MySQL

```powershell
apt-get install mysql-server
```

> 安装时会询问root用户的密码

3.安装Apache2

```shell
apt-get install apache2
```

4.安装PHP

```shell
apt-get install php
```
5.安装php连接mysql的插件
```
apt-get -y install php-fpm php-mysql
```

6.由于Apache2 可能不能正常解析PHP，因此需要安装PHP针对于apache的模块

```shell
apt-get install php libapache2-mod-php
```

此时软件均安装完成

7.购买一个域名并解析到服务器（可选）

> 如果不购买域名，也可以通过主机ip地址连接到网站

## 更改Apache的默认根目录(可选)

1.创建这个目录

```shell
sudo mkdir -p /var/www/网站域名
```

> 建议使用网站域名做网站的根目录名称，如
>
> ```shell
> sudo mkdir -p /var/www/baidu.com
> ```

2.授予相关权限

```shell
sudo chown -R $USER:$USER /var/www/网站域名
sudo chmod -R 755 /var/www/网站域名
```

3.创建一个索引页面以确定是否成功

```shell
vi /var/www/网站域名/index.html
```

> - index.html是默认的网站主界面
> - 成功时使用域名或者主机IP即可正常访问页面

Apache需要一个虚拟主机文件来提供服务器的内容。它已经创建了用于此目的的默认配置文件，但我们还是为其自定义配置创建一个新的配置文件。

```bash
sudo vi /etc/apache2/sites-available/网站域名.conf
```

输入域名的以下自定义配置详细信息：

```bash
<VirtualHost *:80>
ServerAdmin 管理员邮箱
ServerName 服务名
ServerAlias 域名
DocumentRoot /var/www/网站域名
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

启用使用a2ensite工具创建的配置文件：

```undefined
sudo a2ensite ourtest.com.conf
```

输出将建议激活新配置，但我们可以在运行以下禁用原始配置文件的命令后集体执行此操作：

```cpp
sudo a2dissite 000-default.conf
```

现在重启Apache服务：

```undefined
sudo systemctl restart apache2
```

最后，让我们通过以下命令测试是否存在任何配置错误：

```undefined
sudo apache2ctl configtest
```

## 配置MySQL数据库

1.使用root用户进入数据库面板

```shell
mysql -u root -p
```

2.创建新的数据库

```shell
CREATE DATABASE 数据库名;
```

3.创建新用户

```shell
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

> 主机名不知道时可以使用 localhost

4.授予新用户对新创建的数据库的访问权限

```shell
GRANT ALL PRIVILEGES ON 数据库名.* TO '用户名'@'主机名';
```

5.刷新使更改生效

```shell
FLUSH PRIVILEGES;
```

## 下载配置WordPress

1.下载WordPress

```shell
wget WordPress下载链接
```

> 下载链接可去官网获取，本文撰写时为 `https://cn.wordpress.org/latest-zh_CN.zip`

2.将文件解压到网站根目录

3.使用域名访问网站

4.按提示输入数据库名，用户名和密码，即为前面配置的数据库名，用户名和密码

5.创建成功

> - 可能会出现无法创建wp-config.php文件，此时按提示在网站根目录手动创建并将WordPress的内容放在即可
> - 如果创建失败，检查数据库相关问题
