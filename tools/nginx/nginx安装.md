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


## Nginx的默认路径说明

1. Nginx配置路径：/etc/nginx/

2. PID目录：/var/run/nginx.pid

3. 错误日志：/var/log/nginx/error.log

4. 访问日志：/var/log/nginx/access.log

5. 默认站点目录：/usr/share/nginx/html