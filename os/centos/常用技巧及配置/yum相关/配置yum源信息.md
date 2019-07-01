# 配置yum源信息

## 自定义yum源

### 备份原有yum配置

```
cd /etc 

mv yum.repos.d yum.repos.d.bak
```

### 新建yum配置

```

mkdir yum.repos.d

cd yum.repos.d

touch CentOS-Base.repo

```

### 填写yum内容

```
vi CentOS-Base.repo
```

把以下内容填写到CentOS-Base.repo文件中

```
[nexus]
name=Nexus Repository
baseurl=http://192.168.10.220:8089/repository/yum-central/$releasever/os/$basearch/
enabled=1
gpgcheck=0

[nexus-extras]
name=Nexus Extras Repository
baseurl=http://192.168.10.220:8089/repository/yum-central/$releasever/extras/$basearch/
enabled=1
gpgcheck=0


[epel]
name=Nexus epel
baseurl=http://192.168.10.220:8089/repository/yum-epel/$releasever/$basearch/
enabled=1
gpgcheck=0

```

### 重置yum缓存

```
yum clean all

yum makecache
```