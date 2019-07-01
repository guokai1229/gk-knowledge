# git中用户名密码设置

## 永久保存

```
git config --global credential.helper store
```


再输入一次用户名密码后就可以保存住了。

## 全局初始化

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```