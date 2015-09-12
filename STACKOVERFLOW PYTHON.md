#[STACKOVERFLOW PYTHON 百问](http://www.wklken.me/posts/2013/07/20/python-stackoverflow-vote-top.html#_1)
> 原作地址奉上，此处仅作自己的学习。看完这个，要去重新补习一下基础了，要不然终归只是会个皮毛罢了。只会google，悲哀。
<br/>
---

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

**9. 如何检测一个变量是否存在**
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

**15. Python如何实现mkdir -p功能**<br>/
```python
import os, errno
    try:
        os.makedirs(path)
    except OSError as exc : # Python >2.5
        if exc.errno == errno.EEXIST and os.path.isdir(path):
            pass
        else: raise
```