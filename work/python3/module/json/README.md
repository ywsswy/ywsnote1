import json

du_json = json.dumps(la_json,indent=4)# to str，设置indent能变好看，设置为None就是不换行输出
a = json.loads(s) #from str

dump功能一样，但是是直接to file
load和loads功能一样，但是是直接from file
//////////////////////////////////////////////////////////////////////


# json.dump(info, f, indent=4, ensure_ascii=False) #ensure_ascii参数可以保证保存中文
# encoding参数可以保证中文再次读取出来不乱码
with open('C:/file/projects/py/global/qiao_user_pass.txt', 'w', encoding='utf-8') as wf:
    json.dump(la_json, wf, indent=4)

with open('C:/file/projects/py/global/qiao_user_pass.txt', 'r', encoding='utf-8') as rf:
    mydict = json.load(rf)
//////////////////////////////////////////////////////////////////////

python 和 JSON 互转的过程中tuple会失传，变成list
