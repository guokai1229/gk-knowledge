## docker安装说明



### 备份及创建本地yum源配置

```shell
---解压rpms包
tar -xvf rpms.tar
---把解压后的包拷贝到目标目录
mv rpms /opt
```

local.repo 中包含rpms包的路径，默认是/opt/rpms，如果需要可以修改

``````
---备份原yum源配置
mv yum.repos.d/ yum.repos.d.bak
---创建yum源配置目录
mkdir yum.repos.d
---拷贝本地的yum源配置到yum源配置目录
cp local.repo yum.repos.d
---清理yum源配置缓存
yum clean all
---重建yum源配置缓存
yum makecache
``````

### 安装docker依赖包

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 安装docker

```
yum install docker-ce
```

### 启动docker

```
---查看docker版本
docker --version
---启动docker
systemctl start docker
---配置开机启动
systemctl enable docker
```

## docker常用命令说明

```shell
docker ps --查看当前运行的容器
docker ps -a ---查看所有的容器，包括停止的和当掉的

docker start 容器name/id --启动容器
docker stop 容器name/id --停止容器
docker restart 容器name/id --重新启动

docker images --查看当前镜像

docker rm 容器name/id --删除容器
docker rmi 镜像name/id --删除镜像
```

## docker镜像导入导出



### 导出

```
docker save apache/tomcat:v1 -o tomcat-image.tar
```

### 导入

```shell
docker load -i tomcat-image.tar ---加载镜像文件

docker images ---找到导入的镜像，查看镜像的id，名称和tag应该都是none

docker tag 67cade4433fd(镜像id) apache/tomcat:v1 ---添加tag标识
```



