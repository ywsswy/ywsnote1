不换行，只需要传入end参数
在当前行覆盖：
        print('restart!!{num}&{id} {t}'.format(num=num,id=id,t=time.ctime()),end='\r')
        sys.stdout.flush()
