#!/usr/bin/env python3
import smtplib
from email.mime.text import MIMEText
msg_from='xxx@qq.com'
passwd='xxx'
msg_to='xxx@qq.com'

subject="python代码通知邮件"
content="这是我使用python smtplib及email模块发送的邮件，{}".format()
msg = MIMEText(content)
msg['Subject'] = subject
msg['From'] = msg_from
msg['To'] = msg_to
try:
    s = smtplib.SMTP_SSL("smtp.qq.com",465)
    s.login(msg_from, passwd)
    s.sendmail(msg_from, msg_to, msg.as_string())
    print("发送成功")
except Exception as mye:
    print("发送失败{}".format(mye))
finally:
    s.quit()