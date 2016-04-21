- 1.3 测试一个对象是否是类字符串  
```Python  
def isStringLike(anobj):
    try: anobj + ''
    except: return False
    else: return True
```  

- 1.7 逐字符反转：  
```Python  
string = astring[::-1]
```  
逐词反转并不改变原先的空格  
```Python  
import re  
rewords = re.split(r'(\s+)', astring)
rewords.reverse()
rewords = ''.join(rewords)
```  

- 1.10 过滤字符串中不属于指定集合的字符  
```Python  
import string

allchars = string.maketrans('','')

def makefilter(keep):
    #keep为所需要删除的字符
    delchars = allchars.translate(allchars, keep)
    def thefilter(s):
        return s.translate(allchars, delchars)
    return thefilter

if __name__ == "__main__":
    just_vowels = makefilter('aeiouy')
    print just_vowels('asdfgsedgfsdjkhgfsd')

#set类型可以完成相应操作
import sets
class Keeper(object):
    
    def __init__(self, keep):
        self.keep = sets.Set(map(ord, keep))

    def __getitem__(self, n):
        if n not in self.keep:
            return None
        return unichr(n)

    def __call__(self, s):
        return unicode(s).translate(self)

makefilter = Keeper
if __name__ == '__main__':
    just_vowels = makefilter('awe')
    print just_vowels(u'asdasdgasjkd')
```  

- 2.10 处理字符串中的zip文件  
```Python  
import zipfile
try:
    from cStringIO import StringIO
except:
    from StringIO import StringIO

class ZipString(ZipFile):
    def __init__(self, datastring):
        ZipFile.__init__(self, StringIO(datastring))
```  

- 2.16 遍历目录树  
```Python  
def all_files(root, patterns='*', single_level=False, yield_folders=False):
    patterns = patterns.split(';')
    for path, subdirs, files in os.walk(root):
        if yield_folders:
            files.extend(subdirs)
        files.sort()
        for name in files:
            for pattern in patterns:
                if fnmatch.fnmatch(name, pattern):
                    yield os.path.join(path, name)
                    break
        if single_level:
            break

thefiles = list(all_files('/home', '*.py;*.htm;*.html'))
```  

- 2.17 批量修改文件扩展名  
```Python  
import os

def swapextensions(dir, before, after):
    if before[:1] != '.':
        before = '.' + before
    thelen = -len(before)
    if after[:1] != '.':
        after = '.' + after
    for path, subdirs, files in os.walk(dir):
        for oldfile in files:
            if oldfile[thelen:] == before:
                oldfile = os.file.join(path, oldfile)
                newfile = oldfile[:thelen] + after
                os.rename(oldfile, newfile)

if __name__ == '__main__':
    import sys
    if len(sys.argv) != 4:
        print 'input is wrong'
        sys.exit(100)
    swapextensions(sys.argv[1], sys.argv[2], sys.argv[3])
```  

- 4.6 展开一个嵌套的序列  
```Python  
def list_or_tuple(x):
    return isinstance(x, (list, tuple))

def flatten(sqeuence, to_expand=list_or_tuple):
    for item in sequence:
        if to_expand(item):
            for subitem in flatten(item, to_expand):
                yield subitem
        else:
            yield item 

# 非递归版本  
def flatten(sequence, to_expand=list_or_tuple):
    iterators = [iter(sequence)]
    while iterators:
        for item in iterators[-1]:
            if to_expand(item):
                iterators.append(iter(item))
                break
            else:
                yield item
        else:
            iterators.pop()
```  

- 4.14 反转字典  
```Python
def  invert_dict(d):
    return dict([(v, k) for k, v in d.iteritems()])

#izip更快一些  
from itertools import izip
def invert_dict_fast(d):
    return dict(izip(d.itervalues(), d.iterkeys()))
```  
需要注意的是，如果字典d的值不是唯一的话，那么d无法被真正的反转。即两个相同值作为键时会产生冲突。  

- 5.5 根据内嵌的数字将字符串排序  
```Python
#比如"f10"在"f2"之前  
import re
re_digits = re.compile(r'(\d+)')
def embedded_numbers(s):
    pieces = re_digits.split(s)  # 切成数字与非数字
    pieces[1::2] = map(int, pieces[1::2])  # 将数字部分转成整数
    return pieces

def sort_strings_with_embedded_number(alist):
    return sorted(alist, key=embedded_numbers)
```  

- 5.10 选取序列中最小的第n个元素  
```Python
import random
def select(data, n):
    #寻找第n个元素
    data = list(data)
    if n<0:
        n += len(data)
    if not 0 <= n < len(data):
        raise ValueError, "can't get rank %d out of %d"%(n, len(data))
    while True:
        pivot = random.choice(data)
        pcount = 0
        under, over = [], []
        uappend, oappend = under.append, over.append
        for elem in data:
            if elem < pivot:
                uappend(elem)
            elif elem > pivot:
                oappend(elem)
            else:
                pcount += 1
        number = len(under)
        if n < numunder:
            data = under
        elif n < numunder + pcount:
            return pivot
        else:
            data = over
            n -= numunder + pcount
```

- 5.11 快速排序  
```Python
#内置的sort方法要比这个好
def qsort(L):
    if len(L) <= 1: return L
    return qsort([lt for lt in L[1:] if lt < L[0]]) + L[0:1] + \
            qsort([ge for ge in L[1:] if ge >= L[0]])
```  

- 6.1 温标的转换  
```Python
# 其他转换可以类似的去写  
class Temperature(object):
    coefficients = {'c': (1.0, 0.0, -273.15), 'f': (1.8, -273.15, 32.0),
                    'r': (1.8, 0.0, 0.0)}

    def __init__(self, **kwargs):
        try:
            name, value = kwargs.popitem()
        except KeyError:
            #无参数，默认k=0
            name, value = 'k', 0
        if kwargs or name not in 'kcfr':
            kwargs[name] = value
            raise TypeError, 'invalid arguments %r'%kwargs
        setattr(self, name, float(value))

    def __getattr__(self, name):
        try:
            eq = self.coefficients[name]
        except KeyError:
            raise AttributeError, name
        return (self.k + eq[1]) * eq[0] +eq[2]

    def __setattr__(self, name, value):
        if name in self.coefficients[name]:
            self.k = (value - eq[2]) / eq[0] - eq[1]
        elif name == 'k':
            object.__setattr__(self, name, value)\
        else:
            raise AttributeError, name

    def __str__(self):
        return "%s K" % self.k

    def __repr__(self):
        return "Temperature(k=%r)" % self.k
```  

- 7.4 对类和实例使用cPickle模块  
```Python  
class PrettyClever(object):
    def __init__(self, *stuff):
        self.stuff = stuff

    def __getstate__(self):
        def normalize(x):
            if isinstance(x, file):
                return 1, (x.name, x.mode, x.tell())
            return 0, x
        return [normalize(x) for x in self.stuff]

    def __setstate__(self, stuff):
        def reconstruct(x):
            if x[0] == 0:
                return x[1]
            name, mode, offs = x[1]
            openfile = open(name, mode)
            openfile.seek(offs)
            return openfile
        self.stuff = tuple([reconstruct(x) for x in stuff])
```  

- 7.15 获取数据库游标内容  
```Python  
def pp(cursor, data=None, check_row_lengths=False):
    if not data:
        data = cursor.fetchall()
    names = []
    lengths = []
    reules = []

    for col, field_description in enumerate(cursor.description):
        field_name = field_description[0]
        names.append(field_name)
        field_length = field_description[2] or 12
        field_length = max(field_length, len(field_name))
        if check_row_lengths:
            data_length = max([len(str(row[col])) for row in data])
            field_length = max(field_length, data_length)
        lengths.append(field_length)
        rules.append('-'*field_length)
    format = " ".join(["%%-%ss"%l for l in lengths])
    result = [format % tuple(names), format % tuple(rules)]
    for row in data:
        result.append(format % tuple(row))
    return "\n".join(result)
```  

