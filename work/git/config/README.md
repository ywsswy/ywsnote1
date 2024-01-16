~/.ssh/config
文件的配置

```
Host github.com
IdentityFile ~/.ssh/id_ecdsa
Port 22
ProxyCommand nc -X 5 -x localhost:<dynamic-forward-port> %h %p

Host xxx.com
...
```

如果这里没有指定IdentityFile，可以通过修改$GIT_SSH_COMMAND环境变量来修改git使用私钥的地址：
例如 export GIT_SSH_COMMAND="ssh -i /home/svn/xxx/.ssh/id_rsa"

上例ProxyCommand的效果是让ssh客户端请求github.com:22时，用sock5代理走本机:<dynamic-forward-port>，
所以前提是要先在本机:<dynamic-forward-port>配置好动态端口转发