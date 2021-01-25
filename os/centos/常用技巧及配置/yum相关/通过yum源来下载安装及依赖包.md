# 通过yum源来下载安装及依赖包

## 安装工具

```shell
yum install yum-utils
```

## 创建文件夹

```shell
mkdir /root/mypackages
```

## 下载安装依赖包

```shell
yumdownloader --resolve --destdir /root/mypackages/ ansible
```

## 同步yum库文件到本地

查看库列表

```shell
yum repolist
```

同步库文件

```shell
reposync -r base(库名称)
```