[color]
    diff = auto
[alias]
    st = status
    co = checkout
    cm = commit
    lg = log --graph --pretty=format:\"%C(yellow)%h%Cgreen%d%Cred %an %ci %Creset%s\n\"
    ll = log --graph --simplify-by-decoration --pretty=format:\"%C(yellow)%h%Cgreen%d%Cred %an %ci %Creset%s\n\"
    br = branch
[user]
    name = ywsswy
    email = ywsswy@qq.com
[core]
    quotepath = false
    editor = vim
# 可选 把那两种git地址替换成git@xxx
[url "git@xxx:"]
        insteadOf = https://xxx/
        insteadOf = http://xxx/

```
git log显示的是第一版的时间
ll中显示的日期是真实提交的时间，（可能经过rebase/cherry-pick重新提交的）

实际上这配置文件内容可以通过git config --global a.b c 命令插入文件得到如下效果
[a]
        b = c

git config --global a.b # 获取到配置的值

git config --global -l  # 获取到所有的配置

git config --global --unset a.b  # 删除某配置
```