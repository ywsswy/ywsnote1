# import filttype
def get_file_type(filename):
    kind = filetype.guess(filename)
    if kind is None:
        return 'unknown'
    return kind.extension
                fileType = get_file_type(''.join([filePath, '\\', userCookie]))
                print(fileType)
                if fileType not in ['jpg','png','bmp']:
                    return Response(response='0文件类型错误，仅支持[jpg,png,bmp]图像文件')
