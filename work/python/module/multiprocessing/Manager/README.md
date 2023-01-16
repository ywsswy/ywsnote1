用于多进程之间的数据通信

import multiprocessing
manager = multiprocessing.Manager()
file_list_queue = manager.Queue()
file_list_queue.put('1') 
file_list_queue.put('22') 
file_list_queue.put('333')

flag = True
while flag:
    # 下载单个文件
    try:
        hdfs_file_name = file_list_queue.get(False)  # 从数据队列中取出来，队列空则抛异常
    except:  # pylint: disable=bare-except
        flag = False
        continue
    print("get {}".format(hdfs_file_name))

print("return")