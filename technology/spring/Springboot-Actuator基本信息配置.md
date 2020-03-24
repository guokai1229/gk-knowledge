# Springboot-Actuator基本信息配置

```
###基本信息
info:
  application: 名称
  description: 描述
  version: 版本
  author: 作者
##运行状态 actuator监控
endpoints:
  enabled: true
  info:
    sensitive: false
  health:
    sensitive: false
management:
  ##服务路径
  context-path: /
  security:
    enabled: false
  ##服务端口
  port: 8081
```

从springboot2.0之后发现了很大的变化

现在开启所有的端点：

```
management:
  endpoint:
    health:
      show-details: ALWAYS
  endpoints:
    web:
      exposure:
        include: "*"
```
