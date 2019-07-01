# 搭建基于http的本地yum源服务器

## 概述

yum相较于rpm，能够更好地解决安装软件时的依赖包问题，使用yum安装更简单更方便。搭建本地YUM源服务器，可以避免升级安装软件时占用公网带宽；有了本地YUM源服务器，可以解决无法连接Internet的其他YUM客户端的软件升级和安装。

本文主要介绍了以下内容：

- 安装yum源服务器环境；

- 搭建基于HTTP的yum源服务器；

- 设置客户端来连接yum源服务器

## 安装yum源服务器环境

### 安装Nginx

首先安装yum –y install Nginx 安装html的网站服务器

配置nginx.conf,增加

```
server {
        listen       28023;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
	    autoindex  on;
            root   /opt/BOCO/yumrepo;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }
```

设置的/opt/BOCO/yumrepo就是yum安装的rpm文件放置的地址

### 安装createrepo

yum –y install createrepo 安装建yum源仓库的工具，可以用来建立yum仓库

## 搭建基于HTTP的yum源服务器

### 创建文件夹

我们先创建存放 .RPM的目录

```
[root@IP100-CentOS7 conf]# >>mkdir -p /opt/BOCO/yumrepo/centos/7/os/x86_64/Packages/
```

### 复制package包

我们可以将光盘镜像Packages目录里的 .rpm包复制到 /opt/BOCO/yumrepo/centos/7/os/x86_64/Packages/目录

### 创建索引

使用createrepo，生成创建yum仓库，创建索引信息

```
createrepo /opt/BOCO/yumrepo/centos/7/os/x86_64/
```

## 设置客户端来连接yum源服务器

再客户端机器上，设置yum源

vi /etc/yum.repo.d/Centos-Base.repo

```
[base]
name=BOMC - Base
baseurl=http://10.209.193.12:28023/centos/$releasever/os/x86_64
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
enabled=1
```

清理重建yum缓存

```
yum clean all

yum makecache
```

测试

```
yum list
```