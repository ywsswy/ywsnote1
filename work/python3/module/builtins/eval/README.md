执行字符串表达式
eval('3+4') # 7
执行字符串语句
exec('print("hhh")') # >>> hhh
为了保护全局变量不被修改，应该指明作用域
并且过滤非法字符
 input = request.form.get('input')
        print(input)
        input = re.sub('\n','',input)
        input = re.sub('\r','',input)
        test_input = input
        test_input = re.sub(r' ','',test_input)
        test_input = re.sub(r'[+\-*/()0-9]','',test_input)
        print(test_input)
        scope = {}
        if len(test_input) == 0:
            # 合法
            print(input)
            return '{}'.format(eval(input,scope))
        else:
            return '非法'
