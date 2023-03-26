# define XAPIAN_VISIBILITY_DEFAULT __attribute__((visibility("default")))

class Document //这种在编译的时候指定-fvisibility会被隐藏掉，不会链接成功
class XAPIAN_VISIBILITY_DEFAULT Document //这种才可见