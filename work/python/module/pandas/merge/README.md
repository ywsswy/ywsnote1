类似数据库表的连接操作
left = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                    'A': ['A0', 'A1', 'A2', 'A3'], 
                    'B': ['B0', 'B1', 'B2', 'B3']})
right = pd.DataFrame({'key': ['K0', 'K1', 'K2', 'K3'],
                    'C': ['C0', 'C1', 'C2', 'C3'], 
                    'D': ['D0', 'D1', 'D2', 'D3']})
res = pd.merge(left, right)
res = pd.merge(left, right, on = 'key')
left = pd.DataFrame({'key1': ['K0', 'K1', 'K2', 'K3'],
                     'key2': ['K0', 'K1', 'K2', 'K3'],
                    'A': ['A0', 'A1', 'A2', 'A3'], 
                    'B': ['B0', 'B1', 'B2', 'B3']})
right = pd.DataFrame({'key1': ['K0', 'K1', 'K2', 'K3'],
                      'key2': ['K0', 'K1', 'K2', 'K4'],
                    'C': ['C0', 'C1', 'C2', 'C3'], 
                    'D': ['D0', 'D1', 'D2', 'D3']})
res = pd.merge(left, right, on = ['key1', 'key2'])
res = pd.merge(left, right, on = ['key1', 'key2'], how = 'outer')#外连接
res = pd.merge(left, right, on = ['key1', 'key2'], how = 'outer', indicator = True)#显示连接情况
》》
	A 	B 	key1 	key2 	C 	D 	_merge
0 	A0 	B0 	K0 	K0 	C0 	D0 	both
1 	A1 	B1 	K1 	K1 	C1 	D1 	both
2 	A2 	B2 	K2 	K2 	C2 	D2 	both
3 	A3 	B3 	K3 	K3 	NaN 	NaN 	left_only
4 	NaN 	NaN 	K3 	K4 	C3 	D3 	right_only
res = pd.merge(left, right, how = 'left')
