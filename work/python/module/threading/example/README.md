# 知识点： pring不换行，多线程，输出消息进度显示，超时重试

results_val = {}
def DoWork(results_map, stock_basic, total_size, last_info):
    import threading
    import time
    next_index = -1
    for i in range(total_size):
        if results_map[i] == 0:
            next_index = i
            break
    if next_index == -1:
        return
    else:
        results_map[next_index] = 1
        while True:
            try:
                results_val[next_index] = get(next_index)
            except Exception as mye:
                last_info['info'] = '获取第{}条时异常，再次请求ing'.format(next_index+1)
                time.sleep(1)
            else:
                last_info['info'] = '成功获取到第{}条信息，获取下一条'.format(next_index+1)
                break
        results_map[next_index] = 2
        threading.Timer(0, DoWork, (results_map, stock_basic, total_size, last_info)).start()
def Main():
    import threading
    import time
    results_map = {} #0:not get, 1: getting, 2: got
    total_size = 3610
    for i in range(total_size):
        results_map[i] = 0
    last_info = {'info':''}
    for i in range(4):
        threading.Timer(0, DoWork, (results_map, stock_basic, total_size, last_info)).start()
    while True:
        not_get = 0
        got = 0
        for i in range(total_size):
            if results_map[i] == 0:
                not_get += 1
            elif results_map[i] == 2:
                got += 1
        print('\rnot get:{}, getting:{}, got:{}, 进度：{}%.({})\t'.format(not_get, total_size-not_get-got, got, 100*got/total_size, last_info['info']), end='')
        if got == total_size:
            break
        time.sleep(0.4)
Main()