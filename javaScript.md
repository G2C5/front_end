#  语法和变量

------

## javascript的引入

async、defer（**延迟加载**） 

async：等html全部解析完、js加载完（单线），才执行js，谁先加载完就执行那个js
defer：等html全部解析完同时js加载完（多线），顺序执行js

```html
<html>
<head>
    <title>Document</title>
    <script>
        // 内嵌式
        // alert("月挣零");
    </script>
    //外部
    <script defer src="my.js"></script>
</head>
<body>
    //行内式 on开头
    <input type="text" value="真不错" onclick="alert('月挣零')" >
</body>
</html>er
```

------

## 	变量声明

```javascript
// 1. 不声明 不赋值
console.log(age); //报错
// 2. 只声明，不赋值
var age;
console.log(age);// underfined
// 3. 不声明 只赋值
name='yue';// 不报错 不推荐
// 4.
var num = 1,c,b;// var num = 1; 
console.log(num);// 1
console.log(c);// undefined
console.log(b);// undefined
// 5.
var a = b = c = 9; // var a = 9; b = 9; c = 9;
```

## 输入和输出

```javascript
//prompt(info) 用户输入框
prompt('输入您的序列')

//alert(msg) 弹出警示框 输出展示给用户
alert('计算的结果是')
        
//console(msg) 控制台输出 程序员测试
console.log('程序员看')
```



------

## 	运算符和表达式

------

### 什么是表达式

1.由值、变量、运算符和函数调用组成的任何有效组合。
2.表达式最终都有一个返回值。
3.表达式的值可以是任何数据类型，包括数字、字符串、布尔值、对象、数组和函数。

#### 综合表达式

------

### 运算符

#### 		1.算术运算符

​			用于执行基本数学运算，包括加法（+）、减法（-）、乘法（*）、除法（/）、求余（%）和指数运算（**）。

------

#### 		2.比较运算符

​			用于比较两个值，返回布尔值true或false。比较运算符包括等于（==）、不等于（!=）、全等于（===）、不全等于（!==）、大于（>）、小于（<）、大于等于（>=）和小于等于（<=）。

```JavaScript
//  浮点数精度问题
// 浮点数值的最高精度是 17 位小数  
// 不能直接拿来比较
console.log(0.1 + 0.2);// 0.30000000000000004
console.log(0.07 + 0.03);// 0.1
console.log(0.07 + 0.1);// 0.17
console.log(0.07 * 0.03);// 0.0021000000000000003

// 三目运算
var ifnum = (5 > 6 ? 4 : 2);
console.log(ifnum);// 2

```

------

#### 		3.逻辑运算符

​			用于执行逻辑操作，返回布尔值true或false。逻辑运算符包括逻辑非（!）、逻辑与（&&）和逻辑或（||）。

##### 				短路运算

​			逻辑与（&&）：只有当所有操作数都为 true 时，逻辑与运算符才会返回 true。当有一个操作数为 false 时，短路就会发生，后面的表达式不会再被执行。

```javascript
console.log(123 && 456); // 456
console.log(0 && 123); // 0
console.log(123 && false && 789);// false
```

​			逻辑或（||）：只要有一个操作数为 true，逻辑或运算符就会返回 true。当所有操作数都为 false 时，短路就会发生，后面的表达式不会再被执行。

```javascript
// 逻辑非
console.log(123 || 789); // 123
console.log(0 || ''); // “
console.log(0 || 456); // 456
console.log('' || 456); // kong
console.log(null || undefined || 456); // 456
console.log(0 || false || 789 || 0); // 789
//短路操作
var num4 = 0;
console.log(123 || num4++);// 123
console.log(num4);// 0
```



------

#### 		4.位运算符

​			用于对数字的二进制形式进行操作，包括按位与（&）、按位或（|）、按位异或（^）、按位非（~）、左移（<<）和右移（>>）。

------

#### 		5.赋值运算符

用于给变量赋值，包括等于（=）、加等于（+=）、减等于（-=）、乘等于（*=）、除等于（/=）、求余等于（%=）和指数等于（**=）。

```javascript
 var num = 10;
//后置 后算自己
console.log(num++);// 10  即 （num = 10 ; num+1;）
console.log(num);// 11
//前置 先算自己
var num2 = 10;
console.log(++num2);//  11  即 （num = num + 1）
console.log(num2);// 11
var num3 = 10;
alert(1 + num3++);// 11
var num4 = 10;
alert(1 + ++num4);// 12
```

------

#### 		6.条件运算符

​			也称为三元运算符，用于根据条件返回不同的值。条件运算符的语法是：condition ? value1 : value2。

------

#### 		7.typeof运算符：

​			用于检查值的数据类型，返回一个字符串表示数据类型。例如，typeof 123返回"number"，typeof "hello"返回"string"。

------

#### 		8.delete运算符

​			用于删除对象的属性或数组中的元素。

------

#### 		9.instanceof运算符：

​			用于检查一个对象是否是某个类的实例。

------

#### 		10.in运算符

​			用于检查一个属性是否存在于对象中。

```javascript
		/**
		
		*/

console.log(6%10);// 6

        // 1. 浮点数精度问题
        // 浮点数值的最高精度是 17 位小数  
        // 不能直接拿来比较
        console.log(0.1 + 0.2);// 0.30000000000000004
        console.log(0.07 + 0.03);// 0.1
        console.log(0.07 + 0.1);// 0.17
        console.log(0.07 * 0.03);// 0.0021000000000000003


        // 2. 递增递减

        var num = 10;
        //后置 后算自己
        console.log(num++);// 10   num = 10 ; num+1;
        console.log(num);// 11
        //前置 先算自己
        var num2 = 10;
        console.log(++num2);//  11  // num = num + 1
        console.log(num2);// 11

        var num3=10;
         
         alert(1 + num3++);// 11
         alert(1 + ++num3);// 13

         // 3.比较运算符
        if(num == num2){
            console.log('比较运算结构为'+ (num == num2))
            //比较运算结构为true
        }
        if(num === num2){
            console.log('全等比较运算结构为' 
                + typeof(num === num2));
                //全等比较运算结构为boolean
        }
        // 三目运算
        var ifnum = ( 5 > 6 ? 4 : 2);
        console.log(ifnum);// 2

        // 4. 逻辑中断
        // 逻辑与
        console.log(123 && 456); // 456
        console.log(0 && 123); // 0
        console.log(123 && false && 789);// false
        // 逻辑非
        console.log( 123  || 789); // 123
        console.log( 0 || '' ); // 456

        console.log( 0 || 456 ); // 456
        console.log( '' || 456 ); // kong
        console.log( null || undefined || 456 ); // 456

        console.log( 0 || false || 789 || 0 ); // 789

        //短路操作
        var num4 = 0;
        console.log(123 || num4++);// 123
        console.log(num4);// 0
```

### 运算符优先级

## 	作用域

------

### 作用域链

指变量和函数在代码中被访问的顺序，当被访问时，JavaScript引擎会优先在被访问的变量或函数的作用域中查找，如果没有找到，则会向父级作用域查找，直到找到该变量或函数为止。

```javascript
// 内部函数访问外部函数的变量，采取的是链式查找的方法来决定取值，这种结构我们称之为链
var num = 10;
function fu(){
    var num = 20;
    function fun(){
        console.log(num); // 20 就近原则
    }
}
```

------

### 作用域

es6之前没有块级作用域，但有函数作用域。

```javascript
// 1. js作用域（es6）前： 全局作用域  局部作用域
// 2. es6 增加  块级作用域
// 3. 块级作用域 {} if{} for{}

// java中 
// if(xx){
//     int num = 10;
// }
// 外部调用不了 num

// js可以
if(true){
	var num = 10;
}
console.log(num);

```

------

### 全局作用域和局部作用域(es6前)

```javascript
// 1. javaScript作用域  减少变量命名冲突
// 2. js作用域（es6）前： 全局作用域  局部作用域

//  全局作用域：整个script标签中，单独的js文件
var num = 10;
var num = 30;// 全局变量，浏览器关闭才销毁，占用内存资源

//  局部作用域（函数作用域）：函数内有效
function fn() {
    // 局部变量，程序执行完就销毁，节约内存资源
    var num = 20; 
    num = 40; // 全局变量 不推荐
    console.log(num);
}
fn(num);// 40
console.log(num);// 30
```



------

## 预解析

1. 变量声明提升：在代码执行前，JavaScript 引擎会将所有变量声明提升到当前作用域的顶部。这意味着，无论变量声明出现在什么位置，都可以在其作用域内进行访问。
2. 函数声明提升：在代码执行前，JavaScript 引擎会将所有函数声明提44升到当前作用域的顶部。这意味着，无论函数声明出现在什么位置，都可以在其作用域内进行访问。
3. 处理函数表达式：在 J4avaScript 中，函数表达式是一种定义函数的方式。在预解析阶段，JavaScript 引擎会处理函数表达式，并将其赋值给变量。这意味着，变量可以作为函数使用。
4. 处理 this 和 arguments：在 JavaScript 中，函数内部的 this 和 arguments 对象是在函数被调用时才会被创建。但是，在预解析阶段，JavaScript 引擎会创建一个预先定义的 this 和 arguments 对象，以便在函数被调用时使用。

js代码运行 **预解析** 和 **代码执行** 两步：

(1). 预解析js引擎会把js里的 var 和 function 提升到当前作用域最前面。
(2). 代码执行 从上到下。



------

### 	变量提升

 将变量声明，提升到变量当前的作用域最前面，不提升赋值操作。

```javascript
// 1
console.log(num1); //error: num1 is not defined
```

```javascript
 // 2
 console.log(num2); // undefind
 var num2 = 1;
 //相当于
 // var num2;
 // console.log(num2);
 // nu
```

------

### 	函数提升

将函数声明，提升到函数当前的作用域最前面，不调用函数。

```javascript
fn(); // 正常访问

function fn(){
    console.log(3);
}
//相当于
// function fn(){
//     console.log(3);
// }
// fn();
```

```javascript
//fun(); // error:fun is not a function
var fun = function(){
    console.log(4);
}
// 相当于
// var fun;
// fun();
// fun = function(){
//     console.log(4);
// }
```

```javascript
f1();
console.log(c); 
console.log(b); 
console.log(a); 
function f1() {
    var a = b = c = 9; // var a = 9; b = 9; c = 9;
    console.log(a);
    console.log(b);
    console.log(c);
}
//相当于

// var b, c;
// function f1() {
//     var a;
//     a = b = c = 9;
//     console.log(a); // 9
//     console.log(b); // 9
//     console.log(c); // 9
// }
// f1();
// console.log(c); // 9
// console.log(b); // 9
// console.log(a); // 报错
```

## 流程控制

### 	顺序结构

### 	分支结构

------

### 	循环结构

```javascript
// for循环是最常用的循环结构，用于重复执行一段代码，通常用于遍历数组或执行已知次数的循环
for (let i = 0; i < 5; i++) {
  console.log(i);
}
// while 循环在给定条件为真的情况下重复执行代码块，条件在循环开始前进行检查
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}
// do...while 循环类似于 while 循环，但它首先执行一次循环体，然后在检查条件是否为真
let i = 0;
do {
  console.log(i);
  i++;
} while (i < 5);
// for...in 循环用于遍历对象的可枚举属性，通常用于迭代对象属性
const person = {
  name: 'Alice',
  age: 30,
  gender: 'female'
};
for (const key in person) {
  console.log(`${key}: ${person[key]}`);
}
// for...of 循环用于遍历可迭代对象（如数组、字符串、Map、Set 等），通常用于遍历数组元素或字符串字符
const fruits = ['apple', 'banana', 'cherry'];
for (const fruit of fruits) {
  console.log(fruit);
}
```



------

## 遍历&迭代

#### forEach

1.  没有返回值；

2.  无法中断执行；

3.  可以使用`return`跳过当前循环；

4.  跳过数组的空位，但不会跳过`null`和`undefined`；

5.  item：item是基础数据类型，那么并不会改变数组里面的值，如果是引用类型，那么item和数组里面的值是指向同一内存地址，则都会被改变

6.  手写

    ```javascript
    Array.prototype.new_forEach = function(callback) { // 可以看出时数组原型方法
        for (let i = 0; i < this.length; i++) { 
            callback(this[i], i, this) 
        }
    }
    ```

    `array.forEach(callback(currentValue, index, array), thisArg);`

    -   `callback` 是一个函数，用于对数组中的每个元素执行操作。
    -   `currentValue` 是当前正在处理的元素的值。
    -   `index` 是当前正在处理的元素的索引。
    -   `array` 是正在遍历的数组。
    -   `thisArg` 是可选的，用于设置 `callback` 函数内部的 `this` 值。

------

#### map

1.  具有返回值；
2.  无法中断执行，同forEach；
3.  可以使用`return`跳过当前循环，同forEach；
4.  跳过数组的空位，但不会跳过`null`和`undefined`，同forEach；
5.  改变数组情况，同forEach;
6.  手写map方法；

```javascript
var a = [1,2,3,4,5] 
var b = a.map((item) => { return item = item * 2 }) 
console.log(a) // [1,2,3,4,5] 
console.log(b) // [2,4,6,8,10]
// 手写
Array.prototype.new_map = function(callback) { // 可以看出时数组原型方法
    const res = [] 
    for (let i = 0; i < this.length; i++) { 
        res.push(callback(this[i], i, this)) 
    } 
    return res 
}
```

#### for

1.  for循环是个语句，forEach和map则是表达式；
2.  for循环可以使用`break`结束循环；
3.  for循环可以使用`continue`语句跳过当前循环；
4.  for循环不会跳过数组的空位，会默认空位为undefined；

# 两大数据类型

## 数据类型判断

| 优缺点 | typeof                                                     | instanceof                         | constructor                                 | Object.prototype.toString.call   |
| ------ | ---------------------------------------------------------- | ---------------------------------- | ------------------------------------------- | -------------------------------- |
| 优     | 使用简单                                                   | 能检测出引用类型数据               | 基本能检测所有的类型（除了null和undefined） | 检测出所有的类型                 |
| 缺     | 只能检测出除null外的基本数据类型和引用数据类型中的function | 不能检测出基本类型，且不能跨iframe | constructor易被修改，也不能跨iframe         | IE6下，undefined和null均为Object |

```js

[1, 2, 3] instanceof Array // true or false 
[1,2,3].constructor === Array
Object.prototype.toString.call([1, 2, 3]) // [object Array]
 
Array.prototype.isPrototypeOf([1, 2, 3]) // true or false
Array.isArray([1, 2, 3]) // true or false 通过ES6
[1, 2, 3].__proto__ === Array.prototype // 通过原型链去判断

//typeof 和 instanceof 的结合使用
var value = [];
console.log(typeof value === "object" && value instanceof Array); // true

// 自定义数据类型检测
function isEmail(value) {
  return typeof value === "string" && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
}
console.log(isEmail("example@email.com")); // true

```



## 	基本数据类型

### 	Number

```javascript
// 十进制
var num  = 10;
console.log(typeof num);// 数字型 number
var PI = 3.14;
console.log(typeof PI);// 数字型 number
// 八进制 零开头
var num1 = 010;  
console.log(num1);// 返回8
// 十六进制 0x开头 1~9 a~f
var num2 = 0xff; 
console.log(num2);//输出 255 这里是默认调用了valueOf()
// 特殊的数字型
console.log(typeof NaN); // 不是一个数字，但是是number类型
// 数字型最值
console.log(Number.MAX_VALUE); // 1.7976931348623157e
console.log(Number.MIN_VALUE);// 5e-324
// 无穷大和无穷小
console.log(Number.MAX_VALUE*2);// 无穷大 Infinity
console.log(-Number.MIN_VALUE*2);// 无穷小 -Infinity -1e-323
// 判断是否非数值型
console.log(isNaN('hh')); // ture
console.log(isNaN(412)); // false
```

### 	String

```javascript
//1.字符串转义符 反斜杠开头 写在引号里面
        /* 
        \n  换行
        \\ 单斜杠
        \b 空格
        \t tab缩进
*/
var str1= 'abcdefg';
// 字符串的拼接
var str2 = '我是字符串' +str1;
// 字符串length属性
console.log(str1.length); // 7
```

#### 不可变性

**字符串的所有操作方法都改变不了字符串**

```javascript
// 字符串的所有操作方法都改变不了字符串 
// 操作完返回一个新字符串
var str = '123';
str = '456';
        
// 1. str 原来指向 '123' 的地址 
// 2. str 变成指向 '456' 的地址
// 3. 值 '123' 不可变 不被修改为 '456'
// 4. 只是重新在内存里开辟空间 
// 5. 所以不建议大量拼接字符串

for(var i = 1 ; i<10000000;i++){
    str += i;
}
console.log(str);
```



#### 字符·位置

| 序号 | String内置对象的方法                   | 效果                                        |
| ---- | -------------------------------------- | ------------------------------------------- |
| 1    | charAt(index)                          | 返回对应索引位置的字符                      |
| 2    | charCodeAt(index)                      | 返回对应索引位置的字符的ASCII值             |
| 3    | indexOf('查找的字符',[查询起始索引值]) | 前往后找，返回第一个匹配元素索引，无返回 -1 |
| 4    | lastIndexOf(‘value’)                   | 后往前找，返回第一个匹配元素索引，无返回-1  |
| 5.   | indexOf应用                            | 查找字符串 o 出现次数和位置                 |

```javascript
 var str = 'likeyzl';
 // 1. charAt(index) 字符串 索引返回 字符
 for (var i = 0; i < str.length; i++) {
     console.log(str.charAt(i));
 }
 var nn = false

 // 2. charCodeAt(index) 字符串 索引号 返回 字符的ASCII值
 // 判断用户按下键盘大小写
 console.log(str.charCodeAt(0)); // 108
 console.log(str.charCodeAt(1)); // 105

 // 2.1 str[index] 获取指定位置的字符
 console.log(str[0]); // l
 console.log(str[1]); // i
 console.log(str[2]); // k

var str = 'abbcdeffg';
// 3. indexOf('查找的字符',[查询起始索引值]) 从前往后找 只返回第一个匹配的元素索引 无返回 -1
console.log(str.indexOf('b',0));  // 1
console.log(str.indexOf('b'));  // 1
console.log(str.indexOf('b',3));  // -1

// 4. lastIndexOf() 从后往前找 返回第一个匹配元素索引 无返回-1
console.log(str.lastIndexOf('b')); // 2

// 5. 查找字符串 o 出现次数和位置
var str = 'xxyyoooblacFridaykvivo50';
var num = 0;
var index = str.indexOf('o'); // 返回第一次找到位置
while(index != -1){
    console.log(index);
    num++;
    index = str.indexOf('o',index+1); // 返回第一次找到位置 然后加一往前继续找到'o'的位置
}
console.log('字符o出现次数：'+num);
```

#### 拼接·截取

| 序号 | String内置对象的方法             | 效果              | 注意（都不改变原字符串）   |
| ---- | -------------------------------- | ----------------- | -------------------------- |
| 1    | concat('str1','str2',...)        | 拼接多个字符串    | 返回拼接字符串，等效 +     |
| 2    | substr ( 起始位置，截取字符数 )  | 截取多个字符[x,y) | 返回截取字符串，接受负值   |
| 3    | substring( 起始位置 ，结束位置 ) | 左闭区间截取[x,y) | 返回截取字符串，不接受负值 |
| 4    | slice( 起始位置 ， 结束位置 )    | 左闭区间截取[x,y) | 返回截取字符串，接受负值   |

```javascript
 var str = 'yue';
// 1. 拼接 concat('str1','str2',...); 返回拼接字符串 等效 + 
str = str.concat('str1','str2');
console.log(str); // yuestr1str2

// 2. 个数截取 substr ( 起始位置，截取字符数 )； 返回截取字符串
// str = 'yuestr1str2';
//  y   u   e   s ...  2
//  0   1   2   3 ... 10
// -11 -10 -9  -8 ... -1
 str = str.substr(-11,3); // yue 接受负值
console.log(str); // yue

// 3. 左闭区间截取 substring( 起始位置 ，结束位置 )； 返回截取字符串
// str = 'yue'
//  y   u  e 
//  0   1  2  3
// -3  -2 -1  
str = str.substring(0,3); // 不接受负值
console.log(str); // yue 
        
// 4. 左闭区间截取 slice( 起始位置 ， 结束位置 )； 返回截取字符串
str = str.slice(-3,3); // 接受负值
console.log(str); // yue 
```

#### 替换·数组

| 序号 | String内置对象的方法           | 效果         | 注意                                       |
| ---- | ------------------------------ | ------------ | ------------------------------------------ |
| 1    | str.replace('oldVal','newVal') | 替换字符     | 不改变原来字符串，一次只替换第一个匹配字符 |
| 2    | str.split('分割符')            | 字符串转数组 | 返回值是一个数组                           |

```javascript
// 1. 替换字符 replace('旧字符','新字符')；只替换第一个匹配的字符
var str = 'yuey';
console.log(str.replace('y', 'Y')); // Yuey
console.log(str)
// indexOf实现替换多次匹配的字符
var str1 = 'yzlllyyy';
while (str1.indexOf('y') !== -1) { // 不为-1 则能找到 就替换
    str1 = str1.replace('y', 'Y');
}

// 2. 字符串转数组 split('分割符');  join()是数组转字符串
var str2 = 'red,blue,orange'
var arr = str2.split(',');
console.log(arr);  
```

#### 字符·统计

```javascript
// 1. 查找字符串 o 出现次数和位置
var str = 'xxyyoooblacFridaykvivo50';
var num = 0;
var index = str.indexOf('o'); // 返回第一次找到位置
while(index != -1){
    console.log(index);
    num++;
    index = str.indexOf('o',index+1); // 返回第一次找到位置 然后加一往前继续找到'o'的位置
}
console.log('字符o出现次数：'+num);


// 2. 统计字符串所有 字符出现次数
var str = 'abbcccyyyyzzzzllll';
// var str = 'abbcccyyyyzzzzllllssssssssssss';
var strs = {}; // 空对象
for(var i = 0 ;i<str.length;i++){ // 顺序遍历字符串str
    var chars = str.charAt(i); // 顺序获取字符串str 相应索引位置字符存入 chars
    if(strs[chars]){ // 判读属性值 strs[chars] 
        strs[chars]++; // 若有这个属性值 则赋值+1
    }else{ 
        strs[chars] = 1; // 若没有这个属性值 则赋值为1
    }
}
console.log(strs);
var max = 0; // 存放出现最大次数
var maxs = []; // 存放出现最大次数 的 字符名
// var ch = ''; // 存放出现最大次数 
// 得到最大值
for(k in strs){ 
    if( strs[k] >= max ){
        max = strs[k]; // 得到 出现最大次数  属性值
        // ch = k;  // 存放出现最大次数 的字符  属性名
    }
}
// console.log('最多次出现字符：'+ch); // 

// 将多个最大值属性名存入 
for(k in strs){
    if( strs[k] == max ){
        maxs.push(k); // 得到 最大值属性名
    }
}
// 输出结果
for(k in maxs){
    console.log('出现最多次数的字母是：'+ maxs[k] + ',共'+max+'次');
}
```



### 	Boolean

### 	undefined&null

1.undefined表示一个变量被声明了但没有被初始化。如果一个变量没有被赋值，那么它的值就是undefined。

2.null表示一个变量被明确地赋值为null值，它表示一个空值或者一个不存在的对象。

3.null会被隐式转换为0。undefined表示无，NaN。

```javascript
// undefined
var str;
console.log(str);// undefined
console.log(str + 'hhhh');// undefinedhhhhh
console.log(412 + str);// NaN
console.log(true + str); // NaN
// null
var variable = null;
console.log(typeof variable); // object
console.log('Hello' + variable);// Hellonull
console.log(12 + variable);// 12 number
console.log(true + variable);// 1 number
// 两者关系
console.log(str == variable); // true
console.log(str != variable); // false
console.log(str === variable); //false
console.log(str !== variable); // ture
```

## 基本数据类型转换

### 	显示类型转换

#### 转为number

```javascript
var str = '412';
console.log(parseInt(str));// 412 转换为整型
console.log(parseFloat('4.12'))// 412 转换为浮点数
console.log(Number('15'));// 15 转换为数值型
```

#### 转为string

```javascript
var num = 412;
// console.log(toString(num)); // [object Undefined]
console.log(num.toString()); // 412 string
console.log(String(num)); // 412 string
```

### 	隐式类型转换

#### 转为number

```javascript
/*
 * 隐式转换 - * / 
 */
var str = '412';
console.log(str - 0);// 412  false
console.log(str - false);// 412
console.log(typeof(str - true));// 411 number
console.log(typeof (str - 0)); // number
console.log(typeof (str * 1)); // 412 number
console.log(typeof (str / 1));  // 412 number
// 特别
console.log(Math.abs('-2')); // 2 绝对值隐式转换
```

#### 转为string

```javascript
 //任何数据类型和字符串 相加 都是字符串
 alert(num + '字符串');// 隐式转换
 console.log('' + null);// null
 console.log('' + undefined);// undefined

```



## 引用数据类型

又称对象类型，对象类型都经过new出来，所以不相等，浅拷贝时例外（拷贝的地址值时）。

### 	Arrary

#### 创建·获取

```javascript
// 1.new创建数组
var arr = new Array(); //创建了空数组
var arr = new Array(2); //创建了长度为2的空数组
var arr = new Array(2,3); // 包含元素 2 和 3
var arr = new Array(2,); // 包含元素2

// 2.利用数组字面量创建数组 []
var arr = [2];// 创建了长度为2的空数组
var arr1 = [1,2, 'red',true];

// 3.获取数组元素
console.log(arr1[0]);
```

#### 遍历

```Java
var arr = ['red','green','blue'];

// for循环遍历数组
for(var i = 0; i < arr.length ; i++){
	console.log(arr[i])
}

// 键key
for(k in arr){
	console.log(k); //数组下标 key
}

//对象object
for(k in arr){
	console.log(arr[k]); //数组元素
}
```

#### 查找·索引

| 序号 | 检测方法      | 描述                                       |
| ---- | ------------- | ------------------------------------------ |
| 1    | indexOf()     | 返回第一个满足条件元素的索引值，无返回-1   |
| 2    | lastIndexOf() | 返回最后一个满足条件元素的索引值，无返回-1 |
| 3    | includes()    | 用于检查数组中是否包含特定元素，返回布尔值 |

```javascript
var colorArr = ['red', 'blue', 'yellow', 'purple', 'green', 'black', 'green','green'];
// 1. indexOf() 返回第一个满足条件的元素的索引值 无返回-1
colorArr.indexOf('green'); // 4

// 2. lastIndexOf() 返回最后一个满足条件的元素的索引值 无返回-1
colorArr.lastIndexOf('green'); // 7

//3. 返回一个布尔值，用于检查数组中是否包含特定元素elementToFind：要查找的元素。fromIndex（可选）：搜索的起始索引位置，默认为0
array.includes(elementToFind, fromIndex)
```

#### 类型·检测

| 序号 | 检测方法            | 说明                                           |
| ---- | ------------------- | ---------------------------------------------- |
| 1    | instanceof          | 运算符(`arr instanceof Object返回也是true`)    |
| 2    | Array.isArray(参数) | Array内置对象的方法，H5新增，ie9及以上版本支持 |

```javascript
// 检测是否为数组
// 1. instanceof 运算符 返回值为boolean值
var colorArr = ['red', 'blue', 'yellow', 'purple', 'green', 'black', 'green'];
console.log(colorArr instanceof Array); // true

// 2. Array.isArray(参数) 返回值为boolean值 H5新增 ie9及以上版本支持
console.log(Array.isArray(colorArr));
```

#### 增加·删除

| 序号 | Array内置对象的方法   | 用途             | 注意                                    |
| ---- | --------------------- | ---------------- | --------------------------------------- |
| 1    | array.push(arrEle)    | 末尾追加元素     | 改变原数组，返回值是数字型的数组长度    |
| 2    | array.unshift(arrEle) | 开头追加元素     | 改变原数组，返回值是数字型的数组长度    |
| 3    | array.pop()           | 删除最后一个元素 | 一次删除一个，返回删除元素，改变原数组  |
| 4    | array.shift()         | 删除第一个元素   | 一次删除一个， 返回删除元素，改变原数组 |

#### 连接·截取·删除·插入

| 序号 | Array内置对象的方法                       | 用途               | 说明                                   |
| ---- | ----------------------------------------- | ------------------ | -------------------------------------- |
| 1    | concat(数组1,数组2,...)                   | 连接多个数组       | 返回一个新数组，不影响原数组           |
| 2    | slice(begin,end)                          | 截取元素[ x , y )  | 返回截取元素组成的新数组，不影响原数组 |
| 3    | splice( 起始下标 , 删除个数[，插入元素] ) | 删除、插入数组元素 | 返回删除元素组成的新数组，影响原数组   |

```javascript
var colorArr = ['red', 'blue', 'yellow', 'purple', 'black', 'green'];
var rgbArr = ['#0000EE','#EE0000','#FFFF00']
console.log(colorArr);
// 1. concat(数组1,数组2,...) 连接多个数组 不影响原数组 返回一个新数组
console.log(colorArr.concat(colorArr,rgbArr));
// 2. slice(起始下标,结束下标) [ x , y ) 返回一个截取元素组成的新数组  不影响原数组
console.log(colorArr.slice()); // 返回一个新数组并且和原数组一样
console.log(colorArr.slice(0,2)); // ['red', 'blue'] 
// 3. splice( 起始下标 , 删除个数 ) 删除数组元素 返回删除元素组成的新数组 影响原数组
console.log(colorArr.splice(1,3)); // ['blue', 'yellow', 'purple']
console.log(colorArr); // ['red', 'black', 'green']
```

#### 排序·翻转

| 序号 | Array内置对象的方法&其它 | 用途                 | 说明                         |
| ---- | ------------------------ | -------------------- | ---------------------------- |
| 1    | array.sort()             | 数组排序（冒泡排序） | 只适用于 0~9 排序,改变原数组 |
| 2    | array.sort() 优化        | 数组排序             | 任何情况，改变原数组         |
| 3    | array.reverse()          | 翻转数组             | 改变原数组                   |
| 4    | 运算符检测实现           | 翻转数组             | instanceof检测数组           |

```javascript
// 1. 数组排序（冒泡排序）
var arr = [2, 4, 14, 1, 23, 45, 6];
arr.sort(); // 只适用于 0~9 排序
console.log(arr); // [1, 14, 2, 23, 4, 45, 6]

// 2. 优化
// 简易用法
arr.sort(function (a, b) {
    return a-b; // 升序排列
    // return b - a; // 降序排列
});
console.log(arr); // [1, 2, 4, 6, 14, 23, 45]
// 升级版
var vsinger =[
    { name: 'yzl', hot: 412 },
	{ name: 'lty', hot: 712 },
	{ name: 'yh', hot: 711 }
]
function compare(prop){
	return function(a,b){
        var val1= a[prop];
        var val2= b[prop];
        return val1 - val2;
	}
}
vsinger.sort(compare('hot'));
console.log(vsinger)


// 3. 翻转数组
var colorArr = ['red', 'blue', 'yellow', 'purple', 'green', 'black', 'green'];
colorArr.reverse();
console.log(colorArr);



// 4. 翻转数组
function reverse(arr) {
    if (arr instanceof Array) {
        for (var i = 0; i < Math.floor(arr.length / 2); i++) {
            [arr[i], arr[arr.length - 1 - i]] = [arr[arr.length - 1 - i], arr[i]];
            /*
            var item = arr[i];
            arr[i] = arr[colorArr.length-1-i];
            arr[colorArr.length-1-i] = item;
            */
        }
        return arr;
    } else {
        return '这不是数组';
    }
}
```

#### 分割·转换

| 序号 | Array内置对象的方法 | 效果                                 | 注意                       |
| ---- | ------------------- | ------------------------------------ | -------------------------- |
| 1    | array.toString()    | 将数组转为**逗号**隔开的字符串       | 不影响原数组               |
| 2    | array.join(分隔符)  | 将数组转为**自定义符号**隔开的字符串 | 不影响原数组，默认逗号隔开 |

```javascript
var colorArr = ['red', 'blue', 'yellow', 'purple', 'green', 'black'];
// 数组转字符串 
// 1. toString() 将数组转逗号隔开字符串 返回值为字符串
console.log(colorArr.toString()); //red,blue,yellow,purple,green,black

// 2. join(分隔符)
console.log(colorArr.join()); // red,blue,yellow,purple,green,black
console.log(colorArr.join('-')); // red-blue-yellow-purple-green-black
console.log(colorArr.join('&')); // red&blue&yellow&purple&green&black

```

#### 去重

```javascript
var colorArr = ['red', 'blue', 'yellow', 'purple', 'green', 'black', 'green'];
function unique(arr){
   var newArr = [];
   for(var i = 0 ;i<arr.length;i++){
       if(newArr.indexOf(arr[i])==-1){
           newArr.push(arr[i]);
       }
   }
   return newArr;
}
console.log(unique(colorArr));
```

#### 新增元素

```javascript
var arr = [];
var elm;
while(arr.length<4){
    elm= prompt('输入元素：');
    arr[arr.length] = elm;
}
            
for(var i = 0 ; i < arr.length ; i++){
    console.log(arr[i]);
}
console.log(arr);
```

#### 去除一个元素的所有重复项

```javascript
var arr = [2, 0, 45, 3, 0, 9, 4, 67, 0];
var newArr = [];
for (var i = 0; i < arr.length; i++) {
    if (arr[i] != 0) {
        newArr[newArr.length] = arr[i];
    }
}
for (key in newArr) {
    console.log(newArr[key]); //遍历新数组
}
```

#### 冒泡排序输出元素

```javascript
var arr = [1, 2, 3, 4, 6, 2, 9]; 
for (var i = 0; i < arr.length; i++) {     
    for (var j = 0; j < arr.length - 1 - i; j++) {
        if (arr[j] > arr[j + 1]) {
            var temp = arr[j];
            arr[j] = arr[j+1];
            arr[j+1] = temp;
        }
    }
console.log(arr[i]);
}
```



### 	Function

#### 声明与调用

##### 1.命名函数

  利用函数关键字自定义函数

```javascript
// 1.利用函数关键字 自定义 函数 （命名函数）
// function 函数名(形参1...){};
function sayHi() {
    console.log('hello world!');
}
function say(color,food) {
    return color + food;
}
// 调用函数
sayHi(); // hello world!
console.log(sayHi());// 没有return语句，则返回undefined
console.log(say());// NaN
console.log(say('red', '零食'));// red零食
console.log(say('red'));// redundefined
console.log(say('red', '零食', 'blue'));// red零食 
```

##### 2. 匿名函数

函数表达式声明函数

```javascript
// 2.函数表达式 声明函数 (匿名函数)
// var 变量名 = function(){};
var sing = function (song) {
    return 'sing ' + 'a ' + song;
}
// 调用函数
console.log(sing('song'));

```

##### 3. 区别

1. 匿名函数是一个没有名称的函数，它通常在使用时被定义，或者作为另一个函数的参数传递。它们可以使用函数表达式或箭头函数语法创建。

2. 命名函数是一个有名称的函数，它可以在任何地方定义，并且可以在代码中多次调用。

3. 命名函数会在函数的作用域中创建一个变量名，这可能会导致命名冲突和变量提升的问题。因此，在JavaScript中，使用匿名函数是一种更安全和可维护的方式。

#### arguments

```javascript
function fn(){
    // arguments 接收包所有的实参 返回一个伪数组
    for (var i=0 ; i < arguments.length; i++) {
        console.log(arguments[i]);
    } 
}
var arr = fn(1,2,3,'str',4.12);
// 伪数组：有length属性，索引方式存储，无数组方法pop()、push()等
console.log(Array.isArray(arr)); // false
```

#### 函数应用例子

##### 冒泡排序

```javascript
function arrMax(arr) {
    for (var i = 0; i < arr.length; i++) {
        for (var j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                var temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}
        
console.log(arrMax([1,3,4,2,7,8,0,9]));
```

闰年

```javascript
function isRunYear(year) {
    var flag = false;
    if (year % 400 == 0 || year % 4 == 0 && year % 100 != 0) {
        flag = true;
    }
    return flag;
}
console.log(isRunYear(2000));
```

##### 翻转数组

```javascript
function reverse(arr) {
    for (var i = 0; i < Math.floor(arr.length / 2); i++) {
        [arr[i], arr[arr.length - 1 - i]] = [arr[arr.length - 1 - i], arr[i]];
    }
    return arr;
}

function myReverse(arr) {
    for (var i = 0; i < arr.length - 1 - i; i++) {
        var temp = arr[i];
        arr[i] = arr[arr.length - 1 - i];
        arr[arr.length - 1 - i] = temp;
    }
    return arr;
}

function blackMReverse(arr) {
    var newArr = [];
    for (var i = arr.length - 1; i >= 0; i--) {
        newArr[newArr.length] = arr[i];
    }
    return newArr;
}
var arr = ['red', 'blue', 'yellow', 'purple', 'green', 'black', 'green'];

console.log(reverse(arr)); //调用一次已经改变元素顺序

//console.log(myReverse(arr));
//console.log(blackMReverse(arr));
```

##### 二月天数

```javascript
function backDay() {
    var year = prompt('输入年份:');
    if (isRunYear(year)) {
        alert('闰年2月有29天');
    } else {
        alert('平年2月有28天');
    }
}
```



### 	Object

#### 对象的创建与调用

##### 1. 利用对象字面量创建对象

```javascript
// 1. 利用对象字面量创建对象
var obj = {} // 创建了一个空对象

var FootObj = { // shou
    fname: '客家酿豆腐',
    fPrice: '20￥',
    finformation: function () {
        console.log(FootObj.fname + this.fPrice + '块钱一个');
    }
}
// 调用对象的属性&方法
console.log(FootObj.fname);
console.log(FootObj['fPrice']);
FootObj.finformation();

// 增加属性 和 属性值
FootObj.myfoot = 'addfoot';
```

##### 2. 利用 new Object 创建对象

```javascript
// 2. 利用 new Object 创建对象
var ColorObj = new Object(); // 空对象
ColorObj.rcolor = 'red'; // 属性
ColorObj.redrgb = '#EE0000';
ColorObj.bcolor = 'blue'
ColorObj.bluergb = '#00EEEE';
ColorObj.getRBG = function () {  // 方法
    console.log(ColorObj.rcolor + ' RGBis ' + ColorObj.redrgb);
}
// 调用对象的属性&方法
console.log(ColorObj.rcolor);
console.log(ColorObj['redrgb']);
ColorObj.getRBG();
// 增加属性 和 属性值
ColorObj.mycolor = 'myblue';
```

##### 3. 利用构造函数创建对象

```javascript
// 3. 利用构造函数创建对象
// 自带返回结果
// 构造函数
function Books(bTitle, bAuthor, bPrice) {
    this.title = bTitle;
    this.author = bAuthor;
    this.price = bPrice;
    this.score = function (score) {
        console.log('评分' + score + '分');
    }
    this.getbook = function () {
        console.log('《' + this.title + '》作者是：'
                    + this.author + ',价格' + this.price + '￥');
    }
    return {title= this.}
}
// 对象实例化
// 创建对象
var bookbj = new Books('诛仙', '萧鼎', '200'); // 调用函数返回是一个对象
console.log(typeof bookbj); // object
console.log(bookbj.author);
console.log(bookbj['price']);
bookbj.getbook();
bookbj.score(100);
```

##### 3.1 new关键字执行历程

1.new 构造函数在内存中创建了一个空的对象 
2.this 指向这个新对象
3.执行构造函数里的代码，给这个新对象添加属性和方法
4.返回这个新对象

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const person1 = new Person('Alice', 25);
console.log(person1); // Person { name: 'Alice', age: 25 }
```

1. 创建一个新对象：JavaScript 引擎会创建一个新的空对象，该对象继承自构造函数的 `prototype` 属性。
2. 设置对象的原型：新对象的 `__proto__` 属性会指向构造函数的 `prototype` 属性，这样就可以访问到构造函数的原型中定义的属性和方法。
3. 执行构造函数：将构造函数作为普通函数调用，同时将新对象作为其上下文对象（this）传递进去，构造函数会执行一些操作，例如初始化对象的属性和方法。
4. 返回新对象：如果构造函数没有显式返回一个对象，则 `new` 关键字会返回新创建的对象。如果构造函数返回一个对象，则 `new` 关键字会返回该对象。

#### 对象的遍历

```javascript
var obj = {
    name: '向明',
    age: 18,
    gender: '男',
    fn: function () { }
}
for (var k in obj) {
    console.log(k); // 属性名
    console.log(obj[k]); // 属性值
}
```

#### 对象的判断

instanceof运算符判断一个变量是否为特定的对象类型

```javascript
const date = new Date();
date instanceof Date; // true 只能判断对象类型
```

#### 对象应用例子

##### 对象封装

```javascript
MyMath = {
    max: function () {
        var max = 0;
        for (var i = 0; i < arguments.length; i++) {
          if (arguments[i]) {
                max = arguments[i];
            }
        }
        return max;
    },
    min: function () {
        var max = 0;
        for (var i = 0; i < arguments.length; i++) {
            if (arguments[i]) {
                max = arguments[i];
                    }
        }
        return max;
    }
}

console.log(MyMath.max(1,3,5,7,7));
```

##### 猜数字

```javascript
/**
* 猜，产生的一个随机数
*/

function getRandom(min, max) {
   return Math.floor(Math.random() * (max - min + 1)) + min;
}

var inp, flag = 1;
var num = getRandom(1, 99);
while (inp != num) {
    inp = prompt('第' + flag + '次' + '猜一个数字[1,99]：');
    if (inp > num) {
        alert('太大了');
    } else {
        alert('太小了');
    }
    if (flag == 4) {
        alert('没机会了');
        break;
    }
    flag++;
}
if (flag < 10) {
    alert('对了');
}
```

# 内置对象

## 	Math对象

| 序号 | Math对象               | 说明                        |
| ---- | ---------------------- | --------------------------- |
| 1    | Math.PI                | 圆周率                      |
| 2    | Math.**random**()      | 返回一个随机小数 [0,1)      |
| 3    | Math.max(value)        | 最大值                      |
| 4    | Math.min(value)        | 最小值                      |
| 5    | Math.abs(value)        | 绝对值                      |
| 6    | Math.floor(value)      | 向下取整 不是四舍五入       |
| 7    | Math.ceil(value)       | 向上取整 天花板             |
| 8    | Math.round(value)      | 四舍五入，.5特殊            |
| 9    | 扩展 Math.**random**() | 返回[min,max]之间随机一个数 |

```javascript
// 1. 圆周率
Math.PI; // 3.141592653589793

// 2. 返回一个随机小数 [0,1)
Math.random();

// 3. 最大值
Math.max(-1, -10,2); // 2
Math.max(1, 99, 'hh'); // NaN

// 4. 最小值
Math.min(1, 3, 8, -4); // -4

// 5. 绝对值
console.log(Math.abs(-1)); // 1
console.log(Math.abs('-2')); // 2 隐式转换

// 6. ~ 8. 取整
// (1) Math.floor() 向下取整 不是四舍五入
console.log(Math.floor(4.12)); // 4
console.log(Math.floor(-4.12)); // -5
// (2) Math.ceil() ceil 天花板 向上取整
console.log(Math.ceil(9.1)); // 10
console.log(Math.ceil(-4.12)); // -4
// (3) Math.round() 四舍五入 .5 特殊往大了取
console.log(Math.round(1.1)); // 1
console.log(Math.round(1.5)); // 2
console.log(Math.round(-1.1)); // -1
console.log(Math.round(-1.5)); // 结果-1  
console.log(Math.round(-1.51)); // 结果-2  

// 9. 返回[min,max]之间随机一个数
function getRandom(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
console.log(getRandom(2, 89));
// 随机抽取颜色
var arrColor = ['red','blue','green','black','yellow','purple']
console.log(arrColor[getRandom(0,arrColor.length-1)])

```



## 	Data对象

### 实例化Data对象

```javascript
/*
	January，February，March，April，
	May，June，July，August，
	September，October，November，December
*/

// 1. 创建一个日期对象
var date = new Date();
console.log(date); // Mon Nov 28 2022 14:48:17 GMT+0800 (香港标准时间)

// 2. 参数常用写法 
// 数字型 2022,11,28 或 字符串型 '2022-11-28 6:6:6' '2019/1/1'
var date1 = new Date(2022, 11, 28);
var date2 = new Date('2022-11-28 6:6:6');
// 返回 12月 不是 11月
console.log(date2); // Mon【Nov】28 2022 06:06:06 GMT+0800 (香港标准时间)
console.log(date1); // Wed【Dec】28 2022 00:00:00 GMT+0800 (香港标准时间)


```



### 年月日、星期

```javascript
var date = new Date(); // 声明一个日期对象
```

| 序号 | Data对象的方法          | 说明                  | 注意事项                 |
| ---- | ----------------------- | --------------------- | ------------------------ |
| 1    | date.**getFullYear**()  | 返回一个数字型 年份   |                          |
| 2    | date.**getMonth**() + 1 | 返回一个数字型 月份   | 返回值比实际月份小一个月 |
| 3    | date.**getDate**()      | 返回一个数字型 几号   |                          |
| 4    | date.**getDay**()       | 返回一个数字型 星期几 | 星期天~星期六= [ 0 ~ 6 ] |

```javascript
// 格式化日期 年月日
// 输出一个优雅的日期
function getDate() {
    var date = new Date(); // 声明一个日期对象
    var year = date.getFullYear(); // 几年
    var mouth = date.getMonth() + 1; // 几月 返回值少一个月需要加1
    var dates = date.getDate(); // 几号
    var day = date.getDay(); // 星期几 返回一个数字型  星期天~星期六[0 ~ 6]
    var arrDay = ['星期天', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
    return ('今天是：' + year + '年' + mouth + '月' + dates + '日 ' + arrDay[day]);
}
console.log(getDate()); //  今天是：xxxx年xx月xx日 星期x
```



### 时分秒

```javascript
var date = new Date(); // 声明一个日期对象
```

| 序号 | Data对象的方法        | 说明                        |
| ---- | --------------------- | --------------------------- |
| 1    | date.**getHours**()   | 返回一个数字型 【24小时数】 |
| 2    | date.**getMinutes**() | 返回一个数字型 【分】       |
| 3    | date.**getSeconds**() | 返回一个数字型 【秒】       |

```javascript
function getTime() {
    var time = new Date();  // 1. 实例化一个对象
    var h = time.getHours(); // 时
    h=h < 10 ? '0' + h : h; // 补零
    var m = time.getMinutes(); // 分
    m=m < 10 ? '0' + m : m; // 补零
    var s = time.getSeconds(); // 秒
    s=s < 10 ? '0' + s : s; // 补零
    return h + ':' + m + ':' + s;
}

console.log(getTime()); // xx:xx:xx
```



### 时间戳

```javascript
var date = new Date(); // 声明一个日期对象
```

| 序号 | Data对象的方法     | 说明                                           |
| ---- | ------------------ | ---------------------------------------------- |
| 1    | date.**valueOf**() | 含有隐式转换为其它数据类型（转String来比较等） |
| 2    | date.**getTime**() | 始终返回一个数字型时间戳                       |

#### 书写方式

```javascript
// 1. new实例化
 var date = new Date();
date.valueOf()
date.getTime()

// 2. 常用写法
var date1 = +new Date(); // +new Date() 返回的就是总毫秒数

// 3.  H5 新增静态方法 获取总毫秒数 IE9及以上使用
Date.now()
```

#### 倒计时例子

```javascript
// 1. 输入时间 - 现实时间 = 剩余时间（输入时间<显示时间 = 结果为负数）
// 2. 时间戳转换
// 3. 毫秒数 / 1000 = 秒数
function conutDown(time) {
    // 1. 输入时间  现实时间  剩余时间
    var nowTime = +new Date(); // 返回当前时间总毫秒数
    var inputTime = +new Date(time); // 返回用户输入时间的总毫秒数
    var times = (inputTime - nowTime) / 1000; // 剩余时间
    // 2. 时间戳转换
    var d = parseInt(times / 60 / 60 / 24); // 计算天数
    var h = parseInt(times / 60 / 60 % 24) // 计算小时
    var m = parseInt(times / 60 % 60); // 计算分数
    var s = parseInt(times % 60); // 计算当前秒数

    // 3. 如果：输入时间 - 现实时间 = 负剩余时间，则去除下划线
    if (d < 0 || h < 0 || m < 0 || s < 0) {
        d = remove_(d + '');
        h = remove_(h + '');
        m = remove_(m + '');
        s = remove_(s + '');
    } else {
        // 补零 07天02时53分04秒
        d = d < 10 ? '0' + d : d;
        h = h < 10 ? '0' + h : h;
        m = m < 10 ? '0' + m : m;
        s = s < 10 ? '0' + s : s;
    }
    return d + '天' + h + '时' + m + '分' + s + '秒';
}
// 年月日
function getDate() {
    var date = new Date(); // 声明一个日期对象
    var year = date.getFullYear(); // 几年
    var mouth = date.getMonth() + 1; // 几月 返回值少一个月需要加1
    var dates = date.getDate(); // 几号
    var day = date.getDay(); // 星期几 返回一个数字型  星期天~星期六[0 ~ 6]
    var arrDay = ['星期天', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
    return year + '-' + mouth + '-' + dates + ' ';
}

// 时分秒
function getTime() {
    var time = new Date();
    var h = time.getHours();
    h = h < 10 ? '0' + h : h;
    var m = time.getMinutes();
    m = m < 10 ? '0' + m : m;
    var s = time.getSeconds();
    s = s < 10 ? '0' + s : s;
    return h + ':' + m + ':' + s;
}

// 去除字符 '-'
function remove_(str) {
    while (str.indexOf('-') !== -1) { // 不为-1 则能找到 就替换
        str = str.replace('-', '');
    }
    return str;
}

// 通过返回格式日期 2022-12-4 08:52:42
function getNowTime() {
    return getDate() + getTime();
}

var str = getNowTime();
console.log(str); // 当前时间 

console.log('距离2023-3-12 09:00:00过了：'+ conutDown('2023-3-12 09:00:00')); // 距离 以前过了多久
console.log(conutDown(str)); // 距离 现在过了多久
console.log('距离2023-3-20 21:00:00还有：'+ conutDown('2023-3-20 21:00:00')); // 距离 未来还有多久
```



```javascript
// 计算剩余时间
const countDown = (time) => {
    const inputTime = new Date(time).getTime();
    const nowTime = Date.now();
    const remainingTime = (inputTime - nowTime) / 1000;

    const days = Math.floor(remainingTime / (60 * 60 * 24));
    const hours = Math.floor((remainingTime % (60 * 60 * 24)) / (60 * 60));
    const minutes = Math.floor((remainingTime % (60 * 60)) / 60);
    const seconds = Math.floor(remainingTime % 60);

    return `${days}天${formatTime(hours)}时${formatTime(minutes)}分${formatTime(seconds)}秒`;
};

// 格式化时间，补零
const formatTime = (time) => {
    return time < 10 ? `0${time}` : time;
};

// 获取当前日期字符串
const getDateStr = () => {
  const now = new Date();
    const year = now.getFullYear();
    const month = formatTime(now.getMonth() + 1);
    const day = formatTime(now.getDate());
    return `${year}-${month}-${day}`;
};

// 获取当前时间字符串
const getTimeStr = () => {
    const now = new Date();
    const hours = formatTime(now.getHours());
    const minutes = formatTime(now.getMinutes());
    const seconds = formatTime(now.getSeconds());
    return `${hours}:${minutes}:${seconds}`;
};

// 获取当前日期和时间字符串
const getDateTimeStr = () => {
    return `${getDateStr()} ${getTimeStr()}`;
};

// 测试代码
const nowTime = getDateTimeStr();
console.log(`当前时间：${nowTime}`);
console.log(`距离2023-3-12 09:00:00过了：${countDown('2023-3-12 09:00:00')}`);
console.log(`距离2023-3-20 21:00:00还有：${countDown('2023-3-20 21:00:00')}`);
console.log(`距离${nowTime}过去了：${countDown(nowTime)}`);
```



---------------

# es6

## 面向对象

### 基本思想

面向对象编程的基本思想是将程序看做一组对象的集合，每个对象都有自己的属性和行为，对象之间可以相互调用，组成一个相互协作的系统。面向对象编程强调数据的封装性、继承性和多态性。

### 特性

1. 封装性：将数据和对数据的操作封装在一起，只对外暴露必要的接口，隐藏内部的实现细节。
2. 继承性：通过继承机制，从已有的类中派生出新的类，新的类可以继承父类的属性和方法，并可以添加自己的属性和方法。
3. 多态性：同一个接口可以有不同的实现方式，使得程序具有更好的扩展性和灵活性。

## 语法和变量

------

### 严格模式

```html
<html>
<head>
    <title>Document</title>
</head>
<body>
    <script>
        'use strict'; // 开启脚本严格模式
    </script>
    
    <script>
        // ie10以前
        (function () {
            'use strict'; // 开启脚本严格模式
        })();
    </script>
    
    <script>
        function fn() {
            'use strict'; // 函数严格模式
        }
    </script>
    <script>
        /**
         * 严格模式下
         * 1. 变量必须先声明在使用
         * 2. 不能随意删除声明的变量 delete 变量;
         * 3. 函数this指向undefined，不是window
         * 4. 调用构造函数要new调用，this才不报错
         * 5. 定时器this还指向window
         * 
         * 6. 函数形参不能重名
         * 7. 函数不允许写在非函数代码块（if,for）
         */
        'use strict'; // 哪里要开启严格模式，就设置

        function Singer() {
            name: '邓紫棋'
        }
        var dzq = new Singer()
        console.log(dzq)
    </script>
</body>

</html>
```



------

### 变量块级作用域

#### let关键字

```javascript
/**
 * let 关键字 声明变量
 * 1. 用来声明块级变量
 * 2. let大括号中声明具有块级作用域，var不具备
 * 3. 防止循环变量变为全局变量
 * 4. 不存在变量提升，只能先声明在使用
 * 5. 暂时性死区 , let声明变量会和当前块级绑定
 * 
 * 6. 经典变量提：
 * 
 */
// 1. 
let a = 100;
console.log(a); // 100

// 2. 
if (true) {
    let b = 200;
    console.log(a); // 100
}
// console.log(b); //  b is not defined

// 3.
for (let i = 0; i < 2; i++) {
}
// console.log(i) // i si no defined

// 4. 
 console.log(c) // c is no defined
let c = 300;

// 5.
var d = 0;
if (true) {
    // console.log(d) // Cannot access 'd' before initialization
    let d = 333;
}
console.log(d)
// 6.
console.log('------------经典使用-----------------')
// var 声明循环变量
var arr = [];
for (var i = 0; i < 2; i++) {
    arr[i] = function () { console.log(i) }
}
// i变量提升，全局变量，i循环完值是2，调用函数使用i的最终值2
arr[0](); // 2
arr[1](); // 2

// let 声明循环变量
var arr1 = [];
// 每次循环都产生一个块级作用域，各自绑定let声明的i
for (let i = 0; i < 2; i++) {
    arr1[i] = function () { console.log(i) }
}
// i没有变量提升，局部变量，每次循环的i值都不一样
arr1[0](); // 0
arr1[1](); // 1
```



#### const关键字

```javascript
/**
 * const 关键字 声明常量
 * 1. 声明的常量具有块级作用域
 * 2. 声明常量必须赋予初始值
 * 3. 赋值后，值不能修改
 * （常量对应的地址值不能更改）
 * （定义的复杂数据类型可以修改里面的值，但不能直接修改复杂数据类型）
 * 4. 不具有变量提升
 */
// 2.
//  const a; // 编辑器直接语法提示错误
// 3.
const arr = ['a', 'b']
arr[0] = 1; // 可以修改复杂数据类型里面的值
arr[1] = 1;
arr[2] = 1;
console.log(arr);
// arr = ['1', '2']; // 不可直接修改复杂数据类型
```



------

### 模板字符串

```javascript
/**
 * 模板字符串 `str`
 * 1. 用反引号定义
 * 2. 可以解析变量
 * 3. 可以换行编写
 * 4. 可以调用函数
 */
// 2.可以解析变量
let name = `邓紫棋`;
let sayHello = `Hellow, 我的名字叫${name}`;
console.log(sayHello);
// 4.可以调用函数
const sing = (song) => `唱首${song}给你听！`;
let singasong = `今天，${sing('偶尔')}`;
console.log(singasong)
```

------

### 剩余参数

```javascript
/**
 * 剩余参数：
 * 1. 箭头函数用不了 arguments
 * 2. 剩余参数求和
 * 3. 剩余参数与解构赋值
 */
// 2.
const sum = (...args) => {
    let total = 0;
    args.forEach(item => total += item)
    return total;
}
console.log(sum(10, 20))
console.log(sum(10, 20, 30))
// 3. 
let colors = ['hello', 'world', 'welcome']
let [a, ...n] = colors;
console.log(a);
console.log(n);
```



------

### 正则表达式

```javascript
/**
 * 正则表达式：js中是对象
 * 词：替换，匹配，提取
 * 
 * 创建：
 *      1. RegExp对象 2. 字面量创建
 * 
 * 检测：
 *      1. regObject.test(inputStr),检测表达式是否符合规范
 * 
 * 特殊字符（元字符）：
 *      1.边界符 ^ $
 *      2.字符类 [] 
 *      3.范围 [-]
 *      4.字符组合 [a-zA-Z0-9]
 *      5.取反[^]
 *      6.量词符 
 *      7.优先级 ()
 */
// 创建：正则表达式规则
var regexp = new RegExp(/123/);
var reg = /123/;
// 检测：字符串是否符合
console.log(regexp.test(123));// true

// 边界符 ^ $  (开头与结尾)
var reg1 = /^abc$/;
console.log('边界符' + reg1.test('abcc'));// true
// 字符类 [] 包含其中任何一个就可以
var reg2 = /[abc]/;
console.log('字符类' + reg1.test('abcc'));// true
// 字符类和范围 [-] 包含其中任何一个就可以
var reg3 = /[a-z]/;
console.log('字符类和范围' + reg3.test('nn'));// true
// 字符类和范围组合 [-] 包含其中任何一个就可以
var reg4 = /[a-zA-Z0-9]/;
console.log('字符类和范围组合' + reg4.test('nn12A' + 1));// true
// 字符类取反 [^] 包含其中任何一个就不可以
var reg5 = /^[^a-zA-Z0-9]/;
console.log('字符类和范围组合' + reg5.test('nn12A' + 1));// false

// 算数比较符
// * 等价于 >=0 ,出现次数>=0
var reg1 = /^a*$/
console.log('量词符*:' + reg1.test()); // true
console.log('量词符*:' + reg1.test('aaa')); // true
// ?  1 || 0 次
var reg2 = /a?/
console.log('量词符？:' + reg2.test()); //true
console.log('量词符？:' + reg2.test('aqa')); // true
//{3}重复3次  {3，}大于等于3次  {3，16}大于等于3次，小于等于16次
var reg3 = /a{3}/
console.log('量词符{3}:' + reg3.test('a')); // false
console.log('量词符{3,}:' + /a{3,}/.test('aaaa')); // true
console.log('量词符{3,16}:' + /a{3,16}/.test('aaaa')); // true
// () 变成一个整体
console.log('小括号():' + /(abc){3,16}/.test('aaaa')); // false
console.log('小括号():' + /(abc){3,16}/.test('abcabcabcabc')); // true
```

#### 预定义类

```javascript
/**
 * \d 等价于 [0-9]
 * \D 等价于 [^0-9]
 * \w 等价于 [a-zA-Z0-9]
 * \W 等价于 [^a-zA-Z0-9]
 * \s 等价于 [\t\r\n\v\f] 空格、换行、回车、制表符...
 * \S 等价于 [\t\r\n\v\f] 非空格、换行、回车、制表符...
 * https://c.runoob.com
 */
console.log(/\d/.test(1))
console.log(/\D/.test(1))
```

#### 替换敏感词

```html
<textarea name="" id="message" cols="30" rows="10"></textarea>
<button>检查</button>
<div></div>
<script>
    /**
     * 替换所有匹配的字符串
     * var newStr = oldStr.replace('oldValue','newValue');
     * 
     * var newStr = oldStr.replace(/oldValue/,'newValue');
     * 
     * 正则表达式参数：
     * /表达式/[参数]
     * g:全局匹配
     * i:忽略大小写
     * gi:全局匹配+忽略大小写
     */
    var str = 'hello yzl';
    var text = document.querySelector('#message')
    var btn = document.querySelector('button')
    var div = document.querySelector('div')
    btn.onclick = function () {
        div.innerHTML = text.value.replace(/激情|jiqing/g, '**')
    }
</script>
```



------

### 解构赋值

```javascript
/**
 * 结构赋值：按照一一对应的关系从数组中提取值，然后交给另外的变量
 * 1. 数组解构
 * 2. 对象解构
 */
// 数组解构
let [a, b, c] = [1, 2, 3];
let ary = [1];
let [d, e] = ary;
console.log(e); // undefined
// 对象解构
let Vsinger = { name: '乐正绫', bth: 412 }
let { name, age } = Vsinger;
console.log(name);
//别名
let { name: vname, age: vage } = Vsinger;
console.log(vname)
```

### 扩展运算符

```html
<ul>
    <li>第1个</li>
    <li>第2个</li>
    <li>第3个</li>
    <li>第4个</li>
</ul>
<div></div>
<div></div>
<div></div>
<div></div>
<script>
    /**
     * 扩展运算符 ...
     * 1. 将数组转为用逗号分隔的参数序列
     * 2. 将对象转为用逗号分隔的参数序列
     * * 合并数组 或 对象
     * 
     * 3. 将伪数组转换为真数组
     */
    
    // 1. 将数组转为用逗号分隔的参数序列
    let arr = [1, 2, 3, 4];
    let newArr = [0, ...arr]; // 方法一
    // arr.push(...ary2); // 方法二
    console.log(...arr);
    console.log(...newArr);
    
    // 2. 将对象转为用逗号分隔的参数序列
    const o = { name: '乐正绫', bth: 412 }
    const singer = { age: 15, ...o }
    console.log(o);
    console.log(singer);
    
    // 3. 将伪数组转换为真数组
    var lis = document.querySelectorAll('li');
    // lis.push('a'); // push is no a function
    console.log(lis); // 伪数组
    const newLis = [...lis];
    newLis.push('a');
    console.log(newLis); // 真数组
</script>
```



------

## 类

### 类和对象

| 关键字      | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| class       | 使用 clas 关键字来定义一个类，类名以大写字母开头             |
| this        | this关键字来创建对象属性                                     |
| constructor | constructor 构造函数，它会在创建对象时接收传入的参数，自动调用并返回值， |
| new         | 使用 new 关键字和类名来创建一个对象：                        |



```javascript
/**
 * 1. 创建一个类
 * 2. 创建一个对象
 * 3. 类中添加方法 
 */
// 1. 创建一个类
 class Vsinger {
	// 构造函数 创建实例时自动调用并返回值
     // 接收传递来的参数，同时返回实例对象
     constructor(vname, age) {
         this.vname = vname;
         this.age = age;
     }
     // 类中添加方法
     sing(song) {
         console.log(this.vname + '唱首"' + song + '"给你听!')
     }
}

// 利用new创建对象 
var lty = new Vsinger('洛天依', 16);
var yzl = new Vsinger('乐正绫');

// 调用类中参数
lty.vname;
lty.age;
yzl.vname;

// 调用类中方法
lty.sing('达拉崩吧');
yzl.sing('世末歌者');
```



### 类的继承

```javascript
/**
 * 1. extends 类的继承
 * 2. super() 可以调用父类中的（1）构造参数，（2）普通函数
 * 3. 子类父类同名方法，调用时就近原则
 * 4. 字类独有的方法
 */

// 父类 Vsing
class VSinger {
    constructor(vname, song) {
        this.vname = vname;
        this.song = song;
    }
    can() { return '唱,跳,rap' }
    sing() { return this.vname + '唱：' + this.song) }
   // represents color
    rColor() { return '代表色:' }
}

// 子类 YZLing
class YZLing extends VSinger {
    constructor(vname, song) {
        super(vname, song); // 2.（1）super调用父类中的构造参数 必须在子类this前面
        this.vname = vname;
        this.song = song;
    }
	// 2.（2）super调用父类普通函数
    rColor() { return super.rColor() + '#EE0000'; }
    // 4. 字类独有的方法  Introduce myself
    introduce() { return '我是' + this.vname + ',代表作有：' + this.song }
}

// 实例化对象
var yzl = new YZLing('乐正绫', '梦语');
// 子类调用父类中方法
yzl.can(); // 字类调用父类中普通方法
yzl.sing(); // super()调用父类的 构造参数
// 3. 子类父类同名方法，调用时就近原则
yzl.rColor(); // super()调用父类的 普通方法
// 4. 调用字类独有方法
yzl.introduce(); 

```

### 类的注意事项

```html
<body>
    <button>点击</button>
</body>
<script>
 /**
 * 1. 类的实例化必须写在类的后面
 * 2. 实例化对象就调用方法，必须在构造函数里面调用该方法（方法注意括号情况）
 * 3. 构造函数里面调用方法加括号，实例化对象就直接调用了
 * 
 * this的指向问题
 * 1. 构造函数里this指向实例化对象
 * 2. 谁调用的this就指向谁
 */
var that;
class VSinger {
      constructor(vname, color) {
          that = this;
          this.vname = vname;
          this.color = color;
          // this.sing(); // 2.例化对象就调用方法
          // this.sing; // 区别 
          this.btn = document.querySelector('button');
          this.btn.onclick = this.sing;  
      }
      sing() {
          console.log(this.vname); // this指向btn 所以undefined
          console.log(that.vname); // that全局变量，存储的是实例化对象
      }
}
</script>
```

### 类的本质

```javascript
/**
 * es6 前 构造函数 + 原型 实现面向对象编程
 *  （1） 构造函数有原型对象prototype
 *   (2) 构造函数原型对象prototype里面有constructor 指向构造函数本身
 *   (3) 构造函数可以通过原型对象添加方法
 *   (4) 构造函数创建的实例对象有__proto__ 原型指向构造函数的原型对象
 * es6 后 通过类 实现面向对象编程
 */
 class Singer { }
 console.log(typeof Singer) // 本质还是一个函数，可以简单理解为构造函数
 // 类 有 原型对象prototype
Singer.prototype;
 // 类 原型对象prototype里面有constructor 指向构造函数本身
Singer.prototype.constructor;
 // 类 可以通过原型对象添加方法
 Singer.prototype.sing = function sing() {
     console.log('唱首歌给你听!')
 }

// 类 创建的实例对象有__proto__ 原型指向构造函数的原型对象
 var dzq = new Singer();
 console.log(dzq.__proto__ === Singer.prototype)
```



## 构造函数

### 构造函数和对象

```javascript
/**
 * 1. 利用 new Object() 创建
 * 2. 利用 对象字面量创建对象
 * 3. 利用 构造函数创建对象
 * 
 * ----构造函数---
 * ①实例成员 构造函数内this添加  实例化对象调用
 * ②静态成员  构造函数外部添加   构造函数名调用
 * 
 * 存在问题：
 * 每一次实例对象都会开一个内存空间存放数据
 */

// 1. 利用 new Object() 创建
var obj1 = new Object()
// 2. 利用 对象字面量创建对象
var obj2 = {}
// 3. 利用 构造函数创建对象
function Star(uname, age) {
    // 实例成员（this添加）
    this.uname = uname;
    this.age = age;
    this.sing = function () {
        console.log('唱首歌给你听')
    }
}
// 创建对象
var dzq = new Star('邓紫棋', '20');
var zs = new Star('周深', '18');
zs.sing();

// 静态成员
Star.sex = '男'
console.log(Star.sex) // 通过构造函数读取

```

------

### prototype(共享方法)

```javascript
/**
  * 构造函数的原型对象 prototype
  * 1. 每一个构造函数都有一个 prototype 属性
  * 2. 作用：原型对象中的所有属性和方法都共享给每一个实例对象
 */
function Star(uname, age) {
	this.uname = uname;
    this.age = age;
    // 每一次实例对象都会开一个内存空间存放这些数据
    /* this.sing = function () {
         console.log('唱首歌给你听')
     }*/
}
// 原型对象中一般存放一些公共的方法
// 每一个实例对象都可用，多次实例对象不单独新开辟空间存放这些数据
Star.prototype.sing = function () {
    console.log('唱首歌给你听')
}
var dzq = new Star('邓紫棋', '20');
var zs = new Star('周深', '18');

dzq.sing();
zs.sing();
```

------

###  \__proto__(提供路线)

```javascript
/**
 * 构造函数实例对象的原型 __proto__
 * 1. 指向构造函数的 prototype 原型对象，所以对象可以用构造函数的共享方法
 * 2. __proto__对象原型和原型对象 prototype 是等价的
 * 3. 意义：为对象的查找机制提供一个方向（一条路线），它是一个非标准属性，因此实际开发中，不可以使用这个属性，它只是内部指向原型对象 prototype
 */
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
    // this.sinug = function () {
    //     console.log('唱首歌给你听')
    // }
}
// 原型对象中一般存放一些公共的方法
Star.prototype.sing = function () {
    console.log('唱首歌给你听')
}

var dzq = new Star('邓紫棋', '20');
var zs = new Star('周深', '18');

dzq.sing();
zs.sing();
console.log(dzq); // Star构造函数里面的属性
console.log(dzq.__proto__); // 一些共有属性
console.log(Star.prototype === dzq.__proto__) // ture
```

【Star构造函数】 ------------(Star.prototype)------------ > 【Star原型对象prototype存放sing方法】
		\ 				   < ----(Star.prototype.constructor)----		   ^
			\											 	     					  	/
				\								   					 	  (dzq.\__proto)
					\									  					     	/
						\														    /
							\\------- > 【实例对象dzq】---------- /
                                    ldh.proto__.constructor

------

### construor(多重添加)

```javascript
/**
 * 对象原型（ __proto__）和构造函数（prototype）原型对象
 * 都有一个属性 constructor属性 (构造函数)
 * 它指回构造函数本身
 * 1. 原型对象 prototype.constructor 
 * 2. 对象原型 _proto_.constructor 
 *
 * ---- prototype添加公共方法 ----
 * 1. 赋值为一个函数，一次只能添加一个共享方法
 * 2. 赋值为一个对象，一个对象里放多个共享方法（有坑）
 *    (1) 赋值为对象覆盖了原来的构造函数constructor
 * 	  (2) 要手动指回原来的构造函数
 */

function Star(uname, age) {
    this.uname = uname;
    this.age = age;

}
// 原型对象中一般存放一些公共的方法
/*
Star.prototype.sing = function () {
     console.log('唱首歌给你听')
}
 Star.prototype.say = function () {
     console.log('say hellow')
}*/

// Star.prototype 赋值为对象覆盖了原来的构造函数constructor
Star.prototype = {
    // 手动指回原来的构造函数
    constructor: Star,
    sing() {
        console.log('唱首歌给你听')
    },
    say: function say() {
        console.log('say hellow')
    }
}

var dzq = new Star('邓紫棋', '20');
var zs = new Star('周深', '18');

console.log(Star.prototype)
console.log(Star.prototype.constructor) // Star
console.log(dzq.__proto__.constructor) // Star

```

------

### 原型链

在JavaScript中，每个对象都有一个指向其原型（prototype）对象的内部链接，称为原型链。
通过原型链，子对象可以继承其原型对象和其原型对象的属性和方法。

当我们访问一个对象的属性时，如果该对象本身没有该属性，则JavaScript引擎会沿着原型链向上查找，直到找到该属性或者原型链的顶端（即Object.prototype）为止。

假设我们有一个对象obj，它的原型对象是proto，而proto的原型对象是Object.prototype。如果我们访问obj的一个属性，但是obj本身并没有该属性，那么JavaScript引擎会在proto对象中查找该属性，如果还没有，则继续在proto的原型对象中查找，直到最终在Object.prototype中找到该属性或者到达原型链的顶端。

查找一个对象中的属性时：

1.  在对象本身查找（`this`）
2.  在原型对象上找（`prototype`）
3.  在对象原型上找（`__proto__`）
4.  最终找到或达到原型链的顶端

```javascript
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
}
Star.prototype.sing = function () {
    console.log('唱首歌给你听')
}
var dzq = new Star('邓紫棋', '20');
var zs = new Star('周深', '18');
console.log(dzq.constructor)
console.log(dzq.__proto__) // Object {共有属性、constructor指向Star}
console.log(dzq.__proto__ === Star.prototype) // true
console.log(Star.prototype.__proto__ === Object.prototype) // true
console.log(Object.prototype.__proto__ === null) // true
```

------

### 原型对象this

```javascript
/**
 * this指向实例对象
 * this指向调用者
 */
var that
function Star(uname, age) {
    this.uname = uname;
    this.age = age;
}

Star.prototype.sing = function () {
    console.log('唱首歌给你听')
    that = this // 没调用为undefined
}

var dzq = new Star('邓紫棋', '20');
var zs = new Star('周深', '18');

console.log(that); // undefined
dzq.sing();
console.log(that); // Start
```

------

### 扩展内置对象方法

```javascript
/**
 * 原型对象 prototype
 * 扩展内置对象（Array）
 */

// 数组原型对象中追加求和方法
Array.prototype.sum = function () {
    var sum = 0;
    for (var i = 0; i < this.length; i++) {
        sum += this[i];
    }
    return sum;
}
// 不能实现：赋值为一个对象，一个对象里放多个共享方法（有坑）
/*
Array.prototype = {
    constructor: Array,
    sum() {
        var sum = 0;
        for (var i = 0; i < this.length; i++) {
            sum += this[i];
        }
        return sum;
    }
}*/

console.log(Array.prototype)
var arr = [1, 2, 3]

console.log(arr.sum())
```

### 继承(es5的call方法)

#### call方法

```javascript
/**
 * 1. call 可以调用方法
 * 2. call 可以修改this指向
 * 3. call 可以传递参数
 * 一般指向上下文
 */
function fn(name, say) {
    console.log(this); // Window
    return name + '说:' + say;
}
var o = { o: 0 }

// 可以调用方法
fn.call();
// 可以修改this指向，指向对象o
fn.call(o);
// 可以传递参数
var a = fn.call(o, '小明', 'hello')
console.log(a);
```

#### 借用构造函数继承父类的属性

```javascript
/**
 * 1. 借用父构造函数继承属性
 *	  (1) 子构造函数用call方法接收父级的属性。
 * 2. 借用父构造函数继承方法
 *	  (1) 将子构造函数的原型对象prototype指向父级的实例化对象，实现继承方法。
 *    (2) 将子的原型对象指向父级的对象，GEM.prototype.constructor也会指向父级构造函数。
 *    (3) 要将GEM.prototype.constructor重新指向GEM构造函数
 */
// 父级
function Singer(name, sing) {
    this.name = name
    this.sing = sing
}
Singer.prototype.sayH = function () {
    console.log('hello everybody !')
}
// 子
function GEM(name, sing, hot) {
    // 借用父构造函数继承属性
    Singer.call(this, name, sing)
    this.hot = hot
}
Singer.prototype.sayH = function song() {
    console.log('唱首歌给你听！')
}
// 借用父构造函数继承方法
GEM.prototype = new Singer(); // 将子构造函数的原型对象指向父级的实例化对象，实现继承方法
console.log(GEM.prototype.constructor); // GEM.prototype.constructor也会指向父级构造函数
GEM.prototype.constructor = GEM;// 要将GEM.prototype.constructor重新指向GEM构造函数
console.log(GEM.prototype.constructor); // 指回了子级（本身）
var dzq = new GEM('邓紫棋', '偶尔', 666)

console.log(dzq);
console.log(dzq.name); // 输出：邓紫棋
console.log(dzq.sing); // 输出：偶尔
console.log(dzq.hot); // 输出：666
dzq.sayHello(); // 输出：Hello everybody！
dzq.saySong(); // 输出：唱首歌给你听！
console.log(GEM.prototype.constructor); // 输出：GEM

```

```javascript
// 优化
// 1. 在将GEM的原型指向Singer对象之前，最好先将GEM.prototype设置为一个新对象，以避免直接修改Singer对象的原型。确保GEM对象的原型链不会受到不必要的干扰。
// 2. 继承Singer的方法时，可以使用Object.create()方法创建一个新对象，并将其原型指向。Singer.prototype，这样可以更直观地表达出继承的关系
// 3. 使用Object.create()方法还可以避免调用Singer构造函数，从而避免不必要的开销和副作用。
function Singer(name, sing) {
  this.name = name;
  this.sing = sing;
}

Singer.prototype.sayHello = function() {
  console.log('Hello everybody!');
}

function GEM(name, sing, hot) {
  Singer.call(this, name, sing);
  this.hot = hot;
}

GEM.prototype = Object.create(Singer.prototype);
GEM.prototype.constructor = GEM;

GEM.prototype.saySong = function() {
  console.log('唱首歌给你听！');
}

var dzq = new GEM('邓紫棋', '偶尔', 666);
console.log(dzq);
console.log(dzq.name); // 输出：邓紫棋
console.log(dzq.sing); // 输出：偶尔
console.log(dzq.hot); // 输出：666
dzq.sayHello(); // 输出：Hello everybody！
dzq.saySong(); // 输出：唱首歌给你听！
console.log(GEM.prototype.constructor); // 输出：GEM
```

------



## 函数

### 定义

```javascript
/** 
 *  1. 自定义函数（命名函数）
 *  2. 函数表达式（匿名函数）一般作为回调函数，或使用立即函数调用
 *  3. 利用 new Function('value','value','函数体')
 * 
 * 所有函数都是Function的实例（对象）
 * 函数也属于对象
 */
// 1. 命名函数
function fn() { }
fn();
// 2. 匿名函数
var fun = function () { }

// 3. new
var f = new Function('a', 'b', 'console.log(a + b)');
f(1, 2);
console.dir(f);
//  即是函数也属于对象
console.log(f instanceof Object);  // ture
console.log(f instanceof Function); // ture
```

### 调用·this·指向

```javascript
/**
 * 1. 普通函数 this.指向window
 * 2. 对象方法 this指向函数调用者
 * 3. 构造函数 this指向实例对象
 * 4. 绑定事件函数 this指向触发事件的对象
 * 5. 定时器函数 this指向window
 * 6. 立即执行函数 this指向window
 */
// 1. 
function fn() { }
fn();
// 2. 
var obj = {
    sayH() {
        console.log('hello ！')
    }
}
obj.sayH();
// 3.
function Star() { console.log('匿名函数调用了') }
new Star();
// 4.
// btn.click = function () { };
// 5.
setInterval(function () { console.log('滴——') }, 5000);;
// 6.
(
    function () { console.log('666') }
)()
```

------

### 改变函数this指向

| 方法  | 语法结构                               | 执行时机     | 传参方式 |
| ----- | -------------------------------------- | ------------ | -------- |
| call  | `bind(thisArg[, arg1[, arg2[, ...]]])` | 立即执行     | 随意     |
| apply | `apply(thisArg[, argsArray])`          | 立即执行     | 数组     |
| bind  | `call(thisArg[, arg1[, arg2[, ...]]])` | 不会立即执行 | 随意     |

-   `call` 和 `apply` 是用于立即调用函数并传递参数的方法，它们的区别在于参数的传递方式。
-   `bind` 用于创建一个新函数，该新函数具有指定的上下文，并且可以使用变量接收稍后调用。

```javascript
/**
 *  call(thisArg,arg1,agr2,...) 
 */
//  call
var o = { name: 'yue' }
function sayH(a, b) {
    console.log('hello !')
    console.log(a + b)
}
sayH.call(o, 1, 2) 
// 调用函数，改变this指向，传递参数，实现继承
function Father(name, age) {
    this.name = name
    this.age = age
}
function Son(name, age) {
    Father.call(this, name, age)
}
var son = new Son('邓紫棋', 18)
console.log(son)

```

```javascript
/**
 *  fun.apply(thisArg,[argsArray])
 *  thisArg: 在fun运行时指定的this值
 *  argsArray: 传递的值，必须为数组形式(伪数组)
 * 返回值就是函数的返回值
 */
var obj = {
    name: 'lalala'
}
function lala(array) {
    console.log(this);
}
lala.apply(obj, ['jjj'])
var arr = [99, 33, 4, 323, 412];
var max = Math.max.apply(Math, arr)
console.log(max)
```

```javascript
/**
 * fun.bind(thisArg,arg1,arg2,...)
 * thisArg: fun函数运行时指定的this
 * argx：传递的参数
 * 
 * 不会调用函数，但可以改变函数内部this指向
 * 返回值由指定this和初始化参数改造的原函数拷贝（新函数）
 */
var v = {
    book: '《清时》'
}
function readBook() {
    console.log(this)
}
var newfn = readBook.bind(v);
newfn()
// bind使用场景
var btn = document.querySelector('button')
btn.onclick = function () {
    this.disabled = true; // 禁用按钮
    // var that = this;
    setTimeout(function () {
        // that.disabled =false;
        this.disabled = false;
    }.bind(btn), 3000)
}
```

------

### 闭包

内部函数访问外部函数变量的一种关于函数和作用域的概念。可以用来封装私有变量、实现模块模式、延迟执行函数等。

```javascript
// 闭包closure： 有权访问另一个函数作用域种变量的函数
// 1. 闭包是一个函数
// 2. 延申了变量的作用域
// fun是闭包函数
function fn() {
    var num = 100
    function fun() {
        console.log(num)
    }
    // fun()
    return fun;
}      王海宏
// 1. 情形
// fn()
// 2. 情形
var f = fn();
f();
```

------

#### 特点

1.  延长变量的作用域。
2.  保留作用域链。（通过作用域链，依然可以访问外部函数的变量）
3.  延长变量的生命周期。（外部函数的变量在闭包存在时不会被销毁，哪怕函数执行完毕，因为遍历引用还存在）
4.  创建私有变量和函数。（外部作用域无法直接访问内部变量或函数）

内存泄漏：

1.  **循环引用**：闭包被外部定义的变量接收引用。
2.  **未及时释放资源**：闭包存储在全局变量或长期存在的数据结构中
    1.  明智地管理闭包的生命周期，确保它们在不再需要时被释放。
    2.  避免循环引用，特别是在闭包和外部作用域之间。
    3.  使用局部变量而不是全局变量，以限制闭包的范围。
    4.  在不需要的时候手动解除对闭包的引用，以便垃圾回收器可以回收它们。（设置为null）

------

#### 应用

```html
<body>
    <ul class="nav">
        <li>水煮蛋</li>
        <li>卤鸡蛋</li>
        <li>煎蛋</li>
        <li>茶叶蛋</li>
    </ul>
    <script>
        /**
         * 点击li输出index
         */

        var lis = document.querySelectorAll('li');
        // for (var i = 0; i < lis.length; i++) {
        //     lis[i].index = i;
        //     lis[i].onclick = function () {
        //         console.log(this.index)
        //     }
        // }

        /**
         * 闭包方式：点击li输出index
         */

        // for (var i = 0; i < lis.length; i++) {
        //     // 立即执行函数里面的函数都能使用它的i，产生闭包
        //     (function (i) {
        //         lis[i].onclick = function () {
        //             console.log(i)
        //         }
        //     })(i)
        // }
        
        /**
         * 闭包：定时器
         */

        for (var i = 0; i < lis.length; i++) {
            (function (i) {
                setTimeout(function () {
                    console.log(lis[i].innerHTML)
                }, 2000 * i)
            })(i)
        }
    </script>
</body>
```

```javascript
/**
 * 打车：起步价14（3公里内）,
 * 后每一公里加5块钱，堵车加10块钱，
 * 输入公里数计算预计价格
 */
var car = (function () {
    var start = 13; // 起步价
    var total = 0; // 预计总价
    return {
        // 正常情况
        priceSum: function (n) {
            if (n <= 3) {
                total = start;
            } else {
                total = start + (n - 3) * 5;
            }
            return total;
        },
        // 堵车
        other: function (flag) {
            return flag ? total + 10 : total
        }
    }
})();
console.log(car.priceSum(5))
console.log(car.other(true))
```



------

### 递归

#### 简单演示

```javascript
/**
 * 递归：
 *  1. 必须要retun一个结束条件
 */
function fn(num) {
    console.log(num);
    if (num >= 6) {
        return;
    }
    num++;
    fn(num);
}
fn(2)

```

#### 阶乘n！

```javascript
/**
 * 阶乘n！
 */
function fun(n) {
    if (n == 1) {
        return 1;
    }
    return n * fun(n - 1);
}
console.log('阶乘:' + fun(6))
```

#### 斐波那契数列（兔子数列）

```javascript
/**
 * 求第n个斐波那契数列（兔子数列）的值。
 * 1，1，2，3，5，8，......
 */
function fb(n) {
    if (n == 1 || n == 2) {
        return 1
    }
    return fb(n - 1) + fb(n - 2
}
console.log('第n个斐波那契数列的值' + fb(6))
```

#### 遍历多层次的数据

```javascript
/**
 * 遍历数据
 * 输入id，返回数据对象
 */
var data = [{
    id: 0,
    info: '测试',
    goods: [
        { id: 4, book: '诛仙', price: '66￥', },
        {
            id: 6, book: '茶叶蛋',
            goods: [
                { id: 2, singer: '邓紫棋', song: '偶尔' },
                { id: 3, singer: '周深', song: '达拉崩吧' },
            ]
        },
    ]
}]
function getId(json, id) {
    var o = {}
    json.forEach(function (item) {
        if (item.id == id) {
            o = item;
        } else if (item.goods && item.goods.length > 0) {
            o = getId(item.goods, id);
        }
    });
    return o;
}
console.log(getId(data, 0))
console.log(getId(data, 4))
console.log(getId(data, 3))
```



------

### 其它

函数作为参数与返回值

```javascript
// 高阶函数：1.接收函数作为参数。|| 2. 将函数作为返回值
function fn(a, b, callback) {
    
    console.log(a + b)
    callback && callback();
}
var fun = function () {
    console.log('hello !')
}

fn(1, 2, fun) 
```



------

## 箭头函数

```javascript
/**
 * 箭头函数 ()=>{}
 * 1. 省略大括号：函数体中只有一句，并且运行结果就是函数返回值
 * 2. 省略小括号：形参只有一个的情况下省略
 * 3. 箭头函数不绑定this，它的this指向函数定义位置上下文this
 */
const fn = () => {
    console.log('hello');
}
fn();
// 1.
const fun = () => console.log("hello world!");
fun();
// 2.
const funt = v => { console.log(v) };
funt(20);
// 3.
let btn = document.querySelector('button')
// btn.onclick = function () {
//     console.log(this) // btn
// }
btn.onclick = () => {
    console.log(this); // window
}
```

------

## Set

Set 是一种内置的数据结构，用于存储唯一的值（不允许重复）。可以传递一个可迭代对象（例如数组）来初始化 Set 并添加初始值。

在JavaScript中，`Set`是一种内置对象，用于存储一组唯一的值，它类似于数组，但不允许重复值。

```javascript
/*
 *  Set实例方法
 * 1. add(value) 添加元素,必须是唯一，返回Set本身
 * 2. delete(value) 删除值, 返回布尔值
 * 3. has(value) 确定是否Set成员，返回布尔值
 * 4. clear(value) 清除Set全部成员，无返回值
 * 5. size 获取成员数量
 * 6. 素组去重
 * 7. 遍历
 */
// 创建
const s = new Set();
// 1. 
s.add(1).add(2).add(3);
// 2. 
s.delete(1) // ture
// 3.
s.has(3) // ture
// 4.
 s.clear();
// 5
s.size
// 6. 去重应用(数组去重)
const s3 = new Set(['a', 'b', 'b', 'a']);
console.log(s3.size)
const ary = [...s3];
console.log(ary)
// 7. 遍历Set
const s = new Set(['a', 'b', 'c', 'd']);
s.forEach((value, index) => {
    console.log(value + '-' + index)
})
```

------

## Map

​		`Map` 是 JavaScript 中的一种数据结构，用于存储键值对的集合，其中键是唯一的，每个键都映射到一个值。可以传递一个可迭代对象（例如数组）的数组表示来初始化 Map。

​		在JavaScript中，`map`是一个数组的高阶函数（Higher-Order Function），它用于创建一个新数组，该数组的每个元素都是原始数组中的元素经过特定函数处理后的结果。`map`函数不会修改原数组，而是返回一个新的数组。

```javascript
const newArray = array.map(callback(element, index, array));
/*
array：要操作的原始数组。
callback：用于处理数组中每个元素的回调函数。该回调函数可以接受三个参数：
element：当前被处理的元素。
index（可选）：当前元素在数组中的索引。
array（可选）：原始数组本身。
*/
```

```javascript
/**
 * Map实例方法
 * 1. get(key) 通过 键 获取 值
 * 2. set(key,value) 存入一个键值对
 * 3. has(key) 检查键是否存在于 Map 中，返回布尔值
 * 4. delete(key) 删除某一个键值对，无返回值
 * 5. 遍历 forEach、for...of
 * 6. size 获取键值对数量
 */
// 创建 
const singer = [['name', 'yzl'], ['age', 20]];
const personMap = new Map(singer);
// 1.
const name = personMap.get('name'); // 返回 'yzl'
// 2.
personMap.set('color', 'red');
// 3.
const exists = personMap.has('age'); // 返回 true
// 4.
personMap.delete('name');
// 5.
personMap.forEach((value, key) => {
  console.log(key, value);
});

for (const [key, value] of personMap) {
  console.log(key, value);
}
// 6.
const size = personMap.size;
```



------

## Array

------

### 方法使用建议

- 如果你需要对数组中的元素进行过滤，并返回一个新的数组，那么你可以使用 `Array.prototype.filter()` 方法。
- 如果你需要对数组中的元素进行检查，以确定是否存在至少一个元素符合指定条件，那么你可以使用 `Array.prototype.some()` 方法。
- 如果你需要对数组中的所有元素进行检查，以确定它们是否都符合指定条件，那么你可以使用 `Array.prototype.every()` 方法。
- 如果你需要对数组中的元素进行转换，并返回一个新的数组，那么你可以使用 `Array.prototype.map()` 方法。

------

### es5数组方法

| 15.es5数组方法                    | 功能         | 返回值 | 注意事项                             |
| --------------------------------- | ------------ | ------ | ------------------------------------ |
| forEach(fn(value,index,array){})  | 遍历         |        |                                      |
| array.filter(callback[, thisArg]) | 筛选         | 新数组 | 包含所有匹配值                       |
| array.some(callback[, thisArg])   | 查找         | 布尔值 | 满足指定条件就停止查找               |
| array.map(callback[, thisArg])    | 操作全部元素 | 新数组 | 新数组的长度与原数组相同             |
| array.every(callback[, thisArg])  | 全面检测     | 布尔值 | 检测数组中的所有元素是否符合指定条件 |

callback是一个用于测试每个元素的函数，它也可以接收三个参数。

1. `currentValue`：当前正在被处理的元素。
2. `index`：当前元素的索引（可选）。
3. `array`：原始数组（可选）。
4. `thisArg` 参数，用于指定在 `callback` 函数中使用的 `this` 值（ 默认 `undefined`）。
5. 可以用_占位（其它符号也可以）

```javascript
var arr = [1, 0, 4, 2]
// 1. 遍历
arr.forEach(function (value, index, array) {
    console.log(value + '-' + index)
    console.log(array)
})

// 2. 筛选，返回新数组。包含所有匹配值
var newArr = arr.filter(function (value, index) {
    return value >=2 0;
})
console.log(newArr)

// 3. 查找，返回布尔值 第一个值满足停止查找
var flag = arr.some(function (value, index) {
    return value > 0 ;// 是否有小于0的值
})
console.log(flag)

// 4. map(fn(){})
const numbers = [1, 2, 3, 4, 5];
const doubledNumbers = numbers.map(function(num) {
  return num * 2;
});
console.log(doubledNumbers); // Output: [2, 4, 6, 8, 10]

// 5. array.every(callback[, thisArg])
const numbers = [1, 2, 3, 4, 5];
const allNumbersArePositive = numbers.every(function(number) {
  return number > 0;
});
console.log(allNumbersArePositive); // Output: true

```

### 扩展方法

| 序号 | 方法                                  | 作用                     | 返回值            |
| ---- | ------------------------------------- | ------------------------ | ----------------- |
| 1    | Array.from（oldArr,item）             | 将伪数组转为真数组       | 返回一个新数组    |
| 2    | array.find((item,index)=>{})          | 找出第一个匹配的值       | undefined、匹配值 |
| 3    | array.finIndex((value,index)=>{})     | 找出第一个匹配的值的位置 | -1、匹配值        |
| 4    | array.includes(value[,beginIndex=0]); | 判断有无一个匹配的值     | 布尔值            |

```javascript
// 1. Array.from（oldArr,item） 方法 
var arr = { // 伪数组
    '0': "一",
    '2': "二",
    '3': "三",
    '4': "四",
    '5': "五",
    'length': 5 // 必须
}
// var newArr = Array.from(arr)
var newArr = Array.from(arr, item => item + '^')
console.log(newArr);
// 2. array.find()方法 
var arr = [
    { id: 1, name: '邓紫棋' },
    { id: 2, name: '周深' },
]
let findId = arr.find(item => item.id == 2)
console.log(findId)
// 3. 
let numArr = [10, 20, 30, 40, 50];
let index = numArr.findIndex(value => value == 20);
console.log(index); // 1
// 4.
let result = [1, 2, 3, 4].includes(1);
console.log(result)
```



------

## String

### 去除空格方法

```HTML
<input type="text" name="" id="">
<button>查询</button>
<div></div>
<script>
        /**
         * 1. trim 除去字符串两端空格 返回新字符串
         */
        var input = document.querySelector('input')
        var button = document.querySelector('button')
        var div = document.querySelector('div')

        button.onclick = function () {
            // 去除两端空格
            if (input.value.trim() === '') {
                alert('请输入信息')
            } else {
                console.log(input.value.trim().length)
                div.innerHTML = input.value.trim()
            }
        }
        // 1.
        var str = '     sing a song   ';
        var newStr = str.trim();
        console.log(newStr)
</script>
```

### 扩展

```javascript
/**
 * starsWith()
 * endsWith()
 * 1. 判断字符串是什么开头和结尾
 * 2. 返回布尔值
 */
let str = 'Hello welcome to china';
console.log(str.startsWith('hellow')); // false
console.log(str.endsWith('a')); // ture

/**
 * repeat(n)
 * 1. 将字符串重复n次，返回一个新字符串
 */
let str1 = 'a'.repeat(3);
console.log(str1); // aaa
```



------

## Object

### 内置方法Object.keys()

`Object.keys()` 方法可以用于获取对象的所有可枚举属性，并返回一个包含属性名的数组。
你可以遍历这个数组，然后访问对象的属性。

```javascript
const person = {
  name: 'Alice',
  age: 30,
  gender: 'female'
};

const keys = Object.keys(person);

keys.forEach((key) => {
  console.log(key, person[key]);
});
```

### 内置方法Object.entries()

`Object.entries()` 方法可以用于获取对象的所有可枚举属性键值对，并返回一个包含键值对的数组。你可以遍历这个数组，然后访问键和值。

```javascript
const person = {
  name: 'Alice',
  age: 30,
  gender: 'female'
};

const entries = Object.entries(person);

entries.forEach(([key, value]) => {
  console.log(key, value);
});
```



### 内置方法Object.defineProperty

它允许你在一个对象上定义一个新属性或修改一个已有属性。

```javascript
/**
 *  Object.defineProperty(obj,prop,descriptor) 定义新属性或修改原有属性 
 *  三个参数都不能省略
 *  obj： 目标对象
 *  prop: 需要定义或修改的属性名
 *  descriptor: 目标属性所拥有的特性
 * 
 * 属性配置
 * writable: false, // 不可重写这个属性，默认false
 * enumerable: false,// 不可遍历（可枚举状态） ，默认true
 * configurable: false, // 不可删除且不能修改第三个参数其它特性，默认false
 */
var Singer = {
    name: '邓紫棋',
    hobby: '唱',
    hot: 666
}
Singer.age = 22;
console.log(Singer)

Object.defineProperty(Singer, 'hot', {
    value: 6666,
    writable: false,// 不允许修改这个属性
})

Object.defineProperty(Singer, 'sing', {
    value: '唱首歌给你听！',
    enumerable: false,// 不可枚举 ，默认false
    configurable: false // 不可删除
})
console.log(Singer.hot);
console.log(Object.keys(Singer));

```

### 拷贝

#### 浅拷贝

**本质**：浅拷贝创建了一个新的对象或数组，将原始对象或数组的属性或元素复制到新对象中。然而，对于原始对象或数组中的嵌套对象或数组，浅拷贝只会复制它们的引用，而不会递归地复制它们的内容。这意味着浅拷贝后的新对象仍然与原始对象共享嵌套对象。

Object.assign(target, ...sources)

- `target`：目标对象：即载体，接收复制的属性。
- `sources`：样本：复制的样本、可以是多个对象、数组。（后面的覆盖前面的，相同属性只保留一个）

`Object.assign()` 方法会返回更新的目标对象。
`Object.assign()` 可以用于创建对象的副本或组合对象的属性。

```javascript
// 使用对象展开运算符（...）
const originalObj = { a: 1, b: 2 };
const copiedObj = { ...originalObj };

// 使用 Object.assign() 方法创建副本
const originalObj = { a: 1, b: 2 };
const copiedObj = Object.assign({}, originalObj);

// 组合 obj1 和 obj2 属性
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3 };
const obj3 = Object.assign({}, obj1, obj2);
console.log(obj4); // { a: 1, b: 2, c: 3 }

// 后面的对象属性值 将覆盖前面的对象属性值，相同属性只保留一个
const obj4 = {a: 1,b: 2, c: 3, d: 4 };
const obj5 = { c: 3, d: 5,e: 6 };
const obj6 = Object.assign({}, obj4, obj5);
console.log(obj6); // { a: 1,b: 2, c: 3, d: 5,e: 6 }

// Object.assign() 方法只能复制可枚举的自有属性。它不会复制对象的继承属性和不可枚举属性。
const obj1 = { a: 1 };
const obj2 = Object.create(obj1, {
  b: {
    value: 2,
    enumerable: false
  },
  c: {
    value: 3,
    enumerable: true
  }
});
const properties = Object.getOwnPropertyNames(obj2).concat(Object.getOwnPropertySymbols(obj2));
const obj3 = Object.assign({}, obj2, ...properties.map(prop => ({ [prop]: obj2[prop] })));

console.log(obj3); // { c: 3, b: 2 }
```

#### 深拷贝

**定义**：深拷贝会递归地复制对象和数组中的所有嵌套对象和数组，直到所有属性都是基本类型为止。

```javascript
/**
 * 深拷贝：全部层次拷贝，不是拷贝地址
 * 
 * 手写注意：先判断数组，因为数组是也是对象类型
 */
// 递归函数实现
let o = {a:1,b:{c:1}}
let obj = {}
function deepCopy(newObj, oldObj) {
    for (let k in oldObj) {
        // 判断数据类型?数组、对象、......
        let item = oldObj[k]
        if (item instanceof Array) {
            newObj[k] = [];
            deepCopy(newObj[k], item)
        } else if (item instanceof Object) {
            newObj[k] = {};
            deepCopy(newObj[k], item)
        } else {
            newObj[k] = item;
        }
    }
}
deepCopy(obj, o)

// 使用递归函数
function deepClone(object) {
  if (object === null || typeof object !== 'object') {
    return object;
  }
  const result = Array.isArray(object) ? [] : {};
  for (const key in object) { 
    if (Object.hasOwnProperty.call(object, key)) { // hasOwnProperty只复制对象自身的属性，而不是继承的属性。
      result[key] = deepClone(object[key]);
    }
  }
  return result;
}
const obj1 = {
  a: 1,
  b: {
    c: 2,
    d: [3, 4],
  },
};

const obj2 = deepClone(obj1);

console.log(obj2); // { a: 1, b: { c: 2, d: [3, 4] } }

// 使用第三方库
const _ = require('lodash');
const originalObj = { a: 1, b: { c: 2 } };
const copiedObj = _.cloneDeep(originalObj);  // Lodash 库


// 使用JSON.parse和JSON.stringify进行深拷贝
const originalObject = { a: 1, b: { c: 2 } };
const deepCopyObject = JSON.parse(JSON.stringify(originalObject));
```



# DOM

## DOM基本操作

### 1.获取元素

| 获取DOM元素                                | 描述                                       |
| ------------------------------------------ | ------------------------------------------ |
| `document.getElementById('id')`            | 根据id获取，返回一个对象                   |
| `document.getElementsByTagName('TagName')` | 根据标签名获取，返回一个伪数组（object）   |
| `getElementsByClassName('className')`      | 根据标签类名获取，返回一个伪数组（object） |
| `document.querySelector('')`               | 返回第一个匹配的指定选择器的对象           |
| `document.querySelectorAll()`              | 返回全部匹配的指定选择器的对象             |
| `document.body`                            | 获取body                                   |
| `document.documentElement`                 | 获取html                                   |

```html
<div id="content">盒子1</div>
<div class="box">盒子1</div>
<div class="box">盒子2</div>
<p>测试内容</p>
<p>测试内容</p>
<ul>
    <li>hh</li>
    <li>hh</li>
    <li>hh</li>
    <li>hh</li>
</ul>
<script>
    // 1. 根据【id】获取，返回一个对象
    var div1 = document.getElementById('id');
    // 2. 根据【标签名】获取，返回一个伪数组（object）
    var p = document.getElementsByTagName('p');
    // 3. 根据【标签类名】获取，返回一个伪数组（object）
    var lis = getElementsByClassName('box');
    // 4. 返回第一个匹配的指定选择器的对象
    var firstLi = document.querySelector('li');
    // 5. 返回全部都匹配的指定选择器的对象
    var allLi = document.querySelectorAll('li');
    // 获取body
    var bodyEle = document.body;
    // 获取html
    var htmlEle = documentElement
</script>
```

### 2.事件

***事件三要素 ？***：1.事件源；2.事件类型；3.事件处理程序
***事件执行步骤 ？****：1.获取事件源；2.绑定事件、注册事件；3.添加事件处理程序
**常用事件 ？***：点击事件`onclick`、鼠标经过事件`onmouseover`、鼠标离开事件`onmouseout`

```html
<button id="btn">周杰伦</button>
<script>
    // 事件三要素 
    // 1. 事件源（获取事件源）
    var btn = document.getElementById('btn'); // 获取事件源
    // 2. 事件类型（绑定事件、注册事件））
    btn.onclick = function () { // 点击事件
        // 3. 事件处理程序（添加事件处理程序）
        alert('一首“晴天”送给你'); // 弹出框
    }
</script>
```

------

### 3.操作元素

| 文本内容      | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| ele.innerHTML | 可获取标签文本、修改文本、识别`html`标签，保留空格和换行 包括html标签 |
| ele.innerText | 可获取标签文本、修改文本，去除空格和换行                     |

```javascript
div.innerHTML = '<strong>点击了</strong>';
div.innerText = '<strong>点击了</strong>';
```

| 元素属性                  | 描述                                               |
| ------------------------- | -------------------------------------------------- |
| ele.className             | 通过类名操作样式，会覆盖原有类名，多类名用空格隔开 |
| ele.style.backgroundColor | 直接操作样式，类似行内样式修改                     |
| ele.title                 |                                                    |
| ele.src                   |                                                    |

```javascript
img.src = 'images/1.png'; // 操作src属性
img.title = '骗你的'; // 操作title属性
this.className = 'bgc';
this.style.backgroundColor = '#ee0000'; // 直接操作样式
this.style.width = '100px';
```

| 表单属性     | 描述 |
| ------------ | ---- |
| ele.value    |      |
| ele.disabled |      |

```javascript
input.value = '点了我，就不能点了';
btn.disabled = true; // 禁用按钮
```

------

### 4.自定义属性操作

PS：H新增

```html
<div id="demo" index ="1" class="nva" >我是div</div>
<script>
    var div = document.getElementById('demo');
    /* -------------------【获取属性】----------------------
     1. 获取内置属性值： element.属性 
     2. 获取自定义属性值： element.getAttribute('属性');
    */
    console.log(div.id); // demo
    console.log(div.getAttribute('index')); // 1
    /* -------------------【设置属性值】---------------------
     (1) element.属性 = '值';
     (2) element.setAttribute('属性','值'); 针对自定义属性
    */
    div.id = 'text';
    div.className = 'nvas'; 
    div.setAttribute('index', 2);
    div.setAttribute('class','nvass'); // class特殊
    /* -------------------【移除属性】-----------------------
     (1) removeAttribte('属性');
    */
    div.removeAttribute('index');
</script>


<div class="box" data-index="7" data-list-name="yzling">我是个div</div>
<script>
	/* -------------------【自定义属性命名规范】-----------------------
        规范： data- 开头
     1. 设置(添加)自定义属性
     2. 获取自定义属性
     3. H5 自定义属性 新增获取方法 ie11以上
    */
    var div = document.querySelector('div');
    // 1.
    div.setAttribute('data-time', 20);
    // 2. 
    div.getAttribute('data-time'); // 20
	div.getAttribute('data-list-name'); // 20
    // 3.
    div.dataset; // data-开头的自定义属性集合
    div.dataset.index; // 7
	div.dataset['index']; // 7
	div.dataset.listName; // yzling 驼峰命名法
	div.dataset['listName']; // yzling 
</script>
```

------

### 5.创建元素

***创建元素的三种方式？***：1.`document.writer()`；2.`innerHtml`；3.`document.createElement()`。

```html
<button>点</button>
<p>abc</p>
<div class="inner"></div>
<div class="create"></div>
<script>
    // 方式一
    // 方式二
    // 方式三
</script>
```

```javascript
// 【方式一】 document.writer() 创建标签
document.write('<div>123</div>');
// （1). 当文档流执行完毕，重绘页面（替换当前的html元素）
var btn = document.querySelector('button');
btn.onclick = function () {
    document.write('<div>123</div>'); // 创建元素
}
// （2). html执行完毕就加载js，重绘页面
window.onload =function(){
    document.write('<div>123</div>'); // 创建元素
}
```

```javascript
// 【方式二】 innerHtml 
var inner = document.querySelector('.inner');
// (1). 创建一次 
inner.innerHTML = '<a href="#">链接</a>';  // 创建元素
// (2). 100次 拼接字符串时间久（会吃性能）
for (var i = 0; i < 100; i++) { // 创建100次
    inner.innerHTML += '<a href="#">链接</a>'; // 创建元素
}
```

```javascript
// 【方式三】 document.createElement(); 
var create = document.querySelector('.create');
// (1). 创建一次 
var a = document.createElement('a'); // 创建元素
create.appendChild(a);
// (2). 100次
for (var i = 0; i < 100; i++) {
    var a = document.createElement('a'); // 创建元素
    create.appendChild(a);
}
```

```javascript
// 4. 运行时间对比
function fn() {
    var d1 = + new Date(); // 计时开始
    
    // 1. innerHTML（低）
    // var str = '';
    // for (var i = 0; i < 1000; i++) {
    //     document.body.innerHTML += '<div style="width:100px; height:2px; border: 1px solid blue;"></div>';
    // }
    
    // (1). innerHTML改进(最高效率)
    var inner = document.querySelector('.inner');
    var arr = [];
    for (var i = 0; i < 100; i++) {
        arr.push('<div style="width:100px; height:2px; border: 1px solid blue;"></div>')
    }
    inner.innerHTML = arr.join('');
    
    // // 2. createElement（次）
    // for (var i = 0; i < 1000; i++) {
    //    var div = document.createElement('div');
    //    div.style.width = '100px';
    //    div.style.height = '2px';
    //    div.style.border = '1px solid blue';
    //    document.body.appendChild(div);
    // }
    var d2 = + new Date(); // 计时结束
    console.log(d2 - d1);
}
fn();
```











## 其它

```javascript
console.dir(elementObj); // 查看对象的属性和方法
// 什么是重绘
```





# 问题

1.  js代码的延迟加载？
2.  js的数据类型判断？
3.  null和undefined区别？
4.  等于和全等于区别？
5.  微任务和宏任务？
6.  变量提升？
7.  作用域？
8.  数组的元素的查找、判断？
9.  原型链?
10.  数组、增、删、查（元素、最大值等）、改、去重、？
11.  数组排序？
12.  字符串、去除空格、连接？
13.  字符串字符出现次数、统计次数？
14.  new操作符做了什么？
15.  闭包？
16.  js继承方式？
17.  改变this的方法、区别？
18.  深拷贝、浅拷贝（递归函数）？
19.  本地存储、会话存储？
20.  var、let、const
21.  对象合并
22.  箭头函数、普通函数？
23.  promise？（几种状态）
24.  find、filter区别？
25.  some、every区别？









































