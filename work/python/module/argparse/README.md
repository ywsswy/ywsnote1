if __name__ == '__main__':
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument("-v", "--verbosity", action="store_true",
        help="VERBOSE, output the details")
    parser.add_argument("-s", "--ipaddr", default="xxx:yyy",
        help="ES SERVER_ADDRESS")
    parser.add_argument("-m", default="mapping.json",
        help="mapping_file(json format) what is agreed in the CF")
    parser.add_argument("index",
        help="ES index_name")
    YGlobal.args = parser.parse_args()
    Main()


1）位置参数（必须传入，位置有严格要求）
add_argument("<没有短线的key>',...)

2）可选参数
add_argument("<前面加了短线的参数>",...)
这里可以写多个参数，一个横的短参数，两个横的长参数，甚至可以三个横
用户哪个都可以传递，但是在代码中能访问到的只有先定义的那个长参数，无长参数时才是短参数。

action设置
- store_true：无值参数，默认为False，用户写了则为true
- store_false：无值参数，默认为True，用户写了则为false

old）已经不再维护了
#!/usr/bin/env python3
import optparse
if __name__ == '__main__':
    parser = optparse.OptionParser()
    parser.add_option("-p", default="82", action="store", help="set the webapp's port") # 这种参数名后面要指定值，可以通过options.p访问
    parser.add_option("-e", action="store_true", help="specify security options") # 这种无需跟着值，如果指定了，则为True
    (YGlobal.options, args) = parser.parse_args()

命令行参数解析https://www.cnblogs.com/zwei0227/p/5793522.html
