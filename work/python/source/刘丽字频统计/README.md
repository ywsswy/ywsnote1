# 涉及到 进度条、中文字符判断
#!/usr/bin/env python3
filename_tangpoetry = 'TangPoetry.txt'
# pre-processing( read from file & cut single word into list
def PreProcessing(filename, total_line):
	word_list = []
	with open(filename, 'r', encoding='utf-8') as f:
		line = 1
		data = f.readlines()
		for i in data:
			for j in i:
				# judge chinese word.(https://blog.csdn.net/zhenyu5211314/article/details/51537778)
				if ord(j) >= 13312 and ord(j) <= 40895:
					word_list.append(j)
			DrawProgressbar(line/total_line)
			line = line + 1
	return word_list
	
# count each word
def CountWord(word_list):
	word_dict = {}
	for i in word_list:
		try:
			word_dict[i] = word_dict[i] + 1
		except:
			word_dict[i] = 1
	return word_dict
		
# get the total number of lines of a file
def CountLine(filename):
	count=0
	thefile=open(filename_tangpoetry)
	while True:
		buffer=thefile.read(1024*8192)
		if not buffer:
			break
		count+=buffer.count('\n')
	thefile.close()
	return count
# draw the ProgressBar
def DrawProgressbar(percent):
	length = 3
	num_ok = int(percent * 100 / 3)
	num_no = (int(100/3) - num_ok)
	print('\r {:>5.1f}%% [{}{}]'.format(percent*100, '◼' * num_ok, '◻' * num_no), end='')
	
def Main():
	print('start preprocessing')
	total_line = CountLine(filename_tangpoetry)
	word_list = PreProcessing(filename_tangpoetry, total_line)
	# print(word_list)
	word_dict = CountWord(word_list)
	# print(word_dict)
	sorted_list = sorted(word_dict.items(), key = lambda word_dict:word_dict[1])
	# the highest frequency word
	print(sorted_list[-150:])
Main()

