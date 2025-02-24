cmd = 'xxx'
res = subprocess.run(cmd, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, universal_newlines=True)
print(res, flush=True)


https://stackoverflow.com/questions/53279764/python-unicodedecodeerror-how-to-correctly-read-unicode-strings-from-subproces

如果返回数据是Pb的可能就挂了，但是好像又不推荐用.run以外的方法