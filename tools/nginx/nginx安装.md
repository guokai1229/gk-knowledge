# nginx安装

## yum安装

### 安装命令

```
yum install -y nginx
```

### 启动

```
systemctl start nginx.service
```

### 加入开机启动

```
systemctl enable nginx.service
```

### 修改防火墙

```
##加入防火墙规则

firewall-cmd --zone=public --add-port=80/tcp --permanent

firewall-cmd --reload
```

命令含义:
- --zone #作用域
  
- --add-port=80/tcp  #添加端口，格式为：端口/通讯协议
  
- --permanent   #永久生效，没有此参数重启后失效

## 编译安装

### 安装环境

安装编译的环境

```
yum install gcc gcc-c++ automake pcre pcre-devel zlip zlib-devel openssl openssl-devel 
```

### 下载源码包

通过官方地址下载最新的nginx源码包
```
http://nginx.org/en/download.html
```

### 解压编译

1.解压安装包

```
tar -zxvf nginx-1.8.1.tar.gz
```

2.环境检测

```
./configure --prefix=/usr/local/nginx 
```

3.编译安装

```
make && make install
```

4.添加nginx为系统服务

新建nginx文件 vim /etc/init.d/nginx

添加内容如下

```

#!/bin/sh
#
# nginx - this script starts and stops the nginx daemin
#
# chkconfig:   - 85 15 
# description:  Nginx is an HTTP(S) server, HTTP(S) reverse \
#               proxy and IMAP/POP3 proxy server
# processname: nginx
# config:      /usr/local/nginx/conf/nginx.conf
# pidfile:     /usr/local/nginx/logs/nginx.pid
 
# Source function library.
. /etc/rc.d/init.d/functions
 
# Source networking configuration.
. /etc/sysconfig/network
 
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
 
nginx="/usr/local/nginx/sbin/nginx"
prog=$(basename $nginx)
 
NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"
 
lockfile=/var/lock/subsys/nginx
 
start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    echo -n $"Starting $prog: "
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
 
stop() {
    echo -n $"Stopping $prog: "
    killproc $prog -QUIT
    retval=$?
    echo
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
}
 
restart() {
    configtest || return $?
    stop
    start
}
 
reload() {
    configtest || return $?
    echo -n $"Reloading $prog: "
    killproc $nginx -HUP
    RETVAL=$?
    echo
}
 
force_reload() {
    restart
}
 
configtest() {
  $nginx -t -c $NGINX_CONF_FILE
}
 
rh_status() {
    status $prog
}
 
rh_status_q() {
    rh_status >/dev/null 2>&1
}
 
case "$1" in
    start)
        rh_status_q && exit 0
        $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
        echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
        exit 2
esac
```

修改其权限并开机启动

- 修改权限：chmod 755 /etc/init.d/nginx
- 开机启动：chkconfig nginx on
- 查看开机启动的服务：chkconfig --list

### 常用编译选项说明

configure 脚本确定系统所具有一些特性，特别是 nginx 用来处理连接的方法。然后，它创建 Makefile 文件。

configure 支持下面的选项：

1）目录属性

```
--prefix=<path> - Nginx安装路径。如果没有指定，默认为 /usr/local/nginx。
--sbin-path=<path> - Nginx可执行文件安装路径。只能安装时指定，如果没有指定，默认为<prefix>/sbin/nginx。
--conf-path=<path> - 在没有给定-c选项下默认的nginx.conf的路径。如果没有指定，默认为<prefix>/conf/nginx.conf。
--pid-path=<path> - 在nginx.conf中没有指定pid指令的情况下，默认的nginx.pid的路径。如果没有指定，默认为 <prefix>/logs/nginx.pid。
--lock-path=<path> - nginx.lock文件的路径，默认为<prefix>/logs/nginx.lock
--error-log-path=<path> - 在nginx.conf中没有指定error_log指令的情况下，默认的错误日志的路径。如果没有指定，默认为 <prefix>/logs/error.log。
--http-log-path=<path> - 在nginx.conf中没有指定access_log指令的情况下，默认的访问日志的路径。如果没有指定，默认为 <prefix>/logs/access.log。
--user=<user> - 在nginx.conf中没有指定user指令的情况下，默认的nginx使用的用户。如果没有指定，默认为 nobody。
--group=<group> - 在nginx.conf中没有指定user指令的情况下，默认的nginx使用的组。如果没有指定，默认为 nobody。
--builddir=DIR - 指定编译的目录
```

2）模块

```
--with-rtsig_module - 启用 rtsig 模块
--with-select_module --without-select_module -允许或不允许开启SELECT模式，如果 configure 没有找到更合适的模式，比如：kqueue(sun os),epoll (Linux kenel 2.6+), rtsig(实时信号)或者/dev/poll(一种类似select的模式，底层实现与SELECT基本相 同，都是采用轮训方法) SELECT模式将是默认安装模式
--with-poll_module --without-poll_module - Whether or not to enable the poll module. This module is enabled by default if a more suitable method such as kqueue, epoll, rtsig or /dev/poll is not discovered by configure.
--with-http_ssl_module -开启HTTP SSL模块，使NGINX可以支持HTTPS请求。这个模块需要已经安装了OPENSSL，在DEBIAN上是libssl
--with-http_realip_module - 启用 ngx_http_realip_module
--with-http_addition_module - 启用 ngx_http_addition_module
--with-http_sub_module - 启用 ngx_http_sub_module
--with-http_dav_module - 启用 ngx_http_dav_module
--with-http_flv_module - 启用 ngx_http_flv_module
--with-http_stub_status_module - 启用 "server status" 页
--without-http_charset_module - 禁用 ngx_http_charset_module
--without-http_gzip_module - 禁用 ngx_http_gzip_module. 如果启用，需要 zlib 。
--without-http_ssi_module - 禁用 ngx_http_ssi_module
--without-http_userid_module - 禁用 ngx_http_userid_module
--without-http_access_module - 禁用 ngx_http_access_module
--without-http_auth_basic_module - 禁用 ngx_http_auth_basic_module
--without-http_autoindex_module - 禁用 ngx_http_autoindex_module
--without-http_geo_module - 禁用 ngx_http_geo_module
--without-http_map_module - 禁用 ngx_http_map_module
--without-http_referer_module - 禁用 ngx_http_referer_module
--without-http_rewrite_module - 禁用 ngx_http_rewrite_module. 如果启用需要 PCRE 。
--without-http_proxy_module - 禁用 ngx_http_proxy_module
--without-http_fastcgi_module - 禁用 ngx_http_fastcgi_module
--without-http_memcached_module - 禁用 ngx_http_memcached_module
--without-http_limit_zone_module - 禁用 ngx_http_limit_zone_module
--without-http_empty_gif_module - 禁用 ngx_http_empty_gif_module
--without-http_browser_module - 禁用 ngx_http_browser_module
--without-http_upstream_ip_hash_module - 禁用 ngx_http_upstream_ip_hash_module
--with-http_perl_module - 启用 ngx_http_perl_module
--with-perl_modules_path=PATH - 指定 perl 模块的路径
--with-perl=PATH - 指定 perl 执行文件的路径
--http-log-path=PATH - Set path to the http access log
--http-client-body-temp-path=PATH - Set path to the http client request body temporary files
--http-proxy-temp-path=PATH - Set path to the http proxy temporary files
--http-fastcgi-temp-path=PATH - Set path to the http fastcgi temporary files
--without-http - 禁用 HTTP server
--with-mail - 启用 IMAP4/POP3/SMTP 代理模块
--with-mail_ssl_module - 启用 ngx_mail_ssl_module
--with-cc=PATH - 指定 C 编译器的路径
--with-cpp=PATH - 指定 C 预处理器的路径
--with-cc-opt=OPTIONS - Additional parameters which will be added to the variable CFLAGS. With the use of the system library PCRE in FreeBSD, it is necessary to indicate --with-cc-opt="-I /usr/local/include". If we are using select() and it is necessary to increase the number of file descriptors, then this also can be assigned here: --with-cc-opt="-D FD_SETSIZE=2048".
--with-ld-opt=OPTIONS - Additional parameters passed to the linker. With the use of the system library PCRE in FreeBSD, it is necessary to indicate --with-ld-opt="-L /usr/local/lib".
--with-cpu-opt=CPU - 为特定的 CPU 编译，有效的值包括：pentium, pentiumpro, pentium3, pentium4, athlon, opteron, amd64, sparc32, sparc64, ppc64
--without-pcre - 禁止 PCRE 库的使用。同时也会禁止 HTTP rewrite 模块。在 "location" 配置指令中的正则表达式也需要 PCRE 。
--with-pcre=DIR - 指定 PCRE 库的源代码的路径。
--with-pcre-opt=OPTIONS - Set additional options for PCRE building.
--with-md5=DIR - Set path to md5 library sources.
--with-md5-opt=OPTIONS - Set additional options for md5 building.
--with-md5-asm - Use md5 assembler sources.
--with-sha1=DIR - Set path to sha1 library sources.
--with-sha1-opt=OPTIONS - Set additional options for sha1 building.
--with-sha1-asm - Use sha1 assembler sources.
--with-zlib=DIR - Set path to zlib library sources.
--with-zlib-opt=OPTIONS - Set additional options for zlib building.
--with-zlib-asm=CPU - Use zlib assembler sources optimized for specified CPU, valid values are: pentium, pentiumpro
--with-openssl=DIR - Set path to OpenSSL library sources
--with-openssl-opt=OPTIONS - Set additional options for OpenSSL building
--with-debug - 启用调试日志
--add-module=PATH - Add in a third-party module found in directory PAT
```

## 常见问题

问题：以非root权限启动时，会出现 nginx: [emerg] bind() to 0.0.0.0:80 failed (13: Permission denied) 错误

原因：Linux只有root用户可以使用1024一下的端口

解决方法：将/usr/local/nginx/conf/nginx.conf 文件中的80端口改为1024以上

## Nginx的默认路径说明

1. Nginx配置路径：/etc/nginx/

2. PID目录：/var/run/nginx.pid

3. 错误日志：/var/log/nginx/error.log

4. 访问日志：/var/log/nginx/access.log

5. 默认站点目录：/usr/share/nginx/html