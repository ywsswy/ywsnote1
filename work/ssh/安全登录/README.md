密码+TOTP：

apt install -y libpam-google-authenticator

google-authenticator  # 这里就生成二维码了（Increase the time window? → n）

/etc/pam.d/sshd文件里写两行：后include就是先TOTP后密码

```
auth required pam_google_authenticator.so
@include common-auth
```

/etc/ssh/sshd_config文件里写：

```
KbdInteractiveAuthentication yes
UsePAM yes
PasswordAuthentication yes
AuthenticationMethods keyboard-interactive
```

sshd -t  # 校验

sudo systemctl reload ssh