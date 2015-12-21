> 10月份重新补习了Python基础，今天来开始补习标准库，就这么一路背下来。凡是自己不会的都记录下来，熟悉下. 

# 目录  

- [第一章 文本]()  

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
