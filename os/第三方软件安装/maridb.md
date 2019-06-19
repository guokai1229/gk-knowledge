# maridb安装说明

## yum安装

### 安装命令

```
yum -y install mariadb mariadb-server
```

安装完成MariaDB，首先启动MariaDB

```
systemctl start mariadb
```

设置开机启动

```
systemctl enable mariadb
```

### MariaDB的相关简单配置

```
mysql_secure_installation
```

首先是设置密码，会提示先输入密码

```
Enter current password for root (enter for none):<–初次运行直接回车
```

设置密码

```
Set root password? [Y/n] <– 是否设置root用户密码，输入y并回车或直接回车
New password: <– 设置root用户的密码
Re-enter new password: <– 再输入一次你设置的密码
```

其他配置

```
Remove anonymous users? [Y/n] <– 是否删除匿名用户，回车

Disallow root login remotely? [Y/n] <–是否禁止root远程登录,回车,

Remove test database and access to it? [Y/n] <– 是否删除test数据库，回车

Reload privilege tables now? [Y/n] <– 是否重新加载权限表，回车

```
初始化MariaDB完成，接下来测试登录

```
mysql -uroot -ppassword
```

完成。


## 基本配置

### 配置允许访问的用户，采用授权的方式给用户权限

```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'IDENTIFIED BY '123456' WITH GRANT OPTION;
```
最后配置好权限之后不应该忘记刷新使之生效

```
flush privileges;
```

### 配置MariaDB的字符集

文件/etc/my.cnf

```
vi /etc/my.cnf
```

在[mysqld]标签下添加

```
init_connect='SET collation_connection = utf8_unicode_ci' 
init_connect='SET NAMES utf8' 
character-set-server=utf8 
collation-server=utf8_unicode_ci 
skip-character-set-client-handshake
```