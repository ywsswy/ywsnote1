def predict_price(new_listing_value,feature_column):
    temp_df = train_df
    temp_df['distance'] = np.abs(dc_listings[feature_column] - new_listing_value)
    temp_df = temp_df.sort_values('distance')
    knn_5 = temp_df.price.iloc[:5]
    predicted_price = knn_5.mean()
    return(predicted_price)
test_df['predicted_price'] = test_df.accommodates.apply(predict_price,feature_column='accommodates')#对这个Series中每个元素应用predict_price函数，每个元素的值，即为传递进函数的第一个参数
