[color]
    diff = auto
[alias]
    st = status
    co = checkout
    cm = commit
    lg = log --graph --decorate --oneline
    ll = log --graph --pretty=format:\"%C(yellow)%h%Cgreen%d%Cred %an %ci %Creset%s\"
    br = branch
[user]
    name = ywsswy
    email = ywsswy@qq.com
[core]
    quotepath = false
# 可选 把那两种git地址替换成git@xxx
[url "git@xxx:"]
        insteadOf = https://xxx/
        insteadOf = http://xxx/


```
实际上这配置文件内容可以通过git config --global a.b c 命令插入文件得到如下效果
[a]
        b = c

git config --global a.b # 获取到配置的值

git config --global -l  # 获取到所有的配置

git config --global --unset a.b  # 删除某配置
```