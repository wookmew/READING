#[STACKOVERFLOW PYTHON 百问](http://www.wklken.me/posts/2013/07/20/python-stackoverflow-vote-top.html#_1)
> 原作地址奉上，此处仅作自己的学习。看完这个，要去重新补习一下基础了，要不然终归只是会个皮毛罢了。
<br/>


##目录
> 基础  

- [基本语法控制流相关](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#基本语法控制流相关)
- [字符串相关](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#字符串相关)
- [文件相关](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#文件相关)
- [数学相关](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#数学相关)  

> 基本数据结构  
- [列表](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#列表)
- [元组](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#元组)
- [字典](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#字典)


##基本语法控制流相关
**1. 如何退出一个python脚本**<br/>
```python
import sys
sys.exit()
```

**2. foo is None和foo == None的区别**<br/>
```python
if foo is None: pass
if foo == None: pass
```
如果比较相同的对象实例，is总是返回True而==最终取决于"eq()"<br/>
```python
>>> class foo(object):
    def __eq__(self, other):
        return True

>>> f = foo()
>>> f == None
True
>>> f is None
False

>>> list1 = [1, 2, 3]
>>> list2 = [1, 2, 3]
>>> list1==list2
True
>>> list1 is list2
False
```
另外<br/>
```python
(ob1 is ob2) 等价于(id(ob1) == id(ob2))
```

**3. 如何在循环中获取下标**<br/>
```python
for idx, val in enumerate(squence):
    print idx, val
```

**4. python中如何将一行长代码切成多行**<br/>
例如：
```python
e = 'a' + 'b' + 'c' + 'd'
变成
e = 'a' + 'b' +
    'c' + 'd'
```
括号中，可以直接换行
```python
a = dostuff(blahblah1, blahblah2, blahblah3, blahblah4, blahblah5,
            blahblah6, blahblah7)
```
非括号中可以这么做
```python
a = '1' + '2' + '3' + \
    '4' + '5'
或者
a = ('1' + '2' + '3' +
    '4' + '5')
```

**5. 为何1 in [1, 0] == True执行结果是False**<br/>
例如：
```python
>>> 1 in [1,0]             # This is expected
True
>>> 1 in [1,0] == True     # This is strange
False
>>> (1 in [1,0]) == True   # This is what I wanted it to be
True
>>> 1 in ([1,0] == True)   # But it's not just a precedence issue!
                           # It did not raise an exception on the second example.

Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
      1 in ([1,0] == True)
      TypeError: argument of type 'bool' is not iterable
```
这里python使用了比较运算符链
```python
1 in [1, 0] == True
```
将被转为
```python
(1 in [1, 0]) and ([1, 0] == True)
```
很显然是false的，同样的
```python
a < b < c
```
会被转为
```python
(a < b) and (b < c) # b不会被解析两次
```
[doc](https://docs.python.org/2/reference/expressions.html#not-in)

**6. python中switch替代语法**<br/>
python中没有switch，有什么推荐的处理方法么<br/>
使用字典:
```python
def f(x):
    return {
        'a' : 1,
        'b' : 2,
    }.get(x, 9)
```

**7. 使用'if x is not None'还是'if not x is None'**<br/>
我总想着使用 'if not x is None' 会更加简明,但是google的Python风格指南使用的却是 'if x is not None',性能上没有什么区别，他们编译成相同的字节码:<br/>
在风格上，我尽量避免 'not x is y' 这种形式，虽然编译器会认为和 'not (x is y)'一样，但是读代码的人或许会误解为 '(not x) is y',如果写作 'x is not y' 就不会有歧义<br/>
最佳实践
```python
if x is not None:
    #do something about x
```

**8. 在非创建全局变量的地方使用全局变量**<br/>
如果我在一个函数中创建了全局变量，如何在另一个函数中使用？<br/>
回答：你可以在给全局变量赋值的函数中声明 global<br/>
```python
globvar = 0

def set_globvar_to_one():
    global globvar    # Needed to modify global copy of globvar
    globvar = 1

def print_globvar():
    print globvar     # No need for global declaration to read value of globvar

set_globvar_to_one()
print_globvar()       # Prints 1
```
我猜想这么做的原因是，全局变量很危险，Python想要确保你真的知道你要对一个全局的变量进行操作<br/>

**9. 如何检测一个变量是否存在**<br/>
检测本地变量<br/>
```python
if 'x' in locals():
    #x exists
```
检测全局变量<br/>
```python
if 'x' in globals():
    #x exists
```
检查一个对象是否包含某个属性<br/>
```python
if hasattr(obj, 'attr_name'):
    #obj.attr_name exists
```

**10. python中是否存在三元运算符**<br/>
三元运算符在Python2.5中被加入
```python
a if test else b
```
使用
```python
>>> 'true' if True else 'false'
'true'
>>> 'true' if False else 'false'
'false'
```
[doc](https://docs.python.org/3.3/faq/programming.html#is-there-an-equivalent-of-c-s-ternary-operator)

**11. python中的de-while**<br/>
实现：
```python
while True:
    stuff()
    if fail_condition:
        break
or
stuff()
while not fail_condition:
    stuff()
```

**12. 相对于range() 应该更倾向于实用xrange()?**<br/>
就性能而言, 特别是当你迭代一个大的range, xrange()更优. 但是, 有一些情况下range()更优:<br/>
- 在Python3中, range() 等价于 python2.x的 xrange(), 而xrange()不存在了.(如果你的代码将在2和3下运行, 不能使用xrange)<br/>
- range()在某些情况下更快, 例如, 多次重复遍历同一个序列.<br/>
- xrange()并不适用于所有情况, 例如, 不支持slices以及任意的list方法<br/>

**13. 在Python中，“i += x”和“i = i + x”什么时候不等**<br/>
这完全取决于i这个对象。

+=调用了[__iadd__](https://docs.python.org/2/reference/datamodel.html#object.__iadd__)方法（如果存在—不存在就退一步调用__add__），然而+调用[__add__方法](https://docs.python.org/2/reference/datamodel.html#object.__add__)<br/>
从一个API的角度，__iadd__期望被使用在恰当的位置修改易变的对象（返回的对象也是转变后的），而__add__应该返回某些东西的一个新的实例。对于不可变对象，两种方法都返回新的实例，但__iadd__会把新的实例放在和旧实例名字**相同的命名空间里**。这就是为什么:<br/>
```python
i = 1
i += 1
```
看上去增量i，实际上，你得到了一个新的数值，并且转移到了i的最上面—丢掉了旧的数值的引用。在这种情况下，i += 1和i = i + 1是完全一样的。但是，对于大多数可变对象，这是完全不同的：<br/>
```python
a = [1, 2, 3]
b = a
b += [1, 2, 3]
print a  #[1, 2, 3, 1, 2, 3]
print b  #[1, 2, 3, 1, 2, 3]
```
对比一下<br/>
```python
a = [1, 2, 3]
b = a
b = b + [1, 2, 3]
print a #[1, 2, 3]
print b #[1, 2, 3, 1, 2, 3]
```
注意第一个例子，从b和a代表相同的对象开始，当我对b使用+=，实际上它改变了b（a看起来也改变了- -毕竟他们代表同一个列表）。但是在第二个例子里，当我进行b = b + [1, 2, 3]操作时，b被引用并且和一个新的列表[1, 2, 3]联系了起来。之后在b的命名空间保存了这个关联的列表- -不考虑b之前的序列。<br/>

##字符串相关
**1. 为什么是string.join(list)而不是list.join(string)**<br/>
```python
my_list = ["Hello", "world"]
print "-".join(my_list)
#为什么不是 my_list.join("-") 。。。。
```
因为所有可迭代对象都可以被连接，而不只是列表，但是连接者总是字符串<br/>

**2. 字符如何转为小写**<br/>
```python
s = "Kilometer"
print(s.lower()) #大写 s.upper()
```

**3. 字符串转为float/int**<br/>
此处介绍另外一种方法，用ast模块<br/>
```python
>>> import ast
>>> ast.literal_eval("545.2222")
545.2222
>>> ast.literal_eval("31")
31
```

**4. 如何反向输出一个字符串**<br/>
list可用reverse()也可用切片，此处str方法就是切片<br/>
```python
>>> 'hello world'[::-1]
'dlrow olleh'
```

**5. 如何随机生成大写字母和数字组成的字符串**<br/>
```python
import string, random
''.join(random.choice(string.ascii_uppercase + string.digits) for x in range(N))
```

**6. python中字符串的contains()**<br/>
python中字符串判断contains,使用in关键字<br/>
```python
if not "blah" in somestring: continue
if "blah" not in somestring: continue
```
使用字符串的find/index (注意index查找失败抛异常)<br/>
```python
s = "This be a string"
if s.find("is") == -1:
    print "No 'is' here!"
else:
    print "found 'is' in the string"
```

**7. 如何判断一个字符串是数字**<br/>
可使用isdigit，但缺点在于，对*非整数无能为力*<br/>
```python
a = "03523"
a.isdigit()
```
也可以自己写个：<br/>
```python
def is_number(s):
    try:
        float(s)
        return True
    except ValueError:
        return False
```

**8. 字符串格式化 % vs format**<br/>
.format 看起来更加强大，可以用在很多情况.例如你可以在格式化时重用传入的参数,而你用%时无法做到这点.<br/>
另一个比较讨厌的是，%只处理 一个变量或一个元组, 你或许会认为下面的语法是正确的<br/>
```python
"hi there %s" % name
```
但当name恰好是(1,2,3)时，会抛出TypeError异常.为了保证总是正确的，你必须这么写
<br/>
```python
"hi there %s" % (name,)   # supply the single argument as a single-item tuple
```
什么时候不考虑使用.format<br/>
```python
你对.format知之甚少
使用Python2.5
```

**9. 将一个字符串转为一个字典**<br/>
eval是危险的，从python2.6开始，我们可以使用内建模块 ast.literal_eval<br/>
```python
>>> import ast
>>> ast.literal_eval("{'muffin' : 'lolz', 'foo' : 'kitty'}")
{'muffin': 'lolz', 'foo': 'kitty'}
```

**10. 如何获得一个字符的ASCII码**<br/>
```python
>>> ord('a')
97
>>> chr(97)
'a'
>>> chr(ord('a') + 3)
'd'
```
对于unicode而言：<br/>
```python
>>> unichr(97)
u'a'
>>> unichr(1234)
u'\u04d2'
```

**11. 如何使用不同分隔符切分字符串**<br/>
[re.split](https://docs.python.org/2/library/re.html#re.split)
```python
>>> re.split('\W+', 'Words, words, words.')
['Words', 'words', 'words', '']
>>> re.split('(\W+)', 'Words, words, words.')
['Words', ', ', 'words', ', ', 'words', '.', '']
>>> re.split('\W+', 'Words, words, words.', 1)
['Words', 'words, words.'])
```
或者匹配获得争取的re.findall<br/>
```python
import re
DATA = "Hey, you - what are you doing here!?"
print re.findall(r"[\w']+", DATA)
# Prints ['Hey', 'you', 'what', 'are', 'you', 'doing', 'here']
```

**12. 如何截掉空格(包括tab)**<br/>
空白在字符串左右两边<br/>
```python
s = "  \t a string example\t  "
s = s.strip()
```
空白在字符串右边<br/>
```python
s = s.strip()
```
左边<br/>
```python
s = s.lstrip()
```
也可以指定要截掉的字符作为参数<br/>
```python
s = s.strip(' \t\n\r')
```

**13. 如何街区一个字符串获得子串**<br/>
```python
>>> x = "Hello World!"
>>> x[2:]
'llo World!'
>>> x[:2]
'He'
>>> x[:-2]
'Hello Worl'
>>> x[-2:]
'd!'
>>> x[2:-2]
'llo Worl'
```

**14. python中用==比较字符串，is有时候会返回错误判断**<br/>
is是*身份测试*，而==是相等测试，is等价于id(a) == id(b)<br/>
```python
>>> a = 'pub'
>>> b = ''.join(['p', 'u', 'b'])
>>> a == b
True
>>> a is b
False'
```

**15. 如何填充0到数字字符串中保证统一长度**<br/>
对于字符串<br/>
```python
>>> n = '4'
>>> print n.zfill(3)
>>> '004'
```
对于数字,[相关文档](https://docs.python.org/2/library/string.html#formatexamples)<br/>
```python
>>> n = 4
>>> print '%03d' % n
>>> 004
>>> print "{0:03d}".format(4)  # python >= 2.6 or python 3
>>> 004
```

**16. 如何将字符串转换为datetime**<br/>
str -> time [strptime](https://docs.python.org/2/library/time.html#time.strptime)<br/>
```python
>>> import time
>>> time.strptime("30 Nov 00", "%d %b %y")   
time.struct_time(tm_year=2000, tm_mon=11, tm_mday=30, tm_hour=0, tm_min=0,
                tm_sec=0, tm_wday=3, tm_yday=335, tm_isdst=-1)
```
time -> str [strftime](https://docs.python.org/2/library/time.html#time.strftime)<br/>
```python
>>> from time import gmtime, strftime
>>> strftime("%a, %d %b %Y %H:%M:%S +0000", gmtime())
'Thu, 28 Jun 2001 14:17:15 +0000'
```

**17. 如何将byte array转为string**<br/>
```python
>>> b"abcde"
b'abcde'
>>> b"abcde".decode("utf-8")
'abcde'
```

## 文件相关
**1. 如何判断一个文件是否存在**<br/>
如何检查一个文件是否存在，不适用于try:声明<br/>
```python
import os.path
print os.path.isfile(fname)
print os.path.exists(fname)
```

**2. 如何创建不存在的目录结构**<br/>
```python
if not os.path.exists(directory):
    os.makedirs(directory)
```
需要注意的是，当目录在exists和makedirs两个函数调用之间被创建时，makedirs将抛出OSError<br/>

**3. 如何拷贝一个文件**<br/>
[shutil](https://docs.python.org/2/library/shutil.html)模块<br/>
```python
copyfile(src, dst) #src/dst是字符串
```
将src文件内容拷贝到dst，目标文件夹必须可写，否则将抛出IOError异常，如果目标文件已存在，将被覆盖。另外特殊文件，想字符文件，块设备文件，无法用这个方法进行拷贝<br/>

**4. 逐行读文件去除换行符(perl chomp line)**<br/>
读一个文件，如何获取每一行内容(不包括换行符)<br/>
比较pythonic的做法：<br/>
```python
>>> text = "line 1\nline 2\r\nline 3\nline 4"
>>> text.splitlines()
['line 1', 'line 2', 'line 3', 'line 4']
```
用rstrip,(rstrip/lstrip/strip)<br/>
```python
#去除了空白+换行
>>> 'test string \n'.rstrip()
'test string'
#只去换行
>>> 'test string \n'.rstrip('\n')
'test string '
#更通用的做法，系统相关
>>> import os, sys
>>> sys.platform
'linux2'
>>> "foo\r\n".rstrip(os.linesep)
'foo\r'
```

**5. 如何获取一个文件的创建和修改时间**<br/>
跨平台的获取文件创建及修改时间的方法,可以选择[os.path.getmtime](http://docs.python.org/release/2.5.2/lib/module-os.path.html#l2h-2177)或者[os.path.getctime](http://docs.python.org/release/2.5.2/lib/module-os.path.html#l2h-2178)<br/>
```python
import os.path, time
print "last modified: %s" % time.ctime(os.path.getmtime(file))
print "created: %s" % time.ctime(os.path.getctime(file))
```
或者[os.stat](http://www.python.org/doc/2.5.2/lib/module-stat.html)<br/>
```python
import os, time
import os, time
(mode, ino, dev, nlink, uid, gid, size, atime, mtime, ctime) = os.stat(file)
print "last modified: %s" % time.ctime(mtime)
```
注意，ctime()并非指*nix系统中文件创建时间，而是这个节点数据的最后修改时间<br/>

**6. 如何将字符串转换为datetime**<br/>
与之前提到的time模块一样，使用strptime方法，反向操作是strftime：<br/>
```python
from datetime import datetime
date_object = datetime.strptime('Jun 1 2005  1:33PM', '%b %d %Y %I:%M%p')
```

**7. 找到当前目录及文件所在目录**<br/>
查找当前目录使用os.getcwd()，查找某个文件的目录，使用[os.path](http://docs.python.org/2/library/os.path.html)<br/>
```python
import os.path
os.path.realpath(__file__)
```

**8. 如何找到一个目录下所有.txt文件**<br/>
使用[glob](http://docs.python.org/2/library/glob.html)<br/>
```python
import glob
import os
os.chdir("/mydir")
for files in glob.glob("*.txt"):
    print files
```
使用os.lstdir<br/>
```python
import os 
os.chdir("/mydir")
for files in os.listdir("."):
    if files.endswith(".txt"):
        print files
```
或者便利目录<br/>
```python
import os
for r, d, f in os.walk("/mydir"):
    for files in f:
        if files.endswith(".txt"):
            print os.path.join(r, files)
```

**9. 读文件到列表中**<br/>
```python
f = open('filename')
lines = f.readlines()
f.close()
等价于
with open(fname) as f:
    content = f.radlines()
```

**10. 如何往文件中追加文本**<br/>
```python
with open("fname", "a") as myfile:
    myfile.write("asdfaf")
```
可以使用'a'或'a+b' mode打开文件, [doc](http://docs.python.org/2/library/functions.html#open)<br/>

**11. 如何获得文件扩展名**<br/>
使用os.path.splitext方法<br/>
```python
>>> import os
>>> fileName, fileExtension = os.path.splitext('path')
>>> fileName
'/path/to/somefile'
>>> fileExtension
'.ext'
```

**12. 如何列出一个目录的所有文件**<br/>
1. 使用os.listdir(),得到目录下的所有文件和文件夹<br/>
```python
#只需要文件
from os import listdir
from os.path import isfile, join
onlyfiles = [f for f in listdir(path) if isfile(join(mypath, f))]
```
2. os.walk()<br/>
```python
from os import walk
f = []
for (dirpath, dirnames, filenames) in walk(path):
    f.extend(filenames)
    break
```
3.glob<br/>
```python
import glob
print glob.glob("path")
```
4. import os<br/>
```python
import os
for dirname, dirnames, filenames in os.walk('.'):
    # print path to all subdirectories first.
    for subdirname in dirnames:
        print os.path.join(dirname, subdirname)

    # print path to all filenames.
    for filename in filenames:
        print os.path.join(dirname, filename)
```

**13. 如何从标准输入读取内容stdin**<br/>
使用[fileinput](http://docs.python.org/2/library/fileinput.html)<br/>
```python
import fileinput
for line in fileinput.input():
    pass
```

**14. 如何高效地获取文件行数**<br/>
比较结果python2.6<br/>
```python
mapcount : 0.471799945831
simplecount : 0.634400033951
bufcount : 0.468800067902
opcount : 0.602999973297
```
code:<br/>
```python
from __future__ import with_statement
import time
import map
import random
from collections import defaultdict

def mapcount(filename):
    f = open(filename, "r+")
    buf = mmap.mmap(f.fileno(), 0)
    lines = 0
    readline = buf.readline
    while readline():
        lines += 1
    return Lines

def simplecount(filename):
    lines = 0
    for line in open(filename):
        lines += 1
    return lines

def bufcount(filename):
    f = open(filename)
    lines = 0
    buf_size = 1024*1024
    read_f = f.read() # loop optimization

    buf = read_f(buf_size)
    while buf:
        lines += buf.count('\n')
        buf = read_f(buf_size)
    return lines

def opcount(filename):
    with open(fname) as f:
        for i, l in enumerate(f):
            pass
    return i+1

counts = defaultdict(list)

for i in range(5):
    for func in [mapcount, simplecount, bufcount, opcount]:
        start_time = time.time()
        assert func("filename") == 1209138
        counts[func].append(time.time() - start_time)

for k, v in counts.items():
    print key.__name__, ":", sum(vals) / float(len(vals))
```

**15. Python如何实现mkdir -p功能**<br/>
```python
import os, errno
    try:
        os.makedirs(path)
    except OSError as exc : # Python >2.5
        if exc.errno == errno.EEXIST and os.path.isdir(path):
            pass
        else: raise
```

## 数学相关
**1. 在Python中如何展示二进制字面值**<br/>
Python 2.5 及更早版本: 可以表示为 int('01010101111',2) 但没有字面量<br/>
Python 2.6 beta: 可以使用0b1100111 or 0B1100111 表示<br/>
Python 2.6 beta: 也可以使用 0o27 or 0O27 (第二字字符是字母 O)<br/>
Python 3.0 beta: 同2.6，但不支持027这种语法<br/>

**2. 如何将一个十六进制字符串转为整数**<br/>
```python
>>> int("a", 16)
10
>>> int("0xa",16)
10
```
[doc](http://docs.python.org/2/library/sys.html)<br/>

**3. 如何强制使用浮点数除法**<br/>
可以使用*future*<br/>
```python
>>> from __future__ import division
>>> a = 4
>>> b = 6
>>> c = a / b
>>> c
0.66666666666666663
```
或者转换,如果除数或被除数是浮点数，那么结果也是浮点数<br/>
```python
c = a / float(b)
```

**4. 如何将一个整数转为字符串**<br/>
```python
>>> str(10)
'10'
>>> int('10')
10
```
[int()](http://docs.python.org/2/library/functions.html#int)[str()](http://docs.python.org/2/library/functions.html#str)<br/>

**5. Python的”is”语句在数字类型上表现不正常**<br/>
```python
>>> a = 256
>>> b = 256
>>> id(a)
9987148
>>> id(b)
9987148
>>> a = 257
>>> b = 257
>>> id(a)
11662816
>>> id(b)
11662828
```
编辑：这是我在Python文档中发现的，[7.2.1, “Plain Integer Objects”](https://docs.python.org/2/c-api/int.html):<br/>
> 正在执行的程序会保持一组从-5到256的数值型对象，当你创建一个在这个范围内的数字，你实际上得到了一个已存在的对象的反馈。所以改变1的数值是有可能的，我猜因此这种表现在Python里还定义。<br/>

**6. 将浮点型数字的小数限制为两位**<br/>
此问题有过整理，即计算机二进制计算，所以它并不是平时所谓的十进制的四舍五入。<br/>
你陷入了一个浮点型数据的很老的错误，即所有的数字都不能表示。命令行只能告诉你内存中的全长小数。在浮点里你四舍五入到一个同样的数字。自从计算机是二进制开始，他们把浮点数保存为整数然后除一个2的幂。两位精确的数字有53比特（16位）的精度，常规的浮点数有24比特（8位）的精度。[floating point in python uses double precision](https://docs.python.org/2/tutorial/floatingpoint.html)保存值。<br/>
```python
>>>125650429603636838/(2**53)
13.949999999999999

>>> 234042163/(2**24)
13.949999988079071

>>> a=13.946
>>> print(a)
13.946
>>> print("%.2f" % a)
13.95
>>> round(a,2)
13.949999999999999
>>> print("%.2f" % round(a,2))
13.95
>>> print("{0:.2f}".format(a))
13.95
>>> print("{0:.2f}".format(round(a,2)))
13.95
>>> print("{0:.15f}".format(round(a,2)))
13.949999999999999
```
作为货币如果你只需要小数点后两位的位置，那么你有一对比较好的选择，用整数存储值为分而不是元，之后除以100来得到元。或者用修正过的小数，比如[decimal](https://docs.python.org/2/library/decimal.html)<br/>

**7. 在Python中如何表示逻辑的异或**<br/>
如果你已经将输入值设为布尔型，那么 != 就是异或。<br/>
```python
bool(a) != boll(b)
```

**8. 列出小于N的所有质数的最快方法**<br/>
警告：timeit可能会根据硬件或Python的版本而产生不同的结果。<br/>
[问题](http://stackoverflow.com/questions/2068372/fastest-way-to-list-all-primes-below-n)嗯，目前不能参悟此问题，问题链接放在这里。<br/>

**9. 在Python中，自增和自减操作符的表现**<br/>
首先，Python中并没有*++*或者*--*。*++*不是一个操作符。只是两个*+*操作符, *+*操作符是一个同一性的操作符，什么都做。（解释一下：*+*和*-*作为一元操作符，只对数字起效，但是我假定你不会期望*++*操作符对于字符串也起效）<br/>
```python
++count  其实就是 +(+count)
```
在Python中，可以使用*+=*操作符来实现你的需求：<br/>
```python
count += 1
```
我怀疑*++*和*--*的存在是为了一致性和简单性。我不能确定Guido van Rossum做这个决定的原因，但是我猜测有以下原因：<br/>
- 只是简单的解析。严格来说，*++count*是模糊的，因为它是*+*,*+*,*count*（两个一元*+*操作符）简化为*++*和*count*（只是一元的*++*操作符），不是什么严重的语病，但是他确实存在。<br/>
- 简单说。*++*只不过是*+=* 1的同义词。它是一个简短的发明，因为C编译器太蠢不能把*a += 1*优化为几乎所有电脑都有的*inc*。这个年代，优化编译器和字节码的解释语言，向语言中添加操作符允许程序员优化他们的代码这种事情通常会引起不满，尤其是像Python这种设计为解释和阅读型的语言。<br/>
- 混乱的副作用。在含有*++*的语言中一个常见的新手错误就是混合了它和pre-，post-自增/自减操作符的区别（无论是优先级，还是返回值），而且Python喜欢消除一些语言陷阱。前期增量后期增量的优先级在C语言中表现的非常糟糕，而且难以置信的容易出错。<br/>

## 列表
**1. 序列的切片操作**<br/>
```python
a[start:end] # start 到 end-1
a[start:]    # start 到 末尾
a[:end]      # 0 到 end-1
a[:]         # 整个列表的拷贝
```
还有一个step变量，控制步长,可在上面语法中使用<br/>
```python
a[start:end:step] # start through not past end, by step
```
注意，左闭右开。其他特点，开始或结束下标可能是负数，表示从序列末尾开始计算而非从头开始计算,所以：<br/>
```python
a[-1]    # 最后一个元素
a[-2:]   # 最后两个元素
a[:-2]   # 除了最后两个元素
```
Python对程序员很友好，如果序列中存在的元素数量少于你要的，例如，你请求 a[:-2] 。但是a只有一个元素，你会得到一个空列表，而不是一个错误.有时候你或许希望返回的是一个错误，所以你必须知道这点。<br/>

**2. 判断一个列表为空得最佳实践**<br/>
```python
if not a:
    print "List is empty"
#不要用len(a)来判断
```

**3. 如何合并两个列表**<br/>
```python
listone = [1,2,3]
listtwo = [4,5,6]
#outcome we expect: mergedlist == [1, 2, 3, 4, 5, 6]
```
- 不考虑顺序（原来问题不是很明确）<br/>
```python
listone + listtwo
#linstone.extend(listtwo)也行，就是会修改listone
```
- 考虑顺序做些处理<br/>
```python
>>> listone = [1,2,3]
>>> listtwo = [4,5,6]
>>> import itertools
>>> for item in itertools.chain(listone, listtwo):
...     print item
1
2
3
4
5
6
```

**4. 如何获取一个列表的长度**<br/>
python中是不是只有这种方法可以获取长度？语法很奇怪<br/>
```python
arr.__len__()
```
应该使用这种方式<br/>
```python
mylist = [1, 2, 3, 4, 5]
len(mylist)
```
这样做法，不需要对每个容器都定义一个.length()方法，你可以使用len()检查所有实现了len()方法的对象<br/>

**5. Python中如何复制一个列表**<br/>
可以用切片的方法<br/>
```python
new_list = old_list[:]
```
可以使用list()函数<br/>
```python
new_list = list(old_list)
```
可以使用copy.copy(),比list()稍慢，因为它首先去查询old_list的数据类型<br/>
```python
import copy
mew_list = copy.copy(old_list)
```
如果列表中包含对象，可以使用copy.deepcopy(), 所有方法中最慢，但有时候无法避免<br/>
```python
import copy
mew_list = copy.deepcopy(old_list)
```
例子：<br/>
```python
import copy
class Foo(object):
    def __init__(self, val):
        self.val = val

    def __repr__(self):
        return str(self.val)
foo = Foo(1)

a = ['foo', foo]
b = a[:]
c = list(a)
d = copy.copy(a)
e = copy.deepcopy(a)

# edit orignal list and instance
a.append('baz')
foo.val = 5

print "original: %r\n slice: %r\n list(): %r\n copy: %r\n deepcopy: %r" \
       % (a, b, c, d, e)
```
结果：<br/>
```python
original: ['foo', 5, 'baz']
slice: ['foo', 5]
list(): ['foo', 5]
copy: ['foo', 5]
deepcopy: ['foo', 1]
```
效率简单比较：<br/>
```python
10.59 - copy.deepcopy(old_list)
10.16 - pure python Copy() method copying classes with deepcopy
1.488 - pure python Copy() method not copying classes (only dicts/lists/tuples)
0.325 - for item in old_list: new_list.append(item)
0.217 - [i for i in old_list] (a list comprehension)
0.186 - copy.copy(old_list)
0.075 - list(old_list)
0.053 - new_list = []; new_list.extend(old_list)
0.039 - old_list[:] (list slicing)
```

**5. 列表的append和extend的区别**<br/>
```python
>>> x = [1, 2]
>>> x.append(3)
>>> x
[1, 2, 3]
>>> x.append([4,5])
>>> x
[1, 2, 3, [4, 5]]
>>>
>>> x = [1, 2, 3]
>>> x.extend([4, 5])
>>> x
[1, 2, 3, 4, 5]
```

**6. 如何随机地从列表中抽取变量**<br/>
```python
foo = ['a', 'b', 'c', 'd', 'e']
from random import choice
print choice(foo)
```

**7. 如何利用下标从列表中删除一个元素**<br/>
- del<br/>
```python
In [9]: a = range(10)
In [10]: a
Out[10]: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
In [11]: del a[-1]
In [12]: a
Out[12]: [0, 1, 2, 3, 4, 5, 6, 7, 8]
```
- pop(pop可根据下标来删除)<br/>
```python
a = ['a', 'b', 'c', 'd']
a.pop(1)
# now a is ['a', 'c', 'd']

a = ['a', 'b', 'c', 'd']
a.pop()
# now a is ['a', 'b', 'c']
```

**8. 获取列表的最后一个元素**<br/>
```python
result = l[-1]
result = l.pop() #pop居然可以这么用，删掉l最后一个元素同时，赋值给result
```

**9. 如何将一个列表切分成若干个长度相同的子序列**<br/>
想要得到这样的效果：<br/>
```python
l = range(1, 1000)
print chunks(l, 10) -> [ [ 1..10 ], [ 11..20 ], .., [ 991..999 ] ]
```
使用yield:<br/>
```python
def chunks(l, n):
    """ Yield successive n-sized chunks from l.
    """
    for i in xrange(0, len(l), n):
        yield l[i:i+n]
list(chunks(range(10, 75), 10))
```
直接处理<br/>
```python
def chunks(l, n):
    return [l[i:i+n] for i in range(0, len(l), n)]
```

**10. 如何删除一个list中重复的值同时保证原有顺序**<br/>
最快的方法：<br/>
```python
def f7(seq):
    seen = set()
    seen_add = seen.add
    return [x for x in seq if x not in seen and not seen_add(x)]
```
如果你需要在同一个数据集中多次是哦那个这个方法，或许你可以使用[ordered set](http://code.activestate.com/recipes/528878/)处理。插入，删除和归属判断复杂度都是O(1)。<br/>

**11. 如何在遍历一个list时删除某些元素**<br/>
使用列表解析<br/>
```python
somelist = [x for x in somelist if determine(x)]
```
上面那个操作将产生一个全新的somelist对象，而失去了对原有somelist对象的引用<br/>
```python
#在原有对象上进行修改
somelist[:] = [x for x in somelist if determine(x)]
```
使用itertools<br/>
```python
from itertools import ifilterfalse
somelist[:] = list(ifilterfalse(determine, somelist))
```

**12. 如何获取list中包含某个元素所在的下标**<br/>
```python
>>> ["foo","bar","baz"].index('bar')
1
```
[doc](http://docs.python.org/2/tutorial/datastructures.html#more-on-lists)

**13. 如何扁平一个二维数组**<br/>
```python
problem:
l = [[1,2,3],[4,5,6], [7], [8,9]]
变为[1, 2, 3, 4, 5, 6, 4, 5, 6, 7, 8, 9]
```
列表解析<br/>
```python
[item for sublist in l for item in sublist]
```
itertools<br/>
```python
[item for sublist in l for item in sublist]
```
itertools<br/>
```python
>>> import itertools
>>> list2d = [[1,2,3],[4,5,6], [7], [8,9]]
>>> merged = list(itertools.chain(*list2d))

# python >= 2.6
>>> import itertools
>>> list2d = [[1,2,3],[4,5,6], [7], [8,9]]
>>> merged = list(itertools.chain.from_iterable(list2d))
```
sum<br/>
```python
sum(l, []) #碉堡了
```

**14. 列表解析和map**<br/>
更喜欢使用map()而不是列表解析的原因是什么？<br/>
在某些情况下，map性能更高一些(当你不是为了某些目的而使用lambda，而是在map和列表解析中使用相同函数).列表解析在另一些情况下性能更好，并且大多数pythonistas认为这样更简洁直接<br/>
使用相同函数，略微优势<br/>
```python
$ python -mtimeit -s'xs=range(10)' 'map(hex, xs)'
100000 loops, best of 3: 4.86 usec per loop
$ python -mtimeit -s'xs=range(10)' '[hex(x) for x in xs]'
100000 loops, best of 3: 5.58 usec per loop
```
相反情况，使用lambda<br/>
```python
$ python -mtimeit -s'xs=range(10)' 'map(lambda x: x+2, xs)'
100000 loops, best of 3: 4.24 usec per loop
$ python -mtimeit -s'xs=range(10)' '[x+2 for x in xs]'
100000 loops, best of 3: 2.32 usec per loop
```

**15. 列表和元组有什么区别**<br/>
除了元组是不可变的之外，还有一个语义的区别去控制它们的使用。元组是各种结构类型混杂的（比如，它们的入口有不同的含义），列表则是一致的序列。元组有结构，列表有顺序。<br/>
根据这种据别，刻意让代码更为明确和易理解。<br/>
用页数和行数来定义一本书中的位置的例子：<br/>
```python
my_location = (42, 11)  # page number, line number
```
然后你可以在一个字典里把这个当做键保存位置的记录。然而列表可以用来存储多点定位。通常会想添加或移除一个列表中的某个定位，所以列表是可变的。另一方面，为了保持行数的完好无损，而改变页书的值似乎说不通，这很可能会给你一个新的定位。而且，可能有很完美的解决办法用来处理正确的行数（而不是替换所有的元组）<br/>
这一点，有很多有趣的文章，比如[”Python Tuples are Not Just Constant Lists” ](http://jtauber.com/blog/2006/04/15/python_tuples_are_not_just_constant_lists/)or [“Understanding tuples vs. lists in Python”](http://news.e-scribe.com/397)。Python官方文档也提到了[“元组是不可变对象，并且用于包含哪些混杂的序列…”](https://docs.python.org/2/tutorial/datastructures.html#tuples-and-sequences)。<br/>
在一个动态类型语言比如haskell，元组通常有不同的类型，并且元组的长度必须是固定的。在列表中值必须是同一种类型，值长度不需要固定。所以区别很明显。<br/>
最后，Python中还有一个[nametuple](https://docs.python.org/dev/library/collections.html#collections.namedtuple)，合理表示一个元组已经有的结构。这些突出的想法明确了元组是类和实例的一种轻型的替换。<br/>

**16. Python 2.7里[...]是什么？**<br/>
[问题](http://stackoverflow.com/questions/17160162/what-is-in-python-2-7).建议观看原问题链接<br/>
它意味着你在它的内部创建了一个无限嵌套的不能打印的列表。p包含一个包含p的p...等等。[...]就是一种让你知道问题的标记，为了通告信息，它不能被而代表。<br/>

## 元组
**1. Python中的命名元组是什么？**<br/>
命名元组基本上是一种易创建，轻量的对象类型。命名元组实例像值引用一样被对象引用或者使用基础的元组语法。他们可以像*struct*或者其他常用的记录类型一样被使用，除了他们是不可变的。他们在Python 2.6和Python 3.0中被加入。尽管有一个在[Python2.4中实现的秘诀](http://code.activestate.com/recipes/500261/)<br/>
举个例子，通常我们要顶一个点，用元组表示*(x, y)*。用代码写出来像下面这样：<br/>
```python
pt1 = (1.0, 5.0)
pt2 = (2.5, 1.5)

from math import sqrt
line_length = sqrt((pt1[0]-pt2[0])**2 + (pt1[1]-pt2[1])**2)
```
使用命名元组可以更可读：<br/>
```python
from collections import namedtuple
Point = namedtuple('Point', 'x y')
pt1 = Point(1.0, 5.0)
pt2 = Point(2.5, 1.5)

from math import sqrt
line_length = sqrt((pt1.x-pt2.x)**2 + (pt1.y-pt2.y)**2)
```
然而，命名元组仍然向下兼容普通的元组，所以下面的代码仍然有效：<br/>
```python
Point = namedtuple('Point', 'x y')
pt1 = Point(1.0, 5.0)
pt2 = Point(2.5, 1.5)

from math import sqrt
# use index referencing
line_length = sqrt((pt1[0]-pt2[0])**2 + (pt1[1]-pt2[1])**2)
 # use tuple unpacking
x1, y1 = pt1
```
因此，如果你认为对象标记可以让你的代码更Pythonic、更可读，那么请将所有的元组替换为命名元组。我个人已经开始使用命名元组去替代非常简单的值类型，尤其是那些要作为参数传入函数的东西。它让函数不用知道元组的包装内容，更加具有可读性。<br/>
长远点说，你还可以用命名元组替代不含方法的普通不可变类。你还可以把你的命名元组类型作为基类：<br/>
```python
class Point(namedtuple('Point', 'X y')):
    [...]
```
当然，作为元组，命名元组中的属性仍然是不可变的：<br/>
```python
>>> Point = namedtuple('Point', 'x y')
>>> pt1 = Point(1.0, 5.0)
>>> pt1.x = 2.0
AttributeError: can't set attribute
```
如果你想改变这些值，你需要另一个类型。有一个方便的方法[mutable recordtypes](http://code.activestate.com/recipes/576555/)，它允许你把修改属性为新的值。<br/>
```python
>>> from rcdtype import *
>>> Piont = recordtype('Point', 'x y')
>>> pt1 = Ponit(1.0, 5.0)
>>> pt1.x = 2.0
>>> print(pt1[0])
    2.0
```
我仍然不知道有任何”命名列表“的结构，可以让你添加新的区块。这种情况下你可能只是需要用一个字典。命名元组可以转化成字典，用*pt1._asdict()*可以返回*{'x': 1.0, 'y': 5.0}*而且升级为可以使用所有字典常用的函数。<br/>
关于更多，请看[doc](https://docs.python.org/3/library/collections.html#collections.namedtuple)

## 字典
**1. 使用列表解析创建一个字典**<br/>
python 2.6<br/>
```python
d = dict((key, value) for (key, value) in sequence)
```
python 2.7 or python 3.x, 使用[字典解析语法](http://www.python.org/dev/peps/pep-0274/)<br/>
```python
d = {key: value for (key, value) in sequence}
```

**2. 使用"in"还是"has_key()"**<br/>
```python
d = {'a':1, "b":2}
'a' in d
True
or:
d = {'a':1, 'b':2}
d.has_key('a')
True
```
in更pythonic, 另外has_key()在Python3.x中已经被移除.另外根据网上的一些测试，in的速度要优于has_key()。<br/>

**4. 字典默认值**<br/>
```python
#获取时,如不存在，得到默认值
d.get(key, 0)
#设置时，若key不存在，设置默认值，已存在，返回已存在value
d.setdefault(key, []).append(new_element)
#初始即默认值
from collections import defaultdict
d = defaultdict(lambda: 0)
#or d = defaultdict(int)
```

**5. 如何给字典添加一个值**<br/>
```python
#### Making a dictionary ####
data = {}
# or #
data = dict()

#### Initially adding values ####
data = {'a':1, 'b':2, 'c':3}
# or #
data = dict(a=1, b=2, c=3)

#### Inserting/Updating value ####
data['a'] = 1 # updates if 'a' exists, else adds 'a'
# OR #
data.update({'a':1})
# OR #
data.update(dict(a=1))

#### Merging 2 dictionaries ####
data.update(data2)
```

**6. 如何将字段转换成一个object，然后使用对象-属性的方式读取**<br/>
```python
# 有dict
>>> d = {'a': 1, 'b': {'c': 2}, 'd': ["hi", {'foo': "bar"}]}}
# 想用这种方式访问
>>> x = dict2obj(d)
>>> x.a
1
>>> x.b.c
2
>>> x.d[1].foo
bar
```
使用namedtuple:<br/>
```python
>>> from clooections import namedtuple
>>> MyStruct = namedtuple('MyStruct', 'a b d')
>>> s = MyStruct(a=1, b={'c':2}, d=['hi'])
>>> s
MyStruct(a=1, b={'c':2}, d=['hi'])
>>> s.a
1
>>> s.c
>>> s.d
['hi']
```
使用类<br/>
```python
class Struct:
    def __init__(self, **entries):
        self.__dict__.update(entries)

>>> args = {'a':1, 'b':2}
>>> s = Struct(**args)
>>> s
<__main__.Struct instance at 0x01D6A738>
>>> s.a
1
>>> s.b
2
```

**7. 如何在单一表达式中合并两个Python字典**<br/>
```python
#问题：
>>> x = {'a':1, 'b': 2}
>>> y = {'b':10, 'c': 11}
>>> z = x.update(y)
>>> print z
None
>>> x
{'a': 1, 'b': 10, 'c': 11}
#我想要最终合并结果在z中，不是x，我要怎么做？
```
这种情况下，可以使用<br/>
```python
z = dict(x.items() + y.items())
```
这个表达式将会实现你想要的，最终结果z，并且相同key的值，将会是y中key对应的值<br/>
```python
>>> x = {'a':1, 'b': 2}
>>> y = {'b':10, 'c': 11}
>>> z = dict(x.items() + y.items())
>>> z
{'a': 1, 'c': 11, 'b': 10}
```
如果在Python3中,会变得有些复杂<br/>
```python
>>> z = dict(list(x.items()) + list(y.items()))
>>> z
{'a': 1, 'c': 11, 'b': 10}
```

**8. 如何映射两个列表成为一个字典**<br/>
```python
#两个列表
keys = ('name', 'age', 'food')
values = ('Monty', 42, 'spam')
#如何得到
dict = {'name' : 'Monty', 'age' : 42, 'food' : 'spam'}
```
使用zip<br/>
```python
>>> keys = ['a', 'b', 'c']
>>> values = [1, 2, 3]
>>> d = dict(zip(keys, values))
>>> print d
{'a': 1, 'b': 2, 'c': 3}
```

**9. 排序一个列表中的所有dict，根据dict内值**<br/>
```python
#如何排序如下列表，根据name或age
[{'name':'Homer', 'age':39}, {'name':'Bart', 'age':10}]
```
简单的做法；<br/>
```python
newlist = sorted(list1, key=lambda k:k['name'])
```
高效的做法：<br/>
```python
from operator import itemgetter
newlist = sorted(list1, key=itemgetter('name'))
```

**10. 根据值排序一个字典**<br/>
```python
import operator
x = {1:2, 3:4, 4:3, 2:1, 0:0}
sorted_x = sorted(x.iteritems(), key=operator.itemgetter(1))
#[(0, 0), (2, 1), (1, 2), (4, 3), (3, 4)]
#dict(sorted_x) == x
```

**11. 如何将自定义对象作为字典键值**<br/>
```python
class MyThing:
    def __init__(self, name, location, length):
        self.name = name
        self.location = location
        self.length = length

    def __hash__(self):
        return hash((self.name, self.location))

    def __eq__(self, other)
        return (self.name, self.location) == (other.name, other.location)
```

