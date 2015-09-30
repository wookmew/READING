> 最新版ECMAScript 6标准（简称ES6）已经在2015年6月正式发布了，所以在学习过程中，自己配合[JavaScript教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)在学习。  

# 目录  
- [第一章 JavaScript简介](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8AJavaScript%E9%AB%98%E7%BA%A7%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1%E3%80%8B.md#第一章-javascript简介)
- [第二章 在HTML中使用JavaScript](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8AJavaScript%E9%AB%98%E7%BA%A7%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1%E3%80%8B.md#第二章-在html中使用javascript)
- [第三章 基本概念](https://github.com/GJBLUE/READING-/blob/master/%E3%80%8AJavaScript%E9%AB%98%E7%BA%A7%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1%E3%80%8B.md#第三章基本概念)  
- [第四章 变量，作用域和内存问题]()
- [第五章 引用类型]()

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
**note：所有参数传递的都是值，不可能通过引用传递参数**  
另外js中没有重载，如果定义了两个名字相同的函数，则该名字只属于后定义的函数。  
```javascript  
function addn(num){
    return num + 100;
}
function addn(num){
    return num + 200;
}
var result = addn(300); //500
```  

# 第四章 变量，作用域和内存问题  

### 4.1 基本类型和引用类型的值  
js包含两种不同的数据类型的值：基本类型值和引用类型值。基本类型值指的是简单的数据段，而引用类型值指那些可能由多个值构成的对象。引用类型值是保存在内存中的对象，js不允许直接访问内存中的位置。因此，引用类型的值是按引用访问的。  
```javascript  
//引用类型如下:
var person = new Object();
person.name = "Nicholas";
alert(person.name); //Nicholas
``` 
检测引用类型的值是什么类型时，可以用instanceof：  
```javascript  
alert(p instanceof Object); //检测变量p是否是Object
```   

### 4.2 执行环境及作用域  
在JavaScript中，用var申明的变量实际上是有作用域的。如果一个变量在函数体内部申明，则该变量的作用域为整个函数体，在函数体外不可引用该变量：  
```javascript  
'use strict';

function foo(){
    var x = 1;
    x = x + 1;
}
x = x + 2; // ReferenceError! 无法在函数体外引用变量x
```   
由于JavaScript的函数可以嵌套，此时，内部函数可以访问外部函数定义的变量，反过来则不行：  
```javascript  
'use strict';

function foo(){
    var x = 1;
    function bar(){
        var y = x + 1;// bar可以访问foo的变量x!
    }
    var z = y + 1;// ReferenceError! foo不可以访问bar的变量y!
}
```  
如果内部函数和外部函数的变量名重名,函数在查找变量时从自身函数定义开始，从“内”向“外”查找。如果内部函数定义了与外部函数重名的变量，则内部函数的变量将“屏蔽”外部函数的变量。  
**1. 变量提升**  
JavaScript的函数定义有个特点，它会先扫描整个函数体的语句，把所有申明的变量“提升”到函数顶部：  
```javascript  
'use strict';

function foo(){
    var x = 'Hello, ' + y;
    alert(x);
    y = 'Bob';
}
foo(); //Hello, undefined(y为undefined)
```  
由于JavaScript的这一怪异的“特性”，我们在函数内部定义变量时，请严格遵守“在函数内部首先申明所有变量”这一规则。最常见的做法是用一个var申明函数内部用到的所有变量：  
```javascript  
function foo(){
    var 
        x = 1, // x初始化为1
        y = x + 1, // y初始化为2
        z, i; // z和i为undefined
}
```  
**2. 全局作用域**  
不在任何函数内定义的变量就具有全局作用域。实际上，JavaScript默认有一个全局对象window，全局作用域的变量实际上被绑定到window的一个属性：  
```javascript  
'use strict';

var course = 'Learn JavaScript';
alert(course); // 'Learn JavaScript'
alert(window.course); // Learn JavaScript'
```  
由于函数定义有两种方式，以变量方式var foo = function () {}定义的函数实际上也是一个全局变量，因此，顶层函数的定义也被视为一个全局变量，并绑定到window对象：  
```javascript  
'use strict';

function foo(){
    alert('foo');
}

foo(); // 直接调用foo()
window.foo(); // 通过window.foo()调用
```  
**3. 名字空间**  
全局变量会绑定到window上，不同的JavaScript文件如果使用了相同的全局变量，或者定义了相同名字的顶层函数，都会造成命名冲突，并且很难被发现。减少冲突的一个方法是把自己的所有变量和函数全部绑定到一个全局变量中。例如：  
```javascript  
'use strict';

// 唯一的全局变量MYAPP:
var mvp = {};

// 其他变量:
mvp.name = 'myapp';
mvp.version = 1.0;

// 其他函数:
mvp.foo = function(){
    return 'foo';
};
```  
把自己的代码全部放入唯一的名字空间mvp中，会大大减少全局变量冲突的可能。许多著名的JavaScript库都是这么干的：jQuery，YUI，underscore等等。  
**4. 局部作用域**  
由于JavaScript的变量作用域实际上是函数内部，我们在for循环等语句块中是无法定义具有局部作用域的变量的：  
```javascript  
'use strict';

function foo(){
    for (var i=0; i<100; i++){
        //
    }
    i += 100; // 仍然可以引用变量i
}
```  
为了解决块级作用域，ES6引入了新的关键字let，用let替代var可以申明一个块级作用域的变量：  
```javascript  
'use strict';

function foo(){
    var sum = 0;
    for (let i=0; i<100; i++){
        sum += i;
    }
    i += 1; // SyntaxError
}
``` 
**5. 常量**  
由于var和let申明的是变量，如果要申明一个常量，在ES6之前是不行的，我们通常用全部大写的变量来表示“这是一个常量，不要修改它的值”。ES6标准引入了新的关键字const来定义常量，const与let都具有块级作用域：  
```javascript  
'use strict';

const PI = 3.14;
PI = 3.14;// 某些浏览器不报错，但是无效果！
PI;//3.14
```  

### 4.3 垃圾回收机制  
类似于Python，内存是自动分配和回收的。  
**1. 标记清除**  
JavaScript最常用的垃圾收集方式是标记清除，当变量进入环境，就标记为“进入环境”；离开环境时标记为“离开环境”。  
**2. 引用计数**  
类似于Python，引用一次，引用次数+1.  
**3. 管理内存**  
优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再有用，最好通过将其值设置为null来释放其引用--这个做法叫解除引用。  
```javascript  
'use strict';

function cn(name){
    var lp = new Object();
    lp.name = name;
    return lp;
}
var gp = cn("gjj");

//解除gp的引用
gp = null;
```  

# 第五章 引用类型  
引用类型是一种数据结构，用于将数据和功能组织在一起。它也常被称为类，但这种称呼并不妥当。尽管 ECMAScript从技术上讲是一门面向对象的语言，但它不具备传统的面向对象语言所支持的类和接口等基本结构。引用类型有时候也被称为对象定义，因为它们描述的是一类对象所具有的属性和方法。换句话说，JavaScript不区分类和实例的概念，而是通过原型（prototype）来实现面向对象编程。此处参考了[相关资料](http://howiefh.github.io/2015/08/28/javascript-reference-type/)。  
### 5.1 Object类型  
创建Object实例有两种方式：  
```javascript  
'use strict';

//第一种是new操作符后跟Object构造函数;
var p = new Object();
p.name = "gjj";

//第二种是使用对象字面量表示法;
var p = {
    name: "gjj",
    age: 24
};
```  
开发人员更青睐对象字面量语法，因为这种语法要求的代码量少，而且能够给人封装数据的感觉。实际上，对象字面量也是向函数传递大量可选参数的首选方式。最好的做法是对那些必需值使用命名参数，而使用对象字面量来封装多个可选参数。在通过对象字面量定义对象时，实际上不会调用Object构造函数.  
ES6允许直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。  
```javascript  
'use strict';

var v = 'world';
var obj = {
    v,
    method(){
        return 'Hello!';
    }
};
```  

### 5.2 Array类型  
ECMAScript 数组的每一项可以保存任何类型的数据。创建数组的基本方式有两种。第一种是使用 Array 构造函数。  
```javascript  
'use strict';

var c = new Array(); //new 可以忽略不写
```  
创建数组的第二种基本方式是使用数组字面量表示法。数组字面量由一对包含数组项的方括号表示，多个数组项之间以逗号隔开，如下所示：  
```javascript  
'use strict';

var colors = ["red", "blue", "green"]; // 创建一个包含3 个字符串的数组
var names = []; // 创建一个空数组
var values = [1,2,]; // 不要这样！这样会创建一个包含2 或3 项的数组，对于IE早期版本(<=8)这里将会是三项，最后一项是undefined，下面同理
var options = [,,,,,]; // 不要这样！这样会创建一个包含5 或6 项的数组
```  
ECMAScript数组的大小是可以动态调整的，即可以随着数据的添加自动增长以容纳新增数据。与对象一样，在使用数组字面量表示法时，也不会调用Array构造函数数组的length属性很有特点——它不是只读的。因此，通过设置这个属性，可以从数组的末尾移除项或向数组中添加新项。  
**1. 检测数组**  
instanceof 操作符的问题在于,它假定只有一个全局执行环境。如果网页中包含多个框架,那实际上就存在两个以上不同的全局执行环境,从而存在两个以上不同版本的 Array 构造函数。如果你从一个框架向另一个框架传入一个数组,那么传入的数组与在第二个框架中原生创建的数组分别具有各自不同的构造函数。ECMAScript 5新增了Array.isArray()方法。这个方法的目的是最终确定某个值到底是不是数组，而不管它是在哪个全局执行环境中创建的。  
```javascript  
'use strict';

if (Array.isArray(value)){
    //对数组执行某些操作
}
```  
**2. 转换方法**  
数组的toString()方法会返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。为了创建这个字符串会调用数组每一项的 toString() 方法。toLocaleString()与toString()有些类似，只不过是调用每项的toLocaleString()。而调用 valueOf()返回的还是数组。  
如果使用 join() 方法,则可以使用不同的分隔符来构建这个字符串。 join() 方法只接收一个参数,即用作分隔符的字符串,然后返回包含所有数组项的字符串。如果不给 join() 方法传入任何值,或者给它传入 undefined ,则使用逗号作为分隔符。IE7 及更早版本会错误的使用字符串 “undefined” 作为分隔符。  
```javascript  
'use strict';

var colors = ["red", "green", "blue"];
alert(colors.join()); //red,green,blue
alert(colors.join("||")); //red||green||blue
```  
如果数组中的某一项的值是 null 或者 undefined，那么该值在 join()、toLocaleString()、toString() 和 valueOf() 方法返回的结果中以空字符串表示。  
**3. 栈方法**  
ECMAScript为数组专门提供了push()和pop()方法，以便实现类似栈的行为。这两个方法会改变length的值。  
```javascript  
'use strict';

var colors = ["red", "green", "blue"];
alert(colors.join()); //red,green,blue
alert(colors.join("||")); //red||green||blue
```  
如果数组中的某一项的值是 null 或者 undefined，那么该值在 join()、toLocaleString()、toString() 和 valueOf() 方法返回的结果中以空字符串表示。  
**4. 栈方法**  
ECMAScript为数组专门提供了push()和pop()方法，以便实现类似栈的行为。这两个方法会改变length的值。  
```javascript  
'use strict';

var colors = new Array();// 创建一个数组
var count = colors.push("red", "green");// 推入两项
alert(count);//2
var item = colors.pop();// 取得最后一项
alert(item);//"black"
alert(colors.length);//1
```  
**5. 队列方法**  
shift() 能够移除数组中的第一个项并返回该项,同时将数组长度减 1。结合使用 shift() 和 push() 方法,可以像使用队列一样使用数组。  
unshift()与shift()的用途相反：它能在数组前端添加任意个项并返回新数组的长度。因此，同时使用unshift()和pop()方法，可以从相反的方向来模拟队列，即在数组的前端添加项，从数组末端移除项.  
**6. 重排序方法**  
数组中已经存在两个可以直接用来重排序的方法：reverse()和 sort().  
sort()方法会调用每个数组项的 toString()转型方法，然后比较得到的字符串，以确定如何排序。即使数组中的每一项都是数值，sort()方法比较的也是字符串.  
sort()方法可以接收一个比较函数作为参数，以便我们指定哪个值位于哪个值的前面。比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回0，如果第一个参数应该位于第二个之后则返回一个正数.  
```javascript  
'use strict';

function compare(v1, v2){
    if (v1 < v2){
        return -1;
    }else if (v1 > v2){
        return 1;
    }else{
        return 0;
    }
}
var values = [0, 1, 5, 10, 15];
values.sort(compare);
alert(values); //0,1,5,10,15
values.sort();
alert(values);//0,1,10,15,5
```  
**7. 操作方法**  
concat()方法可以基于当前数组中的所有项创建一个新数组。具体来说，这个方法会先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。在没有给concat()方法传递参数的情况下，它只是复制当前数组并返回副本。  
slice()能够基于当前数组中的一或多个项创建一个新数组。slice()方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下, slice() 方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数,该方法返回起始和结束位置之间的项——但不包括结束位置的项。**注意, slice() 方法不会影响原始数组**  
```javascript  
'use strict';

var colors = ["red", "green", "blue", "yellow", "purple"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);
alert(colors2); //green,blue,yellow,purple
alert(colors3); //green,blue,yellow
```  
如果slice()方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置。例如，在一个包含5项的数组上调用slice(-2,-1)与调用slice(3,4)得到的结果相同。如果结束位置小于起始位置，则返回空数组。  
splice()方法，这个方法恐怕要算是最强大的数组方法了，它有很多种用法。splice()的主要用途是向数组的中部插入项，但使用这种方法的方式则有如下3种。  
- 删除：可以删除任意数量的项，只需指定 2 个参数：要删除的第一项的位置和要删除的项数。例如，splice(0,2)会删除数组中的前两项。  
- 插入：可以向指定位置插入任意数量的项，只需提供3个参数：起始位置、0（要删除的项数）和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，splice(2,0,”red”,”green”)会从当前数组的位置2开始插入字符串”red”和”green”。  
- 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，splice (2,1,”red”,”green”)会删除当前数组位置 2 的项，然后再从位置 2 开始插入字符串”red”和”green”。  
splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项（如果没有删除任何项，则返回一个空数组）。   
**8. 位置方法**  
ECMAScript 5为数组实例添加了两个位置方法：indexOf()和lastIndexOf()。这两个方法都接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中，indexOf()方法从数组的开头（位置0）开始向后查找，lastIndexOf()方法则从数组的末尾开始向前查找。  
这两个方法都返回要查找的项在数组中的位置，或者在没找到的情况下返回-1。在比较第一个参数与数组中的每一项时，会使用全等操作符；也就是说，要求查找的项必须严格相等（就像使用===一样）。  
**9. 迭代方法**  
ECMAScript 5为数组定义了5个迭代方法。每个方法都接收两个参数：要在每一项上运行的函数和（可选的）运行该函数的作用域对象——影响 this 的值。传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。根据使用的方法不同，这个函数执行后的返回值可能会也可能不会影响方法的返回值。以下是这5个迭代方法的作用。  
- every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回 true。  
- filter()：对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。  
- forEach()：对数组中的每一项运行给定函数。这个方法没有返回值。  
- map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。  
- some()：对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true。  
```javascript  
'use strict';

var num = [1, 2, 3, 4, 5, 4, 3, 2, 1];

var er = num.every(function(item, index, array){
    return (item > 2);
});
alert(er); //false

var sr = num.some(function(item, index, array){
    return (item > 2);
});
alert(sr); //true
```   
**10. 缩小方法(归并)**  
ECMAScript 5还新增了两个归并数组的方法：reduce()和 reduceRight()。这两个方法都会迭代数组的所有项，然后构建一个最终返回的值。其中，reduce()方法从数组的第一项开始，逐个遍历到最后。而reduceRight()则从数组的最后一项开始，向前遍历到第一项。  
这两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为归并基础的初始值。传给 reduce()和 reduceRight()的函数接收 4个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项。  
```javascript  
'use strict';

var values = [1, 2, 3, 4, 5];
var sum = values.reduce(function(prev, cur, index, array){
    return prev + cur;
});
//第一次执行回调函数, prev 是 1, cur 是 2。第二次, prev 是 3(1 加 2 的结果), cur 是 3(数组的第三项)。这个过程会持续到把数组中的每一项都访问一遍,最后返回结果。
```  

### 5.3 Date类型  
ECMAScript 中的 Date 类型是在早期 Java 中的 java.util.Date 类基础上构建的。在调用Date 构造函数而不传递参数的情况下，新创建的对象自动获得当前日期和时间。如果想根据特定的日期和时间创建日期对象,必须传入表示该日期的毫秒数(即从 UTC 时间 1970 年 1 月 1 日午夜起至该日期止经过的毫秒数)。为了简化这一计算过程,ECMAScript 提供了两个方法: Date.parse()和 Date.UTC() 。  
Date.parse()方法接收一个表示日期的字符串参数，然后尝试根据这个字符串返回相应日期的毫秒数。ECMA-262没有定义Date.parse()应该支持哪种日期格式，因此这个方法的行为因实现而异，而且通常是因地区而异。如果传入Date.parse()方法的字符串不能表示日期，那么它会返回NaN。实际上，如果直接将表示日期的字符串传递给Date构造函数，也会在后台调用Date.parse()。  
Date.UTC()方法同样也返回表示日期的毫秒数，但它与 Date.parse()在构建值时使用不同的信息。Date.UTC()的参数分别是年份、基于 0的月份（一月是 0，二月是 1，以此类推）、月中的哪一天（1到 31）、小时数（0到 23）、分钟、秒以及毫秒数。在这些参数中，只有前两个参数（年和月）是必需的。如果没有提供月中的天数，则假设天数为 1；如果省略其他参数，则统统假设为 0。Date 构造函数会模仿 Date.parse()和 Date.UTC()，但模仿后者有一点明显不同：日期和时间都基于本地时区而非GMT来创建。不过，Date 构造函数接收的参数仍然与Date.UTC()相同。  
```javascript  
'use strict';

var y1 = new Date(Date.UTC(2000, 0));
var y2 = new Date(2000, 0);
var y3 = new Date(Date.parse("May 25, 2004"));
var y4 = new Date("May 25, 2004");
```  
ECMAScript 5添加了Data.now()方法，返回表示调用这个方法时的日期和时间的毫秒数。这个方法简化了使用Data对象分析代码的工作。使用+操作符把Data对象转换成数值，也可以达到同样的目的。  
```javascript  
'use strict';

//取得开始时间 和Date.now()等效
var start = +new Date();
```  
**1. 继承的方法**  
Date类型的toLocaleString()方法会按照与浏览器设置的地区相适应的格式返回日期和时间。这大致意味着时间格式中会包含AM或PM，但不会包含时区信息（当然，具体的格式会因浏览器而异）。而toString()方法则通常返回带有时区信息的日期和时间，其中时间一般以军用时间（即小时的范围是0到23）表示。至于Date类型的valueOf()方法，则根本不返回字符串，而是返回日期的毫秒表示。  
**2. 日期格式化方法**  
Date类型还有一些专门用于将日期格式化为字符串的方法，这些方法如下。  
- toDateString()——以特定于实现的格式显示星期几、月、日和年；  
- toTimeString()——以特定于实现的格式显示时、分、秒和时区；  
- toLocaleDateString()——以特定于地区的格式显示星期几、月、日和年；  
- toLocaleTimeString()——以特定于实现的格式显示时、分、秒；  
- toUTCString()——以特定于实现的格式完整的UTC日期。  
与toLocaleString()和toString()方法一样，以上这些字符串格式方法的输出也是因浏览器而异的，因此没有哪一个方法能够用来在用户界面中显示一致的日期信息。除了前面介绍的方法之外,还有一个名叫 toGMTString() 的方法,这是一个与toUTCString() 等价的方法,其存在目的在于确保向后兼容。不过,ECMAScript 推荐现在编写的代码一律使用 toUTCString() 方法。  

### 5.4 RegEXP类型  
使用下面类似 Perl 的语法,就可以创建一个正则表达式。  
```javascript  
'use strict';

var expression = / pattern / flags;
```  
其中的模式(pattern)部分可以是任何简单或复杂的正则表达式,可以包含字符类、限定符、分组、向前查找以及反向引用。每个正则表达式都可带有一或多个标志(flags),用以标明正则表达式的行为。正则表达式的匹配模式支持下列 3 个标志。  
- g :表示全局(global)模式,即模式将被应用于所有字符串,而非在发现第一个匹配项时立即停止;  
- i :表示不区分大小写(case-insensitive)模式,即在确定匹配项时忽略模式与字符串的大小写;  
- m :表示多行(multiline)模式,即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项。  
ES6中新增两个标志  
- u :表示“Unicode模式”，用来正确处理大于\uFFFF的Unicode字符。也就是说，会正确处理四个字节的UTF-16编码。  
- y :表示“粘连”（sticky）模式。y修饰符的作用与g修饰符类似，也是全局匹配，后一次匹配都从上一次匹配成功的下一个位置开始。不同之处在于，g修饰符只要剩余位置中存在匹配就可，而y修饰符确保匹配必须从剩余的第一个位置开始，这也就是“粘连”的涵义。  
```javascript  
'use strict';

var s = "𠮷";
//如果不添加u修饰符，正则表达式就会认为字符串为两个字符，从而匹配失败。
/^.$/.test(s) // false
/^.$/u.test(s) // true
//如果不加u修饰符，正则表达式无法识别\u{61}这种表示法，只会认为这匹配61个连续的u。
/\u{61}/u.test('a') // true
/\u{20BB7}/u.test('𠮷') // true
//使用u修饰符后，所有量词都会正确识别大于码点大于0xFFFF的Unicode字符。
/𠮷{2}/.test('𠮷𠮷') // false
/𠮷{2}/u.test('𠮷𠮷') // true
//u修饰符也影响到预定义模式，能否正确识别码点大于0xFFFF的Unicode字符。\S是预定义模式
/^\S$/.test('𠮷') // false
/^\S$/u.test('𠮷') // true
//有些Unicode字符的编码不同，但是字型很相近，比如，\u004B与\u212A都是大写的K。
/[a-z]/i.test('\u212A') // false
/[a-z]/iu.test('\u212A') // true
```  
y修饰符号隐含了头部匹配的标志ˆ。y修饰符的设计本意，就是让头部匹配的标志ˆ在全局匹配中都有效。  
```javascript  
'use strict';

var s = "aaa_aa_a";
var r1 = /a+/g;
var r2 = /a+/y;
r1.exec(s) // ["aaa"]
r2.exec(s) // ["aaa"]
r1.exec(s) // ["aa"]
//和g一样都是从_aa_a开始，但是y标志要求必须以a开头，所以返回null
r2.exec(s) // null
```  
如果同时使用g修饰符和y修饰符，则y修饰符覆盖g修饰符。模式中使用的所有元字符都必须转义。正则表达式中的元字符包括：**( [ { \ ^ $ | ) ? * + .]}**  
```javascript  
'use strict';

// 匹配所有以"at"结尾的 3 个字符的组合,不区分大小写
var pattern3 = /.at/gi;
// 匹配所有".at",不区分大小写
var pattern4 = /\.at/gi;
```  
这些例子都是以字面量形式来定义的正则表达式。另一种创建正则表达式的方式是使用RegExp 构造函数,它接收两个参数:一个是要匹配的字符串模式,另一个是可选的标志字符串。可以使用字面量定义的任何表达式,都可以使用构造函数来定义,如下面的例子所示。  
```javascript  
'use strict';

// 匹配第一个"bat"或"cat",不区分大小写
var pattern1 = /[bc]at/i;
// 与 pattern1 相同,只不过是使用构造函数创建的
var pattern2 = new RegExp("[bc]at", "i");
```  
**1. RegExp 实例属性**  
RegExp的每个实例都具有下列属性，通过这些属性可以取得有关模式的各种信息。  
- global：布尔值，表示是否设置了g标志。  
- ignoreCase：布尔值，表示是否设置了i标志。  
- lastIndex：整数，表示开始搜索下一个匹配项的字符位置，从0算起。  
- multiline：布尔值，表示是否设置了m标志。  
- source：正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回。  
ES6新增属性  
- sticky:布尔值，表示是否设置了y标志。  
- flags: 字符串，表示正则表达式的标志。  
**2. RegExp 实例方法**  
RegExp对象的主要方法是exec()，该方法是专门为捕获组而设计的。exec()接受一个参数，即要应用模式的字符串，然后返回包含第一个匹配项信息的数组；或者在没有匹配项的情况下返回null。返回的数组虽然是Array 的实例，但包含两个额外的属性：index 和input。其中，index 表示匹配项在字符串中的位置，而 input 表示应用正则表达式的字符串。在数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串（如果模式中没有捕获组，则该数组只包含一项）。  
```javascript  
'use strict';

var text = "mom and dad and baby";
var pattern = /mom( and dad( and baby)?)?/gi;
var matches = pattern.exec(text);
alert(matches.index); // 0
alert(matches.input); // "mom and dad and baby"
alert(matches[0]); // "mom and dad and baby"
alert(matches[1]); // " and dad and baby"
alert(matches[2]); // " and baby"
```  
对于 exec()方法而言，即使在模式中设置了全局标志（g），它每次也只会返回一个匹配项。在不设置全局标志的情况下，在同一个字符串上多次调用 exec()将始终返回第一个匹配项的信息。而在设置全局标志的情况下，每次调用exec()则都会在字符串中继续查找新匹配项。  

### 5.5 Function类型  
说起来 ECMAScript中什么最有意思，我想那莫过于函数了——而有意思的根源，则在于函数实际上是对象。每个函数都是Function类型的实例，而且都与其他引用类型一样具有属性和方法。**由于函数是对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。**