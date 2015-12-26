> 10月份重新补习了Python基础，今天来开始补习标准库，就这么一路背下来。凡是自己不会的都记录下来，熟悉下. 优先处理自己要学习的东西。  

# 目录  

- [第一章 文本](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#第一章-文本)  
&emsp;- [1.1 string-文本常量和模块](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#11-string-文本常量和模块)  
&emsp;- [1.2 textwrap---格式化文本段落](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#12-textwrap---格式化文本段落)  
&emsp;- [1.4 difflib---比较序列](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#14-difflib---比较序列)  

- [第六章 文件系统](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#第六章-文件系统)  
&emsp;- [6.1 os.path--平台独立的文件名管理](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#61-ospath--平台独立的文件名管理)
&emsp;- [6.5.2 复制文件元数据]()  

- [第七章 数据持久存储与交换](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#第七章-数据持久存储与交换)  
&emsp;- [7.7 csv--逗号分隔值文件](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#77-csv--逗号分隔值文件)

- [第十章 进程与线程](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#第十章-进程与线程)  
&emsp;- [10.1 subprocess---创建附加进程](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#101-subprocess---创建附加进程)  

- [第十六章 开发工具](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#第十六章-开发工具)  
&emsp;- [16.4 traceback---异常和栈轨迹](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#164-traceback---异常和栈轨迹)



# 第一章 文本  

### 1.1 string-文本常量和模块  

- 函数  
&emsp;string.capitalize() -> string;Return a copy of the string S with only its first character capitalized.  
&emsp;string.maketrans(frm, to) -> string;Return a translation table (a string of 256 bytes long):  
&emsp;S.translate(table [,deletechars]) -> string;根据table表来进行转换，比replace要高效.  

- 模板  
&emsp;string.Template();这个方法类似于%s这种，但是用$var这种方式来标志变量，输入的变量形式为字典，即values = {'var':'lol'},这个样子。之后还需要String.substitute()来实现。
```Python  
values = {'var':'lol'}

t = string.Template("""
val : $var
""")

t.substitute(values)
Out[27]: '\nval : lol\n'
```  
模版和标准字符串最大的的区别在于:*模版中值会先转化为字符串，以字符串形式插入*，使用**safe_substitute()**可以捕获参数不对时的异常。  

- 高级模板  
&emsp;加入了正则，进行替换。  

### 1.2 textwrap---格式化文本段落  
- 填充段落  
textwrap.fill(text, width=70, **kwargs),Fill a single paragraph of text, returning a new string.  
- 去除现有缩进  
textwrap.dedent(text),Remove any common leading whitespace from every line in `text`.  

### 1.4 difflib---比较序列  
difflib模块包含一些用来计算和处理序列之间差异的工具  
- 比较文本体  
```Python
In [1]: t1 = '123456789'

In [2]: t2 = '1234asd89'

In [3]: import difflib 

In [4]: d = difflib.Differ()

In [5]: diff = d.compare(t1, t2)

In [6]: print '\n'.join(diff)
  1
  2
  3
  4
- 5
- 6
- 7
+ a
+ s
+ d
  8
  9

In [7]: s = difflib.unified_diff(t1, t2, lineterm='',)

In [8]: print '\n'.join(list(diff))


In [9]: print '\n'.join(list(s))
--- 
+++ 
@@ -2,8 +2,8 @@
 2
 3
 4
-5
-6
-7
+a
+s
+d
 8
 9

```  

- 忽略无用数据  
```Python
from difflib import SequenceMatcher

s = SequenceMatcher(lambda x: x == " ",
                    "private Thread currentThread;",
                    "private volatile Thread currentThread;")

print (s.find_longest_match(0, 20, 0, 20))
Match(a=0, b=0, size=8)
```  

- 比较任意类型  
```Python
In [1]: from difflib import SequenceMatcher

In [2]: t1 = ' qwer'

In [3]: t2 = 'asdf asdf'

In [4]: match = SequenceMatcher(None, t1, t2)

In [5]: match.get_opcodes()
Out[5]: [('insert', 0, 0, 0, 4), ('equal', 0, 1, 4, 5), ('replace', 1, 5, 5, 9)]

```  


# 第六章 文件系统  

### 6.1 os.path--平台独立的文件名管理  
- 6.1.1 解析路径  
路径解析依赖于os中定义的一些变量  
&emsp;- os.sep--路径各部分之间的分隔符("/"or"\")  
&emsp;- os.extsep--文件名与文件“扩展名”之间的分隔符(".")  
&emsp;- os.pardir--路径中表示目录树上一级的部分("..")  
&emsp;- os.curdir--路径中指示当前目录的部分(".")  
os.path.split()函数将路径分解为两个单独的部分，并返回包含这些结果的一个tuple。这个tuple的第二个元素是路径的最后一部分，第一个元素则是最后这个部分之前的所有内容。  
```Python
In [5]: import os.path

In [6]: for path in ['/one/two',
   ...:              '/one/two/',
   ...:              '/',
   ...:              '.',
   ...:              '']:
   ...:     print '%s: %s'%(path, os.path.split(path))
   ...:     
/one/two: ('/one', 'two')
/one/two/: ('/one/two', '')
/: ('/', '')
.: ('', '.')
: ('', '') 
```  
os.path.basename()返回的值等价于os.path.split()值的第二部分，而os.path.basename()值则对应第一部分。  
os.path.splitext()会根据扩展名分隔符来分割路径。  
```Python  
In [5]: import os.path

In [8]: for path in ['f.txt',
             'f',
             '/s/f.txt',
             'sss.',
             '',]:
    print '%s: %s'%(path, os.path.splitext(path))  
f.txt: ('f', '.txt')
f: ('f', '')
/s/f.txt: ('/s/f', '.txt')
sss.: ('sss', '.')
: ('', '')
```  
os.path.commonprefix()可以获得所有路径中都出现的公共前缀，但这个前缀不一定存在。  

- 6.1.2 建立路径  
os.path.join()知道，目前用这个也够用了。  
os.apth.expanduser()可以将波浪线(~)转换为用户主目录。如果找不到用户主目录，则字符串不做任何改动直接返回  
os.path.expandvars()会扩展路径中出现的所有shell环境变量  

- 6.1.3 规范化路径  
os.path.normpath可以清除路径中多余的分隔符或相对路径。  
```Python  
In [9]: os.path.normpath('o/./123//')
Out[9]: 'o/123'
```  
os.path.abspath()可以将一个相对路径转化为绝对路径  
```Python  
In [10]: os.path.abspath('/code')
Out[10]: '/code'

In [11]: os.path.abspath('./code')
Out[11]: '/home/jblue13/code'

In [12]: os.path.abspath('../code')
Out[12]: '/home/code'
```  

- 6.1.4 文件时间  
&emsp;- os.path.getatime()返回访问时间  
&emsp;- ospath.getmtime()返回修改时间  
&emsp;- os.path.getctime()返回创建时间  
&emsp;- os.path.getsize()返回文件中的数据量，尝试了下对文件夹使用显示的都是4096(UNIX下)，推测只可以对文件使用。  

- 6.1.5 测试文件  
这一块方法比较多，自己有用过：os.path.isfile(), os.path.isdir(), os.path.exists(),其他用到了再说。  

- 6.1.6 遍历一个目录树  
介绍的os.path.walk()方法，但查阅了一些资料之后，os.walk()要更优一些。  
os.walk明显比os.path.walk要简洁一些，起码它不需要回调函数，遍历的时候一目了然：root，subdirs，files。  

### 6.5 shutil--高级文件操作  

- 6.5.1 复制文件  
copyfilr()将源的内容复制到目标，如果没有权限写木匾文件则产生IOError  
```Python  
from shutil import * 

copyfile('sss.py', 'sss.py.copy')
```  
copyfile()的实现使用了底层函数copyfileobj().copyfile()的参数是文件名，但copyfileobj()的参数是打开的文件句柄。还可以有第三个参数：用于读入块的一个缓冲区长度。默认是大数据块读取，使用-1会一次读入所有输入，使用正数可以设置特定的块大小。  
类似于UNIX命令行工具cp，copy()函数会用同样的方式解释输出名。如果指定的目标指示一个目录而不是一个文件，会使用源文件的基名在该目录中穿件一个新文件。文件的权限会随内容复制  
```Python  
from shutil import * 
import os

os.mkdir('example')
copyfile('sss.py', 'example')
print os.listdir('example')

['sss.py']
```  
copy2()的工作类似于copy(),不过复制到新文件的元数据中会包含访问和修改时间。

- 6.5.2 复制文件元数据  
默认地，在UNIX下创建一个新文件时，他会根据当前用户的umask接受权限。要把权限从一个文件复制到另一个文件，可以使用copymode().  
copystat()智慧复制与文件关联的权限和日期。  

- 6.5.3 处理目录树  
shutil包含3个函数用来处理目录树。要把一个目录从一个位置复制到另一个位置，可以使用copytree，当然，目标目录不能已存在。  
```Python  
from shutil import * 

copytree('../shutil', '/tmp/example')
```  
要删除一个目录及其中的内容，用rmtree()。默认地，错误会作为异常产生，不过如果第二个参数为true，就可以忽略这些异常。可以在第三个参数中提供一个特殊的错误处理函数。  
```Python  
from shutil import * 

rmtree('/tmp/example')
```  
move()的语义与UNIX命令mv类似，如果源和目标都在同一个文件系统内，则会重命名源文件。否则，源文件会复制到目标文件，然后将源文件删除。  
```Python  
from shutil import * 

move('example.txt', 'example.out')
```  


# 第七章 数据持久存储与交换  

### 7.7 csv--逗号分隔值文件  
- 7.7.1 读文件  
读文件的时，输入数据的每一行都会解析，自动处理嵌在行字符串中的换行符，并转化为一个字符串list.
```Python
import csv
import sys

with open(sys.argv[1], 'rt') as f:
    reader = csv.reader(f)
    for row in reader:
        print row
```  

- 7.7.2 写文件  
使用writer()创建一个对象来写数据，然后使用writerow()迭代处理文本进行打印。  
```Python
import csv
import sys

with open(sys.argv[1], 'wt') as f:
    writer = csv.writer(f)
    writer.writerow(('Title 1', 'Title 2', 'Title 3'))

print open(sys.argv[1], 'rt').read()
Title 1, Title 2, Title 3

#引号需要加入额外参数，这里给出包含非数值内容的列周围加引号
writer = csv.writer(f, quoting=csv.QUOTE_NONNUMERIC)
"Title 1", "Title 2", "Title 3"
```  

- 7.7.3 方言  
由于用逗号分格文件有一定局限，可以将多个参数组在一起，构成一个方言(dialect)对象。  
```Python
# 示例一下注册方言，其他的用到再说  
import csv 

csv.register_dialect('pipes', delimiter='/')

with open('testdata.pipes', 'r') as f:
    reader = csv.reader(f, dialect='pipes')
    for row in reader:
        print row
```  

- 7.7.4 使用字段名  
csv中可以将航作为字典处理，从而对字段命名。DictReader和DictWriter类将行转换为字典而不是列表。字典的键可以传入，也可以由第一行推导得出(如果行包含首部).  
```Python
# 示例一下注册方言，其他的用到再说  
import csv 

csv.register_dialect('pipes', delimiter='/')

with open('testdata.pipes', 'r') as f:
    reader = csv.DictReader(f)
    for row in reader:
        print row
```  



# 第十章 进程与线程  

### 10.1 subprocess---创建附加进程  

- 10.1.1 运行外部命令  
函数call(),check_call()和check_output()都是Popen类的包装器。
```Python
In [1]: import subprocess

In [2]: subprocess.call(['ls', '-l'])  # ('ls -l')这种形式是错的呢
total 48
drwxrwxr-x 2 jblue13 jblue13 4096 Dec 22 18:11 code
drwxrwxr-x 2 jblue13 jblue13 4096 Sep  2 11:46 conversion
drwxrwxr-x 6 jblue13 jblue13 4096 Oct 29 14:37 demo
drwxr-xr-x 3 jblue13 jblue13 4096 Nov 25 16:23 Desktop
drwxrwxrwx 6 jblue13 jblue13 4096 Sep  9 13:25 detect_manga
drwxrwxr-x 6 jblue13 jblue13 4096 Nov  4 17:25 Jblue
drwxrwxr-x 7 jblue13 jblue13 4096 Sep 11 17:45 log_analyze
drwxrwxr-x 4 jblue13 jblue13 4096 Sep 11 17:04 logs
drwxrwxr-x 5 jblue13 jblue13 4096 Nov  1 13:55 manage
drwxr-xr-x 2 jblue13 jblue13 4096 Aug 14 10:20 Public
drwxrwxr-x 3 jblue13 jblue13 4096 Sep  6 13:34 src
drwxrwxr-x 3 jblue13 jblue13 4096 Sep  3 09:53 testvirtual
Out[2]: 0

In [4]: subprocess.call(['ls', '-l'], shell=True)
code  conversion  demo  Desktop  detect_manga  Jblue  log_analyze  logs  manage  Public  src  testvirtual
Out[4]: 0
```  
加上shell=True，意味着运行命令前会先处理命令串中的变量什么的。  
call()的返回值是程序的退出码，非0就是错，可以用check_call()来机型检查。  
```Python
try:
    subprocess.check_call(['false'])    
except subprocess.CalledProcessError as err:
    print 'ERROR:', err   
ERROR: Command '['false']' returned non-zero exit status 1
```  
对于call()启动的进程，其标准输入和输出通道会绑定到父进程的输入和输出。可以使用check_output()捕获输出  
```Python
In [9]: subprocess.check_output(["ls", "-l"])
Out[9]: 'total 48\ndrwxrwxr-x 2 jblue13 jblue13 4096 Dec 22 18:11 code\ndrwxrwxr-x 2 jblue13 jblue13 4096 Sep  2 11:46 conversion\ndrwxrwxr-x 6 jblue13 jblue13 4096 Oct 29 14:37 demo\ndrwxr-xr-x 3 jblue13 jblue13 4096 Nov 25 16:23 Desktop\ndrwxrwxrwx 6 jblue13 jblue13 4096 Sep  9 13:25 detect_manga\ndrwxrwxr-x 6 jblue13 jblue13 4096 Nov  4 17:25 Jblue\ndrwxrwxr-x 7 jblue13 jblue13 4096 Sep 11 17:45 log_analyze\ndrwxrwxr-x 4 jblue13 jblue13 4096 Sep 11 17:04 logs\ndrwxrwxr-x 5 jblue13 jblue13 4096 Nov  1 13:55 manage\ndrwxr-xr-x 2 jblue13 jblue13 4096 Aug 14 10:20 Public\ndrwxrwxr-x 3 jblue13 jblue13 4096 Sep  6 13:34 src\ndrwxrwxr-x 3 jblue13 jblue13 4096 Sep  3 09:53 testvirtual\n'

#加上shell=True，和在call里面效果一样
In [12]: subprocess.check_output(["ls", "-l"], shell=True)
Out[12]: 'code\nconversion\ndemo\nDesktop\ndetect_manga\nJblue\nlog_analyze\nlogs\nmanage\nPublic\nsrc\ntestvirtual\n'

#加上stderr=subprocess.STDOUT，如果出错，错误信息将不会输出到控制台
In [13]: subprocess.check_output(["ls", "-l"], shell=True, stderr=subprocess.STDOUT)
Out[13]: 'code\nconversion\ndemo\nDesktop\ndetect_manga\nJblue\nlog_analyze\nlogs\nmanage\nPublic\nsrc\ntestvirtual\n'
```  

- 10.1.2 直接处理管道  
要运行一个进程并读取它的所有输出，可以设置stdout值为PIPE并调用communicate().  
```Python
In [5]: p = subprocess.Popen('ls', stdout=subprocess.PIPE)

In [6]: s = p.communicate()[0]

In [7]: s
Out[7]: 'code\nconversion\ndemo\nDesktop\ndetect_manga\nJblue\nlog_analyze\nlogs\nmanage\nPublic\nsrc\ntestvirtual\n'
```  
要把数据一次性发送到进程的标准输入通道，可以把数据传递到communicate().  
```Python
In [9]: p = subprocess.Popen(['cat', '-'], stdin=subprocess.PIPE)

In [10]: p.communicate('\tstdin: to stdin\n')
    stdin: to stdin
Out[10]: (None, None)
```  
类似的，还有错误捕捉。不过要将错误输出从进程定位到标准输出通道，应是**stderr=subprocess.STDOUT**  

- 10.1.3 连接管道段  
多个命令可以连接为一个管线(pipeline),类似于UNIX shell的做法，即单独创建Popen实例，把它们的输入和输出串联在一起。一个Popen实例的stdout属性用作管线中下一个Popen实例的stdin参数，而不是常量PIPE。
```Python
In [11]: cat = subprocess.Popen(['cat', 'index.txt'], stdout=subprocess.PIPE)

In [12]: gerp = subprocess.Popen(['grep','..include::'], stdin=cat.stdout, stdout=subprocess.PIPE)
```  

- 10.1.5 进程间传递信号  
信号由signal模块获得，sys.stout.flush()进行刷新。  
因为Popen创建的进程创建了子进程，这些子进程不会接收发送父进程的信号。所以进行进程组通信，让子进程也能接受信号。进程组用os.setsid()创建，将“会话id”设置为当前进程的进程id。所有的子进程都会从其父进程继承会话id。
```Python
import os
import signal
import subprocess
import tempfile
import time
import sys

script = '''#!/bin/sh
echo "Shell script in process $$"
set -x
python signal_child.py
'''
script_file = tempfile.NamedTemporaryFile('wt')
script_file.write(script)
script_file.flush()

def show_setting_sid():
    print 'Calling os.setsid() from %s'% os.getpid()
    sys.stdout.flush()
    os.setsid()

proc = subprocess.Poen(['sh', script_file.name],
                        close_fds=True,
                        preexec_fn=show_setting_sid)
sys.stdout.flush()
print 'Signaling process group %s'%proc.pid
sys.stdout.flush()
os.killpg(proc.pid, signal.SIGUSR1)
time.sleep(3)
```  


# 第十六章 开发工具  

### 16.4 traceback---异常和栈轨迹  
This module provides a standard interface to extract, format and print stack traces of Python programs. It exactly mimics the behavior of the Python interpreter when it prints a stack trace. This is useful when you want to print stack traces under program control, such as in a “wrapper” around the interpreter.  
- 16.4.2 处理异常  
print_exc()使用sys.exc_info()来得到当前线程的异常信息，格式化结果，并把文本打印到一个文件句柄(默认为sys.stderr)。
```Python
import traceback
import sys

try:
    xxx
except Exception, err:
    traceback.print_exc(file=sys.stdout)
```  
print_exc()是print_exception()缩写，使用它需要三个显示参数:**异常类型、异常值和traceback**，与之相似的是format_exception()，后者主要用于准备文本。
```Python
import traceback
import sys

try:
    xxx
except Exception, err:
    exc_type, exc_value, exc_tb = sys.exc_info()
    traceback.print_exception(exc_type, exc_value, exc_tb)

try:
    xxx
except Exception, err:
    exc_type, exc_value, exc_tb = sys.exc_info()
    pprint(traceback.format_exception(exc_type, exc_value, exc_tb))
```  
traceback.extract_tb()返回值是一个项列表，每一项是一个元组，包括4部分：**源文件名、该文件中的行号、 函数名和该行的源文本**  

- 16.4.3 处理栈  
print_stack()是对当前调用栈而不是traceback完成的操作，会打印当前栈，而不是报错。与之对应的还有format_stack(),extrack_stack()类似于extract_tb()。