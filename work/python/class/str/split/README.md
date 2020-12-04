a = 'hello ab   c'
a.split()#['hello', 'ab', 'c']
#以空白字符切分成字符串list
split(...)
    S.split(' ',sep=None, maxsplit=-1) -> list of strings
    
    Return a list of the words in S, using sep as the
    delimiter string.  If maxsplit is given, at most maxsplit
    splits are done. If sep is not specified or is None, any
    whitespace string is a separator and empty strings are
    removed from the result.


反函数是join # '='.join(['3', '1+2'])