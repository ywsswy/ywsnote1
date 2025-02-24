# 这是旧版了

out_file_name_1 = 'eat.csv'
in_file_name_1 = 'find_list.txt'
import sys
import urllib.request
import re
import ssl
import zlib
import time
import json
old_stdout = sys.stdout
out_file_1 = open(out_file_name_1,'w')
def MakeRequest(raw_path, url_host, url_change_dict, raw_data, decode_type, show_flag):
# raw_path 存储的不是header标签，是raw标签，即第一行有完整url
# raw_data 没有可以给None/同url_host，none应该为不变
# url_host 有必要吗（190618），因为url的改动最好在外部确定，不然很难传递参数进来，但是可以改成None不变
	with open(raw_path, 'r') as f:
		firstline = f.readline()
		firstline_split = firstline.split(' ')
		if len(firstline_split) != 3:
			print('{}文件内容错误'.format(raw_path))
			exit(0)
		method = firstline_split[0] # 请求方式
		data_list = []
		while True:
			try:
				temp = f.readline()
				if len(temp) <=1:
					break
				else:
					data_list.append(temp)
			except EOFError:
				break
			## 把每个请求头的键值对解析到字典中
		data_dict = {}
		for i in data_list:
			data_dict[re.sub(r'([^ :]*)(.*$)',r'\1',i,flags=re.DOTALL)] = re.sub(r'([^ ]* )(.*$)(\s)',r'\2',i)
		## 用url_change_dict修改请求url
		url_split = re.split(r'[?&]',firstline_split[1])
		for i in range(len(url_split)):
			for k,v in url_change_dict.items():
				if url_split[i].split('=')[0] == k:
					url_split[i] = r'{}={}'.format(k,v)
		url_split[0] = url_host
		firstline_split[1] = '&'.join(url_split)
		firstline_split[1] = '?'.join(firstline_split[1].split('&',1))
		if show_flag:
			print('当前请求页面url为：{}'.format(firstline_split[1]))
		req = urllib.request.Request(firstline_split[1], data = raw_data, method = method)
		for k,v in data_dict.items():
			req.add_header(k,v)
		context = ssl._create_unverified_context()
		res = urllib.request.urlopen(req,context = context).read()
		if decode_type is 1:
			res = zlib.decompress(res, 16 | zlib.MAX_WBITS )  # 响应数据可能是加密的，此时选择解密方式gzip/zlib
			# res = res.decode('unicode-escape')
			res = res.decode('utf-8')
		elif decode_type is 1:
			res = res.decode('utf-8')
		else:
			res = 'else'
		return res


def GetInfo1(data):
	res = json.loads(data)['count']  # from str
	# from pyquery import PyQuery as pq
	# doc = pq(res) #str to dom
	# return doc('div#home').eq(0).find('div.word').text()
	return None

def Main():
	data = MakeRequest(in_file_name_1, 'https://restapi.amap.com/v3/place/around', {}, None, 1, True)
	data1 = GetInfo1(data)
	sys.stdout = out_file_1
	print('餐饮店名,服务分数')
	print('{},{}'.format(\
data1['can_yin_dian_ming'],\
data1['fu_wu_fen_shu']))
	sys.stdout = old_stdout

Main()
