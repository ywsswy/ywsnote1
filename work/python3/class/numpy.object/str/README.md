#dc_listings['price'] = dc_listings.price.str.replace("\$|,",'').astype(float)#$或者,都消掉，$1,250.00
dc_listings.price就是由object组成，.str即获取表面字符串
