把某一属性设置为这个DataFrame的索引（每一行的行前标），【然后属性中就不存在这一列了！！！！太危险】
df = df.set_index('Name')#设置之后从这个DataFrame取出来的series也是以'Name'为索引的
df.head()
df['Age'][:5]
#Name
Braund, Mr. Owen Harris                                22.0
Cumings, Mrs. John Bradley (Florence Briggs Thayer)    38.0
Heikkinen, Miss. Laina                                 26.0
Futrelle, Mrs. Jacques Heath (Lily May Peel)           35.0
Allen, Mr. William Henry                               35.0
Name: Age, dtype: float64
