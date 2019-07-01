# docker设置

## 私服设置

修改文件/etc/docker/daemon.json

```
{
  "registry-mirrors":[
        "http://192.168.10.220:8083"
    ],
 
    "insecure-registries": [
       "http://192.168.10.220:8083"
    ]
}
```


docker pull 192.168.10.220:8083/redis:5.0

firewall-cmd --zone=public --add-port=6379/tcp --permanent

systemctl restart firewalld

## gitlab docker启动命令

```
docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 8228:22 \
  --name gitlab \
  --privileged=true \
  --restart always \
  --volume /opt/gitlab/config:/etc/gitlab \
  --volume /opt/gitlab/logs:/var/log/gitlab \
  --volume /opt/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```