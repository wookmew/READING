#[STACKOVERFLOW PYTHON 百问](http://www.wklken.me/posts/2013/07/20/python-stackoverflow-vote-top.html#_1)
> 原作地址奉上，此处仅作自己的学习。
<br/>
##基本语法控制流相关
---
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
'''python
if 'x' in locals():
    #x exists
'''
检测全局变量<br/>
'''python
if 'x' in globals():
    #x exists
'''
检查一个对象是否包含某个属性<br/>
'''python
if hasattr(obj, 'attr_name'):
    #obj.attr_name exists
'''

**10. python中是否存在三元运算符**<br/>
三元运算符在Python2.5中被加入
'''python
a if test else b
'''
使用
'''python
'true' if True else 'false'
'true'
'true' if False else 'false'
'false'
'''
[doc](https://docs.python.org/3.3/faq/programming.html#is-there-an-equivalent-of-c-s-ternary-operator)

**11. python中的de-while**<br/>
实现：
'''python
while True:
    stuff()
    if fail_condition:
        break
or
stuff()
while not fail_condition:
    stuff()
'''

**12. 相对于range() 应该更倾向于实用xrange()?**<br/>
就性能而言, 特别是当你迭代一个大的range, xrange()更优. 但是, 有一些情况下range()更优:<br/>
- 在Python3中, range() 等价于 python2.x的 xrange(), 而xrange()不存在了.(如果你的代码将在2和3下运行, 不能使用xrange)<br/>
- range()在某些情况下更快, 例如, 多次重复遍历同一个序列.<br/>
- xrange()并不适用于所有情况, 例如, 不支持slices以及任意的list方法<br/>

**13. 在Python中，“i += x”和“i = i + x”什么时候不等**<br/>
这完全取决于i这个对象。

+=调用了[__iadd__](https://docs.python.org/2/reference/datamodel.html#object.__iadd__)方法（如果存在—不存在就退一步调用__add__），然而+调用[__add__方法](https://docs.python.org/2/reference/datamodel.html#object.__add__)<br/>
从一个API的角度，__iadd__期望被使用在恰当的位置修改易变的对象（返回的对象也是转变后的），而__add__应该返回某些东西的一个新的实例。对于不可变对象，两种方法都返回新的实例，但__iadd__会把新的实例放在和旧实例名字**相同的命名空间里**。这就是为什么:<br/>
'''python
i = 1
i += 1
'''
看上去增量i，实际上，你得到了一个新的数值，并且转移到了i的最上面—丢掉了旧的数值的引用。在这种情况下，i += 1和i = i + 1是完全一样的。但是，对于大多数可变对象，这是完全不同的：<br/>
'''python
a = [1, 2, 3]
b = a
b += [1, 2, 3]
print a  #[1, 2, 3, 1, 2, 3]
print b  #[1, 2, 3, 1, 2, 3]
'''
对比一下<br/>
'''python
a = [1, 2, 3]
b = a
b = b + [1, 2, 3]
print a #[1, 2, 3]
print b #[1, 2, 3, 1, 2, 3]
'''
注意第一个例子，从b和a代表相同的对象开始，当我对b使用+=，实际上它改变了b（a看起来也改变了- -毕竟他们代表同一个列表）。但是在第二个例子里，当我进行b = b + [1, 2, 3]操作时，b被引用并且和一个新的列表[1, 2, 3]联系了起来。之后在b的命名空间保存了这个关联的列表- -不考虑b之前的序列。<br/>