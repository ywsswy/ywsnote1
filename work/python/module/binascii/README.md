# 计算 CRC16 哈希值（跟redis的slot计算一致）

import binascii
key="123"
binascii.crc_hqx(key.encode('utf-8'), 0) % 16384



# 下面这个算出来的不对
import crcmod

crc16 = crcmod.predefined.mkCrcFun('crc-16')
crc16(key.encode('utf-8')) % 16384
