#!/usr/bin/env python
# encoding=utf-8

class YGlobal(object):
    options = None

def Main():
    import os
    print(YGlobal.options.i)
    os.system('''./jieba -input_words {input}'''.format(input=YGlobal.options.i))


if __name__ == '__main__':
    import optparse
    parser = optparse.OptionParser()
    parser.add_option("-i", default="看看你能把我分词成什么", action="store", help="INPUT: 待分词的原始数据")
    (YGlobal.options, args) = parser.parse_args()
    Main()