df['Age'].value_counts() # 默认降序，按次数排序（即统计每个值出现的次数） 可以设置df['Age'].value_counts(ascending = True)
功能上类似 df.groupby('Age')['Age'].count()
df['Age'].value_counts(ascending = True,bins = 5)
#把分布区间分为5份去统计个数
(64.084, 80.0]       11
(48.168, 64.084]     69
(0.339, 16.336]     100
(32.252, 48.168]    188
(16.336, 32.252]    346
返回Series，index为值，值为次数，按次数从多到少排序
如果想按值从小到大排序，则再.sort_index()
