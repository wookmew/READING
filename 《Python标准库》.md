> 10月份重新补习了Python基础，今天来开始补习标准库，就这么一路背下来。凡是自己不会的都记录下来，熟悉下. 优先处理自己要学习的东西。  

# 目录  

- [第一章 文本](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8APython%E6%A0%87%E5%87%86%E5%BA%93%E3%80%8B.md#第一章-文本)  
- [第十章 进程与线程]()  
- [第十六章 开发工具]()  



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
 