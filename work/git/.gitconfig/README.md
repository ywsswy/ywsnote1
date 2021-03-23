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
    email = 1726208887@qq.com
[core]
    quotepath = false
# 可选 把那两种git地址替换成git@xxx
[url "git@xxx:"]
        insteadOf = https://xxx/
        insteadOf = http://xxx/