1.3 测试一个对象是否是类字符串  
```Python  
def isStringLike(anobj):
    try: anobj + ''
    except: return False
    else: return True
```  

