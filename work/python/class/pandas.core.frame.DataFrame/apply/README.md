data = pd.DataFrame({'food':['A1','A2','B1','B2','B3','C1','C2'],'data':[1,2,3,4,5,6,7]})
print(data)
def food_map(series):
    if series['food'] == 'A1':
        return 'A'
    elif series['food'] == 'A2':
        return 'A'
    elif series['food'] == 'B1':
        return 'B'
    elif series['food'] == 'B2':
        return 'B'
    elif series['food'] == 'B3':
        return 'B'
    elif series['food'] == 'C1':
        return 'C'
    elif series['food'] == 'C2':
        return 'C'
data['food_map'] = data.apply(food_map,axis = 'columns')
data
#应用映射
或者用
food2Upper = {
    'A1':'A',
    'A2':'A',
    'B1':'B',
    'B2':'B',
    'B3':'B',
    'C1':'C',
    'C2':'C'
}
data['upper'] = data['food'].map(food2Upper)
data
