# nfs共享文件

## 简介

linux当ssh和ftp传输文件不可用的时候，需要传输文件，就只能通过共享文件来进行

## 安装

### 查看nfs服务有没有

```
service nfs status
```

### 如果有，修改nfs配置文件/etc/exports，添加如下一行

```
/home/yourname/sharedir 10.1.60.34(rw,sync,no_root_squash)

第一个参数是你要让客户机访问的目录，第二个是你允许的主机IP，最后的（）内是访问控制方式。

```

<font color="#ff0000">注意，上面的主机IP不能使用＊来通配，否则在客户机上会出现访问拒绝，但是如果我们要设置局域网访问呢？怎么办，使用子网掩码例如：<br>10.1.60.0/255.255.254.0即可让10.1.60.*和10.1.61.*都可以访问,还可以使用10.1.60/23这种方式类确定子网</font>

### 启动nfs服务

```
service nfs start
```

### 客户端操作

挂载

```
sudo mount 主机IP:/home/yourname/sharedir /root/nfsshare
```

<font color="#ff0000">注:/root/nfsshare必须先存在。</font>

取消挂载

```
sudo umount /root/nfsshare
```
