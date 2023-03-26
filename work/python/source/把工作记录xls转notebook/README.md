import pandas as pd
title_row = ['上午','下午','晚上','计划与总结']
output = ['|18周工作记录|Mon.|Tues.|Wed.|Thur.|Fri.|Sat.|Sun.|',
          '|-|-|-|-|-|-|-|-|']
mydata = pd.read_excel('logging-18th-week.xlsx',dtype=str)
#print(type(mydata))
#print(mydata)
for rindex in range(0,4):
    out_row = '|'+title_row[rindex]+'|'
    df_row = mydata.iloc[rindex]
    #print(rindex,'行')
    #print(df_row)
    for cindex in range(0,7):
        pix_data = df_row.iloc[cindex]
        if pix_data != 'nan':
            out_row += ('<p>'+pix_data.replace('\n','</p><p>')+'</p>')
        #print(cindex,'列')
        #print(df_row.iloc[cindex])
        out_row += '|'
    output.append(out_row)
for i in output:
    print(i)
print(mydata.iloc[0,7])

