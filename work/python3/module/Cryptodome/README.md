install pycryptodomex(https://pycryptodome.readthedocs.io/en/latest/src/installation.html)

from Cryptodome.Cipher import AES
data = b'i love u'
key = b'Sixteen byte key'
cipher = AES.new(key, AES.MODE_EAX)
nonce = cipher.nonce
ciphertext, tag = cipher.encrypt_and_digest(data)


cipher = AES.new(key, AES.MODE_EAX, nonce=nonce)
plaintext = cipher.decrypt(ciphertext)
try:
    cipher.verify(tag)
    print("The message is authentic:", plaintext)
except ValueError:
    print("Key incorrect or message corrupted")

//////////////////////////////////////////////////////////////////////
import Cryptodome.Cipher.AES
def SavaFileEncryptContent(path, content):
    data = content.encode(encoding='utf-8')
    key = YGlobal.pwd_md5
    cipher = Cryptodome.Cipher.AES.new(key, Cryptodome.Cipher.AES.MODE_EAX)
    nonce = cipher.nonce
    ciphertext, tag = cipher.encrypt_and_digest(data)
    # print(ciphertext)
    # print(tag)
    cipher = Cryptodome.Cipher.AES.new(key, Cryptodome.Cipher.AES.MODE_EAX, nonce=nonce)
    plaintext = cipher.decrypt(ciphertext)
    try:
        cipher.verify(tag)
        print("The message is authentic:", plaintext)
    except ValueError:
        print("Key incorrect or message corrupted")

