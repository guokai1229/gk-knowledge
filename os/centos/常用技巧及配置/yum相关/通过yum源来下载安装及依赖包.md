# 通过yum源来下载安装及依赖包

## 安装工具

```
yum install yum-utils
```

## 创建文件夹

```
mkdir /root/mypackages
```

## 下载安装依赖包

```
yumdownloader --resolve --destdir /root/mypackages/ ansible
```