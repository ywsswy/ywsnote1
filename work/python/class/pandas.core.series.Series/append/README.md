data = [100,110]
index = ['h','k']
s2 = pd.Series(data = data,index = index)
s3 = s1.append(s2)
#增加还可以直接s3['j'] = 500
s1.append(s2,ignore_index = True)#ignore_index表明是否生成一个新索引（Ture）还是用原来的索引（默认False）
