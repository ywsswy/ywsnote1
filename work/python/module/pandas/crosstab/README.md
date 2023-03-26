df.pivot_table(index = 'Sex',columns='Pclass',values='Fare',aggfunc='count')#计数，类似pandas.crosstab(index = df['Sex'],columns = df['Pclass'])

