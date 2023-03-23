~/.ssh/config
文件的配置

```
Host github.com
IdentityFile ~/.ssh/id_ecdsa
Port 22

Host xxx.com
...
```

如果这里没有指定IdentityFile，可以通过修改$GIT_SSH_COMMAND环境变量来修改git使用私钥的地址：
例如 export GIT_SSH_COMMAND="ssh -i /home/svn/xxx/.ssh/id_rsa"