- 1.3 测试一个对象是否是类字符串  
```Python  
def isStringLike(anobj):
    try: anobj + ''
    except: return False
    else: return True
```  

- 1.7  
逐字符反转：  
```Python  
string = astring[::-1]
```  
逐词反转并不改变原先的空格  
```Python  
import re  
rewords = re.split(r'(\s+)', astring')
rewords.reverse()
rewords = ''.join(rewords)
```  