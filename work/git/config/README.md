~/.ssh/config文件内容


```
Host github.com  # HostName不写时这个就充当HostName
  IdentityFile ~/.ssh/id_ecdsa  # 默认不写是~/.ssh/id_rsa
  Port 36000  # 远程仓库都是22，这个最好不要省略，因为不同机器配置的默认值可能不同
  ProxyCommand nc -X 5 -x localhost:<dynamic-forward-port> %h %p
  User root  # 需要登陆终端的时候写（不可省略），如果只是远程仓库的话可以不写

Host xxx.com
...
```

如果这里没有指定IdentityFile，可以通过修改$GIT_SSH_COMMAND环境变量来修改git使用私钥的地址：
例如 export GIT_SSH_COMMAND="ssh -i /home/svn/xxx/.ssh/id_rsa"

上例ProxyCommand的效果是让ssh客户端请求github.com:22时，用sock5代理走本机:<dynamic-forward-port>，
所以前提是要先在本机:<dynamic-forward-port>配置好动态端口转发