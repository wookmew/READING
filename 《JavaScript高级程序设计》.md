# 目录  
- [第一章 JavaScript简介]()
- [第二章 在HTML中使用JavaScript]()
- [第三章 基本概念]()

# 第一章 JavaScript简介  

### 1.2 JavaScript实现  
JavaScript = 核心(ECMAScript) + 文档对象模型(DOM) + 浏览器对象模型(BOM)  

# 第二章 在HTML中使用JavaScript  

### 2.1 <script\>元素  
- async：可选， 规定异步执行脚本（仅适用于外部脚本）。  
- charset：可选，规定在外部脚本文件中使用的字符编码。  
- defer：可选， 规定是否对脚本执行进行延迟，直到页面加载为止（仅适用于外部脚本）。  
- src：可选，规定外部脚本文件的 URL。  
- type：可选，用来表示编写代码使用的脚本语言的内容类型。  
如果<script\>标签中含有src，则在<script\></script\>中，不可以有代码，有也会被忽略。  

# 第三章 基本概念  

### 3.1 语法  
- 区分大小写  
- //为单行注释， /\*...\*/为多行注释。  
- 使用"use strict"来确定js语法是否有错误。  
- JavaScript的语法和Java语言类似，每个语句以**;**结束，语句块用**{...}**。但是，JavaScript并不强制要求在每个语句的结尾加**;**，浏览器中负责执行JavaScript代码的引擎会自动在每个语句的结尾补上**;**。  

### 3.3 变量  
变量在JavaScript中就是用一个变量名表示，变量名是大小写英文、数字、$和_的组合，且不能用数字开头。变量名也不能是JavaScript的关键字，如if、while等。申明一个变量用**var**语句。  
```javascript
var a; // 申明了变量a，此时a的值为undefined
var $b = 1; // 申明了变量$b，同时给$b赋值，此时$b的值为1
var s_007 = '007'; // s_007是一个字符串
var Answer = true; // Answer是一个布尔值true
var t = null; // t的值是null
```  
JavaScript在设计之初，为了方便初学者学习，并不强制要求用var申明变量。这个设计错误带来了严重的后果：如果一个变量没有通过var申明就被使用，那么该变量就自动被申明为**全局变量**：  

###  3.4 数据类型  
ECMAScript中有5种简单数据类型：Undefined, Null, Boolean, Number, string。还有一种复杂数据类型--Object。  
**1. typeof操作符**  
类似于Python里的type()函数，可以写成：  
```javascript
var m = "aa";
alert(typeof m);  //"string"
alert(typeof(m));  //"string"
```  
**2. Undefined类型**  
undefined表示值未定义,仅仅在判断函数参数是否传递的情况下有用。  
**3. Null类型**  
null表示一个“空”的值，它和0以及空字符串''不同，0是一个数值，''表示长度为0的字符串，而null表示“空”。  
**4. Boolean类型**  
布尔值和布尔代数的表示完全一致，一个布尔值只有true、false两种值，要么是true，要么是false，可以直接用true、false表示布尔值，也可以通过布尔运算计算出来.  
**5. Number类型**  
JavaScript不区分整数和浮点数，统一用Number表示，以下都是合法的Number类型：  
```javascript
123; // 整数123
0.456; // 浮点数0.456
1.2345e3; // 科学计数法表示1.2345x1000，等同于1234.5
-99; // 负数
NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
Infinity; // Infinity表示无限大，当数值超过了JavaScript的Number所能表示的最大值时，就表示为Infinity
```  
有3个函数可以把非数值转换为数值：Number(), parseInt(), parseFloat().  
parseInt()比Number()更为准确，parseFloat()是字符串第一个小数有效，第二个就无效了。  
```javascript
var n1 = Number("Hello world!"); //NaN
var n2 = Number(""); //0
var n3 = Number("000011"); //11
var n4 = Number("true"); //1

var n1 = parseInt("1234blue"); //1234
var n2 = parseInt(""); //NaN
var n3 = parseInt("0xA"); //10(十六进制)
var n4 = parseInt("22.5"); //22
var n5 = parseInt("070"); //56(八进制)
var n6 = parseInt("70"); //70(十进制)
var n7 = parseInt("0xf"); //15(十六进制)
var n8 = parseInt("AF", 16); //175(第二个参数定义为十六进制)

var n1 = parseFloat("1234blue"); //1234
var n2 = parseFloat("0xA"); //0
var n3 = parseFloat("22.5"); //22.5
var n4 = parseFloat("22.34.5"); //22.34
var n5 = parseFloat("0908.5"); //908.5
var n6 = parseFloat("3.123e7"); //31250000
```  
**6. String类型**  
字符串是以单引号'或双引号"括起来的任意文本，比如'abc'，"xyz"等等。请注意，''或""本身只是一种表示方式，不是字符串的一部分，因此，字符串'abc'只有a，b，c这3个字符。  
字符串是无法更改的，这点和Python类似。有toString()和String()两个方法，将值转化为字符串。  
**7. Object类型**  
JavaScript的对象是一种无序的集合数据类型，它由若干键值对组成。在ECMAScript中，Object类型是所有它的实例的基础。  

### 3.5 操作符  
借鉴于c，这一段书中解释很详细，没接触过c的可以细看。  

### 3.6 语句  
**1. if语句**  
JavaScript使用if () { ... } else { ... }来进行条件判断。例如，根据年龄显示不同内容，可以用if语句实现如下：  
```javascript
var age = 20;
if (age >= 18) { // 如果age >= 18为true，则执行if语句块
    alert('adult');
} else { // 否则执行else语句块
    alert('teenager');
}
```  
如果还要更细致地判断条件，可以使用多个if...else...的组合：  
```javascript
var age = 3;
if (age >= 18) {
    alert('adult');
} else if (age >= 6) {
    alert('teenager');
} else {
    alert('kid');
}
```   
**2. do-while语句**  
do { ... } while()循环，它和while循环的唯一区别在于，不是在每次循环开始的时候判断条件，而是在每次循环完成的时候判断条件：  
```javascript
var n = 0;
do{
    n = n + 1;
}while(n < 100);
n; //100
```   
用do { ... } while()循环要小心，循环体会至少执行1次，而for和while循环则可能一次都不执行。  
**3. while语句**  
for循环在已知循环的初始和结束条件时非常有用。而上述忽略了条件的for循环容易让人看不清循环的逻辑，此时用while循环更佳。  
while循环只有一个判断条件，条件满足，就不断循环，条件不满足时则退出循环。比如我们要计算100以内所有奇数之和，可以用while循环实现：  
```javascript
var x = 0;
var n = 99;
while(n > 0){
    x = x + n;
    n = n - 2;
}
x; //2500 
```  
在循环内部变量n不断自减，直到变为-1时，不再满足while条件，循环退出。  
**4. for语句**  
for循环最常用的地方是利用索引来遍历数组  
```javascript
var arr = ['Apple', 'Google', 'Microsoft'];
var i, x;
for(i=0, i<arr.length; i++){
    x = arr[i]
    alert(x);
}
```   
for循环的3个条件都是可以省略的，如果没有退出循环的判断条件，就必须使用break语句退出循环，否则就是死循环：  
```javascript
var x = 0;
for(::){// 将无限循环下去
    if(x > 100){// 通过if判断来退出循环
    break;
    }
    x ++;
}
```  
**5. for ... in语句**  
for循环的一个变体是for ... in循环，它可以把一个对象的所有属性依次循环出来：  
```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'beijing'
};
for(var key in o){
    alert(key); // 'name', 'age', 'city'
}
```  
要过滤掉对象继承的属性，用hasOwnProperty()来实现：  
```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'beijing'
};
for(var key in o){
    if(o.hasOwnProperty(key)){
        alert(key); // 'name', 'age', 'city'
    }
}
```   
由于Array也是对象，而它的每个元素的索引被视为对象的属性，因此，for ... in循环可以直接循环出Array的索引：  
```javascript
var a = ['A', 'B', 'c'];
for(var i in a){
    alert(i);// '0', '1', '2'
    alert(a[i]);// 'A', 'B', 'C'
}
```  
**请注意，for ... in对Array的循环得到的是String而不是Number。**  
**6. label语句**  
使用label语句可以在代码中添加标签：  
```javascript
start: for(var i=0; i<count; i++){
    alert(i);
}
```  
这个例子中定义的start标签可以在将来由break或continue语句引用。加标签的语句要与for语句等循环语句配合使用。  
**7. break & continue**  
break语句会立即退出循环，强制继续执行循环后面的语句；而continue语句虽然也是立即退出循环，但退出循环后会从循环的顶部继续执行。
```javascript  
var num = 0;
for(var i=1; i<10; i++){
    if(i%5 == 0){
        continue;
    }
    num++;
}
alert(num); //8
```  
此处循环8次，即i==5这次没计数，而换成break，则只计数4次。  
**8. with语句**  
with语句的作用是将代码的作用域设置到一个特定的对象中。  
```javascript  
var qs = location.search.substring(1);
var hostName = location.hostname;
var url = location.href;
//可以简写为
with(location){
    var qs = search.substring(1);
    var hostName = hostname;
    var url = href;
}
```
大量使用会使性能降低，所以不建议使用。  
**9. switch语句**  
也是借鉴于c，并有break关键字用来跳出语句。  
```javascript  
switch(i){
    case 25:
        //
    case 35:
        alert("25 or 35");
        break;
    case 45:
        alert("45");
        break;
    default:
        alert("Other");
}
```  

### 3.7 函数  
```javascript  
function abs(x){
    if(x >= 0){
        return x;
    }else{
        return -x;
    }
}
```  
上述abs()函数的定义如下：  
- function指出这是一个函数定义；  
- abs是函数的名称；  
- (x)括号内列出函数的参数，多个参数以,分隔；  
- { ... }之间的代码是函数体，可以包含若干语句，甚至可以没有任何语句。  
**请注意，函数体内部的语句在执行时，一旦执行到return时，函数就执行完毕，并将结果返回。因此，函数内部通过条件判断和循环可以实现非常复杂的逻辑。**  
函数可以通过函数名来调用，圆括号中多个参数可以用,隔开：  
```javascript  
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
```  
