# 使用谷歌认证器，手机端账户名并不参与验证，仅密钥即可生成验证码

import pyotp

# secret_key = pyotp.random_base32(64)

def verify_code_func(secret_key, verify_code):
  t = pyotp.TOTP(secret_key)
  result = t.verify(verify_code)
  return result
