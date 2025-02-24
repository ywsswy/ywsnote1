def Str2Img(img_src):
    import base64
    img_b64_data = img_src
    img_data = base64.b64decode(img_b64_data)  # base64解码
    img_file = open('res1.png','wb')
    img_file.write(img_data)
    img_file.close()
