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

> 进阶  

- [函数](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#函数)
- [内置函数](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#内置函数)
- [异常](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#异常)
- [模块](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#模块)
- [标准库](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#标准库)
- [日期](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#日期)
- [oop](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#oop)

> 其他  

- [pip/easy_install](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#pip/easy_install)
- [其他](https://github.com/GJBLUE/READING-/blob/master/STACKOVERFLOW%20PYTHON.md#其他)


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

## 函数
**1. 如何获取一个函数的函数名字符串**<br/>
```python
my_function.__name__
>>> import time
>>> time.time.__name__
'time'
```

**2. 用函数名字符串调用一个函数**<br/>
假设模块foo有函数bar:<br/>
```python
import foo
methodToCall = getattr(foo, 'bar')
result = methodToCall()
# or
result = getattr(foo, 'bar')()
```

**3. Python中\*和\*\*的作用**<br/>
\*args和\*\*kwargs允许允许函数拥有任意数量的参数，具体可以查看[more on defining functions](http://docs.python.org/dev/tutorial/controlflow.html#more-on-defining-functions)<br/>
\*args将函数所有参数转为序列<br/>
```python
In [1]: def foo(*args):
...:     for a in args:
...:         print a

In [2]: foo(1)
1

In [4]: foo(1,2,3)
1
2
3
```
\*\*kwargs 将函数所有关键字参数转为一个字典<br/>
```python
In [5]: def bar(**kwargs):
...:     for a in kwargs:
...:         print a, kwargs[a]

In [6]: bar(name="one", age=27)
age 27
name one
```
两种用法可以组合使用<br/>
```python
def foo(kind, *args, **kwargs):
    pass
```
\*的另一个用法是用于函数调用时的参数列表解包(unpack)<br/>
```python
def foo(kind, *args, **kwargs):
    pass
```
\*l的另一个用法是用于函数调用时的参数列表解包(unpack)<br/>
```python
In [9]: def foo(bar, lee):
...:     print bar, lee

In [10]: l = [1,2]

In [11]: foo(*l)
1 2
```
在Python3.0中，可以将*l放在等号左边用于赋值 Extended Iterable Unpacking<br/>
```python
first, *rest = [1,2,3,4]
first, *l, last = [1,2,3,4]
```

**4. 在Python中使用\*\*kwargs的合适方法**<br/>
当存在默认值的时候，如何合适的使用\*\*kwargs?<br/>
\*\*kwargs返回一个字典，但是这是不是设置默认值的最佳方式？还是有其他方式？我只能将其作为一个字典接受，然后用get函数获取参数？<br/>
```python
class ExampleClass:
    def __init__(self, **kwargs):
        self.val = kwargs['val']
        self.val2 = kwargs.get('val2')
```
你可以用get函数给不在字典中的key传递一个默认值<br/>
```python
self.val2 = kwargs.get('val2', "default value")
```
但是，如果你计划给一个特别的参数赋默认值，为什么不在前一个位置使用命名参数？<br/>
```python
def __init__(self, val2="default value", **kwargs):
```

**5. 构造一个基本的Python迭代器**<br/>
python中的迭代器对象遵守迭代器协议,也就意味着python会提供两个方法:iter() 和 next().方法iter 返回迭代器对象并且在循环开始时隐含调用.方法next()返回下一个值并且在每次循环中隐含调用.方法next()在没有任何值可返回时,抛出StopIteration异常.之后被循环构造器捕捉到而停止迭代.<br/>
下面是简单的计数器例子:<br/>
```python
class Counter:
    def __init__(self, low, high):
        self.current = low
        self.high = high

    def __iter__(self):
        return self

    def next(self): # Python 3: def __next__(self)
        if self.current > self.high:
            raise StopIteration
        else:
            self.current += 1
            return self.current - 1
#输出
3
4
5
6
7
8
```
使用生成器会更简单,包含了先前的回答:<br/>
```python
def counter(low, high):
    current = low
    while current <= high:
        yield current
        current += 1

for c in counter(3, 8):
    print c
```
输出是一样的.本质上说,生成器对象支持迭代协议并且大致完成和计数器类相同的事情.David Mertz的文章, [Iterators and Simple Generators](http://www.ibm.com/developerworks/library/l-pycon.html),是一个非常不错的介绍.<br/>

**6. \*args和\*\*kwargs是什么**<br/>
真正的语法是*\**和*\*\**，*\*args*和*\*\*kwargs*这两个名字只是约定俗成的，但并没有硬性的规定一定要使用它们。<br/>
当你不确定有多少个参数会被传进你的函数时，你可能会使用*\*args*，也就是说它允许你给你的函数传递任意数量的参数，举个例子：<br/>
```python
>>> def print_everything(*args):
        for count, thing in enumerate(args):
            print '{0}. {1}'.format
>>> print_everthing(‘apple’, ‘banana’, ‘cabbage’)
0. apple
1. banana
2. cabbage
```
类似的，*\*\*kwargs*允许你处理那些你没有预先定义好的已命名参数<br/>
```python
>>> def table_everything(*args):
        for name, value in kwargs.items():
            print ‘{0} = {1}’.format(name, value)

>>> table_everthing(apple = ‘fruit’, cabbage = ‘vegetable’)
cabbage = vegetable
apple = fruit
```
你一样可以使用这些命名参数。明确的参数会优先获得值，剩下的都会传递给*\*args*和*\*\*kwargs*，命名参数排在参数列表前面，举个例子：<br/>
```python
def table_things(titlestring, **kwargs)
```
你可以同时使用它们，但是*\*args*必须出现在*\*\*kwargs*之前。调用一个函数时也可以用\*和\*\*语法，举个例子：<br/>
```python
>>> def print_three_things(*args):
            print ‘a = {0}, b = {1}, c = {2}’.format(a, b , c)

>>> mylist = [‘aardvark’, ‘baboon’, ‘cat’]
>>> print_three_things(*mylist)
a = aardvark, b = baboon, c = cat
```
正如你看到的，它得到了一个list或者tuple，并解包这个list。通过这种方法，它将这些元素和函数中的参数匹配。当然，你可以同时在函数的定义和调用时使用*\**。<br/>

**7. 在Python中，如何干净、pythonic的实现一个复合构造函数**<br/>
实际上 **None**比“魔法”值好多了：<br/>
```python
class Cheese():
    def __init__(self, num_holes = None):
        if (num_holes is None):
```
现在如果你想完全自由地添加更多参数：<br/>
```python
class Cheese():
    def __init__(self, *args, **kwargs):
        #args -- tuple of anonymous arguments
        #kwargs -- dictionary of named arguments
        self.num_holes = kwargs.get('num_holes',random_holes())
```
[http://docs.python.org/reference/expressions.html#calls](http://docs.python.org/reference/expressions.html#calls)<br/>

**8. 为什么在Python的函数中，代码运行速度更快**<br/>
你可能要问为什么在存储在本地变量的比全局变量运行速度更快。这是一个CPython执行细节。<br/>
记住Cpython在解释器运行时，是编译成字节编码的。当一个函数编译完成，本地变量就全部被存储在一个固定长度的数组中了（而不是字典）而且名字被指定了索引。这是合理的，因为你不能自动添加本地变量到你的函数中去。在指针中循环检索一个本地变量加入到列表中，并且计算琐碎的**PyObject**的增加。<br/>
不同的是全局查找（**LOAD_GLOBAL**），是一个涉及哈希查找的字典等等。顺带的，这就是为什么当你需要一个全局变量时，要说明**global i**。如果你曾经在一个范围内给一个变量赋值了，那么编译器会为它的入口发布一些**STORE_FAST**。除非你告诉它不要这样做。<br/>
顺便说一句，全局查找仍然是非常棒的。属性查找foo.bar是非常慢的。<br/>

**9. 如果把一个变量作为引用传入**<br/>
[问题](http://stackoverflow.com/questions/986006/how-do-i-pass-a-variable-by-reference)出在[pass by assignment](https://docs.python.org/3/faq/programming.html#how-do-i-write-a-function-with-output-parameters-call-by-reference),它背后的原理可以分为两部分：<br/>
```python
1、 传入的参数实际上是一个对象的引用（但是引用的是值）
2、 有些数据类型是可变的的，有些不是
```
所以:<br/>
- 如果你向方法中传递了一个可变的对象，那么方法得到了这些对象的一个引用，只要你开心，就可以随意改变它。但是如果你在方法中重新定义了引用，外部是不知道的，所以当你改变了它，其他的引用仍然指向根对象。<br/>
- 如果你向方法中传递了一个不可变对象，那么你不会重新定义外部的引用，你甚至不能改变对象。<br/>
我们做一些示例，让它们更清晰。<br/>
**列表-一种可变类型**<br/>
```python
def try_to_change_list(the_list):
    print 'got', the_list
    the_list.append('four')
    print 'changed to', the_list

outer_list = ['one', 'two', 'three']

print 'before, outer_list = ', outer_list
try_to_change_list(outer_list)
print 'after, outer_list = ', outer_list
#输出
before, outer_list = ['one', 'two', 'three']
got ['one', 'two', 'three']
changed to ['one', 'two', 'three', 'four']
after, outer_list = ['one', 'two', 'three', 'four']
```
一个参数传入的是**outer_list**的一个引用，而不是它的复制，我们可以使用改变列表的方法去改变它，并且把改变反馈给其他的范围。<br/>
现在让我们看一下当我们试着改变这个作为参数传入的引用：<br/>
```python
def try_to_change_list_reference(the_list):
    print 'got', the_list
    the_list = ['and', 'we', 'can', 'not', 'lie']
    print 'set to', the_list

outer_list = ['we', 'like', 'proper', 'English']

print 'before, outer_list =', outer_list
try_to_change_list_reference(outer_list)
print 'after, outer_list =', outer_list
#输出
before, outer_list = ['we', 'like', 'proper', 'English']
got ['we', 'like', 'proper', 'English']
set to ['and', 'we', 'can', 'not', 'lie']
after, outer_list = ['we', 'like', 'proper', 'English']
```
**the_list**参数传递的是值，重新定义一个新的列表，对于方法外部的代码，没有任何影响。**the_list**只是**outer_list**的一个引用，我们让**the_list**指向了一个新的列表，但是我们没有办法修改**outer_list**的指向。<br/>
**字符串-不可变类型**<br/>
它是不可变的，所以我们没有办法改变字符串的内容。我们试着改变一下引用。<br/>
```python
def try_to_change_string_reference(the_string):
    print 'got', the_string
    the_string = 'In a kingdom by the sea'
    print 'set to', the_string

outer_string = 'It was many and many a year ago'

print 'before, outer_string =', outer_string
try_to_change_string_reference(outer_string)
print 'after, outer_string =', outer_string

#输出
before, outer_string = It was many and many a year ago
got It was many and many a year ago
set to In a kingdom by the sea
after, outer_string = It was many and many a year ago
```
再一次，**the_string**参数通过值传递，定义一个新的字符串对于外部的代码是不起作用的。**the_string**是**outer_string**的一个引用的复制，我们把**the_string**指向了一个新的字符串。但是我们并没有改变**outer_string**的指向。<br/>
**我们怎么避免这些？**<br/>
像@Andrea的我回答所展示的，你可以返回一个新的值。这不会改变传入的值，但是确实可以让你得到你想要输出的信息：<br/>
```python
def return_new_string(the_string):
    new_string = something_old_string(the_string)
    return new_string

# then you could call it like
my_string = return_new_string(my_string)
```
如果你确实想避免使用一个返回的值，你可以创建一个类承载你的值，并把他传入一个函数或者用一个已知的类，像列表一样,虽然这样看起来有些笨重：<br/>
```python
def use_a_wrapper_to_simulate_pass_by_reference(stuff_to_change):
    new_string = something_to_do_with_the_old_string(stuff_to_change[0])
    stuff_to_change[0] = new_string

# then you could call it like
wrapper = [my_string]
use_a_wrapper_to_simulate_pass_by_reference(wrapper)

do_something_with(wrapper[0])
```

## 内置函数
**1. 如何flush Python的print输出**<br/>
```python
import sys
sys.stdout.flush()
```
参考：[http://docs.python.org/reference/simple_stmts.html#the-print-statement](http://docs.python.org/reference/simple_stmts.html#the-print-statement)
[http://docs.python.org/library/sys.html](http://docs.python.org/library/sys.html)
[http://docs.python.org/library/stdtypes.html#file-objects](http://docs.python.org/library/stdtypes.html#file-objects)<br/>

**2. Python如何检查一个对象是list或者tuple，但是不是一个字符串**<br/>
原来的做法是<br/>
```python
assert isinstance(lst, (list, tuple))
```
有没有更好的做法<br/>
我认为下面的方式是你需要的<br/>
```python
assert not isinstance(lst, basestring)
```
原来的方式，你可能会漏过很多像列表，但并非list/tuple的<br/>

**3. Python中检查类型的权威方法**<br/>
检查一个对象是否是给定类型或者对象是否继承于给定类型？<br/>
比如给定一个对象o,如何判断是不是一个str<br/>
检查是否是str<br/>
```python
type(o) is str
```
检查是否是str或者str的子类<br/>
```python
isinstance(o, str)
```
下面的方法在某些情况下有用<br/>
```python
issubclass(type(o), str)
type(o) in ([str] + str.__subclass__()) 
```
注意，你或许想要的是<br/>
```python
isinstance(o, basestring)
```
因为unicode字符串可以满足判定(unicode 不是str的子类，但是str和unicode都是basestring的子类)可选的，isinstance可以接收多个类型参数，只要满足其中一个即True<br/>
```python
isinstance(o, (str, unicode))
```

**4. 你是否能解释Python中的闭包**<br/>
参考文章：[Closure on closures](http://mrevelle.blogspot.com/2006/10/closure-on-closures.html)<br/>
```python
对象是数据和方法关联
闭包是函数和数据关联
```
例如：<br/>
```python
def make_counter():
    i = 0
    def counter(): # counter() is a closure
        nonlocal i
        i += 1
        return i
    return counter

c1 = make_counter()
c2 = make_counter()

print (c1(), c1(), c2(), c2())
# -> 1 2 1 2
```
其他解释(感觉英文更精准)<br/>
```python
A function that references variables from a containing scope, potentially after flow-of-control has left that scope

A function that can refer to environments that are no longer active.
A closure allows you to bind variables into a function without passing them as parameters.
```

**5. Python Lambda - why?**<br/>
Are you talking about lambda functions? Like<br/>
```python
f = lambda x: x**2 + 2*x - 5
```
非常有用, python支持函数式编程, 你可以将函数作为参数进行传递去做一些事情<br/>
```python
m = filter(lambda x: x % 3 == 0, [1, 2, 3, 4, 5, 6, 7, 8, 9])
# sets m to [3, 6, 9] 
```
这样相对于完整地函数更为简短<br/>
```python
def filterfunc(x):
    return x % 3 == 0
m = filter(filterfunc, [1, 2, 3, 4, 5, 6, 7, 8, 9])
```
当然, 这个例子你也可以使用列表解析进行处理<br/>
```python
def filterfunc(x):
    return x % 3 == 0
m = filter(filterfunc, [1, 2, 3, 4, 5, 6, 7, 8, 9])
```
当然, 这个例子你也可以使用列表解析进行处理<br/>
```python
m = [x for x in [1, 2, 3, 4, 5, 6, 7, 8, 9] if x % 3 == 0]
```
甚至是<br/>
```python
range(3, 10, 3)
```
lambda function may be the shortest way to write something out<br/>

**6. Python中str和repr的区别**<br/>
Alex总结的很好，但是，有点出乎意料的精简了。首先，让我重复一下Alex回答的重点：<br/>
- 缺省实现是没用的（很难想象，但是的确是这样的）<br/>
- **__repr__**的目的是清晰<br/>
- **__str__**的目的是可读性<br/>
- 容器的**__str__**使用已包含对象**__repr__**<br/>
**缺省实现是没用的**<br/>
这实在是奇怪，因为Python的缺省实现是为了完全的可用。然而，在这种情况下，使用缺省的**__repr__**会表现的像这样：<br/>
```python
return "%s(%r)" % (self.__class__, self.__dict__)
```
这很危险（举个例子。如果对象被引用，太容易陷入无限循环）。所以，Python选择逃避。注意有一个缺省是正确的：如果**__repr__**已经定义，而**__str__**没有，对象会表现出**__str__=__repr__**。<br/>
这意味着，简单讲：几乎所有你实现的对象都应该有一个**__repr__**函数用来理解这个对象。实现**__str__**是一个选择：如果你需要一个"良好的打印”函数（举个例子，用来表现一个生成器）<br/>
**__repr__的目标是清晰的**<br/>
让我实话实说-我并不相信调试器。我不知道如何使用任何一种调试器，而且没有认真使用过任何一款。我觉的调试器的最大错误是它们的本质--我调试错误发生在很久很久以前，超级久远。这意味着我有着宗教热情一般的相信日志。日志是一切一劳永逸的服务器系统的生命之血。Python可以轻松的记录日志：可能某些项目有特殊的包装，但是你需要的仅仅是一句：<br/>
```python
(INFO, "I am in the weird function and a is", a, "and b is", b, "but I got a null C — using default", default_c)
```
但是你需要做最后一步，所有你实现的对象有一个有效的repr，所以上面这种代码才会工作。这就是为什么"eval"这种东西出现：如果你又足够的信息可以让**eval(repr(c)) == c**，这意味着你知道所有的东西，像知道**c**一样。如果它足够简单，至少通过模糊的方法，实现它。如果不是这样的，请无论如何了解c的足够信息。我通常使用一个类eval的格式：**"My class(this=%r, that=%r)" % (self.this, self.that)"**。这不意味着你可以真正的结构化MyClass或者他们全都是正确的结构器参数。但是他们是很有用的形式代表“这个实例中你需要了解的信息就是这些”。<br/>
注意：我用的是**%r**而不是**%s**。你总是想用**repr()**[或者**%r**格式等效的角色]在一个可实现的**__repr__**中，或者你被repr的目的打败的。你需要知道**MyClass(3)**和**Myclass("3")**的区别。<br/>
**__str__的目标是可读性**<br/>
实际上，它不是为了更清晰--注意**str(3) == str("3")**。同样的，如果你实现了一个IP的抽象，有一个字符串看上去像192.168.1.1是Ok的。当实现一个日期或时间的抽象时，字符串可以是“2014/4/12 15:35:33"等等。所以它的目标是通过一种方式让用户，而不是一个程序员，可以正常的阅读它。去掉那些无用的字码，假装成其他的类--在它支持可读性之后，这是一种进化。<br/>
容器的**__str__**使用已包含对象**__repr__**<br/>
这看上去很奇怪，是不是？确实有一点，但是可读性是这样吗？：<br/>
```python
[moshe is, 3, hello world, this is a list, oh I don't know, containing just 4 elements]
```
不一定。尤其是，容器内部的字符串会找到一个很容易实现的方式去构建它们所代表的东西。面对这种含糊不清的东西，记住，Python禁止猜测。如果你打印一个列表，想要上述的表现形式时，只需要<br/>
```python
print "["+", ".join(l)+"]"
```
**总结**<br/>
为你所有实现的类，实现**__repr__**。这应该是第二特性。实现**__str__**，对于那些你认为使用可视化字符串会更好的表现出错误的可读性，并阻止含糊不清的东西。<br/>

## 异常
**1. Python中声明exception的方法**<br/>
在python2.6中定义异常得到警告<br/>
```python
>>> class MyError(Exception):
...     def __init__(self, message):
...         self.message = message

>>> MyError("foo")
_sandbox.py:3: DeprecationWarning: BaseException.message has been deprecated as of Python 2.6
```
问题很长，大意如标题<br/>
回答，或许我理解错了，但是为什么不这样做<br/>
```python
class MyException(Exception):
    pass
```
如果要重写什么，例如传递额外参数，可以这么做<br/>
```python
class ValidationError(Exception):
    def __init__(self, message, Errors):
        # Call the base class constructor with the parameters it needs
        Exception.__init__(self, message)

        # Now for your custom code...
        self.Errors = Errors
```
你可以通过第二个参数传递error 字典, 之后通过e.Errors获取<br/>

**2. 如何人为地抛出一个异常**<br/>
```python
# pythonic
raise Exception("I know python!")
```
更多可参考[文档](http://docs.python.org/2/reference/simple_stmts.html#the-raise-statement)

**3. 如何一行内处理多个异常**<br/>
我知道可以这么做<br/>
```python
try:
    # do something that may fail
except:
    # do this if ANYTHING goes wrong
```
也可以<br/>
```python
try:
    # do something that may fail
except IDontLikeYourFaceException:
    # put on makeup or smile
except YouAreTooShortException:
    # stand on a ladder
```
如果想在一行里处理多个异常的话<br/>
```python
try:
    # do something that may fail
except IDontLIkeYouException, YouAreBeingMeanException: #没生效
except Exception, e: #捕获了所有
    # say please
```
答案：<br/>
```python
# as在python2.6,python2.7中仍然可以使用
except (IDontLIkeYouException, YouAreBeingMeanException) as e:
    pass
```

**4. Python assert最佳实践**<br/>
回答1<br/>
Assert仅用在，测试那些从不发生的情况！目的是让程序尽早失败<br/>
Exception用在，那些可以明确知道会发生的错误，并且建议总是创建自己的异常类<br/>
例如，你写一个函数从配置文件中读取配置放入字典，文件格式不正确抛出一个ConfigurationSyntaxError,同时你可以assert返回值非None<br/>
在你的例子中，如果x是通过用户接口或外部传递设置的，最好使用exception<br/>
如果x仅是同一个程序的内部代码，使用assert<br/>
回答2<br/>
这个函数是为了能够当x小于0的时候，原子性的抛出一个异常。你可以使用[class descriptors](https://docs.python.org/2/reference/datamodel.html#implementing-descriptors)有一个例子：<br/>
```python
class ZeroException(Exception):
    pass

class variable(object):
    def __init__(self, value=0):
        self.__x = value

    def __set__(self, obj, value):
        if value < 0:
            raise ZeroException('x is less than zero')

        self.__x = value

    def __get__(self, obj, objType):
        return self.__x

class MyClass(object):
    x = variable

>>> m = MyClass()
>>> m.x = 10
>>> m.x -= 20
Traceback (most recent call last):
   File "<stdin>", line 1, in <module>
   File "my.py", line 7, in __set__
      raise ZeroException('x is less than zero')
ZeroException: x is less than zero
```

**5. 如何打印到stderr**<br/>
经常这么干<br/>
```python
import sys
sys.stderr.write('spam\n')

from __futrue__ import print_function
print('spam', file=sys.stderr) 
```
但是不够pythonic, 有没有更好的方法?<br/>
回答, 我发现这种方式是最短/灵活/可扩展/可读的做法<br/>
```python
from __futrue__ import print_function
def warning(*objs):
    print("WARNING: ", *objs, file=sys.stderr)
```

##模块
**1. __init__.py是做什么用的**<br/>
这是包的一部分，[具体文档](http://docs.python.org/2/tutorial/modules.html#packages)<br/>
__init__.py让Python把目录当成包，最简单的例子，__init__.py仅是一个空文件，但它可以一样执行包初始化代码或者设置__all__变量，后续说明<br/>

**2. 如何使用绝对路径import一个模块**<br/>
```python
import imp

foo = imp.load_source('module.name', '/path/to/file.py')
foo.MyClass() 
```

**2. 获取Python模块文件的路径**<br/>
如何才能获取一个模块其所在的路径:<br/>
```python
# 回答
import a_module
print a_module.__file__
```
获取其所在目录，可以<br/>
```python
import os
path = os.path.dirname(amodule.__file__)
```

**3. 谁可以解释一下all么？**<br/>
该模块的公有对象列表, **all**指定了使用import module时，哪些对象会被import进来.其他不在列表里的不会被导入.<br/>
```python
__all__ = ["foo", "bar"]
```
it's a list of public objects of that module -- it overrides the default of hiding everything that begins with an underscore<br/>

**4. 如何重新加载一个python模块**<br/>
使用reload内置函数<br/>
```python
reload(module_name)
import foo
while True:
    # Do some things.
    if is_change(foo):
        foo = reload(foo)
```

**5. 在Python中，如何表示Enum(枚举)**<br/>
Enums已经添加进了Python 3.4，详见PEP435。同时在pypi下被反向移植进了3.3，3.2，3.1，2.7，2.6，2.5和2.4。<br/>
通过**$ pip install enum34**来使用向下兼容的Enum，下载**enum**（没有数字）则会安装完全不同并且有冲突的版本。<br/>
```python
from enum import Enum
Animal = Enum('Animal', 'ant bee cat dog')
```
等效的：<br/>
```python
class Animals(Enum):
    ant = 1
    bee = 2
    cat = 3
    dog = 4
```
在早期的版本中，实现枚举的一种方法是：<br/>
```python
def enum(**enums):
    return type('Enum', (), enums)
```
使用起来像这样：<br/>
```python
>>> Numbers = enum(ONE=1, TWO=2, THREE='three')
>>> Numbers.ONE
1
>>> Numbers.TWO
2
>>> Numbers.THREE
'three'
```
也可以轻松的实现自动列举像下面这样：<br/>
```python
def enum(*sequential, **named):
    enums = dict(zip(sequential, range(len(sequential))), **named)
    return type('Enum', (), enums)
```
使用起来像这样：<br/>
```python
>>> Numbers = enum('ZERO', 'ONE', 'TWO')
>>> Numbers.ZERO
0
>>> Numbers.ONE
1
```
支持把值转换为名字，可以这样添加：<br/>
```python
def enum(*sequential, **named):
    enums = dict(zip(sequential, range(len(sequential))), **named)
    reverse = dict((value, key) for key, value in enums.iteritems())
    enums['reverse_mapping'] = reverse
    return type('Enum', (), enums)
```
这将会根据名字重写任何东西，但是对于渲染你打印出的枚举值很有效。如果反向映射不存在，它会抛出KeyError。看一个例子：<br/>
```python
>>> Numbers.reverse_mapping[‘three’]
’THREE’
```

**6. if name == “main”做了什么？**<br/>
稍微拓展一下Harley的答案...<br/>
当Python的解释器读一个源文件时，它执行了里面能找到的所有代码。在执行之前，它会定义少数几个变量。举个例子，如果Python解释器把该模块（即源文件）当做主程序运行，它就会把特殊的__name__变量的值设置为“__main__”。如果这个文件被其他模块引用，__name__就会被设置为其他模块的名字。<br/>
就你的脚本来说，我们假设把它当做主函数来执行，你可能会在命令行上这样用：<br/>
```python
python threading_example.py
```
设置好特殊变量之后，它会执行import声明并加载其他的模块。然后它会预估def的缩进，创建一个函数对象和一个指向函数对象的值叫做myfunction。之后它将读取if语句，确定__name__等于”__main__”后，执行缩进并展示。<br/>
这样做的主要原因是，有时候你写了一个可以直接执行的模块（一个.py文件），同时，它也可以被其他模块引用。通过执行主函数检查，你可以让你的代码只在作为主程序时执行，而在被其他模块引用或调用其中的函数时不执行。<br/>
[doc](http://ibiblio.org/g2swap/byteofpython/read/module-name.html)可以看到更多的细节。<br/>

**7. 通过相对路径引用一个模块**<br/>
假设你的两个文件夹都是真实的python包（都有__init__.py文件在里面），这里有一个可以安全的把相对路径模块包含进本地的脚本。<br/>
我假设你想这样做，因为你需要在脚本中包含一系列的模块。我在许多产品的生产环境和不同的情景下用过这个：调用其他文件夹下的脚本或者在不打开一个新的解释器的情况下在Python中执行。<br/>
```python
import os, sys, inspect
# realpath() will make your script run, even if you symlink it :)
cmd_folder = os.path.realpath(os.path.abspath(os.path.split(inspect.getfile( inspect.currentframe() ))[0]))
if cmd_folder not in sys.path:
    sys.path.insert(0, cmd_folder)

# use this if you want to include modules from a subfolder
cmd_subfolder=os.path.realpath(os.path.abspath(os.path.join(os.path.split(inspect.getfile( inspect.currentframe() ))[0],"subfolder")))
if cmd_subfolder not in sys.path:
    sys.path.insert(0, cmd_subfolder)

# Info:
# cmd_folder = os.path.dirname(os.path.abspath(__file__)) # DO NOT USE__file__ !!!
# __file__ fails if script is called in different ways on Windows
# __file__ fails if someone does os.chdir() before
# sys.argv[0] also fails because it doesn't not always contains the path
```
通过这个途径，确实迫使Python使用你的模块，而不用系统自带的那些。<br/>
但是注意。在**egg**文件中的模块会发生什么我确实不知道。可能会失败。如果你知道更好的解决办法请留言，我会花几个小时去改进它。<br/>

**8. Python中如何进行间接引用**<br/>
[问题](http://stackoverflow.com/questions/72852/how-to-do-relative-imports-in-python)

## 标准库
**1. json和simplejson的区别**<br/>
json就是simplejson，加入到标准库. json在2.6加入，simplejson在2.4+,2.6+,更有优势.<br/>
另外，simplejson更新频率更高，如果你想使用最新版本，建议用simplejson.好的做法是:<br/>
```python
try:import simple as json
except ImportError:import json
```

**2. 如何用http下载一个文件**<br/>
直接使用urllib<br/>
```python
import urllib
urllib.urlretrieve("http://www.example.com/songs/mp3.mp3", "mp3.mp3")
```
使用urllib2,并提供一个进度条<br/>
```python
import urllib2
url = "http://download.thinkbroadband.com/10MB.zip"

file_name = url.split('/')[-1]
u = urllib2.urlopen(url)
f = open(file_name, 'wb')
meta = u.info()
file_size = int(meta.getheaders("Content-Length")[0])
print "Downloading: %s Bytes: %s" % (file_name, file_size)

file_size_dl = 0
block_sz = 8192
while True；
    buffer = u.read(block_sz)
    if not buffer:
        break

    file_size_dl += len(buffer)
    f.write(buffer)
    status = r"%10d  [%3.2f%%]" % (file_size_dl, file_size_dl * 100. / file_size)
    status = status + chr(8)*(len(status)+1)
    print status,

f.close()
```
使用第三方requests包<br/>
```python
>>> import requests
>>>
>>> url = "http://download.thinkbroadband.com/10MB.zip"
>>> r = requests.get(url)
>>> print len(r.content)
10485760
```

**3. argparse可选位置参数**<br/>
脚本运行 usage: installer.py dir [-h] [-v],dir是一个位置参数，定义如下:<br/>
```python
parser.add_argument('dir', default=os.getcwd())
```
我想让dir变为可选，如果未设置，使用os.getcwd(),不幸的是，当我不指定dir时，得到错误 "Error: Too few arguments"<br/>
尝试使用 nargs='?'<br/>
```python
parser.add_argument('dir', nargs='?', default=os.getcwd())
```
例子：<br/>
```python
>>> import os, argparse
>>> parser = argparse.ArgumentParser()
>>> parser.add_argument('-v', action='store_true')
_StoreTrueAction(option_strings=['-v'], dest='v', nargs=0, const=True, default=False, type=None, choices=None, help=None, metavar=None)
>>> parser.add_argument('dir', nargs='?', default=os.getcwd())
_StoreAction(option_strings=[], dest='dir', nargs='?', const=None, default='/home/vinay', type=None, choices=None, help=None, metavar=None)
>>> parser.parse_args('somedir -v'.split())
Namespace(dir='somedir', v=True)
>>> parser.parse_args('-v'.split())
Namespace(dir='/home/vinay', v=True)
>>> parser.parse_args(''.split())
Namespace(dir='/home/vinay', v=False)
>>> parser.parse_args(['somedir'])
Namespace(dir='somedir', v=False)
>>> parser.parse_args('somedir -h -v'.split())
usage: [-h] [-v] [dir]

positional arguments:
dir

optional arguments:
-h, --help  show this help message and exit
-v
```

**4. 有什么方法可以获取系统当前用户名么?**<br/>
至少在Linux和Windows下都可用.就像 os.getuid<br/>
```python
>>> os.getuid()
42
>>> os.getusername()
'slartibartfast'
```
可以看看[getpass](http://docs.python.org/2/library/getpass.html)模块<br/>
```python
>>> import getpass
>>> getpass.getuser()
'kostya'
```
可用: Unix, Windows<br/>

**5. 在Python中如何解析xml**<br/>
如何解析获取xml文件中内容:<br/>
```python
<foo>
<bar>
    <type foobar="1"/>
    <type foobar="2"/>
</bar>
</foo>
```
我建议使用[ElementTree](http://docs.python.org/2/library/xml.etree.elementtree.html)(有其他可用的实现，例如[lxml](http://lxml.de/)，他们只是更快, ElementTree提供更简单的编程api).<br/>
在使用XML建立Element实例之后，例如使用[XML](http://docs.python.org/2/library/xml.etree.elementtree.html#xml.etree.ElementTree.XML)函数<br/>
```python
for atype in e.findall('type'):
    print atype.get('foobar')
```

**6. 如何使用Python创建一个GUID**<br/>
如何使用原生的python创建一个GUID?Python2.5及以上版本,使用[uuid](http://docs.python.org/2/library/uuid.html)模块<br/>
```python
>>> import uuid
>>> uuid.uuid1()
UUID('5a35a426-f7ce-11dd-abd2-0017f227cfc7')
```

**7. Python中是否存在方法可以打印对象的所有属性和方法**<br/>
```python
# 使用pprint
from pprint import pprint
pprint (vars(your_object))
```

**8. 如何展示一个正在运行的Python应用的堆栈踪迹**<br/>
我一般在这种情况下用这个模块-当一个进程会跑很长时间，但是有时被莫名其妙而且不重复出现的原因卡住。看上去有点黑客的感觉，并且只在unix上生效（依赖信号）：<br/>
```python
import code, traceback, signal

def debug(sig, frame):
    """Interrupt running process, and provide a python prompt for
    interactive debugging."""
    d = {'_frame': frame}     # Allow access to frame object.
    d.update(frame.f_globals) # Unless shadowed by global
    d.update(frame.f_locals)

    i = code.InteractiveConsole(d)
    meaasge = "Signal received : entering python shell.\nTraceback:\n"
    message += ''.join(traceback.format_stack(frame))
    i.insert(message)

def listen():
    signal.signal(signal.SIGUSR1, debug) # Register handler
```
使用时，当你启动程序时，只需要调用listen()这个函数（你甚至可以放到site.py里，对所有Python程序都起效），让它运行起来。任何时候，都可以对进程发出一个SIGUSR1的信号，使用kill或者在Python中：<br/>
```python
os.kill(pid, signal.SIGUSR1)
```
这会使Python控制台程序在当时停止，并给你展示堆栈的踪迹，允许你操作所有的变量。使用control-d(EOF)继续运行（在发出信号的一刻，通过注释你可以打断任何I/O等等，所以它不是完全无害的）。<br/>
我还有一个可以做同样事情的脚本，唯一不同的是它可以和正在运行的进程通过pip互通（允许在后台进行程序排错等等）。粘贴到这可能太长了，我已经把它加进了[python cookbook recipe](http://code.activestate.com/recipes/576515/)。

**9. 在Python中，有什么办法杀死一个线程**<br/>
不论是在Python中还是其他语言，突然停止一个线程通常来讲是一个很不好的习惯。考虑下面的情况：<br/>
- 线程携带了非常重要的数据必须要正确的关闭<br/>
- 线程创建了其余的进程，杀死这一个，其余的也要被杀掉<br/>
如果你承担得起（比如你再操纵自己的线程），那么比较好的做法是创建一个exit_request标记，每一个线程都会定时的检查是不是轮到它退出了。<br/>
举个例子：<br/>
```python
import threading
class StoppableThread(threading.Thread):
    """Thread class with a stop() method. The thread itself has to check
    regularly for the stopped() condition."""

    def __init__(self):
        super(StoppableThread, self).__init__()
        self._stop = threading.Event()

    def stop(self):
        self._stop.set()

    def stopped(self):
        return self._stop.isSet()
```
上述代码，当你想要停止这个线程时，你可以调用stop()方法，然后等待线程退出，最好用join()。线程应该定时的检查stop这个标记。<br/>
当然，某些情况下，你有足够的理由必须杀死一个线程。比如你正在封装一个外部的库，而且它已经持续很长时间了，你需要打断它。<br/>
下面的代码允许（可能有些限制）在Python中触发一个异常。<br/>
```python
def _async_raise(tid, exctype):
    '''Raises an exception in the threads with id tid'''
    if not inspect.isClass(exctype):
        raise TypeError("Only types can be raised (not instance)")
    res = ctypes.pythonapi.PyThreadState_SetAsyncExc(tid, ctypes.py_object(exctype))

    if res == 0:
        raise ValueError("invalid thread id")
    else res != 1:
        # "if it returns a number greater than one, you're in trouble,
        # and you should call it again with exc=NULL to revert the effect"
        ctypes.pythonapi.PyThreadState_SetAsyncExc(tid, 0)
        raise SystemError("PyThreadState_SetAsyncExc failed")

class ThreadWithExc(threading.Thread):
    '''A thread class that supports raising exception in the thread from
       another thread.
    '''
    def _get_my_tid(self):
        """determines this (self's) thread id

        CAREFUL : this function is executed in the context of the caller
        thread, to get the identity of the thread represented by this
        instance.
        """
        if not self.isAlive():
            raise threading.ThreadError("the thread is not active")

        # do we have it cached?
        if hasattr(self, "_thread_id"):
           return self._thread_id 

        # no, look for it in the _active dict
        for tid, tobj in threading._active.items():
            if tobj is self:
                self._thread_id = tid
                return tid

        # TODO: in python 2.6, there's a simpler way to do : self.ident

        raise AssertionError("could not determine the thread's id")

    def raiseExc(self, exctype):
        """Raises the given exception type in the context of this thread.

        If the thread is busy in a system call (time.sleep(),
        socket.accept(), ...), the exception is simply ignored.

        If you are sure that your exception should terminate the thread,
        one way to ensure that it works is:

            t = ThreadWithExc( ... )
            ...
            t.raiseExc( SomeException )
            while t.isAlive():
                time.sleep( 0.1 )
                t.raiseExc( SomeException )

        If the exception is to be caught by the thread, you need a way to
        check that your thread has caught it.

        CAREFUL : this function is executed in the context of the
        caller thread, to raise an excpetion in the context of the
        thread represented by this instance.
        """
        _async_raise( self._get_my_tid(), exctype )
```
正如注释中所写，这并不是一个万能钥匙，如果一个线程在Python解释器之外跑着，那么它不会被抓住并打断。<br/>
上面这段代码的一个好用法，就是让线程捕获一个特殊的异常并且表现的很干净。这样才能如你所愿的利落的中断一个任务。<br/>

**10. Python中isinstance()和type()的区别**<br/>
总结一下其他人的答案（已OK的）作为内容。**isinstance**为继承者们服务（某个父类的实例也是基类的一个实例），而只检查是否相等的**type**则不是这样（他要求某个类型的id，拒绝子类型的实例，也可以叫做子类）<br/>
在Python中，通常你希望你的代码支持继承，当然（自从继承变得很容易之后，通过使用你自己的而不是使用它本身来停止某些代码是很不好的），所以**isinstance**看上去比检查id的**type**要好一些，因为它无缝支持继承。<br/>
这不是说**isinstance**就绝对好，提醒一下，只是比对比类型是否相等好一点。通常从Pythonic的角度出发，最好的解决办法是永远的鸭子类型：试着像用一个渴望得到的类型去使用某个参数，用**try/except**声明捕获所有该参数不属于我们的渴望的类型而引起的异常（或者任何类鸭子类型；-），然后在**except**从句中，试着做一些其他的事情（假设这个参数“可能是”某些类型)<br/>
**basestring**时一个奇葩，这是一个存在的意义仅仅是为了让你使用**isinstance**的内建类型（同理他们的子类**str**和**Unicode**）。字符串是有序的（你可以循环，排序，切片...），但是通常你对待它们就像标准类型--由于某些原因它不太好用（但是我们相当频繁的使用他们），原因是我们对于所有的字符串（或者其他标准类型，比如一个不能回环的东西）都用同一种方法，所有的容器（列表，集合，字典...），另一方面**basestring**加上**isinstance**可以帮你做这种事情--这个习惯用法的全部结构看起来像这样：<br/>
```python
if isinstance(x, basestring)
    return treatasscalar(x)
try:
    return treatasiter(iter(x))
except TypeError:
    return treatasscalar(x)
```
你可能会说**basestring**是一个抽象类--在子类中没有提供具体的功能，更多的时候充当一个标记，为了让我们使用**isinstance**。从[PEP 3119](http://www.python.org/dev/peps/pep-3119/)开始，这种观念越来越明显，介绍了它的泛型，在Python2.6和3.0中被采用并实现。<br/>
PEP明确了抽象基类可以代替鸭子类型，而且实现起来没什么压力（看[这里](https://www.python.org/dev/peps/pep-3119/#abcs-vs-duck-typing)）。最新版本的Python中实现的抽象基类提供了额外的更吸引人的东西：**isinstance**（和**issubclass**)现在不仅仅是一个子类的实例（特别是所有的类都可以注册成为抽象基类并展示为某个子类，它可以实例化抽象基类的实例），抽象基类还为具体的子类提供了更多的便利-自然的通过模板方法来设计模仿应用。（看[这里](http://en.wikipedia.org/wiki/Template_method_pattern)和[这里](http://www.catonmat.net/blog/learning-python-design-patterns-through-video-lectures/)（第三部分）了解Python中特殊不特殊的更多TM DP，以及抽象基类的支持）。<br/>
对于那些Python 2.6中潜在的关于抽象基类的技巧，看[这里](https://docs.python.org/2/library/abc.html)，对于3.1版本，同样，看[这里](https://docs.python.org/3.1/library/abc.html)。两种版本的基本库模块[collections](https://docs.python.org/3.1/library/collections.html#module-collections)（这是3.1版本，几乎一样的2.6版本，看[这里](https://docs.python.org/2/library/collections.html#module-collections)）都提供了多种有用的抽象基类。<br/>
这个答案的目的是，抽象基类保留的那些关键东西（我们可以超过论证横向比较TM DP的功能性，和Python传统的mixin类，比如[UserDict.DictMixin](https://docs.python.org/2/library/userdict.html#UserDict.DictMixin)）是他们让**isinstance**(和**issubclass**)比之前（Python2.5及之前）更有魅力且无处不在（在Python2.6以及更新的版本中）。因此，相比之下，在Python中检查类型相等更恶心了，比之前的版本还糟糕。<br/>

##日期
**1. 如何将一个Python time.struct_time对象转换为一个datetime对象**<br/>
使用 time.mktime() 将time元组(本地时间)转成秒， 然后使用 datetime.fromtimestamp() 转成datetime对象<br/>
```python
from time import mktime
from datetime import datetime
dt = datetime.fromtimestamp(mktime(struct))
```

**2. python中如何获取当前时间**<br/>
时间日期<br/>
```python
>>> import datetime
>>> datetime.datetime.now()
datetime(2009, 1, 6, 15, 8, 24, 78915)
```
如果仅获得时间<br/>
```python
>>> datetime.datetime.time(datetime.datetime.now())
datetime.time(15, 8, 24, 78915))
#等价
>>> datetime.datetime.now().time()
```
可以从文档中获取更多[文档](http://docs.python.org/2/library/datetime.html),如果想避免额外的datetime.<br/>
```python
>>> from datetime import datetime
```

**3. Python - time.clock() vs. time.time() - 更精确?*<br/>
哪一个更适合于计时? 哪个更精确:<br/>
```python
start = time.clock()
... do something
el = (time.clock() - start)

and

start = time.time()
... do something
el = (time.time() - start)
```
区别<br/>
```python
clock() -> floating point number

Return the CPU time or real time since the start of the process or since
the first call to clock().
This has as much precision as the system records.

time() -> floating point number

Return the current time in seconds since the Epoch.
Fractions of a second may be present if the system clock provides them.
```
根据[time module doc](http://docs.python.org/lib/module-time.html)<br/>
```python
clock()

On Unix, return the current processor time as a floating point number expressed in seconds. The precision, and in fact the very definition of the meaning of ``processor time'', depends on that of the C function of the same name,

but in any case, this is the function to use for benchmarking Python or timing algorithms.
```
简而言之, time.clock()更精确些, 但是如果涉及cpu外的硬件时间统计(e.g. gpu), 只能使用time.time().另外基于性能的评估, 可以去看下timeit模块<br/>

**4. python和javascript中josn的datetime**<br/>
怎样从python->javascript传递json的datetime?<br/>
可以**json.dumps**中加入**default**参数<br/>
```python
>>> dthandler = lambda obj: (
...     obj.isoformat()
...     if isinstance(obj, datetime.datetime)
...     or isinstance(obj, datetime.date)
...     else None)
>>> json.dumps(datetime.datetime.now(), default=dthandler)
'"2010-04-20T20:08:21.634121"'
```
一个更好理解的handler<br/>
```python
def handler(obj):
    if hasattr(obj, 'isoformat'):
        return obj.isoformat()
    else isinstance(obk, ...):
        return ...
    else:
        raise TypeError, 'Object of type %s with value of %s is not JSON serializable' % (type(obj), repr(obj))
```
但是上面这种做法, 对于json中包含其他的类型, 会返回None<br/>
```python
import json
import datetime
class DateTimeJSONEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, datetime.datetime):
            return obj.isoformat()
        else:
            return super(DateTimeJSONEncoder, self).default(obj)

>>> DateTimeJSONEncoder().encode([datetime.datetime.now()])
'["2010-06-15T14:42:28"]'
```

## oop
**1. Python 'self' 解释**<br/>
使用self关键字的原因是，Python没有@语法用于引用实例属性.Python决定用一种方式声明方法:实例对象自动传递给属于它的方法,但不是接收自动化：方法的第一个参数是调用这个方法的实例对象本身.这使得方法整个同函数一致,并且由你自己决定真实的名（虽然self是约定，但当你使用其他名的时候，通常人们并不乐意接受）.self对于代码不是特殊的，只是另一个对象.<br/>
Python本来可以做一些用来区分真实的名字和属性的区别 —— 像Ruby有的特殊语法，或者像C++/Java的命令声明,或者其他可能的的语法 —— 但是Python没有这么做.Python致力于使事情变得明确简单，让事情是其本身，虽然并不是全部地方都这么做，但是实例属性是这么做的！这就是为什么给一个实例属性赋值时需要知道是给哪个实例赋值,并且，这就是为什么需要self:<br/>
```python
class Vector(object):
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def length(self):
        return math.sqrt(self.x ** 2 + self.y ** 2)
#等价于
def length_global(vector):
    return math.sqrt(vector.x ** 2 + vector.y ** 2)
```
另外<br/>
```python
v_instance.length()
转为
Vector.length(v_instance)
```

**2. 为什么Python的'private'方法并不是真正的私有方法**<br/>
Python允许我们创建'private' 函数：变量以两个下划线开头，像这样： __myPrivateMethod(). 但是，如何解释：<br/>
```python
>>> class MyClass:
...     def myPublicMethod(self):
...             print 'public method'
...     def __myPrivateMethod(self):
...             print 'this is private!!'
...
>>> obj = MyClass()
>>> obj.myPublicMethod()
public method
>>> obj.__myPrivateMethod()
Traceback (most recent call last):
File "", line 1, in
AttributeError: MyClass instance has no attribute '__myPrivateMethod'
>>> dir(obj)
['_MyClass__myPrivateMethod', '__doc__', '__module__', 'myPublicMethod']
>>> obj._MyClass__myPrivateMethod()
this is private!!
```
dir(obj) 和 obj._MyClass__myPrivateMethod()<br/>
回答:‘private'只是用作，确保子类不会意外覆写父类的私有方法和属性.不是为了保护外部意外访问而设计的！<br/>
```python
>>> class Foo(object):
...     def __init__(self):
...         self.__baz = 42
...     def foo(self):
...         print self.__baz
...
>>> class Bar(Foo):
...     def __init__(self):
...         super(Bar, self).__init__()
...         self.__baz = 21
...     def bar(self):
...         print self.__baz
...
>>> x = Bar()
>>> x.foo()
42
>>> x.bar()
21
>>> print x.__dict__
{'_Bar__baz': 21, '_Foo__baz': 42}
```
当然，这对于两个同名的类没有作用.另外，可以查看diveintopython的解释[入口](http://www.faqs.org/docs/diveintopython/fileinfo_private.html#d0e11521)<br/>

**3. Python中类方法的作用是什么**
我现在意识到，我不需要像我在使用java的static方法那样使用类方法，但是我不确定什么时候使用<br/>
谁能通过一个好的例子解释下Python中的类方法，至少有人能告诉我什么时候确实需要使用类方法<br/>
类方法用在：当你需要使用不属于任何明确实例的方法,但同时必须涉及类.<br/>有趣的是，你可以在子类中覆写，这在Java的static方法和Python的模块级别函数中是不可能做到的
如果你有一个MyClass, 并且一个模块级别函数操作MyClass(工厂，依赖注入桩等等), 声明一个类方法.然后这个类方法可以在子类中调用<br/>

**4. Python中 new 和 init的用法**<br/>
使用**new**,当你需要控制一个实例的生成<br/>
使用**init**,当你需要控制一个实例的初始化<br/>
**new**是实例创建的第一步.最先被调用，并且负责返回类的一个新实例.<br/>
相反的,**init**不返回任何东西，只是负责在实例创建后进行初始化<br/>
通常情况下，你不必重写**new**除非你写一个子类继承不可变类型，例如str,int,unicode或tuple<br/>
你必须了解到，你尝试去做的用[Factory](http://en.wikipedia.org/wiki/Factory_object)可以很好地解决，并且是最好的解决方式.使用**new**不是一个简洁的处理方式,一个[factory例子](http://code.activestate.com/recipes/86900/)

**5. 如何获取一个实例的类名**<br/>
```python
x.__class__.__name__
```

**6. @staticmethod和@classmethod的区别**<br/>
staticmethod，静态方法在调用时，对类及实例一无所知.
仅仅是获取传递过来的参数，没有隐含的第一个参数，在Python里基本上用处不大，你完全可以用一个模块函数替换它<br/>
classmethod, 在调用时，将会获取到其所在的类，或者类实例，作为其第一个参数.当你想将函数作为一个类工厂时，这非常有用: 第一个参数是类，你可以实例化出对应实例对象，甚至子类对象。<br/>
可以观察下 dict.fromkey(),是一个类方法，当子类调用时，返回子类的实例<br/>
```python
>>> class DictSubclass(dict):
...     def __repr__(self):
...         return "DictSubclass"
...
>>> dict.fromkeys("abc")
{'a': None, 'c': None, 'b': None}
>>> DictSubclass.fromkeys("abc")
DictSubclass
>>>
```

**7. 如何定义静态方法(static method)**<br/>
使用[staticmethod](http://docs.python.org/2/library/functions.html#staticmethod)装饰器<br/>
```python
class MyClass(object):
    @staticmethod
    def the_static_method(x):
        print x
MyClass.the_static_method(2) # outputs 2
```

**8. Python中的类变量(环境变量)**<br/>
在类中定义的变量，不在方法定义中，成为类变量或静态变量<br/>
```python
>>> class MyClass:
...     i = 3
...
>>> MyClass.i
3
```
i是类级别的变量，但这里要和实例级别的变量i区分开<br/>
```python
>>> m = MyClass()
>>> m.i = 4
>>> MyClass.i, m.i
>>> (3, 4)
```
这和C++/java完全不同，但和C#区别不大，C#不允许类实例获取静态变量<br/>
具体见[what the Python tutorial has to say on the subject of classes and class objects](http://docs.python.org/2/tutorial/classes.html#SECTION0011320000000000000000)<br/>
另外，静态方法<br/>
```python
class C:
    @staticmethod
    def f(arg1, arg2, ...): ...
```

**9. 如何判断一个对象是否拥有某个属性**<br/>
```python
if hasattr(a, 'property'):
    a.property
```
两种风格<br/>
- EAFP(easier to ask for forgiveness than permission)<br/>
- LBYL(look before you leap)<br/>
相关内容 <br/>
- [EAFP vs LBYL (was Re: A little disappointed so far)](http://web.archive.org/web/20070929122422/http://mail.python.org/pipermail/python-list/2003-May/205182.html)<br/>
- [EAFP vs. LBYL @Code Like a Pythonista: Idiomatic](http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html#eafp-vs-lbyl)<br/>
- [python](http://python.net/~goodger/projects/pycon/2007/idiomatic/handout.html#eafp-vs-lbyl)<br/>
```python
try:
    doStuff(a.property)
except AttributeError:
    otherStuff()
or

if hasattr(a, 'property'):
    doStuff(a.property)
else:
    otherStuff()
```

**10. Python中有没有简单优雅的方式定义单例类**<br/>
我不认为有必要，一个拥有函数的模块（不是类）可以作为很好的单例使用，它的所有变量被绑定到这个模块，无论如何都不能被重复实例化<br/>
如果你确实想用一个类来实现，在python中不能创建私有类或私有构造函数,所以你不能隔离多个实例而仅仅通过自己的API来访问属性<br/>
我还是认为将函数放入模块，并将其作为一个单例来使用是最好的办法<br/>

**11. 理解Python的Super()和init方法**<br/>
尝试着去理解super().从表面上看，两个子类都能正常创建.我只是好奇他们两者之间的不同点<br/>
```python
class Base(object):
    def __init__(self):
        print "Base created"

class ChildA(Base):
    def __init__(self):
        Base.__init__(self)

class ChildB(Base):
    def __init__(self):
        super(ChildB, self).__init__()

print ChildA(),ChildB()
```
回答:<br/>
Super让你避免明确地引用基类，这是一点。最大的优势是，当出现多重继承的时候，各种[有趣的情况](http://www.artima.com/weblogs/viewpost.jsp?thread=236275)就会出现。查看super[官方文档](http://docs.python.org/library/functions.html#super).<br/>
另外注意，在Python3.0中，可以使用super().init() 代替 super(ChildB, self).init().IMO略有优势.<br/>

**12. 在Python中，如何判断一个对象iterable?**<br/>
1. 检查**iter**对序列类型有效，但是对例如string，无效<br/>
```python
try:
    iterator = iter(theElement)
except TypeError:
    # not iterable
else:
    # iterable

# for obj in iterator:
#     pass
```
2. 使用collections<br/>
```python
import collections

if isinstance(e, collections.Iterable):
    # e is iterable
```

**13. 构建一个基本的Python迭代器**<br/>
在Python中，迭代器对象遵循迭代器协议，这意味着它提供了两种方法:**__iter__()**和**next()**。**__iter__()**返回一个迭代器对象并且在循环开始时就隐式的被调用。**next()**方法返回下一个值，并在循环的每一次增量中被调用。当没有值需要返回时，**next()**引发一个StopIteration异常，这个异常被循环结构隐式的捕获从而停止迭代。<br/>
这有一个简单计数例子：<br/>
```python
class Counter:
    def __init__(self, low, high):
        self.current = low
        self.high = high

    def __iter__(self):
        return self

    def next(self): # Python 3: def __next__(self)
        if self.current > self.high:
            raise StopIteration
        else:
            self.current += 1
            return self.current - 1
for c in Counter(3, 8):
    print c
#上述会打印出：
3
4
5
6
7
8
```
这个用生成器写会更简单一些，下面是之前答案的翻写：<br/>
```python
def counter(low, high):
    current = low
    while current <= high:
        yield current
        current += 1
for c in Counter(3, 8):
    print c
```
打印出来的内容是一样的。在后台，生成器对象支持迭代器协议，大体上对Counter类做一些同样事情。<br/>
[Iterators and Simple Generators](http://www.ibm.com/developerworks/library/l-pycon.html)，David Mertz的这篇文章，是一篇对迭代器非常好的介绍。<br/>

**14. 在Python中，抽象类和接口有什么区别？**<br/>
看看下面这个：<br/>
```python
class Abstract1(object):
    """Some description that tells you it's abstract,
    often listing the methods you're expected to supply."""
    def aMethod(self):
        raise NotImplementedError( "Should have implemented this" )
```
因为在Python中没有（也不需要）一个正式的接口协议，类Java的抽象类和接口的区别并不存在。如果有人尝试定义一个正式的接口，它其实也是一个抽象类。唯一的不同就是在文档注释的表述。<br/>
并且当你使用鸭子类型时，抽象类和接口的区别有点吹毛求疵了。<br/>
Java使用接口是因为它没有多重继承。<br/>
因为Python有多重继承，你可能还会看到类似这样的东西：<br/>
```python
class SomeAbstraction( object ):
    pass # lots of stuff - but missing something

class Mixin1( object ):
    def something( self ):
        pass # one implementation

class Mixin2( object ):
    def something( self ):
        pass # another

class Concrete1( SomeAbstraction, Mixin1 ):
    pass

class Concrete2( SomeAbstraction, Mixin2 ):
    pass
```
这是一种使用混合抽象超类去创建不相交的具体子类的方法。<br/>

**15. Python 的slots**<br/>
**__slots__**的正确使用方法是保存对象的空间。取代使用允许任何时间给类添加属性的动态字典，有一种在创建之后不允许添加的静态结构。使用slots节省了给所有对象同一个字典的系统开销。有时候这是一个很有效的优化，但它也会变得毫无用处，前提是Python的解释器足够动态化，可以在确实需要为对象增加某些东西时只需要字典。<br/>
不幸的是，使用slots有一个副作用。他们通过一种方法改变了那些带有slots的对象的表现形式，使它们被古怪的控制者和细小的静态归类滥用。这很糟糕，因为古怪的控制者应该滥用元类，而细小的静态归类应该滥用生成器。但是从Python开始，只有这一种显著的方法了。<br/>
将CPython做的很聪明，聪明到可以不用**__slots__**保存空间，是一个主要的工作，这也就是为什么它不在P3k的更改列表中（到目前为止）。<br/>

**16. Python中，新式类和旧式类的区别**<br/>
根据[https://docs.python.org/2/reference/datamodel.html#new-style-and-classic-classes](https://docs.python.org/2/reference/datamodel.html#new-style-and-classic-classes)<br/>

**17. Python中，@property和getters and setters哪一个更好**<br/>
用属性更好，这也是他们存在的原因。<br/>
原因是在Python中，所有属性都是公共的。名字由单下划线或双下划线开始的，只不过是一个警告，表示这个属性的值在只是一个执行细节，在未来的版本中可能不会保持一致。他并没有阻止你去获取或者设置这个属性。因此，标准的属性访问途径便是是公认最好的，Pythonic的。<br/>
属性的优点是他们和访问属性的语法上保持一致，所以你可以在不改变客户端的情况下把属性从一个改变成另一个值。你甚至可以有一个不再生产环境版本的类用来保存属性，不用改变代码就可以使用它们（用来debug或者上下文代码）。同时，你不需要为所有东西写获取和设置因为在之后你可能需要更好的控制。<br/>

**18. 在Python中，链式调用父类构造器**<br/>
```python
class A(object):
    def __init__(self):
        print "Constructor A was called"

class B(A):
    def __init__(self):
        super(B,self).__init__()
        print "Constructor B was called"

class C(B):
    def __init__(self):
        super(C,self).__init__()
        print "Constructor C was called"

c = C()
```
你正在做的事情，确实是值得推荐的（对于Python 2.x来说）.<br/>
是否把类明确的传递给**super**是一个风格而不是功能上的问题。把类明确的传递给**super**符合Python哲学的“明了胜于晦涩”。<br/>

**19. Python中，一个对象前面带单下划线和双下划线的含义**<br/>
单下划线: 在一个类中，单下划线开头的单纯为了告诉其他程序员，这些属性或者方法意味着私有的。然而，这些属性或者方法本身并没什么特别的。引述[PEP-8](http://www.python.org/dev/peps/pep-0008/):<br/>
```python
_single_leading_underscore: weak "internal use" indicator. E.g. from M import * does not import objects whose name starts with an underscore.
```
双下划线,来自[Python文档](http://docs.python.org/tutorial/classes.html#private-variables-and-class-local-references)：<br/>
```python
Any identifier of the form __spam (at least two leading underscores, at most one trailing underscore) is textually replaced with _classname__spam, where classname is the current class name with leading underscore(s) stripped. This mangling is done without regard to the syntactic position of the identifier, so it can be used to define class-private instance and class variables, methods, variables stored in globals, and even variables stored in instances. private to this class on instances of other classes.
```
同一页还有一个警告：<br/>
```python
Name mangling is intended to give classes an easy way to define “private” instance variables and methods, without having to worry about instance variables defined by derived classes, or mucking with instance variables by code outside the class. Note that the mangling rules are designed mostly to avoid accidents; it still is possible for a determined soul to access or modify a variable that is considered private.
```
举例：<br/>
```python
>>> class MyClass():
...     def __init__(self):
...             self.__superprivate = "Hello"
...             self._semiprivate = ", world!"
...
>>> mc = MyClass()
>>> print mc.__superprivate
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: myClass instance has no attribute '__superprivate'
>>> print mc._semiprivate
, world!
>>> print mc.__dict__
{'_MyClass__superprivate': 'Hello', '_semiprivate': ', world!'}
```

**20. 在一个已存在的对象里，加一个方法**<br/>
在Python中，函数和约束方法还是有一些区别。<br/>
```python
>> def foo():
...     print "foo"
...
>>> class A:
...     def bar( self ):
...         print "bar"
...
>>> a = A()
>>> foo
<function foo at 0x00A98D70>
>>> a.bar
<bound method A.bar of <__main__.A instance at 0x00A9BC88>>
>>>
```
约束方法被约束到一个实例上，当方法调用时这个实例会被当做第一个参数传入。<br/>
在类（与实例相反）中，那些作为属性的可调用者仍然能是有限制的，尽管，你可以随时修改这个类的定义。<br/>
```python
>>> def fooFighters( self ):
...     print "fooFighters"
...
>>> A.fooFighters = fooFighters
>>> a2 = A()
>>> a2.fooFighters
<bound method A.fooFighters of <__main__.A instance at 0x00A9BEB8>>
>>> a2.fooFighters()
fooFighters
```
这样之前定义的实例也回随着更新（只要他们没有重写这个属性）：<br/>
```python
>>> a.fooFighters()
fooFighters
```
问题出现在当你想把一个方法固定在某一个实例时：<br/>
```python
>>> def barFighters( self ):
...     print "barFighters"
...
>>> a.barFighters = barFighters
>>> a.barFighters()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: barFighters() takes exactly 1 argument (0 given)
```
当你想直接固定到一个实例上时，函数不是自动约束的:<br/>
```python
>>> a.barFighters
<function barFighters at 0x00A98EF0>
```
为了绑定它，我盟可以用[types模块中的方法类函数](http://docs.python.org/library/types.html?highlight=methodtype#module-types):<br/>
```python
>>> import types
>>> a.barFighters = types.MethodType( barFighters, a )
>>> a.barFighters
<bound method ?.barFighters of <__main__.A instance at 0x00A9BC88>>
>>> a.barFighters()
barFighters
```
这时候，类的其他实例不会受到影响：<br/>
```python
>>> a2.barFighters()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: A instance has no attribute 'barFighters'
```
更多的信息，可以在阅读[descriptors](http://users.rcn.com/python/download/Descriptor.htm)和[metaclass](http://www.onlamp.com/pub/a/python/2003/04/17/metaclasses.html)以及[programming](http://www.gnosis.cx/publish/programming/metaclass_2.html)中发现。<br/>

**21. 在Python中，metaclass是什么**<br/>
**类对象**<br/>在理解metaclass之前，你需要掌握Python中的类。而且Python的类的设计，非常的特别，借鉴了Smalltalk语言。<br/>
大多数语言中，类只是一段代码用来描述如何生产一个对象。在Python中也有几分这个意思：<br/>
```python
>>> class ObjectCreator(object):
...       pass
...

>>> my_object = ObjectCreator()
>>> print(my_object)
<__main__.ObjectCreator object at 0x8974f2c>
```
但是Python中的类不仅仅如此。类，也是对象。<br/>
对，对象。<br/>
当你使用**class**这个关键字时，Python执行它并创造一个对象，示例：<br/>
```python
>>> class ObjectCreator(object):
...       pass
...
```
在内存中创建了一个对象名字是"ObjectCreator"。这个对象（类）有能力创造对象（实例），这也是为什么它是类。<br/>
但它仍然是一个类，因此：<br/>
- 你可以把它当做一个变量
- 你可以复制它
- 你可以给它添加属性
- 你可以把它当成一个函数的参数
举例：<br/>
```python
>>> print(ObjectCreator) # you can print a class because it's an object
<class '__main__.ObjectCreator'>
>>> def echo(o):
...       print(o)
...
>>> echo(ObjectCreator) # you can pass a class as a parameter
<class '__main__.ObjectCreator'>
>>> print(hasattr(ObjectCreator, 'new_attribute'))
False
>>> ObjectCreator.new_attribute = 'foo' # you can add attributes to a class
>>> print(hasattr(ObjectCreator, 'new_attribute'))
True
>>> print(ObjectCreator.new_attribute)
foo
>>> ObjectCreatorMirror = ObjectCreator # you can assign a class to a variable
>>> print(ObjectCreatorMirror.new_attribute)
foo
>>> print(ObjectCreatorMirror())
<__main__.ObjectCreator object at 0x8997b4c>
```
动态创建类<br/>
类就是对象，你可以快速创建它，像任何其他对象一样。<br/>
首先，你可以在一个函数里创建一个类，用**class**:<br/>
```python
>>> def choose_class(name):
...     if name == 'foo':
...         class Foo(object):
...             pass
...         return Foo # return the class, not an instance
...     else:
...         class Bar(object):
...             pass
...         return Bar
...
>>> MyClass = choose_class('foo')
>>> print(MyClass) # the function returns a class, not an instance
<class '__main__.Foo'>
>>> print(MyClass()) # you can create an object from this class
<__main__.Foo object at 0x89c6d4c>
```
但是它不是很动态，你仍然需要手写你的类。<br/>
既然类是对象，他们一定可以被什么东西生成。<br/>
当你使用**class**关键字时，Python自动创建了这个对象，但是像Python中的其他东西一样，它给你了一个方法手动实现。<br/>
还记得**type**函数吗。一个让你知道对象类型的古老的函数：<br/>
```python
>>> print(type(1))
<type 'int'>
>>> print(type("1"))
<type 'str'>
>>> print(type(ObjectCreator))
<type 'type'>
>>> print(type(ObjectCreator()))
<class '__main__.ObjectCreator'>
```
哦，**type**有另外一种完全不同的功能，它也可以迅速创建类。**type**可以把类的描述作为参数，并返回一个类。(我知道同一个函数根据你传入的值有两种不同的用法是很蠢的，但是它是Python中的一种向后兼容的问题)<br/>
**type**这样工作：<br/>
```python
type(name of the class,
     tuple of the parent class (for inheritance, can be empty),
     dictionary containing attributes names and values)
```
举个例子<br/>
```python
>>> class MyShinyClass(object):
...       pass
```
可以通过这种方法手动生成：<br/>
```python
>>> MyShinyClass = type('MyShinyClass', (), {}) # returns a class object
>>> print(MyShinyClass)
<class '__main__.MyShinyClass'>
>>> print(MyShinyClass()) # create an instance with the class
<__main__.MyShinyClass object at 0x8997cec>
```
你可能会注意到我们使用"MyShinyClass"做为类的名字并且作为变量并且作为类的参考。他们可以不同，但是没有必要把事情搞复杂。<br/>
**type**接受一个字典，定义一个类的参数，所以：<br/>
```python
>>> class Foo(object):
...       bar = True
```
可以理解成：<br/>
```python
>>> Foo = type('Foo', (), {'bar':True})
```
并且可以当成一个普通类来使用：<br/>
```python
>>> print(Foo)
<class '__main__.Foo'>
>>> print(Foo.bar)
True
>>> f = Foo()
>>> print(f)
<__main__.Foo object at 0x8a9b84c>
>>> print(f.bar)
True
```
当然，你可以继承它，所以：<br/>
```python
>>> FooChild = type('FooChild', (Foo,), {})
>>> print(FooChild)
<class '__main__.FooChild'>
>>> print(FooChild.bar) # bar is inherited from Foo
True
```
最后你可能想要在你的类里添加方法。只要适当的定义一个函数，然后把它标记为属性。<br/>
```python
>>> def echo_bar(self):
...       print(self.bar)
...
>>> FooChild = type('FooChild', (Foo,), {'echo_bar': echo_bar})
>>> hasattr(Foo, 'echo_bar')
False
>>> hasattr(FooChild, 'echo_bar')
True
>>> my_foo = FooChild()
>>> my_foo.echo_bar()
True
```
可以回顾一下：在Python中，类就是对象，你可以动态的创造一个类。<br/>
这就是当你使用**class**关键字时Python做的事情，使用metaclass时，也是一样的。<br/>
**什么是metaclass（最终版本）**<br/>
Metaclass是创建类的原料。<br/>
你定义类就是为了创建对象，对不对？<br/>
但是我们知道Python类本身就是对象。<br/>
所以，这些对象就是metaclass创建的。他们是类的类，你可以这样表述：<br/>
```python
MyClass = MetaClass()
MyObject = MyClass()
```
刚才你看到了type允许你做类似这样的事情：<br/>
```python
MyClass = type('MyClass', (), {})
```
这是因为**type**这个函数实际上是一个metaclass。**type**就是metaclass -- Python用来在后台创造一切类。<br/>
现在你知道为什么这个东西他喵的写成小写的，而不是**Type**了吧。<br/>
嗯，我想同样的问题可能发生在用来创造字符串对象的**str**这个类上，**int**是创造整数对象的类，**type**是用来创造类对象的类。<br/>
通过查看**__class__**参数验证。<br/>
所有的东西，我是说所有，在Python中都是对象。包括整数，字符串，函数，类。他们全是对象。他们全都由一个类创造而来：<br/>
```python
MyClass = MetaClass()
MyObject = MyClass()
```
刚才你看到了type允许你做类似这样的事情：<br/>
```python
>>> age = 35
>>> age.__class__
<type 'int'>
>>> name = 'bob'
>>> name.__class__
<type 'str'>
>>> def foo(): pass
>>> foo.__class__
<type 'function'>
>>> class Bar(object): pass
>>> b = Bar()
>>> b.__class__
<class '__main__.Bar'>
```
现在，看看所有的**__class__**的**__class__**是什么？<br/>
```python
>>> age.__class__.__class__
<type 'type'>
>>> name.__class__.__class__
<type 'type'>
>>> foo.__class__.__class__
<type 'type'>
>>> b.__class__.__class__
<type 'type'>
```
所以，metaclass就是用来创造类对象的原料。<br/>
如果你想，你可以把他叫做类工厂。<br/>
**type**是Python使用的内建的metaclass，当然，你可以创造你自己的metaclass。<br/>
**__metaclass__属性**<br/>
你可以给你写的类添加一个**__metaclass__属性:**<br/>
```python
class Foo(object):
  __metaclass__ = something...
  [...]
```
如果你这样做，Python会使用metaclass创建**Foo**这个类。<br/>
小心点，这很复杂。<br/>
你先写了**class Foo(object)**，但是现在在内存中，还没有创建这个类对象Foo。<br/>
Python会在类的定义时，检查**__metaclass__**。如果找到了，Python就用它创造一个类对象Foo。如果没有，就用type创造类。<br/>
多读几次。<br/>
当你这样：<br/>
```python
class Foo(Bar):
  pass
```
Python会做下面这些事情：<br/>

Foo里面有__metaclass__这个属性吗？<br/>

如果有，在内存中创建一个类对象（我是说一个类对象，与我同在）通过使用__metaclass__创建一个同样的名字Foo。<br/>

如果Python找不到__metaclass__，它会在模块层找这个__metaclass__，试图通过同样的方式。（但是仅对于那些没有继承任何东西的类，基本上都是旧式类）<br/>

之后，如果哪都找不到__metaclass__，就使用Bar（第一层父类）自带的metaclass（有可能就是缺省的type）来创建类对象。<br/>

注意，这里的__metaclass__不会被继承，父类的会被继承(Bar.__class__)。如果Bar使用一个用type（而不是type.__new__()）创建Bar本身的__metaclass__属性，那么子类不会继承这个行为。<br/>

现在一个大问题出现了，你可以在__metaclass__里面放什么呢？<br/>

答案是：一些可以创建类的东西。<br/>

然而什么可以创建类的呢？type或者是它的任何子类，或者使用它的东西。<br/>

一个metaclass的主要目的就是当一个类创建的时候，自动的改变它。<br/>

你通常对接口做这些事情，比如你想要创建一个符合当前上下文的类。<br/>

试想一个愚蠢的例子，你决定让你的模块里的所有类的所有属性都用大写。有几种方法可以实现，其中一种是在模块层使用__metaclass__。<br/>

通过这种方法，该模块的所有类在创建时都会使用这个metaclass，而我们只需要告诉metaclass把所有属性都变成大写。<br/>
幸运的是，__metaclass__可以通过任何方式调用，不需要一个正规的类（我知道，有些名字里带着class的东西不一定是类，想想看吧，它很有用）。<br/>
所以我们通过一个函数，从简单的例子开始：<br/>
```python
# the metaclass will automatically get passed the same argument
# that you usually pass to `type`
def upper_attr(future_class_name, future_class_parents, future_class_attr):
  """
    Return a class object, with the list of its attribute turned
    into uppercase.
  """

  # pick up any attribute that doesn't start with '__' and uppercase it
  uppercase_attr = {}
  for name, val in future_class_attr.items():
      if not name.startswith('__'):
          uppercase_attr[name.upper()] = val
      else:
          uppercase_attr[name] = val

  # let `type` do the class creation
  return type(future_class_name, future_class_parents, uppercase_attr)

__metaclass__ = upper_attr # this will affect all classes in the module

class Foo(): # global __metaclass__ won't work with "object" though
  # but we can define __metaclass__ here instead to affect only this class
  # and this will work with "object" children
  bar = 'bip'

print(hasattr(Foo, 'bar'))
# Out: False
print(hasattr(Foo, 'BAR'))
# Out: True

f = Foo()
print(f.BAR)
# Out: 'bip'
```
现在，我们做同样的事情，但是对metaclass使用真正的类：<br/>
```python
# remember that `type` is actually a class like `str` and `int`
# so you can inherit from it
class UpperAttrMetaclass(type):
    # __new__ is the method called before __init__
    # it's the method that creates the object and returns it
    # while __init__ just initializes the object passed as parameter
    # you rarely use __new__, except when you want to control how the object
    # is created.
    # here the created object is the class, and we want to customize it
    # so we override __new__
    # you can do some stuff in __init__ too if you wish
    # some advanced use involves overriding __call__ as well, but we won't
    # see this
    def __new__(upperattr_metaclass, future_class_name,
                future_class_parents, future_class_attr):

        uppercase_attr = {}
        for name, val in future_class_attr.items():
            if not name.startswith('__'):
                uppercase_attr[name.upper()] = val
            else:
                uppercase_attr[name] = val

        return type(future_class_name, future_class_parents, uppercase_attr)
```
但是这并不符合面向对象的思想。我们直接调用type，不重写或者调用父类的__new__方法。试一下：<br/>
```python
class UpperAttrMetaclass(type):

    def __new__(upperattr_metaclass, future_class_name,
                future_class_parents, future_class_attr):

        uppercase_attr = {}
        for name, val in future_class_attr.items():
            if not name.startswith('__'):
                uppercase_attr[name.upper()] = val
            else:
                uppercase_attr[name] = val

        # reuse the type.__new__ method
        # this is basic OOP, nothing magic in there
        return type.__new__(upperattr_metaclass, future_class_name,
                            future_class_parents, uppercase_attr)
```
你可能会主要到多余的参数upperattr_metaclass。它没什么特殊的：一个方法总是接受当前的实例作为第一个参数。就像你在普通的方法中使用self。<br/>
当然，我这里使用这么长的名字是为了更清楚，但是像self一样，所有参数有惯用的名字。所以一个真正的生产环境的metaclass看起来可能是这样：<br/>
```python
class UpperAttrMetaclass(type):

    def __new__(cls, clsname, bases, dct):

        uppercase_attr = {}
        for name, val in dct.items():
            if not name.startswith('__'):
                uppercase_attr[name.upper()] = val
            else:
                uppercase_attr[name] = val

        return type.__new__(cls, clsname, bases, uppercase_attr)
```
我们可以通过使用super简化继承，让它更清晰。（因此，你可以拥有metaclasses，继承metaclass，继承type）<br/>
```python
class UpperAttrMetaclass(type):

    def __new__(cls, clsname, bases, dct):

        uppercase_attr = {}
        for name, val in dct.items():
            if not name.startswith('__'):
                uppercase_attr[name.upper()] = val
            else:
                uppercase_attr[name] = val

        return super(UpperAttrMetaclass, cls).__new__(cls, clsname, bases, uppercase_attr)
```
差不多就这样，metaclass真没什么更多的内容了。<br/>

使用metaclass的代码非常复杂的背后原因不是metaclass本身，而是你把metaclass用在了那些自我实现，多重继承的东西上，比如__dict__等等。<br/>

总之，metaclass有特殊的技巧实现黑魔法，当然包括复杂的东西。但是对他们自己来说，他们很简单：<br/>

- 拦截一个类的创建<br/>

- 装饰一个类<br/>

- 返回装饰过的类<br/>
**为什么使用metaclass替代函数**<br/>
既然__metaclass__可以接受任何调用，为什么你还要使用明显更复杂的类呢？<br/>

有几个原因：<br/>

- 目的更明确。当你看到UpperAttrMetaclass(type)的时候就，你知道接下去会发生什么<br/>

- 你可以使用面向对象，metaclass可以继承metaclass，重写父类的方法，Metaclass也可以使用metaclass。<br/>

- 你可以更好的组织你的代码结构。不要像上面的例子哪样琐碎的使用metaclass。对某些东西来说它通常是复杂的。创造几个方法并把它们整合到一个类里是很有用的，可以让代码更易读。<br/>

- 关联使用__new__，__init__，和__call__。它们允许你做不同的东西，尽管你可以把它们都做在__new__里面，有些人用__init__更舒服。<br/>

- 这些都叫metaclass，靠，它们肯定很有意义。<br/>
**你他喵为什么会使用metaclass**<br/>
现在有一个大问题，为什么使用这种倾向于引起不清晰的错误的特性？<br/>

通常你不会这样：<br/>
```python
Metaclass的99%的使用者都不必担心它的深度魔法。如果你不知道你是否需要它们，就别用（那些需要它们的人知道为何用它们，而且不需要解释）
```
Python Guru Tim Peters<br/>

metaclass的主要作用就是创造一个接口。典型的用法就是Django ORM。<br/>

它允许你这样定义这些东西：<br/>
```python
class Person(models.Model):
    name = models.CharField(max_length=30)
    age = models.IntegerField()
```
但是你这样用：<br/>
```python
guy = Person(name='bob', age='35')
print(guy.age)
```
它不会返回一个IntegerField对象。它会返回一个int，甚至能直接从数据库里拿。<br/>

这可能是由于models.Model为它定义了__metaclass__，使用一些魔法方法，让你可以定义一些可以做复杂的事情的简单声明关联数据库。<br/>

Django让一些复杂的事情看起来很简单，通过暴露一个简单的接口，使用metaclass，重构了接口的代码让真正的行为在幕后执行。<br/>
**结语**<br/>
首先你知道类是对象而且可以创建实例。<br/>

实际上，类本身也是实例，是metaclass的实例。<br/>
```python
>>> class Foo(object): pass
>>> id(Foo)
142630324
```
所有的东西都是对象，在Python中，它们不是一个类的实例，就是metaclass的实例。<br/>

除了type。<br/>

type是它自己的metaclass。这些东西在纯净的Python环境下是看不到的，他们在执行层做了一些交互来实现。<br/>

第二，metaclass是复杂的。你不需要在每一个简单的类里使用它。你可以用两种不同的方法来改变一个类：<br/>

- [monkey patching](http://en.wikipedia.org/wiki/Monkey_patch)<br/>

- 类的装饰器<br/>

99%的当你需要改变一个类的时刻，你需要用这些东西。<br/>

但是99%的时间里，你不根本不需要改变一个类。<br/>

**22. Python的"最小惊奇"：多重默认参数**<br/>
实际上，这不是一个设计瑕疵，而且不它不是因为内部或者表现问题。<br/>

它单纯是来自Python中，函数是第一梯队的对象的事实，而且不仅仅是一段代码。<br/>

从这个角度，你会发现它很明智：一个函数是一个对象取决于它的定义；默认参数是一种类似丛书数据，而且它们的状态可能从一次到另一次的调用过程中发生改变-和在其他对象中一样。<br/>

不管怎样，Effbot在[Default Parameter Values in Python](http://effbot.org/zone/default-values.htm)中对这种表现有一个很好的解释。<br/>

我发现它很干净，我强烈推荐阅读以下，并且对函数对象是如何工作的掌握更多知识。<br/>

## pip/easy_install
**1. 如何使用 pip 更新所有包**<br/>
如何使用pip更新python的所有包<br/>
没有内置的标志可以实现，但是你可以这么做<br/>
```python
pip freeze --local | grep -v '^\-e' | cut -d = -f 1  | xargs pip install -U
```

**2. 如何删除Python easy_install安装的包**<br/>
pip, setuptools/easy_install的另一种选择，提供uninstall命令<br/>
首先，移除依赖<br/>
```python
$ easy_install -m [PACKAGE]
```
然后，手动删除egg文件:<br/>
```python
$ rm -rf .../python2.X/site-packages/[PACKAGE].egg
```

**3. 如何获取Python的site-packages目录位置**<br/>
参考[How to Install Django" documentation](http://docs.djangoproject.com/en/dev/topics/install/#remove-any-old-versions-of-django),可以在shell中执行<br/>
```python
python -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"
```
更好的可读性<br/>
```python
from distutils.sysconfig import get_python_lib
print(get_python_lib())
```

**4. setup.py安装后如何卸载**<br/>
使用下面命令安装的包如何卸载<br/>
```python
python setup.py install
```
手工删除的话<br/>
```python
python setup.py install --record files.txt
cat files.txt | xargs rm -rf
```

**5. 如何获取安装的python模块列表**<br/>
```python
>>> help('modules')
```

**6. 如何在正确使用pip，virtualenv，distribute构建Python环境**<br/>
你可以不向Python本身添加任何东西,你不需要sudo或者任何权限,你不需要编辑任何文件。<br/>
在自引导的虚拟环境里安装virtualenv。通过这个虚拟环境，它可以创建更多。自从virtualenv搭载了pip和distribute，你就可以从其中一个获得所有东西。<br/>
- 下载virtualenv<br/>
- 解压源码<br/>
- 用解压好的源文件创建一个干净的虚拟环境。选择正确的命令，虚拟环境会自带pip和distribute。<br/>
- 在virtualenv中安装virtualenv<br/>
- 使用第一个自建的环境去创建更多！<br/>

## 其他
**1. 在virtualenv中如何使用不同的python版本**<br/>
在创建virtualenv实例时，使用-p选项<br/>
```python
virtualenv -p /usr/bin/python2.6 <path/to/new/virtualenv/>
```

**2. 如何离开virtualenv**<br/>
使用virtualenv时<br/>
```python
me@mymachine:~$ workon env1
(env1)me@mymachine:~$ workon env2
(env2)me@mymachine:~$ workon env1
(env1)me@mymachine:~$
```
如何退出某个环境<br/>
```python
$ deactivate
```

**3. Python中什么项目结构更好**<br/>
假设你要开发一个较大的客户端程序(非web端),如何组织项目目录和递归？<br/>
不要太在意这个.按你高兴的方式组织就行.Python项目很简单，所以没有那么多愚蠢的规则<br/>
```python
/scripts or /bin  命令行脚本
/tests 测试
/lib C-语言包
/doc 文档
/apidoc api文档
```
并且顶层目录包含README和Config<br/>
难以抉择的是，是否使用/src树. /src,/lib,/bin在Python中没有明显的区别，和Java/c不同.因为顶层/src文件夹显得没有什么实际意义，你的顶层目录可以是程序顶层架构的目录<br/>
```python
/foo
/bar
/baz
```
我建议将这些文件放入到"模块名"的目录中，这样，如果你在写一个应用叫做quux, /quux目录将包含所有这些东西.你可以在PYTHONPATH中加入 /path/to/quux/foo,这样你可以QUUX.foo中重用模块<br/>
另一个回答:<br/>
```python
Project/
|-- bin/
|   |-- project
|
|-- project/
|   |-- test/
|   |   |-- __init__.py
|   |   |-- test_main.py
|   |
|   |-- __init__.py
|   |-- main.py
|
|-- setup.py
|-- README
```

**4. 在Python中使用Counter错误**<br/>
当使用Counter时，出现异常<br/>
```python
AttributeError: 'module' object has no attribute 'Counter'

from collections import Counter
ImportError: cannot import name Counter
```
原因：版本问题，Counter在 python2.7中才被加入到这个模块，你可能使用了Python2.6或更老的版本,可以看下[文档](http://docs.python.org/2/library/collections.html#collections.Counter)<br/>
如果要在 Python2.6或2.5版本使用，可以看[这里](http://code.activestate.com/recipes/576611-counter-class/)<br/>

**5. 如何测试一个python脚本的性能**<br/>
引入<br/>
```python
import cProfile
cProfile.run('foo()')
```
执行脚本<br/>
```python
python -m cProfile myscript.py
```
结果<br/>
```python
1007 function calls in 0.061 CPU seconds

Ordered by: standard name
ncalls  tottime  percall  cumtime  percall filename:lineno(function)
    1    0.000    0.000    0.061    0.061 <string>:1(<module>)
1000    0.051    0.000    0.051    0.000 euler048.py:2(<lambda>)
    1    0.005    0.005    0.061    0.061 euler048.py:2(<module>)
    1    0.000    0.000    0.061    0.061 {execfile}
    1    0.002    0.002    0.053    0.053 {map}
    1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler objects}
    1    0.000    0.000    0.000    0.000 {range}
    1    0.003    0.003    0.003    0.003 {sum}
```

**6. 如何获取5分钟之后的unix时间戳**<br/>
使用[calendar.timegm](http://docs.python.org/3.3/library/calendar.html#calendar.timegm)<br/>
```python
future = datetime.datetime.now() + datetime.timedelta(minutes = 5)
return calendar.timegm(future.utctimetuple())
```
strftime的%s在windows中无法使用<br/>

**7. 在python中如何调用外部命令?**<br/>
可以看下标准库中的[subprocess](http://docs.python.org/library/subprocess.html)<br/>
```python
from subprocess import call
call(["ls", "-l"])
```
subprocess相对于system的好处是, 更灵活。但是 quick/dirty/one time scripts, os.system is enough<br/>

**8. 用Python在终端打印出有颜色的文字?**<br/>
某种程度上这取决于你使用的平台。通常的方法是用ANSI转义序列。举个简单的例子，这有一些来自[blender build](https://svn.blender.org/svnroot/bf-blender/trunk/blender/build_files/scons/tools/bcolors.py)scripts的代码：<br/>
```python
class bcolors:
    HEADER = '\033[95m'
    OKBLUE = '\033[94m'
    OKGREEN = '\033[92m'
    WARNING = '\033[93m'
    FAIL = '\033[91m'
    ENDC = '\033[0m'
```
为了使用上面那种代码，你应该这样做：<br/>
```python
print bcolors.WARNING + "Warning: No active frommets remain. Continue?"
      + bcolors.ENDC
```
这在unixes包括OS X，linux和windows(为你提供[enable ansi.sys](http://support.microsoft.com/kb/101875))上会生效。有ansi代码来设置颜色，移动光标，做更多事情。<br/>
如果你想了解更为复杂的内容（这听上去好像你正在写一款游戏），你应该深入“cursor”这个没款，它会为你处理很多复杂的部分。[Python Curses HowTO](https://docs.python.org/2/howto/curses.html)是一篇很好的介绍。<br/>
如果你是用的不是ASCII（或者说你没有用PC)，你面对诸如ascii字符低于127，’#’，‘@’可能是你得到空格的最好的赌注。如果你确定你的终端使用[IBM extended ascii character set](http://telecom.tbi.net/asc-ibm.html)，你将会有更多的选择，字符176，字符177，字符178，和字符219代表”空格字符”。<br/>
一些流行的基于文本的程序，比如”Dwarf Fortress”，仿照文本的模式做成了图形模式，使用传统PC的字体图片。你可以在[Dwarf Frotress Wiki](http://dwarffortresswiki.org/DF2014:Tilesets)找一些这种点阵图看看（[user-made tilesets](http://dwarffortresswiki.org/Tileset_repository)）。<br/>
[Text Mode Demo Contest](http://en.wikipedia.org/wiki/TMDC)有更多把图片处理成文本的源码。<br/>

**8. 我该如何保护我的Python代码**<br/>
Python作为字节码编译的解释型语言，是很难封闭的。几遍你使用一种exe包比如[py2exe](http://py2exe.org/)，可执行文件的结构依然是清晰可见的，而且Python的字节编码是非常易懂的。<br/>
通常是这样，你必须要想出一个折衷的办法。保护代码究竟重不重要。里面是不是有很私密的东西(比如银行的对称加密秘钥），或者你是一个偏执狂。选择一门可以让你更快速开发优秀产品的语言，对于你的奇特想法从现实主义考虑一下它的价值。<br/>
如果你确定使用要强制授权保证安全，可以写一个小的C拓展，那么这个授权检验就会变得很难逆转（但不是完全不可能）。然后把你的大批代码放进Python。<br/>

**9. 首选的Python单元测试框架**<br/>
nose实际上不是一个单元测试的框架。它是一个测试的执行器，并且是最好的一款。它可以运行通过pyUnit，py.test和doctest创建的测试。<br/>
我首选的单元测试框架是pyUnit。它和其他xUnit框架一样，而且可以让没有Python基础的人很好上手。而且它对Eclipse/PyDev提供非常好的支持。<br/>
在py.test中，我发现了很多层级的安装/卸载混淆在一起。我还发现它生成了很多非常无组织和难阅读的单元测试。<br/>
doctest对于简单的东西来说还好，但是它很有限，不能真正的用来测试复杂和交互的代码。<br/>

**10. Python的单元测试放在哪？**<br/>
对于一个文件module.py来说，单元测试通常叫做test_module.py，遵循Python的命名规则。<br/>
在以下几种地方放置test_module.py都是可以接受的：<br/>
- 和module.py放在同一个文件夹。<br/>
- 在../tests/test_module.py（与代码文件夹的同级）<br/>
- 在tests/test_module.py （在代码文件夹下的同层）<br/>
我倾向第一种方法，它可以更直观的被找到并且引入。不管你在使用什么样的开发系统，你都可以轻松的配置并找到以test_开头的文件。实际上，方便查找的缺省的单元测试模型是test*.py。<br/>

**11. distribute, distutils, setuptools和distutils2的区别**<br/>
到2014年9月，所有其他回答的时间都超过一年了。当你寻求Python打包的建议时，记得看一下发布的日期，而且不要相信过时的信息。<br/>
这篇搭建在Readthedocs的文章Python Packaging User Guide值得一读。每一页都有一个最近时间展示，所以你可以检查最新的手册，而且它相当的全面。Python 3.4的官方文档已经从信任的角度把这个链接加进来了。<br/>
推荐 Setuptools，除非你的需求非常基础，那么你可能只需要 Distutils。Setuptools在Virtualenv和Pip上的表现非常好，我强烈推荐。<br/>
作为一个边注，我建议使用Virtualenv1.10或者更高的版本，因为对Python2或3来说，它是第一个识别Setuptools/Distribute并合并的版本。<br/>


> 学习完毕，虽然有些内容还不明白，但还是学到了不少东西。