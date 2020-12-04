ts = pd.Timestamp('2017-11-24')
ts2 = pd.to_datetime('2017-11-24')
print(ts)
print(ts2)
print(type(ts))
print(type(ts2))
>>
2017-11-24 00:00:00
2017-11-24 00:00:00
<class 'pandas._libs.tslib.Timestamp'>
<class 'pandas._libs.tslib.Timestamp'>
pd.to_datetime('24/11/2017')
s = pd.Series(['2017-11-24 00:00:00','2017-11-25 00:00:00','2017-11-26 00:00:00'])
ts = pd.to_datetime(s)#字符串转换
ts.dt.day #如此访问
ts.dt.weekday
