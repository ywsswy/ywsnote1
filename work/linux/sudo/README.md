修改/etc/sudoers

可以给一个用户添加sudo权限

# 用户可以sudo执行任何命令，但是要输入密码
root    ALL=(ALL)       ALL

# 用户仅可以执行pwd命令，但是不需要密码
hill    ALL=(ALL)       NOPASSWD:/usr/bin/pwd