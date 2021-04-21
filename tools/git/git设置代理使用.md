# git设置代理使用

为了解决git获取github项目慢的问题

```
git config --global http.proxy socks5://127.0.0.1:1080

git config --global https.proxy socks5://127.0.0.1:1080
```

也可以直接修改~/.gitconfig文件。

vi ~/.gitconfig

新建或修改这两项配置

```
[http]
    proxy = socks5://127.0.0.1:1080
[https]
    proxy = socks5://127.0.0.1:1080
```
