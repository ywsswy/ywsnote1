split -l 800 data


文件每800行切割一个文件，得到的文件命名为xaa xab xac ...



split --number=K/N posting_list_data.data >out

厉害了，把文件分成N份中的第K（1s）份输出到out中