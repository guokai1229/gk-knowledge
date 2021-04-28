# kubernetes集群

## 部署主机 

### 主机环境

| 软硬件            | 最低配置                                                     | 推荐配置                |
| ----------------- | ------------------------------------------------------------ | ----------------------- |
| **CPU和内存**     | Master: 至少2 Core和4G内存Node：至少4 Core和16G内存          | Master：4 core和16G内存 |
| **Linux操作系统** | 基于x86_64架构的各种Linux发行版本，Kernel版本要求在3.10及以上 | Red Hat Linux 7CentOS 7 |

 说明：

　　以上为建议配置，实际安装过程中，Master必须2 core 及以上（否则安装失败，切记），Node可以采用1 core。

## 环境准备

### 关闭防火墙

CentOS Linux 7 默认开起来防火墙服务（firewalld），而Kubernetes的Master与工作Node之间会有大量的网络通信，安全的做法是在防火墙上配置Kbernetes各组件（api-server、kubelet等等）需要相互通信的端口号。在安全的内部网络环境中可以关闭防火墙服务。

关闭防火墙的命令：
```
firewall-cmd --state           #查看防火墙状态
systemctl stop firewalld.service        #停止firewall
systemctl disable firewalld.service     #禁止firewall开机启动
```

如果你不想关闭防火墙，请把以下端口开放（在防火墙开放以下端口）

![336878-20200420101140629-474350150](../../../images/336878-20200420101140629-474350150.png)

### 关闭SELinux

建议禁用SELinux，让容器可以读取主机文件系统

执行命令：

```
getenforce        #查看selinux状态
setenforce 0       #临时关闭selinux
sed -i 's/^ *SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config  #永久关闭（需重启系统）
shutdown -r now   #重启系统(非必须)
```

### ntp时钟同步

参照[ntp时钟同步](../常用技巧及配置/ntp时钟同步.md)

### 禁用swap

Kubeadm建议关闭交换空间的使用，简单来说，执行swapoff -a命令，然后在/etc/fstab中删除对swap的加载，并重新启动服务器即可。

临时禁用，执行以下命令：

```shell
swapoff -a
```

永久禁用，需要在swapoff -a之后，执行以下命令：

```
sed -i.bak '/swap/s/^/#/' /etc/fstab
```

### 配置主机名

 Master主机改为master01，执行以下命令：

```shell
hostnamectl set-hostname master01 #修改主机名称为master01
more /etc/hostname  #查看修改结果
```

所有节点的主机都使用同样的方法进行修改

### 修改hosts文件

```shell
cat >> /etc/hosts << EOF
192.168.0.6     master01
192.168.0.10   node01
192.168.0.12   node02
EOF
```





