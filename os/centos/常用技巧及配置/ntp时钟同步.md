# ntp时钟同步

## 安装ntp软件

```
yum install ntp -y

systemctl enable ntpd

systemctl start ntpd
```

## 设置ntp服务器

注释所有server

修改配置文件 vi /etc/ntp.conf

```
# 在 /ntp.conf 中定义的 server 都不可用时，将使用 local 时间作为 ntp 服务提供给 ntp 客户端。建议配置，否则 ntp 服务器无法与公网 ntp 服务器同步时，其客户端也会无法同步
server 127.127.1.0

fudge 127.127.1.0 stratum 10
```

重启ntp服务 systemctl restart ntpd

## 设置ntp客户端

注释所有server

修改配置文件 vi /etc/ntp.conf

```
server 172.25.194.253
fudge 172.25.194.253 stratum 10
```

重启ntp服务 systemctl restart ntpd

ntpq -p 查看状态

ntpstat 查看同步状态

```
[root@localhost opt]# ntpstat
synchronised to NTP server (172.25.194.253) at stratum 12
   time correct to within 654 ms
   polling server every 64 s
```

如出现如上显示，则同步成功，**注意：ntp启动后需要3-5分钟才会同步成功，请隔段时间再看**

## ntp常用命令

```
# 查看ntp版本
ntpq -c version

# 上层 ntp 的状态
ntpq -p

# ntp 同步状态
ntpstat

```

