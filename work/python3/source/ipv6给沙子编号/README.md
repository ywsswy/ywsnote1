#Filename:xml3.py
import math
print('''求证：ipv6是否能给地球上每一粒沙子编号？''')
num = (2**128)-1
print('''ipv6最多有{}个编号'''.format(num))
r = 6378140
print('''地球的赤道半径约为{}米'''.format(r))
v = math.pi*(r**3)*4/3
print('''地球的体积约有{}立方米'''.format(int(v)))
per_mm = 0.01
print('''把每粒沙子想象成边长为{}毫米的正方体'''.format(per_mm))
cal = (1000**3)*((1/per_mm)**3)
print('''那么每立方米就有{}粒沙子'''.format(int(cal)))
total = cal*v
print('''假设地球全部都是由沙子构成的，以此把沙子想象的尽可能的多''')
print('''算得地球上约有{}粒沙子'''.format(int(total)))
if total>num :
	print('''因为沙子的数量比ipv6能表示的编号多，所以ipv6不足以给地球上每一粒沙子编号''')
else :
	print('''因为沙子的数量比ipv6能表示的编号少，所以ipv6足以给地球上每一粒沙子编号''')
