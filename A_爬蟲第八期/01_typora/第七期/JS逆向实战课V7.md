

# JavaScript高级逆向V7

# 一、JavaScript基础与进阶

## 1.1、JavaScript基础语法

### 【1】JavaScript初识

 JavaScript 是一种用于在客户端（即用户的浏览器）上运行的编程语言。它是一种脚本语言，可以直接嵌入到 HTML 页面中，并由浏览器解释和执行。

在客户端环境中，JavaScript 可以与用户交互，操作和修改网页的内容、结构和样式。它可以用于处理用户输入、响应事件、进行表单验证、创建动态效果等。JavaScript 还可以通过浏览器提供的 API 访问浏览器功能，例如操作 DOM（文档对象模型）、发送网络请求、处理本地存储等。

需要注意的是，JavaScript 也可以在服务器端运行，例如使用 Node.js 环境。在服务器端，JavaScript 可以用于构建网络应用、处理后端逻辑等。但在这种情况下，它并不是典型的“客户端语言”，而是作为一种通用的脚本语言使用。

**demo服务端：**

```python
from flask import Flask, render_template, jsonify
import datetime

app = Flask(__name__, template_folder="templates", static_folder='static')


@app.get("/index")
def index():
    timer = datetime.datetime.now().strftime("%Y-%m-%d:%X")
    return render_template("index.html", **{"timer": timer})

@app.get("/books")
def get_books():
    # 数据库获取书籍
    books = ["聊斋志异", "金瓶梅", "国色天香", "剪灯新话"]
    return jsonify(books)

app.run()

```

**demo客户端：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        p {
            color: rebeccapurple;
        }
    </style>
</head>
<body>

<h3>Hello Alex!</h3>
<p>当前时间：{{timer}}</p>
<p class="books">四大名著</p>
<p class="books_box"></p>

<script>
    var dom = document.getElementsByTagName("h3")[0]
    dom.onclick = function () {
        this.style.color = "green"
    }
</script>
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.7.1/jquery.js"></script>
<script>
    $(".books").click(function () {
        $.ajax({
            url: "http://127.0.0.1:5000/books",
            success: function (res) {
                console.log("响应数据:", res);
                $(".books_box").append(res.join("，"))
            }
        })
    })
</script>

</body>
</html>
```

登陆页面：

```html
<!DOCTYPE html>
<html>
<head>
  <title>Login</title>
    <meta charset="utf8">
  <script>
    function validateForm() {
      var username = document.getElementById("username").value;

      // 验证用户名是否为有效的电子邮件地址或手机号码
      var emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      var phoneRegex = /^\d{10}$/;
      if (!emailRegex.test(username) && !phoneRegex.test(username)) {
        alert("请输入有效的电子邮件地址或手机号码作为用户名！");
        return false; // 验证失败，阻止表单提交
      }
    }
  </script>
</head>
<body>
  <h1>Login</h1>
  <form onsubmit="return validateForm()">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>
    <br><br>
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required>
    <br><br>
    <input type="submit" value="Login">
  </form>
</body>
</html>
```

### 【2】基本语法

```js
// JS代码导入方式：
// 1 直接编写
   /*
    <script>
        console.log('hello yuan')
    </script>
    */
// 2 导入文件

// <script src="hello.js"></script>


// 单行注释
/*
    这是
    多行
    注释
        */
     
// 语句分隔符 ; 表示一行代码的结束
```

### 【3】变量与数据类型

```js
// (1) 声明变量
var x;  // 声明一个变量叫x, x没有值. 此时打印x的内容是undefined(没有被定义)
x = 10
console.log(x);

// (2) 基本数据类型：number string  bool

var x = 3.1415; 
console.log(typeof x); // 可以查看数据类型. 但是只能看基础数据类型.

var x = "hello yuan";
console.log(typeof x); // string 字符串

var x1 = true;
var x2 = false; // 布尔值. python里面是大写的. js里面是小写.

console.log(typeof x1); // boolean 布尔. python简化成bool

// (3) 高级数据类型：  数组[]    对象{}    

var x = [1,2,3]; // 数组. => python的列表.
console.log(typeof x);  // 类型叫object

var x = {name:"yuan",age:22}; // 对象 => python的字典.
console.log(typeof x); // 也是object

// (4) 两个特殊值:   undefined null

var x; // undefined
console.log(typeof x);  // undefined类型... 不占用内存空间的

var x = null; // 空.... 是python的None, 要使用堆内存的....
console.log(typeof x); // object类型

// 一次性可以声明多个变量
var name="yuan", age=20, job="lecturer";
var a, b, c=3, d, e = 5; // 从左到右,声明5个变量, 其中, c=3, e=5

```

类型转换：
```js
// 在js中. 所有的数据和字符串相加. 结果都是字符串拼接.(重点)
var a = 123;
var b = "我爱你";
console.log(a + b);
console.log(1+1); // 2

var a = "123"; // 字符串
// 字符串转化成整数
// parse 转换
// Int 整数
a = parseInt(a);
console.log(a + 3); // 126 数学上的加法. 证明转化成功了.

var a = 123; // 转化成字符串
var b = a.toString(); // 把数字, 转化成字符串
console.log(b + 3); //"1233"

// 一般的程序员不用toString()的.
var a = 123;
var b = a + ""; // 在逆向的过程中. 这种写法是最多的.
console.log(b+333); // "123333" 字符串


var a = 123;
// 数字的toString()可以指定进制
var b = a.toString(16);
console.log(b);

var m = '7b'; // 眼睛看着是字符串. 但是我们知道它应该是一个十六进制的字符串.
// 字符串, 转化成整数   parseInt
var n = parseInt(m, 16);
console.log(n)
```

### 【4】运算符

```js
// （1） 科学运算符
 + - * / %

// （2）比较运算符
> <   <=  >= === !==

// （3） 赋值运算符 
+= -= *= /=  ++

// 在单独使用a++和++a是没有区别的. 都是让a自增1
var b = a ++;  // 先赋值, 后运算...

console.log(a); // 11
console.log(b); // 10

var b = ++ a; // 先计算, 后赋值
console.log(a);
console.log(b);

// （4）逻辑运算符： Python中的and, or, not
// &&, 并且, 左右两端必须同时为真, 结果才能是真..
// ||, 或者, 左右两端, 有一个是真, 结果就是真
// !, 非/不, 非真即假, 非假既真.

console.log(1 > 2 || 3 < 5);
console.log(1 > 2 && 3 < 5);

console.log(!true);
console.log(!false);

// x() && y()
// 该逻辑叫短路...如果前面的表达式已经得到结果了. 后面就不运行了. 相同的逻辑还有||
// 如果x()返回真. 则运行y()
// 如果x()返回假, 则不运行y(). 直接结束判断了.


// （5）逗号运算符
a = 1,2,3,4
b = (1,"yuan",true)
// （6）三目运算：条件? 成立: 不成立
// 案例1
var a = 100;
var b = 20;
var c = a > b ? a: b;
console.log(c);
// 案例2:终极一练

let a = 10;
let b = 20;
let c = 5;
let d = 17;

let e;
let m;
e = (e = a > 3 ? b : c, m = e < b++ ? c-- : a = 3 > b % d ? 27: 37, m++);
// e = a > 3 ? b : c; // e = 20
// b = 21 , m 和 a都是37
// m = e < b++ ? c-- : a = 3 > b % d ? 27: 37;
// e = m++; // e 37  m 38  a 37
```

### 【5】流程控制语句

分支语句

```js
// 分支语句
if(条件){
     // 条件为true时,执行的代码
   }else{
     // 条件为false时,执行的代码
 }  

switch(条件){
    case 结果1:
         // 满足条件执行的结果是结果1时,执行这里的代码..
         break;
    case 结果2:
         // 满足条件执行的结果是结果2时,执行这里的代码..
         break;
    ...
    default:
         // 条件和上述所有结果都不相等时,则执行这里的代码
   }


// （1）双分支
var age = 23
if (age > 18) {
    console.log("播放成人电影！")
} else {
    console.log("播放未成年电影")
}
// （2）多分支
var score = 45;
if (score >= 90) {
    console.log("A");
} else if (score >= 80) {
    console.log("B");
} else if (score >= 70) {
    console.log("C");
} else if (score >= 60) {
    console.log("D");
} else {
    console.log("E");
}
// （3）switch多分支
var week = 3;
switch(week){
    case 1:
        console.log("星期一");
        break;
    case 2:
        console.log("星期二");
        break;
    case 3: // 某一个case匹配成功. 那么后面的case就不判断了, 直接被执行.
        console.log("星期三");
        break;
    case 4:
        console.log("星期四");
        break;
    case 5:
        console.log("星期五");
        break;
    case 6:
        console.log("星期六");
        break;
    case 7:
        console.log("星期天");
        break;
    default:
        console.log("啥也不是!");
        break;
}
```

循环语句

```js
/*

// 循环语句
while(循环的条件){
      // 循环条件为true的时候,会执行这里的代码
   }
   
// 循环三要素
for(1.声明循环的开始; 2.条件; 4.循环的计数){
   // 3. 循环条件为true的时候,会执行这里的代码
} 
 
 * 1. 首先, 会执行语句1, 通常会在语句1里完成对循环变量的初始化
 * 2. 然后, 判断表达式2计算的结果是否是真, 如果是真, 则执行循环体, 如果是假, 结束循环
 * 3. 执行完循环体后. 执行表达式3，然后回到第二步...
 
 */

// while 循环
var a = 0; // 1. 初始变量
while (a < 100) {  // 2. 条件判断
    console.log("我爱你");  // 业务需求...循环内容...
    a++;  // 3. 步进语句
}


for(var i = 0; i < 10; i++){
    console.log(i);
}

```

### 【6】字符串对象

```js
// js的字符串对象
var s = "Hello Yuan      "
console.log(s.toUpperCase())
console.log(s.toLowerCase())
console.log(s.slice(0, 5))
console.log(s.split(" ")) // ["Hello","Yuan"]
console.log(s.length)
console.log(s.trim(" ").length)
console.log(s.trimEnd(" ").length)
console.log(s.indexOf("Yuan"))
console.log(s.replace("Yuan","Rain"))
console.log(s.startsWith("Hel"))
console.log(s.charCodeAt(i)) // 某个位置的字符的ascii
console.log(s.charAt(i)) //  第i索引位置的字符  s[i]
```

### 【7】数组对象

```js
// 注意, 前端js里面的数组. 相当于python的列表, java的List

// 创建数组的方法;
// 1. 直接赋值一个数组
var arr1 = [11,22,33];
console.log(arr1);

// new表示创建对象.  理解成分配内存.
var arr2 = new Array();  // var arr2 = []

// 1. push 在数组的末尾添加数据...
// 2. pop 在数组的末尾删除数据
var arr = [11,22,33,44];
arr.push(55);
var item = arr.pop();
console.log(item);


// 1 unshift 在数组的首位添加数据...  2 shift 删除数组的首位数据

var arr = [11,22,33];
arr.unshift(0); // 在数组的前面插入数据
// console.log(arr);
var item = arr.shift();
console.log(item);
console.log(arr);

var arr = ["a","b","c"];
arr.splice(1,1);
arr.splice(1,0,"b")
arr.splice(1,1,"B")

arr.reverse();

// slice(开始下标,结束下标)  切片,开区间
arr.slice(1,3)

var arr = ["北京","深圳","广州"];
console.log(arr.join(",")); // 把数组转化成字符串

// 循环和遍历
//  key       0         1       2         3          4         5
var arr = ["高圆圆", "赵丽颖", "范冰冰", "波多野结衣", "森泽佳奈", "橘梨纱"];

// 三要素for循环
for(var i = 0; i < arr.length; i++){
    console.log(arr[i]);
}

// for-in
for(var x in arr){ // 此时拿到的是数组的索引(key)
    console.log(x);
}

// for-of
for (var x of arr){ // 此时拿到的是数组的元素(value)
    console.log(x);
}

// arr中的每一项循环出来. 分别去调用function函数, 会自动的将`数据`传递给函数的第一个参数
arr.forEach(function(e, i){  // 第二个参数是可选的
    console.log(i+"__"+e);
});

// arr中的每一个元素执行里面传递进去的函数
// 回调函数的参数是 元素和索引

console.log(ret);

// 高阶函数 map filter
```

### 【8】object对象

```js
var obj = {
          "name": "yuan",
          age: 18,
          girl_friend: {
              name: "高圆圆",
              age: 28
          }
      }

// 当他是python的字典, 无缝衔接的. `[]` 也可以翻译成`的`
console.log(obj['girl_friend']['name']);
// 当他是对象... 对象.属性  `.`可以翻译成`的`
console.log(obj.name)
console.log(obj.girl_friend.name)

// 它俩可以混着来.
var w = "girl_friend"
console.log(obj[w].name);
console.log(obj.girl_friend['name']);
// js的对象在使用的时候. 既可以通过`[]`来获取属性的值. 也可以通过`.`来获取属性的值
// 区别: []里面必须放字符串.
//       .后面放的必须是属性名(不可以是字符串)
console.log(obj.girl_friend.name); // 68

// 如果想要对代码进行伪装,混淆. 就不能用`.`

console.log(obj['girl_friend']['name']); // 高圆圆
console.log(obj.girl_friend.name); // 高圆圆


var obj = {};
obj.name = "alvin";  // 对象.属性 = 值. 可以给对象增加新的属性(设置一个新的属性)
obj['age'] = 28;

console.log(obj);
```

序列化和反序列化：

| 方法                      | 描述                                          |
| ------------------------- | --------------------------------------------- |
| **`JSON.stringify(obj)`** | 把obj对象转换成json格式字符串，会移除对象方法 |
| **`JSON.parse(str)`**     | 把符合json语法的字符串转换成js对象            |

### 【9】其它常用对象

```js
// Math对象 Date类
var now=new Date();
console.log(now.toLocaleString( ));

// console.log(Math.random());
console.log( Math.round(Math.random() * 10 ));
```

## 1.2、函数

### 【1】函数初识

```js
// 函数声明
function 函数名(形参){
   return // 返回值
}

// 函数调用
函数名(实参)

// 编译运行
// 默认返回undefined
// arguments参数

```

### 【2】作用域

作用域（Scope）是指在程序中定义变量的可访问性和可见性的范围。它决定了在代码中的哪些位置可以访问或引用特定变量、函数或对象。

在大多数编程语言中，包括 JavaScript，在不同的作用域中定义的变量具有不同的可见性和生命周期。常见的作用域类型有以下几种：

1. 全局作用域（Global Scope）：全局作用域是在整个程序中都可访问的作用域。在全局作用域中声明的变量可以在程序的任何位置被访问。
2. 函数作用域（Function Scope）：函数作用域是在函数内部定义的作用域。在函数作用域中声明的变量只能在函数内部被访问，它们对于函数外部是不可见的。每当函数被调用时，都会创建一个新的函数作用域。
3. 块级作用域（Block Scope）：块级作用域是在代码块（通常由花括号 `{}` 包围）内定义的作用域。在块级作用域中声明的变量只能在该块内部被访问，而在块外部是不可见的。在 ES6 中引入的 `let` 和 `const` 关键字可以用来创建块级作用域。

```js
//  首先熟悉下var

var name = "yuan"; // 声明一个全局变量 name并赋值”yuan“
name = "张三";  // 对已经存在的变量name重新赋值 ”张三“
console.log(name);

age = 18   // 之前不存在age变量，这里等同于var age = 19 即声明全局变量age并赋值为18

var  gender = "male"
var  gender = "female" // 原内存释放与新内存开辟，指针指向新开辟的内存
console.log(gender)
```

作用域案例：

```js
var num = 10; // 在函数外部声明的变量, 全局变量
function func(){
    // num = 20; // 函数内部直接使用变量,则默认调用了全局的变量,
    //var num = 20;                  
    console.log("函数内部num：",num)
}
func();
console.log("全局num：",num);
```

作用域的正确使用可以避免变量名冲突和数据泄漏，并提供更好的代码封装和隔离性。理解作用域的概念对于编写可维护和可扩展的代码非常重要。

### 【3】匿名函数

在 JavaScript 中，匿名函数是一种没有名称的函数定义。匿名函数可以被直接传递给其他函数作为参数，也可以被赋值给变量或对象的属性，以便稍后调用。

```js
// 匿名函数赋值变量
 var foo = function () {
     console.log("这是一个匿名函数！")
 };

// 匿名函数的自执行
(function (x,y) {
     console.log(x+y);
 })(2,3)

// 匿名函数作为一个高阶函数使用
function bar() {

  return function () {
      console.log("inner函数！")
  }
}

bar()()
```

匿名函数常用于回调函数、事件处理程序、异步操作等场景，它们允许我们在需要时动态定义函数逻辑，并将其作为值传递或存储，而不需要为函数命名。这提供了更灵活和简洁的编程方式。

### 【4】闭包函数

闭包（closures）是 Javascript 语言的一个难点，也是它的特色，很多高级应用都是依靠闭包实现的。闭包与变量的作用域以及变量的生命周期密切相关，本节我们就来简单介绍一下。

> 在计算机科学中，闭包（英语：Closure），又称词法闭包（Lexical Closure）或函数闭包（function closures），是引用了自由变量（外部非全局）的函数。

简单来说就是一个函数定义中引用了函数外定义的变量，并且该函数可以在其定义环境外被执行。这样的一个函数我们称之为闭包函数。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script>

        function getCounter() {

            let count = 0

            function counter() {
                let name = "yuan"
                count++  // count = count+1
                return count
            }

            window.yuan = counter

            // counter()
            // counter()
            // counter()
            return counter
        }

        // c1 = getCounter()
        // c1()
        // c1()
        // c2 = getCounter()
        // c2()
        // c2();

        getCounter()

        let c3 = window.yuan
        console.log(c3())
        console.log(c3())
        console.log(c3())
    </script>
    <script>
        var count = 100
    </script>
</head>
<body>

<button onclick="c1()">add1</button>
<button onclick="c2()">add2</button>
</body>
</html>
```

### 【5】ES6中的箭头函数

在ES6中简化了函数的声明语法. 

```js
// es6允许使用“箭头”(=>)定义函数。
var f = v => v*v
// 等同于
var f = function(v) {
 return v
}

// 如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
var f = () => 5
// 等同于
var f = function() {
   return 5
}
 
var sum = (num1, num2) => num1 + num2
// 等同于
var sum = function(num1, num2) {
   return num1 + num2
}

// 如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
var sum = (num1, num2) => {return num1 + num2}

//由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。

// 报错
let getUser = id => {id: id, name: "yuan"}
// 不报错
let getUser = id => ({id: id, name: "yuan"})


// 箭头函数的一个用处是简化回调函数。
// 正常函数写法
[1, 2, 3].map(function(x) {
 return x * x
})
// 箭头函数写法
[1, 2, 3].map(x => x * x)
```

### 【6】导入模块

当涉及到实际案例时，我将给出一个常见的示例，以说明如何在 JavaScript 中使用 `require` 或 `import` 导入模块。

假设我们有两个文件：`math.js` 和 `main.js`。

首先，让我们创建 `math.js` 文件，它包含一个简单的数学函数：

```js
// math.js
const add = (a, b) => {
  return a + b;
};

const multiply = (a, b) => {
  return a * b;
};

module.exports = {
  add,
  multiply
};
```

在 `math.js` 中，我们定义了两个函数 `add` 和 `multiply`，并使用 `module.exports` 导出这些函数，使其可以在其他文件中使用。

接下来，我们创建 `main.js` 文件，它将使用 `math.js` 中的函数：

使用 `require`（CommonJS 模块系统）的示例：

```js
// main.js
const math = require('./math');

console.log(math.add(2, 3)); // 输出：5
console.log(math.multiply(2, 3)); // 输出：6
```

在这个示例中，我们使用 `require` 函数将 `math.js` 文件导入到 `main.js` 文件中。`require('./math')` 表示我们导入与 `main.js` 文件相同目录下的 `math.js` 文件。

然后，我们可以使用 `math.add()` 和 `math.multiply()` 调用 `math.js` 中导出的函数。

使用 `import`（ES 模块系统）的示例：

```js
function add(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

export default {
    add: add,
    multiply: multiply,
}
```

```js
// main.js
import cal from './cal.js'

console.log(cal.add(1,2))
console.log(cal.multiply(1,2))
```

```html
 <script type="module" src="main.js"></script>
```

## 1.3、Window对象

window 是客户端浏览器对象模型的基类，window 对象是客户端 JavaScript 的全局对象。一个 window 对象实际上就是一个独立的窗口，对于框架页面来说，浏览器窗口每个框架都包含一个 window 对象。

### 【1】全局作用域

在客户端浏览器中，window 对象是访问 BOM 的接口，如引用 document 对象的 document 属性，引用自身的 window 和 self 属性等。同时 window 也为客户端 JavaScript 提供全局作用域。

由于 window 是全局对象，因此所有的全局变量都被解析为该对象的属性。

```html
<script>

    var username = "yuanhao";  //全局变量
    function f() {  //全局函数
        console.log(username);
    }
  
    console.log(window.username);  //返回字符串“yuanhao”
    window.f();  //返回字符串“yuanhao”

</script>


```

### 【2】使用系统对话框

window 对象定义了 3 个人机交互的方法，主要方便对 JavaScript 代码进行调试。

- alert()：确定提示框。由浏览器向用户弹出提示性信息。该方法包含一个可选的提示信息参数。如果没有指定参数，则弹出一个空的对话框。
- confirm()：选择提示框。。由浏览器向用户弹出提示性信息，弹出的对话框中包含两个按钮，分别表示“确定”和“取消”按钮。如果点击“确定”按钮，则该方法将返回 true；单击“取消”按钮，则返回 false。confirm() 方法也包含一个可选的提示信息参数，如果没有指定参数，则弹出一个空的对话框。
- prompt()：输入提示框。可以接收用户输入的信息，并返回输入的信息。prompt() 方法也包含一个可选的提示信息参数，如果没有指定参数，则弹出一个没有提示信息的输入文本对话框。

### 【3】使用定时器

window 对象包含 4 个定时器专用方法，说明如下表所示，使用它们可以实现代码定时执行，或者延迟执行，使用定时器可以设计演示动画。

| 方法            | 说明                                             |
| --------------- | ------------------------------------------------ |
| setInterval()   | 按照执行的周期（单位为毫秒）调用函数或计算表达式 |
| setTimeout()    | 在指定的毫秒数后调用函数或计算表达式             |
| clearInterval() | 取消由 setInterval() 方法生成的定时器            |
| clearTimeout()  | 取消由 setTimeout() 方法生成的定时器             |

setTimeout() 方法能够在指定的时间段后执行特定代码。用法如下：

`var o = setTimeout(code, delay)`

参数 code 表示要延迟执行的字符串型代码，将在 Windows 环境中执行，如果包含多个语句，应该使用分号进行分隔。delay 表示延迟时间，以毫秒为单位。

该方法返回值是一个 Timer ID，这个 ID 编号指向延迟执行的代码控制句柄。如果把这个句柄传递给 clearTimeout() 方法，则会取消代码的延迟执行。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<input id="ID1" type="text" >
<button onclick="begin()">开始</button>
<button onclick="end()">停止</button>

<script>
    function showTime(){
           var nowd2=new Date().toLocaleString();
           var temp=document.getElementById("ID1");
           temp.value=nowd2;

    }
    var ID;
    function begin(){
        if (ID==undefined){
             showTime();
             ID=setInterval(showTime,1000);
        }
    }
    function end(){
        clearInterval(ID);
        ID=undefined;
    }

</script>

</body>
</html>
```

### 【4】访问客户端对象

使用 window 对象可以访问客户端其他对象，这种关系构成浏览器对象模型，window 对象代表根节点，浏览器对象关系的关系如图所示，每个对象说明如下。

- window：客户端 JavaScript 顶层对象。每当 <body> 或 <frameset> 标签出现时，window 对象就会被自动创建。
- navigator：包含客户端有关浏览器信息。
- screen：包含客户端屏幕的信息。
- history：包含浏览器窗口访问过的 URL 信息。
- location：包含当前网页文档的 URL 信息。
- document：包含整个 HTML 文档，可被用来访问文档内容及其所有页面元素。

<img src="./assets/image-20240528194905528-6896946.png" alt="image-20240528194905528" style="zoom:50%;" />

[window对象树](https://www.processon.com/view/link/66497878cfe67e27899b9e70?cid=66488d812b49420be3452fa3)

# 二、JS逆向必备知识点



## 2.1、编码

### 【1】url编码

```python
import urllib.parse 

# s = 'a'
s = ' 123'
ret = urllib.parse.quote(s)
print(ret)
s = urllib.parse.unquote(ret)
print(s)

params = {'name': '张三', 'age': 20, 'address': '北京市海淀区'}
query_string = urllib.parse.urlencode(params)
print(query_string)

query_string = 'name=%E5%BC%A0%E4%B8%89&age=20&address=%E5%8C%97%E4%BA%AC%E5%B8%82%E6%B5%B7%E6%B7%80%E5%8C%BA'
params = urllib.parse.parse_qs(query_string)
print(params, type(params))
```

### 【2】 base64编码

#### （1）base64是什么

Base64编码，是由64个字符组成编码集：**26个大写字母A~Z，26个小写字母a~z，10个数字0~9，符号“+”与符号“/”**。Base64编码的基本思路是**将原始数据的三个字节拆分转化为四个字节**，然后根据Base64的对应表，得到对应的编码数据。

当原始数据凑不够三个字节时，编码结果中会使用额外的**符号“=”**来表示这种情况。

#### （2）base64原理

![image-20230916190327552](./assets/image-20230916190327552-4862209-7334392.png)

一个Base64字符实际上代表着6个二进制位(bit)，4个Base64字符对应3字节字符串/二进制数据。

3个字符为一组的的base64编码方式如：

![image-20230427174804644](./assets/image-20230427174804644-4862253-4862254-7334392.png)

小于3个字符为一组的编码方式如：

![image-20230427174356120](./assets/image-20230427174356120-4862276-4862277-7334392.png)

总结：base64过程

![v2-05db3ee40c22b15789911f2d57fe3d6f_720w-2589515](./assets/v2-05db3ee40c22b15789911f2d57fe3d6f_720w-2589515-4862303-4862305-7334392.jpg)

最后处理完的编码字符再转字节中不再有base64以外的任何字符。

#### （3）base64测试

```python
import base64

bs = "you".encode("utf-8")
# 把字节转化成b64
print(base64.b64encode(bs).decode())

bs = "yo".encode("utf-8")
# 把字节转化成b64
print(base64.b64encode(bs).decode())

# 猜测结果
bs = "y".encode("utf-8")
# 把字节转化成b64
print(base64.b64encode(bs).decode())


```

注意, b64处理后的字符串长度. 一定是4的倍数. 如果在网页上看到有些密文的b64长度不是4的倍数. 会报错

例如, 

```python
import base64

s = "eW91"
ret = base64.b64decode(s)
print(ret)

s = "eW91eQ=="
ret = base64.b64decode(s)
print(ret)

s = "eW91eQ"
ret = base64.b64decode(s)
print(ret)
```

解决思路. base64长度要求. 字符串长度必须是4的倍数. 填充一下即可

```python
s = "eW91eQ"
# ret = base64.b64decode(s)
# print(ret)

s += ("=" * (4 - len(s) % 4))
print("填充后", s)
ret = base64.b64decode(s).decode()
print(ret)
```

#### （4）base64变种

```python
# 方式1
data = res.text.replace("-", "+").replace("_", "/")
base64.b64decode(data)
# 方式2
data = base64.b64decode(res.text, altchars=b"-_")  # base64解码成字节流
```

#### （5）为什么要base64编码

base64 编码的优点：

- 算法是编码，不是压缩，编码后只会增加字节数（一般是比之前的多1/3，比如之前是3， 编码后是4）
- 算法简单，基本不影响效率
- 算法可逆，解码很方便，不用于私密传输。
- 加密后的字符串只有【0-9a-zA-Z+/=】 ，不可打印字符（转译字符）也可以传输（关键！！！）

有些网络传输协议是为了传输`ASCII文本`设计的，当你使用其传输二进制流时（比如视频/图片），二进制流中的数据可能会被协议错误的识别为控制字符等等，因而出现错误。那这时就要将二进制流传输编码，因为有些8Bit字节码并没有对应的ASCII字符。

比如十进制ASCII码8对应的是后退符号(backspace), 如果被编码的数据中包含这个数值，那么编码出来的结果在很多编程语言里会导致前一个字符被删掉。又比如ASCII码0对应的是空字符，在一些编程语言里代表字符串结束，后续的数据就不会被处理了。

用Base64编码因为限定了用于编码的字符集，确保编码的结果可打印且无歧义。

不同的网络节点设备交互数据需要：设备A把base64编码后的数据封装在`json字符串`里，设备B先解析json拿到value，再进行base64解码拿到想要的数据。

> 1. 早年制定的一些协议都是只支持文本设定的。随着不断发展需要支持非文本了，才搞了一个base64做兼容
>
> 2. 虽然编码之后的数据与加密一样都具有不可见性，但编码与加密的概念并不一样。编码是公开的，任何人都可以解码；而加密则相反，你只希望自己或者特定的人才可以对内容进行解密。

base64处理图片数据：

```python
import base64
source = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAMAAABEpIrGAAAA7VBMVEUAAAD////////s8v////+txP+qwv+4zf/w9f/2+P+hu//Q3f+yyP+4zf/Q3f////+kvv+90P+80f+2yv/S4P/T4P/M2//z9/+cuP/V4P9Whv////9Uhf9Sg/9Pgf9NgP/8/f9di/9Xh/9lkf5aif9qlP7z9//k7P/c5v+2y/94nv51nP6lv/+LrP6Ep/6BpP5gjf7v9P+wxv/U4f/M2/+sxP+vxv73+f/P3v/J2v+5zf+ivP+fuv9xmf+Ytv6Usv6Hqf58of5vl/7m7v/g6f+zyf6QsP75+//q8P/B0v/W4//C1P6+0P6qwv6ct/76fHZiAAAAGnRSTlMAGAaVR/Py45aC9Mfy2b8t9OPZ2ce/v4L0x/e74/EAAAIZSURBVDjLZVPXYuIwEDSmQ4BLv5O0ku3Yhwu2IZTQe0hy7f8/57QSoYR5sVea1c424wgzl324LRRuH7I507hEJluYucCFEOBGhWzmy7X5+N0WwIjTbrcdBsKulM0z96onGCGE2X6n+cTkj/CqJ480igzkNXp26E9JkABSbBz8i4Bn3EkH840mKHoxs49fZQzt2Kd03FQEzSB3WsejB9Jqf1CJQBM0wCurABWBoub0gkDENwyStTHA62pwSWDtklRQ4FLfjnaiPqVW60hAYeLKNHIREOZuKTL80H6XBFCwn4BAmDOyLiOQUIlOSEjaoS+Ju57NZuul73Fml4w6yAivSLBW3MGfcfBmIegmArg3alICdJHgy1jQt8Z/6CcC4DdGXhLIoiWRACpbLYbDYW80GnXp2GH8ShP+PUvEoHsAIFq9Xm8+kXlIwkkI9pm+05Tm3yWqu9EiB0pkwjWBx2i+tND1XqeZqpPU4VhUbq/ekR8CwTRVoRxf3ifTbeIwcONNsJZ2lxFVKDMv1KNvS2zXdrnD+COvR1PQpTZKNlKD3cLCOJNnivgVxkw169BunlKFaV9/B+LQbqOsByY4IVgDB59dl/cjR9TIJV1Lh7CGqUqH/DDPhhZYOPkdLz6m0X7GrzPHsSe6zJwzxvm+5NeNi8U5ABfn7mz7zHJFrZ6+BY6rd7m8kQtcAtwwXzq4n69/vZbP1+pn6/8fsrRmHUhmpYYAAAAASUVORK5CYII="
s = source.split(",")[1]
with open("a.png", "wb") as f:
    f.write(base64.b64decode(s))
```

J的base64编码与解码

```js
// 编码
btoa()
// 解码
atob()
```

## 2.2、摘要&加密算法

###  【1】摘要算法：一切从MD5开始

MD5是一个非常常见的摘要(hash)逻辑.  其特点就是小巧. 速度快. 极难被破解. 所以, md5依然是国内非常多的互联网公司选择的密码摘要算法. 

> 1. 这玩意不可逆. 所以. 摘要算法就不是一个加密逻辑. 
>
> 2. 相同的内容计算出来的摘要是一样的
>
> 3. 不同的内容(哪怕是一丢丢丢丢丢不一样) 计算出来的结果差别非常大

在数学上. 摘要其实计算逻辑就是hash. 

hash(数据)  => 数字

 	1. 密码
 	2. 一致性检测  

md5的python实现:

```python
from hashlib import md5

obj = md5()
obj.update("yuan".encode("utf-8"))
# obj.update("alex".encode('utf-8'))  # 可以添加多个被加密的内容

bs = obj.hexdigest()
print(bs)
```

![image-20210907115443113-2491049](./assets/image-20210907115443113-2491049-4862042-4862044-7334427.png)

我们把密文丢到网页里. 发现有些网站可以直接解密. 但其实不然. 这里并不是直接解密MD5. 而是"撞库". 

就是它网站里存储了大量的MD5的值. 就像这样:

![image-20210907135220771-2491049](./assets/image-20210907135220771-2491049-4862069-4862071-7334427.png)

而需要进行查询的时候. 只需要一条select语句就可以查询到了. 这就是传说中的撞库. 

如何避免撞库: md5在进行计算的时候可以加盐. 加盐之后. 就很难撞库了. 

```python
from hashlib import md5


salt = "我是盐.把我加进去就没人能破解了"
obj = md5(salt.encode("utf-8"))  # 加盐
obj.update("alex".encode("utf-8"))

bs = obj.hexdigest()
print(bs)
```

![image-20210907135627573-2491049](./assets/image-20210907135627573-2491049-4862090-4862091-7334427.png)

扩展; sha256

```python
from hashlib import sha1, sha256
sha = sha256(b'salt')
sha.update(b'alex')
print(sha.hexdigest())
```

不论是sha1, sha256, md5都属于摘要算法. 都是在计算hash值. 只是散列的程度不同而已. 这种算法有一个特性. 他们是散列. 不是加密. 而且, 由于hash算法是不可逆的, 所以不存在解密的逻辑.

### 【2】对称加密（AES与DES）

AES是一种对称加密，所谓对称加密就是加密与解密使用的秘钥是一个。

常见的对称加密: AES, DES, 3DES. 我们这里讨论AES。

安装：

```python
pip install pycryptodome
```

AES 加密最常用的模式就是 CBC 模式和 ECB模式 ，当然还有很多其它模式，他们都属于AES加密。ECB模式和CBC 模式俩者区别就是 ECB 不需要 iv偏移量，而CBC需要。

```apl
"""
key的长度
    16: *AES-128*   
    24: *AES-192*
    32: *AES-256*

iv的字节长度只能是16位

MODE 加密模式. 
    常见的ECB, CBC
    ECB：是一种基础的加密方式，密文被分割成分组长度相等的块（不足补齐），然后单独一个个加密，一个个输出组成密文。
    CBC：是一种循环模式，前一个分组的密文和当前分组的明文异或或操作后再加密，这样做的目的是增强破解难度。
"""
```

CBC加密案例（选择aes-128）：

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import base64

key = '0123456789abcdef'.encode()  # 秘钥: 因为aes-128模式，所以必须16字节
iv = b'abcdabcdabcdabcd'  # 偏移量：因为aes-128模式，所以必须16字节
text = 'alex is a monkey!'  # 加密内容，因为aes-128模式，所以字节长度必须是16的倍数
# while len(text.encode('utf-8')) % 16 != 0:  # 如果text不足16位的倍数就用空格补足为16位
#     text += '\0'
text = pad(text.encode(), 16)
print("完整text:", text)

aes = AES.new(key, AES.MODE_CBC, iv)  # 创建一个aes对象

en_text = aes.encrypt(text)  # 加密明文
print("aes加密数据:::", en_text)  # b"_\xf04\x7f/R\xef\xe9\x14#q\xd8A\x12\x8e\xe3\xa5\x93\x96'zOP\xc1\x85{\xad\xc2c\xddn\x86"

en_text = base64.b64encode(en_text).decode()  # 将返回的字节型数据转进行base64编码
print(en_text)  # X/A0fy9S7+kUI3HYQRKO46WTlid6T1DBhXutwmPdboY=
```

CBC解密案例：

````python
from Crypto.Cipher import AES
import base64
from Crypto.Util.Padding import unpad

key = '0123456789abcdef'.encode()
iv = b'abcdabcdabcdabcd'
aes = AES.new(key, AES.MODE_CBC, iv)

text = 'X/A0fy9S7+kUI3HYQRKO46WTlid6T1DBhXutwmPdboY='.encode()  # 需要解密的文本
ecrypted_base64 = base64.b64decode(text)  # base64解码成字节流
source = aes.decrypt(ecrypted_base64)  # 解密
print("aes解密数据:::", source.decode())
print("aes解密数据:::", unpad(source, 16).decode())
````

> 1. 在Python中进行AES加密解密时，所传入的密文、明文、秘钥、iv偏移量、都需要是bytes（字节型）数据。python 在构建aes对象时也只能接受bytes类型数据。
> 2. 当秘钥，iv偏移量，待加密的明文，字节长度不够16字节或者16字节倍数的时候需要进行补全。
>
> 3. CBC模式需要重新生成AES对象，为了防止这类错误，无论是什么模式都重新生成AES对象就可以了。

### 【3】非对称加密(RSA)

非对称加密. 加密和解密的秘钥不是同一个秘钥. 这里需要两把钥匙. 一个公钥, 一个私钥.  公钥发送给客户端. 发送端用公钥对数据进行加密. 再发送给接收端, 接收端使用私钥来对数据解密. 由于私钥只存放在接受端这边. 所以即使数据被截获了. 也是无法进行解密的. 

[公钥和私钥](https://blog.csdn.net/Ruishine/article/details/114705443)

常见的非对称加密算法: RSA, DSA等等, 我们就介绍一个. RSA加密, 也是最常见的一种加密方案

1. 创建公钥和私钥

```python
from Crypto.PublicKey import RSA

# 生成秘钥
rsakey = RSA.generate(1024)
with open("rsa.public.pem", mode="wb") as f:
    f.write(rsakey.publickey().exportKey())

with open("rsa.private.pem", mode="wb") as f:
    f.write(rsakey.exportKey())
```

2. 加密

```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_v1_5
import base64


# 加密
data = "我喜欢你"
with open("rsa.public.pem", mode="r") as f:
    pk = f.read()
    rsa_pk = RSA.importKey(pk)
    rsa = PKCS1_v1_5.new(rsa_pk)

    result = rsa.encrypt(data.encode("utf-8"))
    # 处理成b64方便传输
    b64_result = base64.b64encode(result).decode("utf-8")
    print(b64_result)

```

3. 解密

```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_v1_5
import base64

data = "BkiKG8jzVGzbWOl4m8NXJEYglgtxhOB05MGmap8JSP97GzoewPBmDTs7c5iACUof3k/uJf0H88GygajVgBvkcbckJp7oO+Qj6VSUQYTOHhKN/VG2a8v+WzL34EO/S7BYoj2oOxIDAr8wDLxYxjBeXq/Be6Q1yBbnZcKaMkifhP8="
# 解密
with open("rsa.private.pem", mode="r") as f:
    prikey = f.read()
    rsa_pk = RSA.importKey(prikey)
    rsa = PKCS1_v1_5.new(rsa_pk)
    result = rsa.decrypt(base64.b64decode(data), None)
    print("rsa解密数据:::", result.decode("utf-8")
```

## 2.3、JS版本的加密算法

### 【1】md5摘要算法

```js
const CryptoJS = require('crypto-js');
// 原始数据
const data = '123456';
// 生成MD5摘要
const md5Digest = CryptoJS.MD5(data).toString();

console.log(md5Digest);
```

### 【2】AES加密

```js
const CryptoJS = require("crypto-js")

// 密钥（128位，16字节）
var key = CryptoJS.enc.Utf8.parse('0123456789abcdef');

// 初始化向量（IV）（128位，16字节）
var iv = CryptoJS.enc.Utf8.parse('1234567890abcdef');

// 待加密的数据
var plaintext = 'Hello, yuan!';

// 进行AES-128加密，使用CBC模式和PKCS7填充
var encrypted = CryptoJS.AES.encrypt(plaintext, key, {
    iv: iv,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7
});

// 获取加密后的密文
var ciphertext = encrypted.toString();

console.log(ciphertext);
```

# 三、JS逆向实战案例



## 1. Webpack解析

Webpack是一个现代的静态模块打包工具，它主要用于前端开发中的模块化打包和构建。通过Webpack，开发者可以将多个模块（包括JavaScript、CSS、图片等）进行打包，生成优化后的静态资源文件，以供在浏览器中加载和运行。

Webpack的主要功能和特点包括：

1. 模块化支持：Webpack将应用程序拆分为多个模块，通过模块化的方式管理和加载依赖关系。它支持CommonJS、ES module、AMD等多种模块化规范，并且能够将这些模块打包成最终的静态资源文件。
2. 打包和压缩：Webpack可以将多个模块打包成一个或多个最终的静态资源文件。它支持对JavaScript、CSS、图片等资源进行压缩、合并和优化，以减小文件大小，提升加载速度和性能。
3. 资源加载管理：Webpack可以处理各种类型的资源文件，例如JavaScript、CSS、图片、字体等。通过加载器（Loader）的配置，Webpack可以对这些资源文件进行转换和处理，使其能够被应用程序正确地引用和加载。

```js
/*
!function(形参){加载器}([模块1，模块2,...])
!function(形参){加载器}({"k1":"模块1","k2":"模块2"}) 
*/
```

```js
window = global;
!function (e) {
    var t = {};

    function n(r) {
        if (t[r])
            return t[r].exports;
        var o = t[r] = {
            i: r,
            l: !1,
            exports: {}
        };
        e[r].call(o.exports, o, o.exports, n);
        return o.exports.exports; 
    }

    window.loader = n;

    // n("1002");
}([ function () {
        console.log("foo");
        this.exports = 100; 
    },
    function () {
        console.log("bar");
        this.exports = 200;
    }]
);

console.log(window.loader(0));
console.log(window.loader(1));
```

```js
window = global;
!function (e) {
    var t = {};

    function n(r) {
        if (t[r])
            return t[r].exports;
        var o = t[r] = {
            i: r,
            l: !1,
            exports: {}
        };
        e[r].call(o.exports, o, o.exports, n);
        return o.exports.exports; // 返回 o.exports.exports，而不是整个 o.exports 对象
    }
    
    window.loader = n;

    // n("1002");
}({
    "1001": function () {
        console.log("foo");
        this.exports = 100; // 直接修改 exports 变量
    },
    "1002": function () {
        console.log("bar");
        this.exports = 200;
    }
});

console.log(window.loader("1001"));
```

## 2. JS逆向实战案例解析【补环境】

### 【1】补环境介绍

**浏览器环境**： 是指 JS代码在浏览器中的运行时环境，它包括V8自动构建的对象（即ECMAScript的内容,如Date、Array），浏览器（内置）传递给V8的操作DOM和BOM的对象（如document、navigator）；

**Node环境**：是基于V8引擎的Js运行时环境，它包括V8与其自己的内置API，如fs，http，path；

**Node环境** 与 **浏览器环境** 的异同点可以简单概括如图：

![image-20240120152715427](./assets/image-20240120152715427-5735636-0955999-5156176.png)

所以我们所说的 **“补浏览器环境**” 其实是补浏览器有 而Node没有的环境，即 补BOM和DOM的对象；

当我们每次把辛辛苦苦扣出来的 **“js加密算法代码”**，并且放在**浏览器环境**中能正确执行后，就需要将它放到**Node环境** 中去执行，而由于**Node环境**与**浏览器环境**之间存在差异，会导致部分JS代码在浏览器中运行的结果 与在node中运行得到的结果不一样，从而影响我们最终逆向成果；

附注：[**window对象结构图**](https://www.processon.com/view/link/66497878cfe67e27899b9e70?cid=66488d812b49420be3452fa3)

![image-20240520124354548](./assets/image-20240520124354548-6180235-6180237-5156176.png)

### 【2】Proxy代理

JavaScript中的Proxy是一种内置对象，它允许你在访问或操作对象之前拦截和自定义底层操作的行为。通过使用Proxy，你可以修改对象的默认行为，添加额外的逻辑或进行验证，以实现更高级的操作和控制。

Proxy对象包装了另一个对象（目标对象），并允许你定义一个处理程序（handler）来拦截对目标对象的操作。处理程序是一个带有特定方法的对象，这些方法被称为"捕获器"（traps），它们会在执行相应的操作时被调用。

```js
var yuan = {
    username: "yuan",
    age: 22
}
yuan = new Proxy(yuan, {
    get(target, p, receiver) {
        console.log("target: ", target);
        console.log("p: ", p);
        // return window['username'];/// 这里如果这样写. 有递归风险的...
        // return Reflect.get(...arguments);
        return Reflect.get(target, p);
    },
    set(target, p, value, receiver) {
        console.log("设置操作")
        Reflect.set(target, p, value);
    }
});

console.log(yuan.username);
console.log(yuan.age);
yuan.username = "rain"
yuan.age = 18
console.log(yuan.username);
console.log(yuan.age);
```

基于Proxy的特性，衍生了两种补环境思路： 

> 1. **Proxy**代理浏览器所有的**BOM、DOM对象及其属性**，再配合**node vm2模块**提供的**纯净V8环境**，就相当于在node中，对整个浏览器环境对象进行了代理，JS代码使用任何**浏览器环境** api都能被我们所拦截。然后我们针对拦截到的环境检测点去补。
> 2. 搭建补环境框架，用JS模拟浏览器基于原型链去伪造实现各个BOM、DOM对象，然后将这些JS组织起来，形成一个**纯JS版浏览器环境**，补环境越完善，越接近真实浏览器环境，能通杀的js环境检测就越多。

基于上面的语法，我们构建一套完整的代理补环境的功能函数

```js
function setProxyArr(proxyObjArr) {
    for (let i = 0; i < proxyObjArr.length; i++) {
        const handler = `{
      get: function(target, property, receiver) {
        console.log("方法:", "get  ", "对象:", "${proxyObjArr[i]}", "  属性:", property, "  属性类型：", typeof property, ", 属性值：", target[property], ", 属性值类型：", typeof target[property]);
        return target[property];
      },
      set: function(target, property, value, receiver) {
        console.log("方法:", "set  ", "对象:", "${proxyObjArr[i]}", "  属性:", property, "  属性类型：", typeof property, ", 属性值：", value, ", 属性值类型：", typeof target[property]);
        return Reflect.set(...arguments);
      }
    }`;
        eval(`try {
            ${proxyObjArr[i]};
            ${proxyObjArr[i]} = new Proxy(${proxyObjArr[i]}, ${handler});
        } catch (e) {
            ${proxyObjArr[i]} = {};
            ${proxyObjArr[i]} = new Proxy(${proxyObjArr[i]}, ${handler});
        }`);
    }
}

```

## 3. 瑞数之补环境

### 3.1、瑞数介绍

![image-20240910下午74639085](./assets/image-20240910%E4%B8%8B%E5%8D%8874639085-5968800.png)

**‌瑞数信息是一家专注于提供互联网动态业务应用安全防护解决方案的公司**

瑞数动态安全 Botgate（[机器人](https://so.csdn.net/so/search?q=机器人&spm=1001.2101.3001.7020)防火墙）以“动态安全”技术为核心，通过动态封装、动态验证、动态混淆、动态令牌等技术对服务器网页底层代码持续动态变换，增加服务器行为的“不可预测性”，实现了从用户端到服务器端的全方位“主动防护”，为各类 Web、HTML5 提供强大的安全保护。

> 瑞数可以理解为是我们进入到JS逆向世界的标志

过瑞数的方法基本上有以下几种

* 自动化工具
* 补环境
* 纯算

### 3.2、瑞数流程

1. rs的网站会请求两次page_url（文档请求），第1次page_url（文档请求）会返回一个cookie1，和一个响应体（HTML源码），以及请求响应码是 202或者 412

![image-20240726下午54233924](./assets/image-20240726%E4%B8%8B%E5%8D%8854233924-1986955-5968741.png)

2. 响应体（HTML源码）包括3个部分：

* **一个meta标签**，其content内容很长且是`动态`的（每次请求会变化），会在eval执行第二层JS代码时使用到；
* **一个ts代码**，下面的自执行函数会解密文件内容生成eval执行时需要的JS源码，也就是第二层vm代码；
* **一个大自执行函数**（每次请求首页都会`动态`变化），主要是解密外链JS内容，给window添加一些属性如$_ts，会在vm中使用;
* 这三个要素用于在本地生成一个cookie2，用于第2次请求

3. 第2次page_url（文档请求）携带cookie1和ncookie2，获取真正的页面内容！

![image-20240726下午54307164](./assets/image-20240726%E4%B8%8B%E5%8D%8854307164-1986988-5968741.png)

瑞数 3、4 代有以 T 和 S 结尾的两个 Cookie，其中以 S 开头的 Cookie 是第一次的 201 那个请求返回的，以 T 开头的 Cookie 是由 JS 生成的，动态变化的，T 和 S 前面一般会跟 80 或 443 的数字，Cookie 值第一个数字为瑞数的版本。

瑞数 5 代也有以 T 和 S 结尾的两个 Cookie，但有些特殊的 5 代瑞数也有以 O 和 P 结尾的，同样的，以 O 开头的是第一次的 412 那个请求返回的，以 P 开头的是由 JS 生成的，Cookie 值第一个数字同样为瑞数的版本。

### 3.3、瑞数案例解析

#### 【1】入口定位

先定位入口，这里涉及一个VM环境

* “VM”表示的是Virtual Machine（虚拟机），这些文件通常表示**由浏览器生成和执行的虚拟机脚本环境中的临时脚本**。这些脚本并不是项目源代码的一部分，也不是实际存在的物理文件。 它们**在浏览器的内存中创建并执行**。

* 比如说，当你在调试一个网页时，如果在某些动态生成并执行的JS代码上设定了断点，Chrome调试器会在一个以"VM"开头的文件中显示这些代码，例如"VM111.js"。这个"VM"文件的存在只是为了调试目的，它并不存在于服务器端，也不会被存储在本地，而是存在于浏览器内存中。一般情况下，这类文件的出现是因为浏览器对JavaScript代码的处理方式，如动态编译或者JavaScript堆栈跟踪。
* 通过eval函数或者new Function方法，Chrome浏览器会创建一个"VM"文件来展示这段临时执行的代码

通过脚本断点可以断住ts和自执行函数，因为第一次响应的index.html是动态的，所以为了后面调试，我们先试用文件替换本地化。

接下来在index.html中直接使用正则快速定位入口函数：

```apl
# 瑞数5:正则搜索
\S{4} = \S{4}\[\S{4}\[\d{2}\]\]\(\S{4},
# 瑞数6:搜索.call
```

![image-20240727下午54820340](./assets/image-20240727%E4%B8%8B%E5%8D%8854820340-2073701-5968741.png)

这个位置通常在分析瑞数的时候作为入口，图中 ==_$BH== 实际上是 ==eval== 方法，传入的第一个参数 ==_$4u== 是 ==Window 对象==，第二个对象 ==_$wt== 是我们前面看到的 VM 虚拟机中的 IIFE 自执行代码。

![image-20240727下午55621694](./assets/image-20240727%E4%B8%8B%E5%8D%8855621694-5968741.png)

最后在入口位置单步调试，即可进入到VM环境！

<img src="./assets/image-20240727%E4%B8%8B%E5%8D%8860038606-2074439-5968741.png" alt="image-20240727下午60038606" style="zoom:50%;" />

后面的断点调试就在这个环境中。

#### 【2】补环境

![image-20240727下午61624696](./assets/image-20240727%E4%B8%8B%E5%8D%8861624696-2075385-5968741.png)

> 注意content的值和auto.js是关联的，必须保持一致！

初始化补环境文件

```js
delete __dirname
delete __filename

top = self = window = global

document = {}
location = {}
navigator = {}
history = {}
screen = {}

function getEnv(proxy_array) {
    for (var i = 0; i < proxy_array.length; i++) {
        handler = `{\n
            get: function(target, property, receiver) {\n
                   console.log('方法：get','    对象：${proxy_array[i]}','    属性：',property,'    属性类型：',typeof property,'    属性值类型：',typeof target[property]);
                   return target[property];
            },
            set: function(target, property, value, receiver){\n
                    console.log('方法：set','    对象：${proxy_array[i]}','    属性：',property,'    属性类型：',typeof property,'    属性值类型：',typeof target[property]);
                    return Reflect.set(...arguments);
            }
        }`
        eval(`
            try {
                ${proxy_array[i]};
                ${proxy_array[i]} = new Proxy(${proxy_array[i]}, ${handler});
            } catch (e) {
                ${proxy_array[i]} = {};
                ${proxy_array[i]} = new Proxy(${proxy_array[i]}, ${handler});
            }
        `)
    }
}

proxy_array = ['window', 'document', 'location', 'navigator', 'history', 'screen']
getEnv(proxy_array)

```

![image-20240727下午62016683](./assets/image-20240727%E4%B8%8B%E5%8D%8862016683-2075618-5968741.png)

接下来就是按之前教给大家的监控器补环境。

## 4. Web自动化-DrissionPage

### 4.1、DP初识

在当今互联网高速发展的时代，网页数据的获取和处理变得愈发重要。传统的自动化工具如 Selenium 在某些方面逐渐显露出一些局限性，而 DrissionPage 正是在这样的背景下崭露头角。
DrissionPage 的发展历程充满了创新与突破。它最初的设计理念是将浏览器自动化和数据包操控相结合，以解决传统工具在面对复杂网页交互和反爬机制时的不足。从早期的版本开始，DrissionPage 就致力于为开发者提供一种更高效、更便捷的网页自动化解决方案。
在发展过程中，DrissionPage 不断优化和改进自身的功能。它摆脱了对 Selenium 的依赖，从 3.0 版本开始，作者另起炉灶，用 chromium 协议自行实现了类似 Selenium 的全部功能，并且在此基础上增加了许多独特的特性。例如，无 webdriver 特征使得它更难被网站识别，减少了被阻止或限制访问的风险。
同时，DrissionPage 还在易用性方面下足了功夫。与其他需要针对不同浏览器版本下载对应驱动的工具不同，它简化了配置过程，使得设置和使用变得更加简单快捷。官方文档详细介绍了安装、配置以及使用方法，为开发者提供了丰富的学习资源。
随着技术的不断进步，DrissionPage 的功能也在不断拓展。它不仅可以用于自动化测试，还在爬虫开发等领域展现出强大的潜力。无论是处理复杂的表单交互，还是绕过前端的一些限制，DrissionPage 都能发挥出重要作用。
总之，DrissionPage 以其独特的设计理念和不断创新的精神，在自动化爬虫领域逐渐崛起，为开发者带来了全新的体验和更多的可能性。

![image-20240921下午80824682](./assets/image-20240921%E4%B8%8B%E5%8D%8880824682-6920506.png)

为什么叫DrissionPag？Selenium框架用于操作浏览器的对象叫 Driver，requests 用于管理请求连接的对象叫 Session，因此Drission 就是它们两者的合体。在旧版本中，是通过对 selenium 和 requests 的重新封装实现的。因此 Drission 读作 “拽神”。但从 3.0 版开始，作者另起炉灶，用 chromium 协议自行实现了 selenium 全部功能，从而摆脱了对 selenium 的依赖，功能更多更强，运行效率更高，开发更灵活。

本库采用全自研的内核，内置了 N 多实用功能，对常用功能作了整合和优化，对比 selenium，有以下优点：

- 无 webdriver 特征
- 无需为不同版本的浏览器下载不同的驱动
- 运行速度更快
- 可以跨 iframe 查找元素，无需切入切出
- 把 iframe 看作普通元素，获取后可直接在其中查找元素，逻辑更清晰
- 可以同时操作浏览器中的多个标签页，即使标签页为非激活状态，无需切换
- 可以直接读取浏览器缓存来保存图片，无需用 GUI 点击另存
- 可以对整个网页截图，包括视口外的部分（90以上版本浏览器支持）
- 可处理非`open`状态的 shadow-root

除了以上优点，本库还内置了无数人性化设计。

- 极简的语法规则，集成大量常用功能，代码更优雅；
- 定位元素更加容易，功能更强大稳定；
- 无处不在的等待和自动重试功能。使不稳定的网络变得易于控制，程序更稳定，编写更省心；
- 提供强大的下载工具。操作浏览器时也能享受快捷可靠的下载功能；
- 允许反复使用已经打开的浏览器。无需每次运行从头启动浏览器，调试超方便；
- 使用 ini 文件保存常用配置，自动调用，提供便捷的设置，远离繁杂的配置项；
- 内置 lxml 作为解析引擎，解析速度成几个数量级提升；
- 使用 POM 模式封装，可直接用于测试，便于扩展；

首先，你需要安装 `drissionpage`。可以通过以下命令进行安装：

```python
pip install drissionpage
```

官网：`https://drissionpage.cn/`

```apl
# 安装及导入
pip install DrissionPage -i https://pypi.org/simple

from DrissionPage import ChromiumPage
from DrissionPage import SessionPage as Session
from DrissionPage import WebPage

# 工具讲解
- ChromiumPage 
    - 类似 selenium的 driver.Chrome(...) 会打开一个浏览器窗口
    - 只能进行GET请求
- SessionPage 
    - 使用起来类似 requests模块的 session,但比session更强大
    - 为了方便我直接取了别名
    - GET/POST 都可以满足以及代理
- WebPage
    - 集成了 ChromiumPage 和 SessionPage,还可以在两者之间自由切换,非常强大
```

我们以ChromiumPage为主，网站以百度一下为案例([https://www.baidu.com/](https://link.zhihu.com/?target=https%3A//www.baidu.com/))

```python
# 案例1
from DrissionPage import ChromiumPage, ChromiumOptions

co = ChromiumOptions()
co.headless(False)
# 1、设置无头模式：co.headless(True)
# 2、设置无痕模式：co.incognito(True)
# 3、设置访客模式：co.set_argument('--guest')
# 4、设置请求头user-agent：co.set_user_agent()
# 5、设置指定端口号：co.set_local_port(7890)
# 6、设置代理：co.set_proxy('http://localhost:2222')
page = ChromiumPage(co)
url = "https://www.baidu.com"
page.get(url)
page.wait.load_start()  # 等待页面加载完成
page.ele("css:#kw").input("美女")
page.ele("css:#su").click()

print(page.html)

# 案例2
from DrissionPage import ChromiumPage

cp = ChromiumPage()
cp.get('https://www.baidu.com/')
cp.ele("#kw").input("IP地址")  # 定位到搜索框并填充数据
cp.wait.load_start()  # 等待页面加载完成
cp.ele("#su").click()  # 找到搜索按钮并点击搜索

cp.listen.start('qifu-api.baidubce.com/ip/local/geo/v1/district')
res = cp.listen.wait()  # 等待并获取一个数据包
print(res.url)  # 输出数据包url
print(res.response.headers)  # 输出响应头
print(res.response.statusText)  # 输出响应状态码
print(res.response.body)  # 输出响应内容
```

### 4.2、重点语法

#### 【1】get方法

该方法用于跳转到一个网址。当连接失败时，程序会进行重试：

![image-20240922下午70100105](./assets/image-20240922%E4%B8%8B%E5%8D%8870100105-7002861.png)

#### 【2】元素查找

```python
# 根据属性查找，@ 后面可跟任意属性
page.ele('@id:ele_id', timeout=2)  # 查找 id 为 ele_id 的元素，设置等待时间2秒  
page.eles('@class')  # 查找所有拥有 class 属性的元素
page.eles('@class:class_name')  # 查找所有 class 含有 ele_class 的元素 
page.eles('@class=class_name')  # 查找所有 class 等于 ele_class 的元素 
 
# 根据 class 或 id 查找
page.ele('#ele_id')  # 等价于 page.ele('@id=ele_id')
page.ele('#:ele_id')  # 等价于 page.ele('@id:ele_id')
page.ele('.ele_class')  # 等价于 page.ele('@class=ele_class')
page.ele('.:ele_class')  # 等价于 page.ele('@class:ele_class')
 
# 根据 tag name 查找
page.ele('tag:li')  # 查找第一个 li 元素  
page.eles('tag:li')  # 查找所有 li 元素  
 
# 根据 tag name 及属性查找
page.ele('tag:div@class=div_class')  # 查找 class 为 div_class 的 div 元素
page.ele('tag:div@class:ele_class') # 查找 class 含有 ele_class 的 div 元素
page.ele('tag:div@class=ele_class') # 查找 class 等于 ele_class 的 div 元素
page.ele('tag:div@text():search_text') # 查找文本含有 search_text 的 div 元素
page.ele('tag:div@text()=search_text') # 查找文本等于 search_text 的 div 元素
 
# 根据文本内容查找
page.ele('search text')  # 查找包含传入文本的元素  
page.eles('text:search text')  # 如文本以 @、tag:、css:、xpath:、text: 开头，则应在前加上 text: 避免冲突  
page.eles('text=search text')  # 文本等于 search_text 的元素
 
# 根据 xpath 或 css selector 查找
page.eles('xpath://div[@class="ele_class"]')  
page.eles('css:div.ele_class')  
 
 
# 查找下级元素
element = page.ele('@id:ele_id')
element.ele('@class:class_name')  # 在 element 下级查找第一个 class 为 ele_class 的元素
element.eles('tag:li')  # 在 ele_id 下级查找所有li元素
 
# 根据位置查找
element.parent  # 父元素  
element.next  # 下一个兄弟元素  
element.prev  # 上一个兄弟元素  
 
# 获取 shadow-root，把它作为元素对待。只支持 open 的 shadow-root
ele1 = element.shadow_root.ele('tag:div')
 
# 串连查找
page.ele('@id:ele_id').ele('tag:div').next.ele('some text').eles('tag:a')
 
# 简化写法
eles = page('@id:ele_id')('tag:div').next('some text').eles('tag:a')
ele2 = ele1('tag:li').next('some text')

```

简化写法对应列表

| 原写法    | 简化写法 | 说明                                                    |
| --------- | -------- | ------------------------------------------------------- |
| `@id`     | `#`      | 表示 id 属性，简化写法只在语句最前面且单独使用时生效    |
| `@class`  | `.`      | 表示 class 属性，简化写法只在语句最前面且单独使用时生效 |
| `text`    | `tx`     | 按文本匹配                                              |
| `@text()` | `@tx()`  | 按文本查找与 @ 或 @@ 配合使用时                         |
| `tag`     | `t`      | 按标签类型匹配                                          |
| `xpath`   | `x`      | 用 xpath 方式查找元素                                   |
| `css`     | `c`      | 用 css selector 方式查找元素                            |

#### 【3】元素操作

```python
element.click(by_js)  # 点击元素，可选择是否用 js 方式点击
element.input(value)  # 输入文本
element.run_script(js)  # 对元素运行 JavaScript 脚本
element.submit()  # 提交
element.clear()  # 清空元素
element.screenshot(path, filename)  # 对元素截图
element.select(text)  # 根据文本选择下拉列表
element.set_attr(attr, value)  # 设置元素属性值
element.remove_attr(attr)  # 删除属性
element.drag(x, y, speed, shake)  # 拖动元素相对距离，可设置速度和是否随机抖动
element.drag_to(ele_or_loc, speed, shake)  # 拖动元素到另一个元素或某个坐标，可设置速度和是否随机抖动
element.hover()  # 在元素上悬停鼠标

```

#### 【4】元素属性和方法

```python
element.html  # 返回元素 outerHTML
element.inner_html  # 返回元素 innerHTML
element.tag  # 返回元素 tag name
element.text  # 返回元素 innerText 值
element.comments  # 返回元素内注释列表
element.link  # 返回元素 href 或 src 绝对 url
element.texts()  # 返回元素内所有直接子节点的文本，包括元素和文本节点，可指定只返回文本节点
element.attrs  # 返回元素所有属性的字典
element.attr(attr)  # 返回元素指定属性的值
element.css_path  # 返回元素绝对 css 路径
element.xpath  # 返回元素绝对 xpath 路径
element.parent  # 返回元素父元素
element.next  # 返回元素后一个兄弟元素
element.prev  # 返回元素前一个兄弟元素
element.parent()  # 返回第 1 级父元素
element.parents(num)  # 返回第 num 级父元素
element.nexts(num, mode)  # 返回后面第几个元素或节点
element.prevs(num, mode)  # 返回前面第几个元素或节点
element.ele(loc_or_str, timeout)  # 返回当前元素下级第一个符合条件的子元素、属性或节点文本
element.eles(loc_or_str, timeout)  # 返回当前元素下级所有符合条件的子元素、属性或节点文本

```

案例：

```python
from DrissionPage import ChromiumPage, ChromiumOptions
from loguru import logger
import platform

if platform.system().lower() == 'windows':
    co = ChromiumOptions().set_paths(browser_path=r"C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe")
else:
    co = ChromiumOptions()  # .set_paths(browser_path=r"/opt/google/chrome/google-chrome")
    co.set_local_port(9211)
    co.set_user_agent(
        user_agent='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36')  # 设置ua
    co.headless(True)  # 设置无头加载  无头模式是一种在浏览器没有界面的情况下运行的模式，它可以提高浏览器的性能和加载速度
    co.incognito(True)  # 无痕隐身模式打开的话，不会记住你的网站账号密码的
    co.set_argument('--guest')  # 设置访客模式，不会记住你的网站账号密码的等cookie
    co.set_argument(
        '--no-sandbox')  # 禁用沙箱 禁用沙箱可以避免浏览器在加载页面时进行安全检查,从而提高加载速度 默认情况下，所有Chrome 用户都启用了隐私沙盒选项  https://zhuanlan.zhihu.com/p/475639754
    co.set_argument("--disable-gpu")  # 禁用GPU加速可以避免浏览器在加载页面时使用过多的计算资源，从而提高加载速度

# 创建浏览器对象
cp = ChromiumPage(co)  # 创建对象
cp.get("https://www.itjuzi.com/ipo", retry=3, interval=2, timeout=15)  # 访问网

detail_links_all = []
for tr in cp.eles("css:.el-table__row"):
    tds = [td.text.strip() for td in tr.eles("x:.//td")[1:]]
    company = tr.ele("tag:a").text
    a_href = tr.ele("tag:a").attr("href")
    detail_links_all.append(a_href)
    logger.info(f"list_page_company is {company} , a_href is {a_href}")
logger.success(f">>>>>total_detail_urls is : {len(detail_links_all)}")
cp.quit()

```

### 4.3、案例解析

#### 【1】招聘平台

```python
import time

from DrissionPage import ChromiumPage, ChromiumOptions
import pandas as pd

page = ChromiumPage()
page.listen.start('/wapi/zpgeek/search/joblist.json')
page.get('https://www.zhipin.com/web/geek/job?query=python&city=101010100')

res = page.listen.wait()
print("res:::",res.response.body)
joblist = res.response.body['zpData']['jobList']
for i in range(10):
    time.sleep(1)
    btn = page.ele('.ui-icon-arrow-right')
    btn_a = btn.parent().attr('class')

    if btn_a != 'disabled':
        btn.click()
        res = page.listen.wait()
        joblist += res.response.body['zpData']['jobList']
        print(joblist)
    else:
        print('爬虫结束！')
        break
# 写入到excel中
df = pd.DataFrame(joblist)
df.to_excel('boss直聘.xlsx', index=False)
page.quit()
```

#### 【2】瑞数案例：SZU

```python
import time

from DrissionPage import WebPage, ChromiumOptions

page = WebPage()
url = "https://sugh.szu.edu.cn/Html/News/Columns/7/Index.html"

page.get(url, retry=3, interval=2, timeout=15)


def show(page):
    els = page.eles('x: //*[@id="zoom"]/li')
    for ele in els:
        # print(ele)
        href = ele.ele("x: ./a").attr("href")
        title = ele.ele("x: ./a").text
        if not title:
            title = ele.ele("x: ./a[2]").text
        print(href, title)


# 展示第一页
show(page)

for i in range(5):
    time.sleep(1)
    btn = page.ele('x: //*[@class="pagination_nextpage ui-state-default ui-corner-all"]')
    print("btn:::", btn)
    btn.click()
    show(page)

```

#### 【3】瑞数案例之YJJ：

```python
from DrissionPage import ChromiumPage

cp = ChromiumPage()
cp.listen.start("/datasearch/data/nmpadata/search")
cp.get("https://www.nmpa.gov.cn/datasearch/home-index.html#category=yp")

cp.eles('tag:input@class=el-input__inner')[1].input("布洛芬")
cp.ele('x=//*[@id="home"]/main/div[1]/div[7]/div/div[2]/div/button').click()

cp.wait.load_start()
cp.get("https://www.nmpa.gov.cn/datasearch/search-result.html")

for i in range(100):
    res = cp.listen.wait()
    data = res.response.body
    print(data)
    cp.ele(".btn-next").click()

```

#### 【4】淘宝案例

```python
import re
import time

from DrissionPage import ChromiumPage
from user import user, pwd
import json

cp = ChromiumPage()

# cp.get("https://login.taobao.com/member/login.jhtml")
# time.sleep(2)
# cp.ele("css:#fm-login-id").input(user)
# time.sleep(2)
# cp.ele("css:#fm-login-password").input(pwd)
# time.sleep(2)
# cp.ele("css:.fm-button.fm-submit.password-login").click()

cp.listen.start('h5/mtop.relationrecommend.wirelessrecommend.recommend/2.0')

cp.get("https://taobao.com/")
res = cp.listen.wait()
body = res.response.body
print("body:::", body)
cp.ele('css:#q').input("iphone 16 pro max")
cp.ele('css:.btn-search.tb-bg').click()

while 1:
    print("接收响应")
    try:
        res = cp.listen.wait()
        body = res.response.body

        if body.find("itemsArray") != -1:
            # print(body)
            ret = re.findall("mtopjsonp\d+\((.*)\)", body)[0]
            # print("ret:::",ret)
            data = json.loads(ret)
            itemsArray = data.get("data").get("itemsArray")
            print("len:::", len(itemsArray))
            for i in itemsArray:
                title = i.get("title")
                priceWap = i.get("priceWap")
                print(title, priceWap)

            break

        time.sleep(1)
    except Exception as e:
        pass

```





# Javascript基础入门

1992年底，美国国家超级电脑应用中心（NCSA）开始开发一个独立的浏览器，叫做Mosaic。这是人类历史上第一个浏览器，从此网页可以在图形界面的窗口浏览。但是该浏览器还没有面向大众的普通用户。

1994年10月，NCSA的一个主要程序员Jim Clark，成立了一家Netscape通信公司。这家公司的方向，就是在Mosaic的基础上，开发面向普通用户的新一代的浏览器Netscape Navigator。

1994年12月，Navigator发布了1.0版，市场份额一举超过90%。

Netscape 公司很快发现，Navigator浏览器需要一种可以嵌入网页的脚本语言，用来控制浏览器行为，因为当时，网速很慢而且上网费很贵，有些操作不宜在服务器端完成。比如，如果用户忘记填写“用户名”，就点了“发送”按钮，到服务器再发现这一点就有点太晚了，最好能在用户发出数据之前，就告诉用户“请填写用户名”。这就需要在网页中嵌入小程序，让浏览器检查每一栏是否都填写了。

管理层对这种浏览器脚本语言的设想是：功能不需要太强，语法较为简单，容易学习。

1995年5月，Netscape 公司只用了10天，就设计完成了这种语言的第一版。

Netscape 公司的这种浏览器脚本语言，最初名字叫做 Mocha，1995年9月改为LiveScript。12月，Netscape公司与Sun公司（Java语言的发明者和所有者）达成协议，后者允许将这种语言叫做JavaScript。这样一来，Netscape公司可以借助Java语言的声势，而Sun公司则将自己的影响力扩展到了浏览器，索引并不是因为JavaScript本身与Java语言有多么深的关系才叫做javaScript。

1996年3月，Navigator 2.0 浏览器正式内置了 JavaScript 脚本语言，也就是可以直接在浏览器中运行JavaScript 脚本语言。

JS的作用：JavaScript, 是一门能够运行在浏览器上的脚本语言. 简称JS。简单来说可以处理前端的一些简单的业务逻辑和用户行为、网页事件的触发和监听。

​		那么既然JS是可以运行在浏览器上的脚本. 并且, 我们知道本质上, 浏览器是执行HTML程序的. 那么如何在HTML中引入JS呢?

​		方案一, 直接在<script>标签中引入编写js代码

​		<img src="D:\BaiduNetdiskDownload\七期\day06-js语法实操01\imgs\image-20210826150049916.png" alt="image-20210826150049916" style="zoom: 33%;" />

​		方案二, 将js代码写在js文件中, 然后通过script标签的src属性进行引入

<img src="D:\BaiduNetdiskDownload\七期\day06-js语法实操01\imgs\image-20210826150217635.png" alt="image-20210826150217635" style="zoom: 33%;" />



## 一. Javascript基本数据类型

JS虽然是一个脚本语言. 麻雀虽小, 五脏俱全. 在js中也是可以像其他编程语言一样. 声明变量, 条件判断, 流程控制等等. 我们先看一下JS中的数据类型

在js中主要有这么几种数据类型

<img src="D:\BaiduNetdiskDownload\七期\day06-js语法实操01\imgs\Snipaste_2024-06-27_09-42-06.png" alt="Snipaste_2024-06-27_09-42-06" style="zoom:75%;" />

在js中声明变量用var来声明

在js中使用// 来表示单行注释. 使用/* */表示多行注释. 

```js
var 变量名; // 创建变量, 此时该变量除了被赋值啥也干不了. 
var 变量名 = 值; // 创建一个变量, 并且有值. 
var 变量名 = 值1, 变量名2 = 值2, 变量名3 = 值3.....; // 一次创建多个变量.并都有值
var 变量名1, 变量名2, 变量名3 = 值3;  // 创建多个变量. 并且只有变量3有值
```



数据类型转换:

```js
// string -> number  :  parseInt(字符串)
var a = "10086";
a = parseInt(a);  // 变成整数
console.log(a + 10); // 10096

// number -> string  : 数字.toString() 或者 数字 + ""
var a = 100;
var b = a.toString();
var c = a + "";  
console.log(b);
console.log(c);

// number -> string: 数字转化成16进制的字符串
var m = 122;
var n = m.toString(16);
console.log(n);

// 进制转换：十六进制的AB的十进制是多少
var d = parseInt("AB", 16); // 171

// 自动转换：弱类型中的变量会根据当前代码的需要,进行类型的自动隐式转化
var box1 = 1 + true;
// true 转换成数值,是1, false转换成数值,是0
console.log(box1); // 2

var box2 = 1 + "200";
console.log(box2); 
// ‘1200’ 原因是,程序中+的含义有2种,第一: 两边数值相加, 第二: 两边字符串拼接.但是在js中运算符的优先级中, 字符串拼接的优先级要高于整数    
// 值的加减乘除,所以解析器优先使用了+号作为了字符串的拼接符号了,因为程序就需要+号两边都是字符串才能完成运算操作,因此1变成字符串了。最终的结果就是 "1" +"200"

var box3 = 1 - "200";
console.log(box3); // -199;因为-号中表示的就是左边的数值减去右边的数值,因此程序就会要求"200"是数值,因此内部偷偷的转换了一下
```



字符串操作:

```js
// split   正则分割,经常用于把字符串转换成数组
var str = "广东-深圳-南山";
var ret = str.split("-");
console.log( ret );

// substr  截取
var str = "hello world";
var ret = str.substr(0,3);
console.log(ret); // hel

// trim    移除字符串首尾空白
var password = "    ge llo   ";
var ret = password.trim();
console.log(password.length); // 13
console.log(ret.length);  // 6

// 切片,当前方法支持使用负数代表倒数下标
// slice(开始下标)   从开始位置切到最后
// slice(开始下标,结束下标)  从开始下标切到指定位置之前
var str = "helloworld";
var ret = str.slice(3,6); // 开区间,不包含结束下标的内容
console.log(ret); // low
var ret = str.slice(5);
console.log(ret); // world
var ret = str.slice(2,-1);
console.log(ret); // lloworl

s.substring(start, end)  //字符串切割, 从start切割到end
s.length  //字符串长度
s.charAt(i) //第i索引位置的字符  s[i]
s.indexOf('xxx')  //返回xxx的索引位置, 如果没有xxx. 则返回-1
s.lastIndexOf("xxx") //返回xxx的最后一次出现的索引位置，如果没有xxx. 则返回-1
s.toUpperCase() //转换成大写字母
s.startsWith("xxx")  //判断是否以xxx开头
s.charCodeAt(i) //某个位置的字符的ascii
String.fromCharCode(ascii) //给出ascii 还原成正常字符
```

字符串正则：

```js
// match  正则匹配
// js中也存在正则,正则的使用符号和python里面是一样的
//语法：/正则表达式主体/修饰符(可选)
//修饰符：
	//i:执行对大小写不敏感的匹配。
	//g:执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。

var str = "我的电话是: 13312345678,你的电话: 13512345678";
var ret = str.match(/\d{11}/g); // 匹配,提取数据
console.log(ret);

// replace  正则替换
var str = "我的电话是: 13512345678";
var ret = str.replace(/(\d{3})\d{4}(\d{4})/,"$1****$2"); // 正则的捕获模式  $1$2表示的正则中第一个和第二个小括号捕获的内容
console.log(ret);

// search  正则查找,如果查找不到,则返回-1
var str = "hello";
var ret = str.search(/l/);
console.log(ret);
```

* undefined类型

undefined类型只有一个值，即 undefined。

 (1) 当声明的变量未初始化时，该变量的默认值是 undefined。

 (2)当函数无明确返回值时，返回的也是值 undefined;

* null类型

另一种只有一个值的类型是 null，它只有一个专用值 null，即它的字面量。值 undefined 实际上是从值 null 派生来的，因此 js 把它们定义为相等的。

尽管这两个值相等，但它们的含义不同。undefined 是声明了变量但未对其初始化时赋予该变量的值，null 则用于表示尚未存在的对象。如果函数或方法要返回的是对象，那么找不到该对象时，返回的通常是 null。

- 原始值和引用值

```js
// 初始值类型
var a = "bobo";
var b = a;
a = "alvin";
console.log(a);//alvin
console.log(b);//bobo

// 对象类型
var arr1=[1,2];
arr2 = arr1;
arr1.push(3);
console.log(arr1)// [1,2,3]
console.log(arr2);//[1,2,3]

arr1=[4,5];
console.log(arr1);//[4,5]
console.log(arr2);//[1,2,3]
```

- 运算符

```js
/*
//算术运算符
   +   数值相加
   -   数值相减
   *   数值相乘
   /   数值相除
   %   数值求余
   **  数值求幂
   a++ 变量被使用后自增1 
     var a = 10
     print(a++) 输出结果为10
     print(a) 就是11
   ++a 变量被使用前自增1  
     var b = 10
     print(++b) 输出的就是11
   b-- 变量被使用后自减1   
   --b 变量被使用前自减1 
   // 在python中是没有++操作的. 但是在js中是有的. 
        a++;  // 翻译一下就是a = a + 1 
        ++a;  // 翻译一下就是a = a + 1
        a--;  // 翻译一下就是a = a - 1
        --a;  // 翻译一下就是a = a - 1
   
//赋值运算符
   =
   +=
   -=
   *=
   /=
   %=
   **=

//比较运算符,比较的结果要么是true, 要么是false
	>   大于
	<   小于
	>=  大于或者等于
	<=  小于或者等于
	!=  不等于[计算数值]
	==  等于[计算]

  !== 不全等[不仅判断数值,还会判断类型是否一致]
	=== 全等[不仅判断数值,还会判断类型是否一致]
	let num1 = 3.14;
    let num2 = "3.14";
    console.log(num1 == num2); // true，因为==运算符会进行类型转换，比较它们的值是否相等
    console.log(num1 === num2); // false，因为===运算符不会进行类型转换，比较它们的值和类型是否都相等

//逻辑运算符
  &&   并且  and    两边的运算结果为true,最终结果才是true
  ||   或者  or     两边的运算结果为false,最终结果才是false
  !    非    not    运算符的结果如果是true,则最终结果是false ,反之亦然.
 

//条件运算符[三目运算符]
	 条件?true:false
	 例如:
	      var age = 12;
        var ret = age>=18?"成年":"未成年"; 
        console.log(ret);
 */
```



## 二. 流程控制语句

编程语言的流程控制分为三种：

- 顺序结构(从上向下顺序执行)
- 分支结构
- 循环结构

之前我们学习的方式就是顺序执行，即代码的执行从上到下，一行行分别执行。

例如：

```js
console.log("星期一");
console.log("星期二");
console.log("星期三");

```

### 分支结构

* if 分支语句

```js
 if(条件){
     // 条件为true时,执行的代码
   }
   
   if(条件){
     // 条件为true时,执行的代码
   }else{
     // 条件为false时,执行的代码
   }
   
   if(条件1){
     // 条件1为true时,执行的代码
   }else if(条件2){
     // 条件2为true时,执行的代码
   
   }....
   
   }else{
     // 上述条件都不成立的时候,执行的代码
   }
```

* switch语句

```js
switch(条件){
      case 结果1:
           满足条件执行的结果是结果1时,执行这里的代码..
           break;
      case 结果2:
      	   满足条件执行的结果是结果2时,执行这里的代码..
      	   break;
      .....
      default:
           条件和上述所有结果都不相等时,则执行这里的代码
   }

switch('a'):
  case 1: //只会会执行case 1下面的xxx代码
  	xxx
  	break;
  case 2:
  	xxx
  	break;
  default:
  	xxx
  	break
```

> 1、switch比if else更为简洁
>
> 2、执行效率更高。switch…case会生成一个跳转表来指示实际的case分支的地址，而这个跳转表的索引号与switch变量的值是相等的。从而，switch…case不用像if…else那样遍历条件分支直到命中条件，而只需访问对应索引号的表项从而到达定位分支的目的。
>
> 3、到底使用哪一个选择语句，代码环境有关，如果是范围取值，则使用if else语句更为快捷；如果是确定取值，则使用switch是更优方案。

### 循环语句

* while循环

```js
   while(循环的条件){
      // 循环条件为true的时候,会执行这里的代码
   }
  
```

循环案例：

```js
 var count = 0
 while (count<10){
     console.log(count);
     count++;
 }
```

* for循环

```js
   
   // 循环三要素
   for(1.声明循环的开始; 2.条件; 4. 循环的计数){
      // 3. 循环条件为true的时候,会执行这里的代码
   }
   
   for(循环的成员下标 in 被循环的数据){
      // 当被循环的数据一直没有执行到最后下标,都会不断执行这里的代码
   }   
```

循环案例：

```js
// 方式1
for (var i = 0;i<10;i++){
	console.log(i)
}

// 方式2
var arr = [111,222,333]
for (var i in arr){
    console.log(i,arr[i])
}

```

* 退出循环（break和continue）

```js
 for (var i = 0;i<100;i++){
           if (i===88){
               continue  // 退出当次循环
               // break  // 退出当前整个循环
           }
           console.log(i)
       }
```



## 三.数组对象

* 创建数组

```js
创建方式1:
var arrname = [元素0,元素1,….];          // var arr=[1,2,3];

创建方式2:
var arrname = new Array(元素0,元素1,….); // var test=new Array(100,"a",true);
```

* 数组方法

```js
var arr = ["A","B","C","D"];
// 内置属性
console.log( arr.length );
// 获取指定下标的成员
console.log( arr[3] ); // D
console.log( arr[arr.length-1] ); // 最后一个成员

// (1) pop()  出栈,删除最后一个成员作为返回值
var arr = [1,2,3,4,5];
var ret = arr.pop();
console.log(arr); // [1, 2, 3, 4]
console.log(ret); // 5


// (2) push() 入栈,给数组后面追加成员
var arr = [1,2,3,4,5];
arr.push("a");
console.log(arr); // [1, 2, 3, 4, 5, "a"]


// (3) shift是将数组的第一个元素删除
var arr = [1,2,3,4,5];
arr.shift()
console.log(arr); // [2, 3, 4, 5]

// (4) unshift是将value值插入到数组的开始
var arr = [1,2,3,4,5];
arr.unshift("bobo")
console.log(arr); // ["bobo",1,2, 3, 4, 5]


// (5) reverse() 反转排列
var arr = [1,2,3,4,5];
arr.reverse();
console.log(arr); // [5, 4, 3, 2, 1]

// (6) slice(开始下标,结束下标)  切片,开区间
var arr = [1,2,3,4,5];
console.log(arr.slice(1,3));


// (7) concat() 把2个或者多个数组合并
var arr1 = [1,2,3];
var arr2 = [4,5,7];
var ret = arr1.concat(arr2);
console.log( ret );


// (8) join()  把数组的每一个成员按照指定的符号进行拼接成字符串
var str = "广东-深圳-南山";
var arr = str.split("-");
console.log( arr ); // ["广东", "深圳", "南山"];

var arr1 = ["广东", "深圳", "南山"];
var str1 = arr1.join("-");
console.log( str1 ); // 广东-深圳-南山
       
```

* 遍历数组

```js
 var arr = [12,23,34]
 for (var i in arr){
       console.log(i,arr[i])
 }
```



## 四.Object对象

### object对象的基本操作

 Object 的实例不具备多少功能，但对于在应用程序中存储和传输数据而言，它们确实是非常理想的选择。
`创建 Object 实例的方式有两种。`

```js
var person = new Object();
person.name = "alvin";
person.age = 18;
```

`另一种方式是使用对象字面量表示法。`对象字面量是对象定义的一种简写形式，目的在于简化创建包含大量属性的对象的过程。下面这个例子就使用了对象字面量语法定义了与前面那个例子中相同的person 对象：

```js
var person = {
                name : "alvin",
                age : 18,
                say: function(){
                      alert(123);
                  }
             }; 
```

* object可以通过. 和 []来访问。

```js
console.log(person["age"]);
console.log(person.age)
```

* object可以通过for循环遍历

```js
 for (var attr in person){
           console.log(attr,person[attr]);
      }
```

- 或者：

- ```js
  function People(name, age){
      this.name = name;
      this.age = age;
      this.chi = function(){
          console.log(this.name, "在吃东西")
      }
  }
  p1 = new People("alex", 18);
  p2 = new People("wusir", 20);
  p1.chi();
  p2.chi();
  ```

### json序列化和反序列化

`JSON`：JavaScript 对象表示法，是一种轻量级的数据交换格式。易于人阅读和编写。

```js
// json是一种数据格式, 语法一般是{}或者[]包含起来
// 内部成员以英文逗号隔开,最后一个成员不能使用逗号!
// 可以是键值对,也可以是列表成员
// json中的成员如果是键值对,则键名必须是字符串.而json中的字符串必须使用双引号圈起来
// json数据也可以保存到文件中,一般以".json"结尾.


{
   "name": "xiaoming",
	 "age":12
}

[1,2,3,4]

{
   "name": "xiaoming",
   "age":22,
   "sex": true,
   "son": {
      "name":"xiaohuihui",
      "age": 2
   },
   "lve": ["篮球","唱","跳"]
}
```

js中也支持序列化和反序列化的方法：

```html 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    // js对象,因为这种声明的对象格式很像json,所以也叫json对象
    var data = {
        name: "xiaoming",
        age: 22,
        say: function(){
            alert(123);
        }
    };

    // 把对象转换成json字符串
    var ret = JSON.stringify(data);
    console.log(ret ); // {"name":"xiaoming","age":22}

    // 把json字符串转换成json对象
    var str = `{"name":"xiaoming","age":22}`;
    var ret = JSON.parse(str);
    console.log(ret);
</script>
</body>
</html>
```

## Date对象

* 创建Date对象

```js
//方法1：不指定参数
var nowd1=new Date(); //获取当前时间
console.log(nowd1);
console.log(nowd1.toLocaleString( ));
//方法2：参数为日期字符串
var d2=new Date("2004/3/20 11:12");
console.log(d2.toLocaleString( ));
var d3=new Date("04/03/20 11:12");
console.log(d3.toLocaleString( ));
```

* 获取时间信息

```java
获取日期和时间
var date=new Date();
date.getDate()();

    getDate()                 获取日
    getDay ()                 获取星期
    getMonth ()               获取月（0-11）
    getFullYear ()            获取完整年份
    getHours ()               获取小时
    getMinutes ()             获取分钟
    getSeconds ()             获取秒
    getMilliseconds ()        获取毫秒
```

## Math对象

```js
// Math对象的内置方法
// abs(x)  返回数值的绝对值
var num = -10;
console.log( Math.abs(num) ); // 10

// ceil(x)  向上取整
var num = 10.3;
console.log( Math.ceil(num) ); // 11

// floor(x) 向下取整
var num = 10.3;
console.log( Math.floor(num) ); // 10

// max(x,y,z,...,n)
console.log( Math.max(3,56,3) ); // 56
// min(x,y,z,...,n)


// random()  生成0-1随机数
console.log( Math.random() );

// 生成0-10之间的数值
console.log( Math.random() * 10 );

// round(x) 四舍五入
// 生成0-10之间的整数
console.log( Math.round( Math.random() * 10 ) );
```

## 五. JS中的函数(重点)

​		函数在程序中代表的就是一段具有功能性的代码，可以让我们的程序编程更加具有结构性和提升程序的复用性,也能让代码变得更加灵活强大

### 声明函数

```js
/*
// 函数的定义方式1
function 函数名 (参数){
    函数体;
    return 返回值;
}
功能说明：
    可以使用变量、常量或表达式作为函数调用的参数
    函数由关键字function定义
    函数名的定义规则与标识符一致，大小写是敏感的
    返回值必须使用return
    
//  函数的定义方式2
    
用 Function 类直接创建函数的语法如下：
var 函数名 = new Function("参数1","参数n","function_body");

虽然由于字符串的关系，第二种形式写起来有些困难，但有助于理解函数只不过是一种引用类型*/  
   
```

### 函数调用

```js
    //f(); --->OK
    function f(){
        console.log("hello")

    }
    f() //----->OK

```

>不同于python，js代码在运行时，会分为两大部分———检查装载 和 执行阶段。
>
>- 检查装载阶段：会先检测代码的语法错误，进行变量、函数的声明
>- 执行阶段：变量的赋值、函数的调用等，都属于执行阶段。

### 函数参数

（1） 参数基本使用

```js
// 位置参数
function add(a,b){

    console.log(a);
    console.log(b);
}
add(1,2)
add(1,2,3)
add(1)


// 默认参数
function stu_info(name,gender="male"){
    console.log("姓名："+name+" 性别："+gender)
}

stu_info("bobo")
```

### 函数返回值

在函数体内，使用 return 语句可以设置函数的返回值。一旦执行 return 语句，将停止函数的运行，并运算和返回 return 后面的表达式的值。如果函数不包含 return 语句，则执行完函数体内每条语句后，返回 undefined 值。

```js
function add(x,y) {
          return x,y
      }

var ret = add(2,5);
console.log(ret)
```

> 1、在函数体内可以包含多条 return 语句，但是仅能执行一条 return 语句
>
> 2、函数的参数没有限制，但是返回值只能是一个；如果要输出多个值，可以通过数组或对象进行设计。

### 函数作用域

作用域是JavaScript最重要的概念之一。

JavaScript中，变量的作用域有全局作用域和局部作用域两种。

```js
// 局部变量,是在函数内部声明,它的生命周期在当前函数被调用的时候, 当函数调用完毕以后,则内存中自动销毁当前变量
// 全局变量,是在函数外部声明,它的生命周期在当前文件中被声明以后就保存在内存中,直到当前文件执行完毕以后,才会被内存销毁掉
```

首先熟悉下var

```js
    var name = "bobo"; // 声明一个全局变量 name并赋值”bobo“
    name = "张三";  // 对已经存在的变量name重新赋值 ”张三“
    console.log(name);

    var  gender = "male"
    var  gender = "female" // 原内存释放与新内存开辟，指针指向新开辟的内存
    console.log(gender)
```

  作用域案例：

```js
 var num = 10; // 在函数外部声明的变量, 全局变量
function func(){
  //千万不要再函数内部存在和全局变量同名的变量
  num = 20; // 函数内部直接使用变量,则默认调用了全局的变量,
}
func();
console.log("全局num：",num);
```

### 匿名函数

匿名函数，即没有变量名的函数。在实际开发中使用的频率非常高！也是学好JS的重点。

```js
      // 匿名函数赋值变量
       var foo = function () {
           console.log("这是一个匿名函数！")
       };
			foo() //调用匿名函数
			

      // 匿名函数的自执行
      (function (x,y) {
           console.log(x+y);
       })(2,3)


     // 匿名函数作为一个高阶函数使用
     function bar() {

        return function () {
            console.log("inner函数！")
        }
    }
    bar()()
        
```

 

# Javascript进阶



## 一. 变量提升(不正常现象)

看以下代码, 或多或少会有些问题的.

```js
function fn(){
    console.log(name);
    var name = '大马猴';
}
fn()
```

发现问题了么. 这么写代码, 在其他语言里. 绝对是不允许的. 但是在js里. 不但允许, 还能执行. 为什么呢?  因为在js执行的时候. 它会首先检测你的代码.  发现在代码中会有name使用. OK. 运行时就会变成这样的逻辑:

```js
function fn(){
    var name;
    console.log(name);
    name = '大马猴';
}
fn()
console.log(a);
```

看到了么. 实际运行的时候和我们写代码的顺序可能会不一样....这种把变量提前到代码块第一部分运行的逻辑被称为**变量提升**. 这在其他语言里是绝对没有的. 并且也不是什么好事情. 正常的逻辑不应该是这样的. 那么怎么办?  在新的ES6中. 就明确了, 这样使用变量是不完善的. es6提出. 用let来声明变量. 就不会出现该问题了. 

```js
function fn(){
    console.log(name);  // 直接报错, let变量不可以变量提升.
    let name = '大马猴'; 
}
fn()
```

== 结论一, 用let声明变量是新版本javascript提倡的一种声明变量的方案.== 

let还有哪些作用呢?  

```js
function fn(){
    // console.log(name);  // 直接报错, let变量不可以变量提升.
    // let name = '大马猴';
    var name = "周杰伦";
    var name = "王力宏";
    console.log(name);
}
fn()
```

显然一个变量被声明了两次. 这样也是不合理的. var本意是声明变量. 同一个东西. 被声明两次. 所以ES6规定. let声明的变量. 在同一个作用域内. 只能声明一次. 

```js
function fn(){
    // console.log(name);  // 直接报错, let变量不可以变量提升.
    // let name = '大马猴';
    let name = "周杰伦";
    console.log(name);
    let name = "王力宏";
    console.log(name);
}
fn()
```

注意, 报错是发生在代码检查阶段. 所以. 上述代码根本就执行不了. 

==结论二, 在同一个作用域内. let声明的变量只能声明一次. 其他使用上和var没有差别==



## 二. 闭包函数

我们先看一段代码. 

```js
let name = "周杰伦";
function chi(){
    name = "吃掉";
}
chi();
console.log(name);
```

发现没有, 在函数内部想要修改外部的变量是十分容易的一件事. 尤其是全局变量. 这是非常危险的.  试想, 我写了一个函数. 要用到name, 结果被别人写的某个函数给修改掉了. 多难受. 

接下来. 我们来看一个案例:  

同时运行下面两组代码：

```js
// 1号工具人.
var name = "alex"

setTimeout(function(){
    console.log("一号工具人:"+name) // 一号工具人还以为是alex呢, 但是该变量是不安全的.
}, 5000);

```

```js
// 2号工具人
var name = "周杰伦"
console.log("二号工具人", name);
```

两组代码是在同一个空间内执行的. 他们拥有相同的作用域. 此时的变量势必是非常非常不安全的. 那么如何来解决呢?  注意, 在js里. 变量是有作用域的. 也就是说一个变量的声明和使用是有范围的. 不是无限的. 这一点, 很容易验证.

```js
function fn(){
    let love = "爱呀"
}
fn()
console.log(love)
```

直接就报错了.  也就是说. 在js里是有全局和局部的概念的. 

直接声明在最外层的变量就是全局变量. 所有函数, 所有代码块都可以共享的. 但是反过来就不是了. 在函数内和代码块内声明的变量. 它是一个局部变量. 外界是无法进行访问的. 我们就可以利用这一点来给每个工具人创建一个局部空间. 就像这样:

```js
// 1号工具人.
(function(){
    var name = "alex";
    setTimeout(function(){
        console.log("一号工具人:"+name) // 一号工具人还以为是alex呢, 但是该变量是不安全的.
    }, 5000);
})();

// 二号工具人
(function(){
    var name = "周杰伦"
    console.log("二号工具人", name);
})();
```

这样虽然解决了变量的冲突问题. 但是...我们想想. 如果在函数外面需要函数内部的一些东西来帮我进行相关操作怎么办...比如, 一号工具人要提供一个功能(加密). 外界要调用. 怎么办?  

```js
// 1号工具人.
let encrypt_tool = (function(){
    let log_msg = '开始加密......\n'
    // 我是一个加密函数
    let encrypt = function(data){  // 数据
        console.log(log_msg) //打印日志信息（访问外部变量）
        // 返回密文
        return atob(data);
    }
    // 外面需要用到这个功能啊. 你得把这个东东返回啊. 返回加密函数
    return encrypt;
})();

//外部调用
console.log(encrypt_tool('i love you'));
```

注意了. 我们如果封装一个加密js包的时候. 好像还得准备出解密的功能. 并且, 不可能一个js包就一个功能吧. 那怎么办?  我们可以返回一个对象. 对象里面可以存放好多个功能. 而一些不希望外界触碰的功能. 就可以很好的保护起来. 

```js
// 1号工具人.
let encrypt_tool = (function(){
    let log_msg_1 = '开始加密......'
    let log_msg_2 = '开始解密......'

    // 我是一个加密函数
    let encrypt = function(data){  // 被加密数据
        console.log(log_msg_1) //打印日志信息（访问外部变量）
        // 返回密文
        return atob(data);
    };
    //解密函数
    let decrypt = function(en_data){  // 加密后的数据
        console.log(log_msg_2) //打印日志信息（访问外部变量）
        // 返回解密后的原数据
        return btoa(en_data);
    };
    // 外面需要用到这个功能啊. 你得把这个东东返回啊. 返回加密函数
    return {'encrypt':encrypt,'decrypt':decrypt};
})();

//外部调用
en_data = encrypt_tool.encrypt('i love you');
de_data = encrypt_tool.decrypt(en_data)
console.log(en_data);
console.log(de_data);
```



OK. 至此. 何为闭包? 上面这个就是闭包. 相信你百度一下就会知道. 什么内层函数使用外层函数变量. 什么让一个变量常驻内存.等等. 其实你细看. 它之所以称之为闭包~. 它是一个封闭的环境. 在内部. 自己和自己玩儿. 避免了对该模块内部的冲击和改动. 避免的变量之间的冲突问题. 

闭包的特点:

1. 内层函数对外层函数变量的使用. 
2. 会让变量常驻与内存. 





## 三. JS中的各种操作(非交互)

### 3.1 定时器

在JS中, 有两种设置定时器的方案

```js
// 语法规则
t = setTimeout(函数, 时间)
// 经过xxx时间后, 执行xxx函数

// 示例：3秒后打印我爱你
t = setTimeout(function(){
    console.log("我爱你")
}, 3000);

window.clearTimeout(t)  // 停止一个定时器
```

### 3.2 eval函数(必须会. 隔壁村老太太都会.)

eval本身在js里面正常情况下使用的并不多. 但是很多网站会利用eval的特性来完成反爬操作. 我们来看看eval是个什么鬼?  

从功能上讲, eval非常简单. 它和python里面的eval是一样的. 它可以动态的把字符串当成js代码进行运行. 

```js
s = "console.log('我爱你')";
eval(s);
```

也就是说. eval里面传递的应该是即将要执行的代码(字符串). 那么在页面中如果看到了eval加密该如何是好?  其实只要记住了一个事儿. 它里面不论多复杂. 一定是个字符串. 

比如, 

```js
eval(function(p,a,c,k,e,d){e=function(c){return(c<a?'':e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d[e(c)]=k[c]||e(c);k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('0.1(\'我爱你\')',62,2,'console|log'.split('|'),0,{}))
```

这一坨看起来, 肯定很不爽. 怎么变成我们看着很舒服的样子呢?  记住. eval()里面是字符串. 记住~!!

那我想看看这个字符串长什么样?  就把eval()里面的东西拷贝出来. 执行一下. 最终一定会得到一个字符串. 要不然eval()执行不了的. 对不...于是就有了下面的操作.

![image-20210828160541107](D:\BaiduNetdiskDownload\七期\day07-js语法实操02\imgs\image-20210828160541107.png)

http://tools.jb51.net/password/evalencode, 在赠送你一个在线JS处理eval的网站. 大多数的eval加密. 都可以搞定了. 



# Javascript进阶



## 一. 变量提升(不正常现象)

看以下代码, 或多或少会有些问题的.

```js
function fn(){
    console.log(name);
    var name = '大马猴';
}
fn()
```

发现问题了么. 这么写代码, 在其他语言里. 绝对是不允许的. 但是在js里. 不但允许, 还能执行. 为什么呢?  因为在js执行的时候. 它会首先检测你的代码.  发现在代码中会有name使用. OK. 运行时就会变成这样的逻辑:

```js
function fn(){
    var name;
    console.log(name);
    name = '大马猴';
}
fn()
console.log(a);
```

看到了么. 实际运行的时候和我们写代码的顺序可能会不一样....这种把变量提前到代码块第一部分运行的逻辑被称为**变量提升**. 这在其他语言里是绝对没有的. 并且也不是什么好事情. 正常的逻辑不应该是这样的. 那么怎么办?  在新的ES6中. 就明确了, 这样使用变量是不完善的. es6提出. 用let来声明变量. 就不会出现该问题了. 

```js
function fn(){
    console.log(name);  // 直接报错, let变量不可以变量提升.
    let name = '大马猴'; 
}
fn()
```

== 结论一, 用let声明变量是新版本javascript提倡的一种声明变量的方案.== 

let还有哪些作用呢?  

```js
function fn(){
    // console.log(name);  // 直接报错, let变量不可以变量提升.
    // let name = '大马猴';
    var name = "周杰伦";
    var name = "王力宏";
    console.log(name);
}
fn()
```

显然一个变量被声明了两次. 这样也是不合理的. var本意是声明变量. 同一个东西. 被声明两次. 所以ES6规定. let声明的变量. 在同一个作用域内. 只能声明一次. 

```js
function fn(){
    // console.log(name);  // 直接报错, let变量不可以变量提升.
    // let name = '大马猴';
    let name = "周杰伦";
    console.log(name);
    let name = "王力宏";
    console.log(name);
}
fn()
```

注意, 报错是发生在代码检查阶段. 所以. 上述代码根本就执行不了. 

==结论二, 在同一个作用域内. let声明的变量只能声明一次. 其他使用上和var没有差别==



## 二. 闭包函数

我们先看一段代码. 

```js
let name = "周杰伦";
function chi(){
    name = "吃掉";
}
chi();
console.log(name);
```

发现没有, 在函数内部想要修改外部的变量是十分容易的一件事. 尤其是全局变量. 这是非常危险的.  试想, 我写了一个函数. 要用到name, 结果被别人写的某个函数给修改掉了. 多难受. 

接下来. 我们来看一个案例:  

同时运行下面两组代码：

```js
// 1号工具人.
var name = "alex"

setTimeout(function(){
    console.log("一号工具人:"+name) // 一号工具人还以为是alex呢, 但是该变量是不安全的.
}, 5000);

```

```js
// 2号工具人
var name = "周杰伦"
console.log("二号工具人", name);
```

两组代码是在同一个空间内执行的. 他们拥有相同的作用域. 此时的变量势必是非常非常不安全的. 那么如何来解决呢?  注意, 在js里. 变量是有作用域的. 也就是说一个变量的声明和使用是有范围的. 不是无限的. 这一点, 很容易验证.

```js
function fn(){
    let love = "爱呀"
}
fn()
console.log(love)
```

直接就报错了.  也就是说. 在js里是有全局和局部的概念的. 

直接声明在最外层的变量就是全局变量. 所有函数, 所有代码块都可以共享的. 但是反过来就不是了. 在函数内和代码块内声明的变量. 它是一个局部变量. 外界是无法进行访问的. 我们就可以利用这一点来给每个工具人创建一个局部空间. 就像这样:

```js
// 1号工具人.
(function(){
    var name = "alex";
    setTimeout(function(){
        console.log("一号工具人:"+name) // 一号工具人还以为是alex呢, 但是该变量是不安全的.
    }, 5000);
})();

// 二号工具人
(function(){
    var name = "周杰伦"
    console.log("二号工具人", name);
})();
```

这样虽然解决了变量的冲突问题. 但是...我们想想. 如果在函数外面需要函数内部的一些东西来帮我进行相关操作怎么办...比如, 一号工具人要提供一个功能(加密). 外界要调用. 怎么办?  

```js
// 1号工具人.
let encrypt_tool = (function(){
    let log_msg = '开始加密......\n'
    // 我是一个加密函数
    let encrypt = function(data){  // 数据
        console.log(log_msg) //打印日志信息（访问外部变量）
        // 返回密文
        return atob(data);
    }
    // 外面需要用到这个功能啊. 你得把这个东东返回啊. 返回加密函数
    return encrypt;
})();

//外部调用
console.log(encrypt_tool('i love you'));
```

注意了. 我们如果封装一个加密js包的时候. 好像还得准备出解密的功能. 并且, 不可能一个js包就一个功能吧. 那怎么办?  我们可以返回一个对象. 对象里面可以存放好多个功能. 而一些不希望外界触碰的功能. 就可以很好的保护起来. 

```js
// 1号工具人.
let encrypt_tool = (function(){
    let log_msg_1 = '开始加密......'
    let log_msg_2 = '开始解密......'

    // 我是一个加密函数
    let encrypt = function(data){  // 被加密数据
        console.log(log_msg_1) //打印日志信息（访问外部变量）
        // 返回密文
        return atob(data);
    };
    //解密函数
    let decrypt = function(en_data){  // 加密后的数据
        console.log(log_msg_2) //打印日志信息（访问外部变量）
        // 返回解密后的原数据
        return btoa(en_data);
    };
    // 外面需要用到这个功能啊. 你得把这个东东返回啊. 返回加密函数
    return {'encrypt':encrypt,'decrypt':decrypt};
})();

//外部调用
en_data = encrypt_tool.encrypt('i love you');
de_data = encrypt_tool.decrypt(en_data)
console.log(en_data);
console.log(de_data);
```



OK. 至此. 何为闭包? 上面这个就是闭包. 相信你百度一下就会知道. 什么内层函数使用外层函数变量. 什么让一个变量常驻内存.等等. 其实你细看. 它之所以称之为闭包~. 它是一个封闭的环境. 在内部. 自己和自己玩儿. 避免了对该模块内部的冲击和改动. 避免的变量之间的冲突问题. 

闭包的特点:

1. 内层函数对外层函数变量的使用. 
2. 会让变量常驻与内存. 





## 三. JS中的各种操作(非交互)

### 3.1 定时器

在JS中, 有两种设置定时器的方案

```js
// 语法规则
t = setTimeout(函数, 时间)
// 经过xxx时间后, 执行xxx函数

// 示例：3秒后打印我爱你
t = setTimeout(function(){
    console.log("我爱你")
}, 3000);

window.clearTimeout(t)  // 停止一个定时器
```

### 3.2 eval函数(必须会. 隔壁村老太太都会.)

eval本身在js里面正常情况下使用的并不多. 但是很多网站会利用eval的特性来完成反爬操作. 我们来看看eval是个什么鬼?  

从功能上讲, eval非常简单. 它和python里面的eval是一样的. 它可以动态的把字符串当成js代码进行运行. 

```js
s = "console.log('我爱你')";
eval(s);
```

也就是说. eval里面传递的应该是即将要执行的代码(字符串). 那么在页面中如果看到了eval加密该如何是好?  其实只要记住了一个事儿. 它里面不论多复杂. 一定是个字符串. 

比如, 

```js
eval(function(p,a,c,k,e,d){e=function(c){return(c<a?'':e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d[e(c)]=k[c]||e(c);k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('0.1(\'我爱你\')',62,2,'console|log'.split('|'),0,{}))
```

这一坨看起来, 肯定很不爽. 怎么变成我们看着很舒服的样子呢?  记住. eval()里面是字符串. 记住~!!

那我想看看这个字符串长什么样?  就把eval()里面的东西拷贝出来. 执行一下. 最终一定会得到一个字符串. 要不然eval()执行不了的. 对不...于是就有了下面的操作.

![image-20210828160541107](D:\BaiduNetdiskDownload\七期\day08-js语法实操03\imgs\image-20210828160541107.png)

http://tools.jb51.net/password/evalencode, 在赠送你一个在线JS处理eval的网站. 大多数的eval加密. 都可以搞定了. 



### 3.3 prototype和\__proto__

逆向代码示例：

```js
function ct(t) {
    pv['$_BCAO'] = t || [];
}

ct.prototype = {
    '$_CAE':function(f) {
               f();
            }
} 

var H = new ct(t)['$_CAE'](function(t) {
        var e = function(t) {
            for (var e = [[1, 0], [2, 0], [1, -1], [1, 1], [0, 1], [0, -1], [3, 0], [2, -1], [2, 1]], r = e['length'],n = 0; n < r; n++){
                var b = 'stuvwxyz~';
            }   
            return b;
        };
    	return e;
    })


```



示例： 

```js
function People(name, age){
    this.name = name;
    this.age = age;
    this.run = function(){
        console.log(this.name+"在跑")
    }
}

p1 = new People("张三", 18);
p2 = new People("李四", 19);

p1.run();
p2.run();
```

我现在代码写完了. 突然之间, 我感觉好像少了个功能. 人不应该就一个功能. 光会跑是不够的. 还得能够学习. 怎么办?  直接改代码?  可以. 但不够好. 如果这个代码封装不是我写的呢? 随便改别人代码是很不礼貌的. 也很容易出错. 怎么办?  我们可以在我们自己代码中对某个类型动态增加功能. 此时就用到了prototype. 

```js
//对象的模版：类
function People(name, age){
    this.name = name;
    this.age = age;
    this.run = function(){
        console.log(this.name+"在跑")
    }
}

//People.prototype原型对象
People.prototype.address = "Beijing";
// 通过prototype可以给People增加功能
People.prototype.study = function(){
    console.log(this.name+"还可以学习");
}
//通过对象模版创建的实例对象
var p1 = new People("张三", 18);
var p2 = new People("李四", 19);

p1.run();
p2.run();

p1.study();
p2.study();
```

能看到一些效果了是吧. 也就是说. 可以通过prototype给我们的对象增加一些功能. 那么，什么是prototype呢？

接下来. 聊几个重要的概念. 

1. 原型对象

   每一个js对象中. 都有一个隐藏属性`__proto__`指向该对象的`原型对象`. （People.prototype）

   原型对象用于存储所有实例对象共享的属性和方法，以减少每个实例对象重复存储相同属性和方法的开销。

   在执行该对象的方法或者查找属性时. 首先, 对象自己是否存在该属性或者方法. 如果存在, 就执行自己的. 如果自己不存在. 就去找`原型对象`. 

   ```js
   p1.toString() //显然p1对象是没有该方法的，但是确实可以调用该方法，通过查看p1的原型，发现在Object中存在toString方法。
   ```

   **实例对象是可以共享它的原型链上所有的原型对象存在的成员**

   对象的原型成员重写：

   ```js
   function Friend(){
       this.chi = function(){
           console.log("我的朋友在吃");
       }
   }
   Friend.prototype.chi = function(){
       console.log("我的原型在吃")
   }
   
   f = new Friend();
   f.chi(); // 此时，该对象中有chi这个方法.  同时, 它的原型对象上, 也有chi这个方法.
   // 运行结果: 我的朋友在吃
   ```

2. `prototype`和`__proto__`有什么关系?

   在js中. 函数名的prototype属性和对象的`__proto__`是一个东西.  都是指向这个`原型对象`. 

   ```js
   f.__proto__ === Friend.prototype   // true
   ```

3. 原型链

   原型链是**JavaScript中对象继承属性和方法的一种方式**。具体介绍如下：

   ```
   原型链是JavaScript中对象继承属性和方法的一种方式。当访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，它会通过原型链去它的原型对象中查找，如果原型对象也没有，会继续在其原型的原型中查找，这样逐级向上，直到找到属性或方法或者达到原型链的末端。
   ```

   这个比较绕了. 我们从前面的学习中, 了解到. 每个对象身体里. 都隐藏着`__proto__`也就是它的`原型对象`. 那么我们看哈, `原型对象`也是`对象`啊, 那么也就是说. `原型对象`也有`__proto__`属性. 

   类似于.....这样:

   ```js
   f.__proto__.__proto__
   ```

   此时. 又出现一堆看不懂的玩意. 这些玩意是什么?  这些其实是Object的原型. 

   ```js
   f.__proto__.__proto__ === Object.prototype
   ```


练习测试：理解下属代码

```js
function ct(t) {
    pv['$_BCAO'] = t || [];
}

var H = new ct(t)['$_CAE'](function(t) {
        var e = function(t) {
            for (var e = [[1, 0], [2, 0], [1, -1], [1, 1], [0, 1], [0, -1], [3, 0], [2, -1], [2, 1]], r = e['length'],n = 0; n < r; n++){
                var b = 'stuvwxyz~';
            }   
            return b;
        };
    	return e;
    })

ct.prototype = {
    '$_CAE':function(f) {
               f();
            }
}
```



重点总结：

	1. prototype关键字是可以给对象动态添加成员
	1. 原型对象中的成员是可以被所有实例对象共享
	1. 对象在调用某一个成员时，该成员有可能是对象自身封装的成员也可能是该对象原型上的成员

### 3.4 神奇的window（浏览器环境）

window对象是一个很神奇的东西. 你可以把这东西理解成javascript的全局. 如果我们默认不用任何东西访问一个标识符. 那么默认认为是在用window对象进行访问该标识符. 

例如: 

```js
var a = 10; 
a === window.a  // true

window.mm = "爱你"
console.log(mm); //"爱你"
```

综上,  我们可以得出一个结论. 全局变量可以用window.xxx来表示. 

注意:window对象实际上表示的是浏览器的窗口。浏览器的窗口只有浏览器有。我们在浏览器中的开发者工具的Console选项卡中是可以直接使用window对象。

但是：在pycharm中直接使用window对象会报错：ReferenceError: window is not defined。why？就是因为在pycharm中是没有浏览器窗口。

所以切记：window对象是专属于浏览器环境下的一个内置对象。就意味着window只可以在浏览器环境下无需声明直接被使用。

ok. 接下来. 注意看了. 我要搞事情了

```js
//如果想在外部调用下面函数的中的chi函数如何实现？
(function(){
    let chi = function(){
        console.log("我是吃")
    }
})();
chi() //会报错，因为chi是一个局部变量

//正确写法：
(function(){
    let chi = function(){
        console.log("我是吃")
    }
    window.chi = chi //全局的
})();
chi();

//换一种写法. 你还认识么?
(function(w){
    let chi = function(){
        console.log("我是吃")
    }
    w.chi = chi
})(window);
```

如何在node环境下执行上述代码？

```
globalThis在浏览器和node中都可以使用。在Pycharm中就可以使用globalThis代替window用于全局变量的声明

//全局变量的声明
globalThis.a = 10;
console.log(a);


```

window是整个浏览器的全局作用域. 

使用 window 对象也可以访问客户端其他对象，这种关系构成浏览器对象模型，window 对象代表根节点，浏览器对象关系的关系如图所示，每个对象说明如下。

- window：客户端 JavaScript 顶层对象。每当 <body> 或 <frameset> 标签出现时，window 对象就会被自动创建。
- navigator：包含客户端有关浏览器信息。
- screen：包含客户端屏幕的信息。
- history：包含浏览器窗口访问过的 URL 信息。
- location：包含当前网页文档的 URL 信息。
- document：包含整个 HTML 文档，可被用来访问文档内容及其所有页面元素。

<img src="D:\BaiduNetdiskDownload\七期\day08-js语法实操03\imgs\image-20240528194905528-6896946.png" alt="image-20240528194905528-6896946" style="zoom:50%;" />



### 3.5 call和apply

对于咱们逆向工程师而言. 并不需要深入的理解call和apply的本质作用. 只需要知道这玩意执行起来的逻辑顺序是什么即可（外部函数关联对象的内部成员）

在运行时. 正常的js调用:

```js
function People(name, age){
    this.name = name;
    this.age = age;
    this.chi = function(){
        console.log(this.name, "在吃东西")
    }
}
var p1 = new People("alex", 18);
var p2 = new People("wusir", 20);
p1.chi();
p2.chi();
```

接下来, 我们可以使用call和apply也完成同样的函数调用

```js
function People(name, age){
    this.name = name;
    this.age = age;
    this.chi = function(){
        console.log(this.name, "在吃东西")
    }
}
p1 = new People("alex", 18);
p2 = new People("wusir", 20);
p1.chi();
p2.chi();

//外部函数关联对象的内部函数：
	//想要让alx吃："馒头", "大饼"，如何调用下面的函数？
function eat(what_1, what_2){
    console.log(this.name, "在吃", what_1, what_2);
}
// call的语法是: 函数.call(对象, 参数1, 参数2, 参数3....)
// 执行逻辑是: 执行函数. 并把对象传递给函数中的this.  其他参数照常传递给函数
eat.call(p1, "馒头", "大饼");
```

apply和他几乎一模一样. 区别是: apply传递参数要求是一个数组

```js
eat.apply(p1, ["馒头", "大饼"]);
```



### 3.6 ES6中的箭头函数

在ES6中简化了函数的声明语法. 

```js
var fn = function(){};
// 如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
var fn = () => {};

var fn = function(name){}
var fn = name => {}
var fn = (name) => {}

var fn = function(name, age){}
var fn = (name, age) => {}

var f = v => v*v
// 等同于
var f = function(v) {
 return v*v
}

```



# 下节课继续将



### 3.7 exports

类似Python中的模块导入

```js
// functions.js文件

// 加法函数
function add(a, b) {
  return a + b;
}

// 乘法函数
function multiply(a, b) {
  return a * b;
}

// 导出函数
exports.add = add;
exports.multiply = multiply;
```

```js
// main.js

// 导入 functions 模块
const functions = require('./functions');

// 使用导入的函数
console.log(functions.add(2, 3));  // 输出: 5
console.log(functions.multiply(4, 5));  // 输出: 20
```

## 四. DOM对象(了解)

DOM  document Object Model 文档对象模型

```js
// 整个html文档,会保存一个文档对象document
// console.log( document ); // 获取当前文档的对象
```

### 查找标签

* 直接查找标签

```js
document.getElementsByTagName("标签名")
document.getElementById("id值")
document.getElementsByClassName("类名")
//返回dom对象，就是标签对象或者数组
```

* CSS选择器查找

```JS
document.querySelector("css选择器")  //根据css选择符来获取查找到的第一个元素，返回标签对象（dom对象）
document.querySelectorAll("css选择器"); // 根据css选择符来获取查找到的所有元素,返回数组
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>


<div id="i1">DIV1</div>

<div class="c1">DIV</div>
<div class="c1">DIV</div>
<div class="c1">DIV</div>


<div class="outer">
    <div class="c1">item</div>
</div>


<div class="c2">
    <div class="c3">
        <ul class="c4">
            <li class="c5" id="i2">111</li>
            <li>222</li>
            <li>333</li>
        </ul>
    </div>
</div>

<script>

   // 直接查找
   var ele = document.getElementById("i1");  // ele就是一个dom对象
   console.log(ele);

   var eles = document.getElementsByClassName("c1"); // eles是一个数组 [dom1,dom2,...]
   console.log(eles);

   var eles2 = document.getElementsByTagName("div"); // eles2是一个数组 [dom1,dom2,...]
   console.log(eles2);

  //定位outer下的c1对应的标签
   var outer = document.getElementsByClassName("outer")[0];
   var te = outer.getElementsByClassName("c1");
   console.log(te);

    // css选择器
		//层级定位(空格可以表示一个或多个层级)
    var dom = document.querySelector(".c2 .c3 .c5");
    console.log(":::",dom);
		//层级定位
    var doms = document.querySelectorAll("ul li");
    console.log(":::",doms);
    
</script>

</body>
</html>
```

### 绑定事件

* 静态绑定 ：直接把事件写在标签元素中

```html
<div id="div" onclick="foo()">click</div>

<script>
    function foo(){           
        console.log("foo函数");   
    }
</script>

```

* 动态绑定：在js中通过代码获取元素对象,然后给这个对象进行后续绑定

```html
<p id="i1">试一试!</p>

<script>

    var ele=document.getElementById("i1");

    ele.onclick=function(){
        console.log("ok");
        
    };

</script>
```



## 五. jQuery和Ajax（了解）

jQuery是一个曾经火遍大江南北的一个Javascript的第三方库. jQuery的理念: write less do more.  其含义就是让前端程序员从繁琐的js代码中解脱出来. 我们来看看是否真的能解脱出来. 

python  => 基础语法 => 系统模块 => 第三方的库

jQuery的下载：

​	jquery:

​	 https://lf6-cdn-tos.bytecdntp.com/cdn/expire-1-M/jquery/1.9.1/jquery.js

 	https://code.jquery.com/jquery-3.6.0.min.js

只需要把上面这个jquery下载的网址复制到浏览器上, 然后保存(ctrl+s)成js文件就可以了.

## 一. jQuery初使用

导入jQuery

```js
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
```

我们用jQuery来完成一个按钮的基本点击效果. 当然, 得和传统的js对比一下

先准备好html. 页面结构. 这里复制粘贴就好

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
    <div class="div-out">
        <input type="button" class="btn" value="我是按钮. 你怕不怕">
        <div class="mydiv">我怕死了...</div>
    </div>
</body>
</html>
```

需求: 点击按钮. 更改mydiv中的内容. 

```js
// 传统js
window.onload = function(){
    document.querySelector(".btn").onclick = function(){
        document.querySelector('.mydiv').innerText = "我好啪啪啊";
    };
}
```

```js
// jQuery
$(function(){  // $(document).ready(function(){
    $(".btn").click(function(){
        $(".mydiv").text('我要上天');
    })
})
```

除了`$`外, 其他东西貌似都挺容易理解的.  而且代码简洁. 异常舒爽. 

`$`是什么鬼, 在jQuery中, `$`可以认为是jQuery最明显的一个标志了. `$()`可以把一个普通的js对象转化成jQuery对象. 并且在jquery中 \$的含义就是jQuery. 



## 二. jQuery选择器

jQuery的逻辑和css选择器的逻辑是一样的. 

```js
// 语法:
$(选择器)
```

可以使用jQuery选择器快速的对页面结构进行操作. 

案例:  

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
	<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>

        $(function(){
            $(".btn").on('click', function(){
                $(".info").text("");
                let username = $("#username").val();
                let password = $("#password").val();
                let gender = $("input:radio[name='gender']:checked").val();  // input标签中radio 并且name是gender的. 并且被选择的.
                let city = $("#city").val();

                let flag = true;
                if(!username){
                    $("#username_info").text('用户名不能为空!');
                    flag = false;
                }

                if(!password){
                    $("#password_info").text('密码不能为空!');
                    flag = false;
                }

                if(!gender){
                    $("#gender_info").text('请选择性别!');
                    flag = false;
                }

                if(!city){
                    $("#city_info").text('请选择城市!');
                    flag = false;
                }

                if(flag){
                    $("#login_form").submit();
                } else {
                    return;
                }
            })
        })

    </script>
</head>
<body>
    <form id="login_form">
        <label for="username">用户名: </label><input type="text" id="username" name="username"><span class="info" id="username_info"></span><br/>
        <label for="password">密码: </label><input type="password" id="password" name="password"><span class="info" id="password_info"></span><br/>
        <label>性别: </label>
            <input type="radio" id="gender_men" name="gender" value="men"><label for="gender_men">男</label>
            <input type="radio" id="gender_women" name="gender" value="women"><label for="gender_women">女</label>
            <span class="info" id="gender_info"></span>
        <br/>

        <label for="city">城市: </label>
            <select id="city" name="city">
                <option value="">请选择</option>
                <option value="bj">北京</option>
                <option value="sh">上海</option>
                <option value="gz">广州</option>
                <option value="sz">深圳</option>
            </select>
            <span class="info" id="city_info"></span>
        <br/>

        <input type="button" class="btn" value="登录">
    </form>
</body>
</html>
```



## 三. 发送ajax请求

- 什么是ajax？

  - AJAX = 异步的javascript和XML（Asynchronous Javascript and XML）

  - 它不是一门编程语言，而是利用JavaScript在保证页面不被刷新、页面链接不改变的情况下与服务器交换数据并更新部分网页的技术。

  - 对于传统的网页，如果想更新内容，那么必须要刷新整个页面，但有了Ajax，便可以在页面不被全部刷新的情况下更新其内容。在这个过程中，页面实际上是在后台与服务器进行了数据交互，获得数据之后，再利用JavaScript改变页面，这样页面内容就会更新了。

  - 简言之，在不重载整个网页的情况下，AJAX通过后台加载数据，并在网页上进行显示。

  - 通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON - 同时您能够把这些外部数据直接载入网页的被选元素中。

  - ```js
    //get()方式
      $.ajax({
         url:'./data/index.txt',
         type:'get',
         success:function(data){
            $('p').html(data);
    
         },
         error:function(error){
            console.log(error)
         }})
    ```

  - ```js
    //post()方式
    $.ajax({
       url:'/index',
       type:'post',
       data:{name:'张三',age:12},//post的请求参数
       success:function(dd){
          $('p').html(dd);
       },
       error:function(error){
          console.log(error)
    }
    ```

  - ```js
    // 创建一个新的XMLHttpRequest对象
    let xhr = new XMLHttpRequest();
    
    // 设置请求方法和URL
    xhr.open('GET', 'https://xxx.com', true);
    
    // 设置请求完成时的回调函数
    xhr.onload = function() {
      if (xhr.status >= 200 && xhr.status < 400) {
        // 请求成功，处理返回的数据
        let data = JSON.parse(xhr.responseText);
        console.log(data);
      } else {
        // 请求失败，处理错误
        console.error('请求失败，状态码：' + xhr.status);
      }
    };
    
    // 设置请求过程中发生错误的回调函数
    xhr.onerror = function() {
      console.error('请求过程中发生错误');
    };
    
    // 发送请求
    xhr.send();
    
    ```



# Javascript进阶



## 一. 变量提升(不正常现象)

看以下代码, 或多或少会有些问题的.

```js
function fn(){
    console.log(name);
    var name = '大马猴';
}
fn()
```

发现问题了么. 这么写代码, 在其他语言里. 绝对是不允许的. 但是在js里. 不但允许, 还能执行. 为什么呢?  因为在js执行的时候. 它会首先检测你的代码.  发现在代码中会有name使用. OK. 运行时就会变成这样的逻辑:

```js
function fn(){
    var name;
    console.log(name);
    name = '大马猴';
}
fn()
console.log(a);
```

看到了么. 实际运行的时候和我们写代码的顺序可能会不一样....这种把变量提前到代码块第一部分运行的逻辑被称为**变量提升**. 这在其他语言里是绝对没有的. 并且也不是什么好事情. 正常的逻辑不应该是这样的. 那么怎么办?  在新的ES6中. 就明确了, 这样使用变量是不完善的. es6提出. 用let来声明变量. 就不会出现该问题了. 

```js
function fn(){
    console.log(name);  // 直接报错, let变量不可以变量提升.
    let name = '大马猴'; 
}
fn()
```

== 结论一, 用let声明变量是新版本javascript提倡的一种声明变量的方案.== 

let还有哪些作用呢?  

```js
function fn(){
    // console.log(name);  // 直接报错, let变量不可以变量提升.
    // let name = '大马猴';
    var name = "周杰伦";
    var name = "王力宏";
    console.log(name);
}
fn()
```

显然一个变量被声明了两次. 这样也是不合理的. var本意是声明变量. 同一个东西. 被声明两次. 所以ES6规定. let声明的变量. 在同一个作用域内. 只能声明一次. 

```js
function fn(){
    // console.log(name);  // 直接报错, let变量不可以变量提升.
    // let name = '大马猴';
    let name = "周杰伦";
    console.log(name);
    let name = "王力宏";
    console.log(name);
}
fn()
```

注意, 报错是发生在代码检查阶段. 所以. 上述代码根本就执行不了. 

==结论二, 在同一个作用域内. let声明的变量只能声明一次. 其他使用上和var没有差别==



## 二. 闭包函数

我们先看一段代码. 

```js
let name = "周杰伦";
function chi(){
    name = "吃掉";
}
chi();
console.log(name);
```

发现没有, 在函数内部想要修改外部的变量是十分容易的一件事. 尤其是全局变量. 这是非常危险的.  试想, 我写了一个函数. 要用到name, 结果被别人写的某个函数给修改掉了. 多难受. 

接下来. 我们来看一个案例:  

同时运行下面两组代码：

```js
// 1号工具人.
var name = "alex"

setTimeout(function(){
    console.log("一号工具人:"+name) // 一号工具人还以为是alex呢, 但是该变量是不安全的.
}, 5000);

```

```js
// 2号工具人
var name = "周杰伦"
console.log("二号工具人", name);
```

两组代码是在同一个空间内执行的. 他们拥有相同的作用域. 此时的变量势必是非常非常不安全的. 那么如何来解决呢?  注意, 在js里. 变量是有作用域的. 也就是说一个变量的声明和使用是有范围的. 不是无限的. 这一点, 很容易验证.

```js
function fn(){
    let love = "爱呀"
}
fn()
console.log(love)
```

直接就报错了.  也就是说. 在js里是有全局和局部的概念的. 

直接声明在最外层的变量就是全局变量. 所有函数, 所有代码块都可以共享的. 但是反过来就不是了. 在函数内和代码块内声明的变量. 它是一个局部变量. 外界是无法进行访问的. 我们就可以利用这一点来给每个工具人创建一个局部空间. 就像这样:

```js
// 1号工具人.
(function(){
    var name = "alex";
    setTimeout(function(){
        console.log("一号工具人:"+name) // 一号工具人还以为是alex呢, 但是该变量是不安全的.
    }, 5000);
})();

// 二号工具人
(function(){
    var name = "周杰伦"
    console.log("二号工具人", name);
})();
```

这样虽然解决了变量的冲突问题. 但是...我们想想. 如果在函数外面需要函数内部的一些东西来帮我进行相关操作怎么办...比如, 一号工具人要提供一个功能(加密). 外界要调用. 怎么办?  

```js
// 1号工具人.
let encrypt_tool = (function(){
    let log_msg = '开始加密......\n'
    // 我是一个加密函数
    let encrypt = function(data){  // 数据
        console.log(log_msg) //打印日志信息（访问外部变量）
        // 返回密文
        return atob(data);
    }
    // 外面需要用到这个功能啊. 你得把这个东东返回啊. 返回加密函数
    return encrypt;
})();

//外部调用
console.log(encrypt_tool('i love you'));
```

注意了. 我们如果封装一个加密js包的时候. 好像还得准备出解密的功能. 并且, 不可能一个js包就一个功能吧. 那怎么办?  我们可以返回一个对象. 对象里面可以存放好多个功能. 而一些不希望外界触碰的功能. 就可以很好的保护起来. 

```js
// 1号工具人.
let encrypt_tool = (function(){
    let log_msg_1 = '开始加密......'
    let log_msg_2 = '开始解密......'

    // 我是一个加密函数
    let encrypt = function(data){  // 被加密数据
        console.log(log_msg_1) //打印日志信息（访问外部变量）
        // 返回密文
        return atob(data);
    };
    //解密函数
    let decrypt = function(en_data){  // 加密后的数据
        console.log(log_msg_2) //打印日志信息（访问外部变量）
        // 返回解密后的原数据
        return btoa(en_data);
    };
    // 外面需要用到这个功能啊. 你得把这个东东返回啊. 返回加密函数
    return {'encrypt':encrypt,'decrypt':decrypt};
})();

//外部调用
en_data = encrypt_tool.encrypt('i love you');
de_data = encrypt_tool.decrypt(en_data)
console.log(en_data);
console.log(de_data);
```



OK. 至此. 何为闭包? 上面这个就是闭包. 相信你百度一下就会知道. 什么内层函数使用外层函数变量. 什么让一个变量常驻内存.等等. 其实你细看. 它之所以称之为闭包~. 它是一个封闭的环境. 在内部. 自己和自己玩儿. 避免了对该模块内部的冲击和改动. 避免的变量之间的冲突问题. 

闭包的特点:

1. 内层函数对外层函数变量的使用. 
2. 会让变量常驻与内存. 





## 三. JS中的各种操作(非交互)

### 3.1 定时器

在JS中, 有两种设置定时器的方案

```js
// 语法规则
t = setTimeout(函数, 时间)
// 经过xxx时间后, 执行xxx函数

// 示例：3秒后打印我爱你
t = setTimeout(function(){
    console.log("我爱你")
}, 3000);

window.clearTimeout(t)  // 停止一个定时器
```

### 3.2 eval函数(必须会. 隔壁村老太太都会.)

eval本身在js里面正常情况下使用的并不多. 但是很多网站会利用eval的特性来完成反爬操作. 我们来看看eval是个什么鬼?  

从功能上讲, eval非常简单. 它和python里面的eval是一样的. 它可以动态的把字符串当成js代码进行运行. 

```js
s = "console.log('我爱你')";
eval(s);
```

也就是说. eval里面传递的应该是即将要执行的代码(字符串). 那么在页面中如果看到了eval加密该如何是好?  其实只要记住了一个事儿. 它里面不论多复杂. 一定是个字符串. 

比如, 

```js
eval(function(p,a,c,k,e,d){e=function(c){return(c<a?'':e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--)d[e(c)]=k[c]||e(c);k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('0.1(\'我爱你\')',62,2,'console|log'.split('|'),0,{}))
```

这一坨看起来, 肯定很不爽. 怎么变成我们看着很舒服的样子呢?  记住. eval()里面是字符串. 记住~!!

那我想看看这个字符串长什么样?  就把eval()里面的东西拷贝出来. 执行一下. 最终一定会得到一个字符串. 要不然eval()执行不了的. 对不...于是就有了下面的操作.

![image-20210828160541107](D:\BaiduNetdiskDownload\七期\day09-js语法实操04\imgs\image-20210828160541107.png)

http://tools.jb51.net/password/evalencode, 在赠送你一个在线JS处理eval的网站. 大多数的eval加密. 都可以搞定了. 



### 3.3 prototype和\__proto__

逆向代码示例：

```js
function ct(t) {
    pv['$_BCAO'] = t || [];
}

ct.prototype = {
    '$_CAE':function(f) {
               f();
            }
} 

var H = new ct(t)['$_CAE'](function(t) {
        var e = function(t) {
            for (var e = [[1, 0], [2, 0], [1, -1], [1, 1], [0, 1], [0, -1], [3, 0], [2, -1], [2, 1]], r = e['length'],n = 0; n < r; n++){
                var b = 'stuvwxyz~';
            }   
            return b;
        };
    	return e;
    })


```

- 原型

  - 概念：原型本身其实是一个function函数，可以将其理解成Python中的class类。

  - 创建一个原型：

    ```js
    function User(name,pwd){
        this.userName = name;
        this.pwd = pwd;
        this.regist = function(){
            console.log(this.name+"在注册")
        }
    }
    ```

- 实例对象

  - 概念：通过new关键字创建的对象称为实例对象。

  - 创建实例对象：

    ```js
    let u1 = User('jay','123');
    let u2 = User('tom','456');
    ```

- 原型对象

  - 概念：原型对象用于存储所有实例对象共享的属性和方法，以减少每个实例对象重复存储相同属性和方法的开销。

  - 原型对象存储所有实例对象共享的属性和方法

    ```js
    //类似于类属性
    User.prototype.address = "BJ";
    User.prototype.gender = "male";
    //类似于类方法
    User.prototype.login = function login(username, password){
        console.log(`${username}在登录`);
    }
    
    //实例对象共享原型对象存储的内容
    u1.login('jay','123');
    u2.login('tom','456');
    console.log(u1.address,u2.address,u1.gender,u2.gender);
    ```

  - 获取原型对象：

    ```js
    User.prototype;   //类似于访问所有的类成员
    u1.__proto__;
    User.prototype === u1.__proto__ //true
    //可以通过原型名访问原型对象或者使用实例名访问原型对象
    ```

- 原型链

  - 原型链是**JavaScript中对象继承属性和方法的一种方式**。具体介绍如下：

    ```
    原型链是JavaScript中对象继承属性和方法的一种方式。当访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，它会通过原型链去它的原型对象中查找，如果原型对象也没有，会继续在其原型的原型中查找，这样逐级向上，直到找到属性或方法或者达到原型链的末端。
    ```

    原型对象本身也是一个对象，它也可以使用\__proto__访问它的原型对象，类似于：

    ```js
    u1.__proto__.__proto__
    ```

  - 原型链的成员访问：

    - 实例对象可以访问其原型内的成员和其原型链上所有原型对象内的成员

    ```js
    User.toString()
    ```

- 浏览器环境navigator分析

  <img src="D:\BaiduNetdiskDownload\七期\day09-js语法实操04\imgs\Snipaste_2024-07-11_09-24-19.png" alt="Snipaste_2024-07-11_09-24-19" style="zoom:75%;" />




练习测试：理解下属代码

```js
function ct(t) {
    pv['$_BCAO'] = t || [];
}

var H = new ct(t)['$_CAE'](function(t) {
        var e = function(t) {
            for (var e = [[1, 0], [2, 0], [1, -1], [1, 1], [0, 1], [0, -1], [3, 0], [2, -1], [2, 1]], r = e['length'],n = 0; n < r; n++){
                var b = 'stuvwxyz~';
            }   
            return b;
        };
    	return e;
    })

ct.prototype = {
    '$_CAE':function(f) {
               f();
            }
}
```

### 3.4 神奇的window（浏览器环境）

window对象是一个很神奇的东西. 你可以把这东西理解成javascript的全局. 如果我们默认不用任何东西访问一个标识符. 那么默认认为是在用window对象进行访问该标识符. 

例如: 

```js
var a = 10; 
a === window.a  // true

window.mm = "爱你"
console.log(mm); //"爱你"
```

综上,  我们可以得出一个结论. 全局变量可以用window.xxx来表示. 

注意:window对象实际上表示的是浏览器的窗口。浏览器的窗口只有浏览器有。我们在浏览器中的开发者工具的Console选项卡中是可以直接使用window对象。

但是：在pycharm中直接使用window对象会报错：ReferenceError: window is not defined。why？就是因为在pycharm中是没有浏览器窗口。

所以切记：window对象是专属于浏览器环境下的一个内置对象。就意味着window只可以在浏览器环境下无需声明直接被使用。

ok. 接下来. 注意看了. 我要搞事情了

```js
//如果想在外部调用下面函数的中的chi函数如何实现？
(function(){
    let chi = function(){
        console.log("我是吃")
    }
})();
chi() //会报错，因为chi是一个局部变量

//正确写法：
(function(){
    let chi = function(){
        console.log("我是吃")
    }
    window.chi = chi //全局的
})();
chi();

//换一种写法. 你还认识么?
(function(w){
    let chi = function(){
        console.log("我是吃")
    }
    w.chi = chi
})(window);
```

如何在node环境下执行上述代码？

```
globalThis在浏览器和node中都可以使用。在Pycharm中就可以使用globalThis代替window用于全局变量的声明

//全局变量的声明
globalThis.a = 10;
console.log(a);


```

window是整个浏览器的全局作用域. 

使用 window 对象也可以访问客户端其他对象，这种关系构成浏览器对象模型，window 对象代表根节点，浏览器对象关系的关系如图所示，每个对象说明如下。

- window：客户端 JavaScript 顶层对象。每当 <body> 或 <frameset> 标签出现时，window 对象就会被自动创建。
- navigator：包含客户端有关浏览器信息。
- screen：包含客户端屏幕的信息。
- history：包含浏览器窗口访问过的 URL 信息。
- location：包含当前网页文档的 URL 信息。
- document：包含整个 HTML 文档，可被用来访问文档内容及其所有页面元素。

<img src="D:\BaiduNetdiskDownload\七期\day09-js语法实操04\imgs\image-20240528194905528-6896946.png" alt="image-20240528194905528-6896946" style="zoom:50%;" />



### 3.5 call和apply

对于咱们逆向工程师而言. 并不需要深入的理解call和apply的本质作用. 只需要知道这玩意执行起来的逻辑顺序是什么即可（外部函数关联对象的内部成员）

在运行时. 正常的js调用:

```js
function People(name, age){
    this.name = name;
    this.age = age;
    this.chi = function(){
        console.log(this.name, "在吃东西")
    }
}
var p1 = new People("alex", 18);
var p2 = new People("wusir", 20);
p1.chi();
p2.chi();
```

接下来, 我们可以使用call和apply也完成同样的函数调用

```js
function People(name, age){
    this.name = name;
    this.age = age;
    this.chi = function(){
        console.log(this.name, "在吃东西")
    }
}
p1 = new People("alex", 18);
p2 = new People("wusir", 20);
p1.chi();
p2.chi();

//外部函数关联对象的内部函数：
	//想要让alx吃："馒头", "大饼"，如何调用下面的函数？
function eat(what_1, what_2){
    console.log(this.name, "在吃", what_1, what_2);
}
// call的语法是: 函数.call(对象, 参数1, 参数2, 参数3....)
// 执行逻辑是: 执行函数. 并把对象传递给函数中的this.  其他参数照常传递给函数
eat.call(p1, "馒头", "大饼");
```

apply和他几乎一模一样. 区别是: apply传递参数要求是一个数组

```js
eat.apply(p1, ["馒头", "大饼"]);
```



### 3.6 ES6中的箭头函数

在ES6中简化了函数的声明语法. 

```js
var fn = function(){};
// 如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
var fn = () => {};

var fn = function(name){}
var fn = name => {}
var fn = (name) => {}

var fn = function(name, age){}
var fn = (name, age) => {}

var f = v => v*v
// 等同于
var f = function(v) {
 return v*v
}

```



### 3.7 exports

类似Python中的模块导入

```js
// functions.js文件

// 加法函数
function add(a, b) {
  return a + b;
}

// 乘法函数
function multiply(a, b) {
  return a * b;
}

// 导出函数
exports.add = add;
exports.multiply = multiply;
```

```js
// main.js

// 导入 functions 模块
const functions = require('./functions');

// 使用导入的函数
console.log(functions.add(2, 3));  // 输出: 5
console.log(functions.multiply(4, 5));  // 输出: 20
```

## 四. DOM对象(了解)

DOM  document Object Model 文档对象模型

```js
// 整个html文档,会保存一个文档对象document
// console.log( document ); // 获取当前文档的对象
```

### 查找标签

* 直接查找标签

```js
document.getElementsByTagName("标签名")
document.getElementById("id值")
document.getElementsByClassName("类名")
//返回dom对象，就是标签对象或者数组
```

* CSS选择器查找

```JS
document.querySelector("css选择器")  //根据css选择符来获取查找到的第一个元素，返回标签对象（dom对象）
document.querySelectorAll("css选择器"); // 根据css选择符来获取查找到的所有元素,返回数组
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>


<div id="i1">DIV1</div>

<div class="c1">DIV</div>
<div class="c1">DIV</div>
<div class="c1">DIV</div>


<div class="outer">
    <div class="c1">item</div>
</div>


<div class="c2">
    <div class="c3">
        <ul class="c4">
            <li class="c5" id="i2">111</li>
            <li>222</li>
            <li>333</li>
        </ul>
    </div>
</div>

<script>

   // 直接查找
   var ele = document.getElementById("i1");  // ele就是一个dom对象
   ele.innerText = 'hello bobo'; //将该标签内容修改成了hello bobo
   console.log(ele);

   var eles = document.getElementsByClassName("c1"); // eles是一个数组 [dom1,dom2,...]
   console.log(eles);

   var eles2 = document.getElementsByTagName("div"); // eles2是一个数组 [dom1,dom2,...]
   console.log(eles2);

  //定位outer下的c1对应的标签
   var outer = document.getElementsByClassName("outer")[0];
   var te = outer.getElementsByClassName("c1");
   console.log(te);

    // css选择器
		//层级定位(空格可以表示一个或多个层级)
    var dom = document.querySelector(".c2 .c3 .c5");
    console.log(":::",dom);
		//层级定位
    var doms = document.querySelectorAll("ul li");
    console.log(":::",doms);
    
</script>

</body>
</html>
```

### 绑定事件

* 静态绑定 ：直接把事件写在标签元素中

```html
<div id="div" onclick="foo()">click</div>

<script>
    function foo(){           
        console.log("foo函数");   
    }
</script>

```

* 动态绑定：在js中通过代码获取元素对象,然后给这个对象进行后续绑定

```html
<p id="i1">试一试!</p>

<script>

    var ele=document.getElementById("i1");

    ele.onclick=function(){
        console.log("ok");
        
    };

</script>
```



## 五. jQuery和Ajax（了解）

jQuery是一个曾经火遍大江南北的一个Javascript的第三方库. jQuery的理念: write less do more.  其含义就是让前端程序员从繁琐的js代码中解脱出来. 我们来看看是否真的能解脱出来. 

python  => 基础语法 => 系统模块 => 第三方的库

jQuery的下载：

​	jquery:

​	 https://lf6-cdn-tos.bytecdntp.com/cdn/expire-1-M/jquery/1.9.1/jquery.js

 	https://code.jquery.com/jquery-3.6.0.min.js

只需要把上面这个jquery下载的网址复制到浏览器上, 然后保存(ctrl+s)成js文件就可以了.

## 一. jQuery初使用

导入jQuery

```js
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
```

我们用jQuery来完成一个按钮的基本点击效果. 当然, 得和传统的js对比一下

先准备好html. 页面结构. 这里复制粘贴就好

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
</head>
<body>
    <div class="div-out">
        <input type="button" class="btn" value="我是按钮. 你怕不怕">
        <div class="mydiv">我怕死了...</div>
    </div>
</body>
</html>
```

需求: 点击按钮. 更改mydiv中的内容. 

```js
// 传统js
window.onload = function(){
    document.querySelector(".btn").onclick = function(){
        document.querySelector('.mydiv').innerText = "我好啪啪啊";
    };
}
```

```js
// jQuery
$(function(){  // $(document).ready(function(){
    $(".btn").click(function(){
        $(".mydiv").text('我要上天');
    })
})
```

除了`$`外, 其他东西貌似都挺容易理解的.  而且代码简洁. 异常舒爽. 

`$`是什么鬼, 在jQuery中, `$`可以认为是jQuery最明显的一个标志了. `$()`可以把一个普通的js对象转化成jQuery对象. 并且在jquery中 \$的含义就是jQuery. 



## 二. jQuery选择器

jQuery的逻辑和css选择器的逻辑是一样的. 

```js
// 语法:
$(选择器)
```

可以使用jQuery选择器快速的对页面结构进行操作. 

案例:  

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
	<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>

        $(function(){
            $(".btn").on('click', function(){
                $(".info").text("");
                let username = $("#username").val();
                let password = $("#password").val();
                let gender = $("input:radio[name='gender']:checked").val();  // input标签中radio 并且name是gender的. 并且被选择的.
                let city = $("#city").val();

                let flag = true;
                if(!username){
                    $("#username_info").text('用户名不能为空!');
                    flag = false;
                }

                if(!password){
                    $("#password_info").text('密码不能为空!');
                    flag = false;
                }

                if(!gender){
                    $("#gender_info").text('请选择性别!');
                    flag = false;
                }

                if(!city){
                    $("#city_info").text('请选择城市!');
                    flag = false;
                }

                if(flag){
                    $("#login_form").submit();
                } else {
                    return;
                }
            })
        })

    </script>
</head>
<body>
    <form id="login_form">
        <label for="username">用户名: </label><input type="text" id="username" name="username"><span class="info" id="username_info"></span><br/>
        <label for="password">密码: </label><input type="password" id="password" name="password"><span class="info" id="password_info"></span><br/>
        <label>性别: </label>
            <input type="radio" id="gender_men" name="gender" value="men"><label for="gender_men">男</label>
            <input type="radio" id="gender_women" name="gender" value="women"><label for="gender_women">女</label>
            <span class="info" id="gender_info"></span>
        <br/>

        <label for="city">城市: </label>
            <select id="city" name="city">
                <option value="">请选择</option>
                <option value="bj">北京</option>
                <option value="sh">上海</option>
                <option value="gz">广州</option>
                <option value="sz">深圳</option>
            </select>
            <span class="info" id="city_info"></span>
        <br/>

        <input type="button" class="btn" value="登录">
    </form>
</body>
</html>
```



## 三. 发送ajax请求

- 什么是ajax？

  - AJAX = 异步的javascript和XML（Asynchronous Javascript and XML）

  - 它不是一门编程语言，而是利用JavaScript在保证页面不被刷新、页面链接不改变的情况下与服务器交换数据并更新部分网页的技术。

  - 对于传统的网页，如果想更新内容，那么必须要刷新整个页面，但有了Ajax，便可以在页面不被全部刷新的情况下更新其内容。在这个过程中，页面实际上是在后台与服务器进行了数据交互，获得数据之后，再利用JavaScript改变页面，这样页面内容就会更新了。

  - 简言之，在不重载整个网页的情况下，AJAX通过后台加载数据，并在网页上进行显示。

  - 通过 jQuery AJAX 方法，您能够使用 HTTP Get 和 HTTP Post 从远程服务器上请求文本、HTML、XML 或 JSON - 同时您能够把这些外部数据直接载入网页的被选元素中。

  - ```js
    //get()方式
      $.ajax({
         url:'./data/index.txt',
         type:'get',
          //当请求成功后，执行下面的匿名函数，函数参数data就是请求到的数据
         success:function(data){
            $('p').html(data);
    
         },
         error:function(error){
            console.log(error)
         }})
    ```

  - ```js
    //post()方式
    $.ajax({
       url:'/index',
       type:'post',
       data:{name:'张三',age:12},//post的请求参数
       success:function(dd){
          $('p').html(dd);
       },
       error:function(error){
          console.log(error)
    }
    ```

  - ```js
    // 创建一个新的XMLHttpRequest对象
    let xhr = new XMLHttpRequest();
    
    // 设置请求方法和URL
    xhr.open('GET', 'https://xxx.com', true);
    
    // 设置请求完成时的回调函数
    xhr.onload = function() {
      if (xhr.status >= 200 && xhr.status < 400) {
        // 请求成功，处理返回的数据
        let data = JSON.parse(xhr.responseText);
        console.log(data);
      } else {
        // 请求失败，处理错误
        console.error('请求失败，状态码：' + xhr.status);
      }
    };
    
    // 设置请求过程中发生错误的回调函数
    xhr.onerror = function() {
      console.error('请求过程中发生错误');
    };
    
    // 发送请求
    xhr.send();
    
    ```

案例：https://www.endata.com.cn/BoxOffice/BO/Year/index.html

### NodeJs

#### V8引擎

我们知道，js是一种可以直接运行在浏览器中的脚本语言。那么为什么浏览器可以直接运行js脚本程序呢？原因就在于浏览器中内置了“V8”引擎。

什么是V8引擎？

```
V8引擎是一款专门解释和执行JavaScript代码的虚拟机。任何程序只要集成了V8引擎，就可以执行JavaScript代码。浏览器集成了V8引擎，可以执行JavaScript代码；
```

#### Node.js

由于浏览器内部集成了V8引擎，因此js程序可以直接运行在浏览器中。那么，如果想要在浏览器外部运行js代码可以吗？

试想一下，如果我们想在自己电脑上运行Python代码，是不是必须事先要装好Pyhon的运行环境（Python解释器）呢。同样，如果我们想在自己电脑上运行js程序的话，也要事先装好js的运行环境。那么这个执行js程序的运行环境就是“Node.js”。

什么是Node.js?

```
Node.js是一个基于Chrome V8引擎的JavaScript运行时环境，它使得JavaScript能够在浏览器外部运行。Node.js的出现将JavaScript从浏览器中解放出来。
```

#### Node.js环境安装

1、下载Node.js
打开官网下载链接:**https://nodejs.org/zh-cn/**
<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\Snipaste_2024-07-14_18-02-25.png" alt="Snipaste_2024-07-14_18-02-25" style="zoom:50%;" />

2、安装

下载完成后，双击“node-v12.16.1-x64.msi”，开始安装Node.js
①点击【Next】按钮
②勾选复选框（I accept the…），点击【Next】按钮
<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\Snipaste_2024-07-14_18-05-06.png" alt="Snipaste_2024-07-14_18-05-06" style="zoom:50%;" />

一直【next】就完了，然后【install】安装，安装完后点击【Finish】按钮完成安装

<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\Snipaste_2024-07-14_18-05-40.png" alt="Snipaste_2024-07-14_18-05-40" style="zoom:50%;" />

安装成功后简单一下测试安装是否成功：

在键盘按下【win+R】键，输入cmd，然后回车，打开cmd窗口

<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\Snipaste_2024-07-14_18-07-39.png" alt="Snipaste_2024-07-14_18-07-39" style="zoom:75%;" />

3、环境变量配置

<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\Snipaste_2024-07-14_18-10-32.png" alt="Snipaste_2024-07-14_18-10-32" style="zoom:75%;" />



4、pycharm配置node环境

1. 打开PyCharm，点击菜单栏中的“File” > “Settings”（对于Mac用户，是“PyCharm” > “Preferences”）。
2. 在弹出的设置窗口中，选择“Languages & Frameworks” > “Node.js and NPM”。
3. 在“Node Interpreter”字段中，点击右侧的“…”按钮，然后选择您安装Node.js的路径。这通常是在您的系统路径下的“node”可执行文件。
4. 确认设置无误后，点击“OK”保存配置。

<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\Snipaste_2024-07-18_19-17-19.png" alt="Snipaste_2024-07-18_19-17-19" style="zoom:50%;" />

### base64编码

#### base64是什么

Base64编码，是由64个字符组成编码集：**26个大写字母A~Z，26个小写字母a~z，10个数字0~9，符号“+”与符号“/”**。Base64编码的基本思路是**将原始数据的三个字节拆分转化为四个字节**，然后根据Base64的对应表，得到对应的编码数据。

当原始数据凑不够三个字节时，编码结果中会使用额外的**符号“=”**来表示这种情况。

#### base64原理

每一个base64的字符会对应有一个索引值（0-63）

<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\image-20230916190327552-4862209.png" alt="image-20230916190327552-4862209" style="zoom:67%;" />

将you进行base64编码过程如下：

<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\image-20230427174804644-4862253-4862254.png" alt="image-20230427174804644-4862253-4862254" style="zoom: 80%;" />

小于3个字符为一组的编码方式如：

![image-20230427174356120-4862276-4862277](D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\image-20230427174356120-4862276-4862277.png)

ASCII：

<img src="D:\BaiduNetdiskDownload\七期\day10-js语法实操05\imgs\Snipaste_2024-07-21_09-13-37.png" alt="Snipaste_2024-07-21_09-13-37" style="zoom:75%;" />

#### base64测试

base64编码示例：

```python
import base64

# 将原始数据转化为二进制/字节数据
data = "you".encode("utf-8")
print(data)

# 把字节转化成b64
bs = base64.b64encode(data).decode()
print(bs)

bs = "yo".encode("utf-8")
# 把字节转化成b64
print(base64.b64encode(bs).decode())

# 猜测结果
bs = "y".encode("utf-8")
# 把字节转化成b64
print(base64.b64encode(bs).decode())
```

base64解码示例：

注意, base64编码处理后的字符串长度. 一定是4的倍数（因为Base64编码的基本思路是**将原始数据的三个字节拆分转化为四个字节**）. 如果在网页上看到有些密文的b64长度不是4的倍数. 会报错

```python
import base64

s = "eW91"
ret = base64.b64decode(s)
print(ret) #正确

s = "eW91eQ=="
ret = base64.b64decode(s)
print(ret) #正确

s = "eW91eQ"
ret = base64.b64decode(s)
print(ret) #报错，s不是4的倍数
```

如果不是4的倍数如何处理呢？解决思路. 使用=填充为4的倍数即可

```python
s = "eW91eQ"
#填充为4的倍数
s += ("=" * (4 - len(s) % 4))
print("填充后", s)
ret = base64.b64decode(s).decode()
print(ret)
```

base64 编码的优点：

- 算法是编码，不是压缩，编码后只会增加字节数（一般是比之前的多1/3，比如之前是3， 编码后是4）
- 算法简单，基本不影响效率
- 算法可逆，解码很方便，不用于私密传输。

### js常见的加密方式

- 加密在前端开发和爬虫中是经常遇见的。掌握了加密、解密算法也是你从一个编程小白到大神级别质的一个飞跃。且加密算法的熟练和剖析也是很有助于帮助我们实现高效的js逆向。下述只把我们常用的加密方法进行总结。不去深究加密的具体实现方式。

- 常见的加密算法基本分为这几类，
  - 线性散列算法（签名算法）MD5
  - 对称性加密算法 AES DES
  - 非对称性加密算法 RSA

### Md5加密（不可逆）

- MD5是一种被广泛使用的线性散列算法，可以产生出一个128位（16字节）的散列值（hash value），用于确保信息传输完整的一致性。且MD5加密之后产生的是一个固定长度（32位或16位）的数据。

  - 结论：一旦看到了一个长度为32位的密文数据，该数据极有可能是通过md5算法进行的加密！

- 解密：

  - 常规讲MD5是不存在解密的。但是理论上MD5是可以进行反向暴力破解的。暴力破解的大致原理就是用很多不同的数据进行加密后跟已有的加密数据进行对比，由此来寻找规律。理论上只要数据量足够庞大MD5是可以被破解的。但是要注意，破解MD5是需要考虑破解的成本（时间和机器性能）。假设破解当前的MD5密码需要目前计算能力最优秀的计算机工作100年才能破解完成。那么当前的MD5密码就是安全的。    

- 增加破解成本的方法（方法很多，这里只说我常用的）。

  - 使用一段无意义且随机的私匙进行MD5加密会生成一个加密串，我们暂且称之为串1
  - 将要加密的的数据跟串1拼接，再进行一次MD5，这时会生成串2
  - 将串2再次进行MD5加密，这时生成的串3就是我们加密后的数据。

- 我们在注册账号时的密码一般都是用的MD5加密。

- 代码实操：

  - Python版本：

    ```python
    from hashlib import md5
    
    obj = md5()
    obj.update("bobo".encode("utf-8"))
    
    bs = obj.hexdigest()
    print(bs)
    ```

  - JS版本：下载安装crypto-js（npm install crypto-js）

    ```js
    var CryptoJS = require('crypto-js');
    // 原始数据
    var data = '123456';
    // 生成MD5摘要
    var md5Digest = CryptoJS.MD5(data).toString();
    
    console.log(md5Digest);
    ```


- 上述代码的核心关键字：md5

### DES/AES加密（可逆）

- DES全称为Data Encryption Standard，即数据加密标准，是一种使用密钥加密的算法。该加密算法是一种对称加密方式，其加密运算、解密运算需要使用的是同样的密钥（一组字符串）即可。

- 注意：

  - 现在用AES这个标准来替代原先的DES。
  - AES和DES的区别：
    - 加密后密文长度的不同:
      - DES加密后密文长度是8的整数倍
      - AES加密后密文长度是16的整数倍
    - 应用场景的不同:
      - 企业级开发使用DES足够安全
      - 如果要求高使用AES

- DES算法的入口参数有三个：

  - Key、Data、Mode，padding、iv。
    - Key为DES算法的工作密钥；
    - Data为要被加密或被解密的数据；
    - Mode为DES的工作模式。最常用的模式就是 CBC 模式和 ECB模式
      - ECB：是一种基础的加密方式，密文被分割成分组长度相等的块（不足补齐），然后单独一个个加密，一个个输出组成密文。
      - CBC：是一种循环模式，前一个分组的密文和当前分组的明文异或后再加密，这样做的目的是增强破解难度。
    - padding为填充模式，如果加密后密文长度如果达不到指定整数倍（8个字节、16个字节），填充对应字符
    - iv：参数中的iv主要用于CBC模式，确保即使加密相同的明文，每次产生的密文也不相同，增强加密的安全性。iv通常是一个16字节的随机字符串。这个字符串在解密时也需要用到，因此需要妥善保存。

- Python版本：

  - 环境安装：

    ```
    pip install pycryptodome
    ```

  - 加密代码：

    ```python
    from Crypto.Cipher import AES
    from Crypto.Util.Padding import pad
    import base64
    
    key = '0123456789abcdef'.encode()  # 秘钥: 必须16字节
    iv = b'abcdabcdabcdabcd'  # 偏移量：16位/字节(字节类型)
    text = 'alex is a monkey!'  # 加密内容
    #设置加密内容的长度填充（位数为16的整数倍）
    text = pad(text.encode(), 16)
    #创建加密对象
    aes = AES.new(key, AES.MODE_CBC, iv)  # 创建一个aes对象
    
    en_text = aes.encrypt(text)  # 加密明文
    print("aes加密数据:::", en_text) #返回二进制类型数据
    
    #二进制密文转换成字符串格式
    en_text = base64.b64encode(en_text).decode()  # 将返回的字节型数据转进行base64编码
    print(en_text) 
    ```

  - 解密代码：

    ```python
    from Crypto.Cipher import AES
    import base64
    from Crypto.Util.Padding import unpad
    
    key = '0123456789abcdef'.encode()
    iv = b'abcdabcdabcdabcd'
    aes = AES.new(key, AES.MODE_CBC, iv)
    #需要解密的文本
    text = 'X/A0fy9S7+kUI3HYQRKO46WTlid6T1DBhXutwmPdboY='.encode()  
    #将密文数据转换为二进制类型
    ecrypted_base64 = base64.b64decode(text)  
    
    source = aes.decrypt(ecrypted_base64)  # 解密
    #未填充数据
    print("aes解密数据:::", source.decode())
    #取消填充数据
    print("aes解密数据:::", unpad(source, 16).decode())
    ```

- JS版本：

  - 加密：

    ```js
    const CryptoJS = require("crypto-js")
    
    // 密钥（128位，16字节）
    var key = CryptoJS.enc.Utf8.parse('0123456789abcdef');
    
    // 初始化向量（IV）（128位，16字节）
    var iv = CryptoJS.enc.Utf8.parse('1234567890abcdef');
    
    // 待加密的数据
    var plaintext = 'Hello, bobo!';
    
    // 进行AES-128加密，使用CBC模式和PKCS7填充
    var encrypted = CryptoJS.AES.encrypt(plaintext, key, {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    });
    
    // 获取加密后的密文
    var ciphertext = encrypted.toString();
    
    console.log(ciphertext);
    ```

  - 解密：

    ```js
    const CryptoJS = require("crypto-js")
    
    // 密钥（128位，16字节）
    var key = CryptoJS.enc.Utf8.parse('0123456789abcdef');
    
    // 初始化向量（IV）（128位，16字节）
    var iv = CryptoJS.enc.Utf8.parse('1234567890abcdef');
    
    // 密文数据
    var encrypText = 'GYc9oxlZB/PeyfFG3ppK6Q==';
    
    // 进行加密，使用CBC模式和PKCS7填充
    var decrypted = CryptoJS.AES.decrypt(encrypText, key, {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    });
    
    // 解密
    var plaintext = decrypted.toString(CryptoJS.enc.Utf8);
    
    console.log(plaintext);
    ```

- 上述代码的关键字：

  - AES，DES
  - encrypt(用于加密的函数名)，decrypt(用于解密的函数名)
  - xxxkey：秘钥一般是单独生成的，一定不会傻到明文的写在代码中！

### RSA加密（可逆）

- RSA加密：
  - RSA加密算法是一种非对称加密算法。在公开密钥加密和电子商业中RSA被广泛使用。
- 非对称加密算法：
  - 非对称加密算法需要两个密钥：
    - 公开密钥（publickey:简称公钥）
    - 私有密钥（privatekey:简称私钥）
    - 公钥与私钥是一对，如果用公钥对数据进行加密，只有用对应的私钥才能解密。因为加密和解密使用的是两个不同的密钥，所以这种算法叫作非对称加密算法。
- 注意：
  - 使用时都是使用公匙加密使用私匙解密。公匙可以公开，私匙自己保留。
  - 算法强度复杂、安全性依赖于算法与密钥但是由于其算法复杂，而使得加密解密速度没有对称加密解密的速度快。
- 使用流程和场景介绍
  - 通过公匙加密，使用私匙解密。私匙是通过公匙计算生成的。假设ABC三方之间相互要进行加密通信。大家相互之间使用公匙进行信息加密，信息读取时使用各自对应的私匙进行信息解密
  - 用户输入的支付密码会通过RSA加密
- 公钥私钥生成方式：
  - 公私匙可以在线生成
    - http://web.chacuo.net/netrsakeypair
- 环境安装：npm install jsencrypt

JS代码示例：

```js
window = globalThis;

const JSEncrypt = require('jsencrypt');


var PUBLIC_KEY = '-----BEGIN PUBLIC KEY-----MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBALyBJ6kZ/VFJYTV3vOC07jqWIqgyvHulv6us/8wzlSBqQ2+eOTX7s5zKfXY40yZWDoCaIGk+tP/sc0D6dQzjaxECAwEAAQ==-----END PUBLIC KEY-----';
        //私钥
var PRIVATE_KEY = '-----BEGIN PRIVATE KEY-----MIIBVQIBADANBgkqhkiG9w0BAQEFAASCAT8wggE7AgEAAkEAvIEnqRn9UUlhNXe84LTuOpYiqDK8e6W/q6z/zDOVIGpDb545NfuznMp9djjTJlYOgJogaT60/+xzQPp1DONrEQIDAQABAkEAu7DFsqQEDDnKJpiwYfUE9ySiIWNTNLJWZDN/Bu2dYIV4DO2A5aHZfMe48rga5BkoWq2LALlY3tqsOFTe3M6yoQIhAOSfSAU3H6jIOnlEiZabUrVGqiFLCb5Ut3Jz9NN+5p59AiEA0xQDMrxWBBJ9BYq6RRY4pXwa/MthX/8Hy+3GnvNw/yUCIG/3Ee578KVYakq5pih8KSVeVjO37C2qj60d3Ok3XPqBAiEAqGPvxTsAuBDz0kcBIPqASGzArumljkrLsoHHkakOfU0CIDuhxKQwHlXFDO79ppYAPcVO3bph672qGD84YUaHF+pQ-----END PRIVATE KEY-----';
//使用公钥加密
var encrypt = new JSEncrypt();//实例化加密对象
encrypt.setPublicKey(PUBLIC_KEY);//设置公钥
var encrypted = encrypt.encrypt('hello bobo!');//对指定数据进行加密
console.log(encrypted);//使用私钥解密

//使用私钥解密
var decrypt = new JSEncrypt();
decrypt.setPrivateKey(PRIVATE_KEY);//设置私钥
var uncrypted = decrypt.decrypt(encrypted);//解密
console.log(uncrypted);
```

Python代码示例：

1.创建公钥和私钥

```python
from Crypto.PublicKey import RSA

# 通过相关算法生成唯一秘钥
rsakey = RSA.generate(1024)
#将秘钥保存到文件中
with open("rsa.public.pem", mode="wb") as f:
    f.write(rsakey.publickey().exportKey())

with open("rsa.private.pem", mode="wb") as f:
    f.write(rsakey.exportKey())
```

2.加密算法实现

```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_v1_5
import base64

# 加密
data = "我喜欢你"
with open("rsa.public.pem", mode="r") as f:
    pk = f.read()
    rsa_pk = RSA.importKey(pk)
    #基于公钥创建加密对象
    rsa = PKCS1_v1_5.new(rsa_pk)

    result = rsa.encrypt(data.encode("utf-8"))
    # 处理成b64方便传输
    b64_result = base64.b64encode(result).decode("utf-8")
    print(b64_result)

```

3.解密算法实现

```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_v1_5
import base64

data = '17wJJtBBujJurHRu3teDC44jSllmqcB9a22U1DQ5/IsI0m3NuShPTxBq8pLTWRdszPapiXfjvOh47P9u5+7U4hvNZL+kKqs3H5m4BKKphORUUfqb9zTowoHb1mM9ji2LuMAYmc1l70ZR+s9XsV3HrHCV/S2FzGWngcWpNVtsTJg='
# 解密
with open("rsa.private.pem", mode="r") as f:
    prikey = f.read()
    rsa_pk = RSA.importKey(prikey)
    #创建解密对象
    rsa = PKCS1_v1_5.new(rsa_pk)
    result = rsa.decrypt(base64.b64decode(data), None)
    print("rsa解密数据:::", result.decode("utf-8"))
```

- 上述代码提取关键字：
  - RSA
  - setPublicKey（设置公钥）
  - setPrivateKey（设置私钥）
  - encrypt,decrypt

### PyExecJS介绍

- PyExecJS 是一个可以使用 Python 来模拟运行 JavaScript 的库。
  - 使用该模块可以通过python程序调用执行js代码，获取js代码返回的结果！
  - 注意事项：电脑必须安装好了nodejs开发环境上述模块才可以生效！
- 环境安装：
  - pip install PyExecJS

- 使用步骤：
  - 导包：
    - import execjs
  - 创建node对象：
    - node = execjs.get()
  - 编译即将被执行的js代码对应的文件，返回上下文对象ctx
    - fp = open(filePath,encoding='utf-8')
    - ctx = node.compile(fp.read())
  - 生成要执行的js函数调用的字符串形式
    - funName = 'getPwd("xxx")'
  - 基于ctx调用eval函数，模拟执行funName表示的js函数
    - result = ctx.eval(funName)

```python
#1.导包
import execjs
#2.创建node对象
node = execjs.get()
#3.编译js文件返回上下文ctx对象(将js文件中的代码读取出来，被compile进行编译)
fp = open('test.js','r',encoding='utf-8')
ctx = node.compile(fp.read())
#4.使用上下文对象ctx调用eval函数执行js文件中的指定函数即可
result = ctx.eval('getPwd("123456")')
print(result)
```

```js
//test.js
function getPwd(pwd){
    return pwd;
}
```

### 微信公众平台案例

https://mp.weixin.qq.com/	：逆向登录密码的加密算法。

分析思路：

- 先通过抓包工具发现，密码是经过加密，并且发现密码的加密后的数据是32位，大概率是md5加密的！
- 发现加密后的数据是被pwd这个请求参数使用的。
- 需要全局搜索pwd或者pwd:，定位加密的环节在哪里
- 进行断点调试，定位加密算法。

实现方案1（大胆）：

- 直接使用python的md5进行数据加密：

  - ```python
    from hashlib import md5
    pwd = '123456';
    obj = md5()
    obj.update(pwd.encode("utf-8"))
    
    bs = obj.hexdigest()
    print(bs)
    ```

- 逆向网站js代码：

  - 断点调试后，发现return P(t) 就是返回加密的内容。逆向P函数即可。

  - 可以使用【发条】进行js代码调试

  - 调试成功后的js代码：

    - ```js
      g={};
      function _(t, i) {
                      var r = (t & 65535) + (i & 65535)
                        , f = (t >> 16) + (i >> 16) + (r >> 16);
                      return f << 16 | r & 65535
                  }
                  function D(t, i) {
                      return t << i | t >>> 32 - i
                  }
                  function R(t, i, r, f, T, O) {
                      return _(D(_(_(i, t), _(f, O)), T), r)
                  }
                  function S(t, i, r, f, T, O, $) {
                      return R(i & r | ~i & f, t, i, T, O, $)
                  }
                  function d(t, i, r, f, T, O, $) {
                      return R(i & f | r & ~f, t, i, T, O, $)
                  }
                  function m(t, i, r, f, T, O, $) {
                      return R(i ^ r ^ f, t, i, T, O, $)
                  }
                  function v(t, i, r, f, T, O, $) {
                      return R(r ^ (i | ~f), t, i, T, O, $)
                  }
                  function o(t, i) {
                      t[i >> 5] |= 128 << i % 32;
                      t[(i + 64 >>> 9 << 4) + 14] = i;
                      var r, f, T, O, $, s = 1732584193, c = -271733879, p = -1732584194, n = 271733878;
                      for (r = 0; r < t.length; r += 16) {
                          f = s;
                          T = c;
                          O = p;
                          $ = n;
                          s = S(s, c, p, n, t[r], 7, -680876936);
                          n = S(n, s, c, p, t[r + 1], 12, -389564586);
                          p = S(p, n, s, c, t[r + 2], 17, 606105819);
                          c = S(c, p, n, s, t[r + 3], 22, -1044525330);
                          s = S(s, c, p, n, t[r + 4], 7, -176418897);
                          n = S(n, s, c, p, t[r + 5], 12, 1200080426);
                          p = S(p, n, s, c, t[r + 6], 17, -1473231341);
                          c = S(c, p, n, s, t[r + 7], 22, -45705983);
                          s = S(s, c, p, n, t[r + 8], 7, 1770035416);
                          n = S(n, s, c, p, t[r + 9], 12, -1958414417);
                          p = S(p, n, s, c, t[r + 10], 17, -42063);
                          c = S(c, p, n, s, t[r + 11], 22, -1990404162);
                          s = S(s, c, p, n, t[r + 12], 7, 1804603682);
                          n = S(n, s, c, p, t[r + 13], 12, -40341101);
                          p = S(p, n, s, c, t[r + 14], 17, -1502002290);
                          c = S(c, p, n, s, t[r + 15], 22, 1236535329);
                          s = d(s, c, p, n, t[r + 1], 5, -165796510);
                          n = d(n, s, c, p, t[r + 6], 9, -1069501632);
                          p = d(p, n, s, c, t[r + 11], 14, 643717713);
                          c = d(c, p, n, s, t[r], 20, -373897302);
                          s = d(s, c, p, n, t[r + 5], 5, -701558691);
                          n = d(n, s, c, p, t[r + 10], 9, 38016083);
                          p = d(p, n, s, c, t[r + 15], 14, -660478335);
                          c = d(c, p, n, s, t[r + 4], 20, -405537848);
                          s = d(s, c, p, n, t[r + 9], 5, 568446438);
                          n = d(n, s, c, p, t[r + 14], 9, -1019803690);
                          p = d(p, n, s, c, t[r + 3], 14, -187363961);
                          c = d(c, p, n, s, t[r + 8], 20, 1163531501);
                          s = d(s, c, p, n, t[r + 13], 5, -1444681467);
                          n = d(n, s, c, p, t[r + 2], 9, -51403784);
                          p = d(p, n, s, c, t[r + 7], 14, 1735328473);
                          c = d(c, p, n, s, t[r + 12], 20, -1926607734);
                          s = m(s, c, p, n, t[r + 5], 4, -378558);
                          n = m(n, s, c, p, t[r + 8], 11, -2022574463);
                          p = m(p, n, s, c, t[r + 11], 16, 1839030562);
                          c = m(c, p, n, s, t[r + 14], 23, -35309556);
                          s = m(s, c, p, n, t[r + 1], 4, -1530992060);
                          n = m(n, s, c, p, t[r + 4], 11, 1272893353);
                          p = m(p, n, s, c, t[r + 7], 16, -155497632);
                          c = m(c, p, n, s, t[r + 10], 23, -1094730640);
                          s = m(s, c, p, n, t[r + 13], 4, 681279174);
                          n = m(n, s, c, p, t[r], 11, -358537222);
                          p = m(p, n, s, c, t[r + 3], 16, -722521979);
                          c = m(c, p, n, s, t[r + 6], 23, 76029189);
                          s = m(s, c, p, n, t[r + 9], 4, -640364487);
                          n = m(n, s, c, p, t[r + 12], 11, -421815835);
                          p = m(p, n, s, c, t[r + 15], 16, 530742520);
                          c = m(c, p, n, s, t[r + 2], 23, -995338651);
                          s = v(s, c, p, n, t[r], 6, -198630844);
                          n = v(n, s, c, p, t[r + 7], 10, 1126891415);
                          p = v(p, n, s, c, t[r + 14], 15, -1416354905);
                          c = v(c, p, n, s, t[r + 5], 21, -57434055);
                          s = v(s, c, p, n, t[r + 12], 6, 1700485571);
                          n = v(n, s, c, p, t[r + 3], 10, -1894986606);
                          p = v(p, n, s, c, t[r + 10], 15, -1051523);
                          c = v(c, p, n, s, t[r + 1], 21, -2054922799);
                          s = v(s, c, p, n, t[r + 8], 6, 1873313359);
                          n = v(n, s, c, p, t[r + 15], 10, -30611744);
                          p = v(p, n, s, c, t[r + 6], 15, -1560198380);
                          c = v(c, p, n, s, t[r + 13], 21, 1309151649);
                          s = v(s, c, p, n, t[r + 4], 6, -145523070);
                          n = v(n, s, c, p, t[r + 11], 10, -1120210379);
                          p = v(p, n, s, c, t[r + 2], 15, 718787259);
                          c = v(c, p, n, s, t[r + 9], 21, -343485551);
                          s = _(s, f);
                          c = _(c, T);
                          p = _(p, O);
                          n = _(n, $)
                      }
                      return [s, c, p, n]
                  }
                  function u(t) {
                      var i, r = "";
                      for (i = 0; i < t.length * 32; i += 8) {
                          r += String.fromCharCode(t[i >> 5] >>> i % 32 & 255)
                      }
                      return r
                  }
                  function b(t) {
                      var i, r = [];
                      r[(t.length >> 2) - 1] = void 0;
                      for (i = 0; i < r.length; i += 1) {
                          r[i] = 0
                      }
                      for (i = 0; i < t.length * 8; i += 8) {
                          r[i >> 5] |= (t.charCodeAt(i / 8) & 255) << i % 32
                      }
                      return r
                  }
                  function L(t) {
                      return u(o(b(t), t.length * 8))
                  }
                  function h(t, i) {
                      var r, f = b(t), T = [], O = [], $;
                      T[15] = O[15] = void 0;
                      if (f.length > 16) {
                          f = o(f, t.length * 8)
                      }
                      for (r = 0; r < 16; r += 1) {
                          T[r] = f[r] ^ 909522486;
                          O[r] = f[r] ^ 1549556828
                      }
                      $ = o(T.concat(b(i)), 512 + i.length * 8);
                      return u(o(O.concat($), 512 + 128))
                  }
                  function y(t) {
                      var i = "0123456789abcdef", r = "", f, T;
                      for (T = 0; T < t.length; T += 1) {
                          f = t.charCodeAt(T);
                          r += i.charAt(f >>> 4 & 15) + i.charAt(f & 15)
                      }
                      return r
                  }
                  function E(t) {
                      return unescape(encodeURIComponent(t))
                  }
                  function I(t) {
                      return L(E(t))
                  }
                  function P(t) {
                      return y(I(t))
                  }
                  function B(t, i) {
                      return h(E(t), E(i))
                  }
                  function l(t, i) {
                      return y(B(t, i))
                  }
                  g.exports = function(t, i, r) {
                      if (!i) {
                          if (!r) {
                              return P(t)
                          } else {
                              return I(t)
                          }
                      }
                      if (!r) {
                          return l(i, t)
                      } else {
                          return B(i, t)
                      }
                  }
      ```

  - python运行代码：

    - ```python
      #1.导包
      import execjs
      #2.创建node对象
      node = execjs.get()
      #3.编译js文件返回上下文ctx对象(将js文件中的代码读取出来，被compile进行编译)
      fp = open('main.js','r',encoding='utf-8')
      ctx = node.compile(fp.read())
      #4.使用上下文对象ctx调用eval函数执行js文件中的指定函数即可
      result = ctx.eval('P("123456")')
      print(result)
      ```

      ## 一. JavaScript前置知识点

      ### 1. js中创建对象的方法

      ```js
      let a = {'name':'bobo'}; //方法1
      
      function B(){this.name="bobo"};
      let b = new B(); //方法2
      
      console.log(a);
      console.log(b);
      ```

      #### 1.1 特别的window对象

      使用 window 对象可以访问客户端其他对象，这种关系构成浏览器对象模型，window 对象代表根节点，浏览器对象关系的关系如图所示，每个对象说明如下。

      - window：客户端 JavaScript 顶层对象。每当 <body> 或 <frameset> 标签出现时，window 对象就会被自动创建。
      - navigator：包含客户端有关浏览器信息。
      - screen：包含客户端屏幕的信息。
      - history：包含浏览器窗口访问过的 URL 信息。
      - location：包含当前网页文档的 URL 信息。
      - document：包含整个 HTML 文档，可被用来访问文档内容及其所有页面元素。

      window对象树：https://www.processon.com/view/link/66497878cfe67e27899b9e70?cid=66488d812b49420be3452fa3

      ### 2. 原型链

      #### 2.1 何为原型链

      原型链是**JavaScript中对象继承属性和方法的一种方式**。具体介绍如下：

      ```
      原型链是JavaScript中对象继承属性和方法的一种方式。当访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，它会通过原型链去它的原型对象中查找，如果原型对象也没有，会继续在其原型的原型中查找，这样逐级向上，直到找到属性或方法或者达到原型链的末端。
      ```

      #### 2.2 原型链相关概念

      - 原型

        - 概念：原型本身其实是一个function函数(构造函数)，可以将其理解成Python中的class类。

        - 创建一个原型：

          ```js
          function User(name,pwd){
              this.userName = name;
              this.pwd = pwd;
              this.regist = function(){
                  console.log(this.name+"在注册")
              }
          }
          ```

        - 注意：

          ```js
          /*
          	如下代码是使用字面量方式创建对象时，直接使用花括号{}来定义对象的属性和方法。这种方式创建的对象是一个简单的键值对集合，没有原型链和构造函数。
          */
          let a = {'name':'bobo'};
          
          /*
          	通过定义一个构造函数B()，然后使用new关键字来创建一个新的对象实例。这种方式创建的对象可以继承构造函数的原型属性和方法，从而实现面向对象编程的特性。
          */
          function B(){
              this.name="bobo";
          }
          let b = new B();
          
          ```

          

      - 实例对象

        - 概念：通过new关键字创建的对象（调用构造函数）称为实例对象。

        - 创建实例对象：

          ```js
          let u1 = new User('jay','123');
          let u2 = new User('tom','456');
          ```

      - 原型对象

        - 概念：原型对象用于存储所有实例对象共享的属性和方法，以减少每个实例对象重复存储相同属性和方法的开销。

        - 原型对象存储所有实例对象共享的属性和方法

          ```js
          //类似于类属性
          User.prototype.address = "BJ";
          User.prototype.gender = "male";
          //类似于类方法
          User.prototype.login = function login(username, password){
              console.log(`${username}在登录`);
          }
          
          //实例对象共享原型对象存储的内容
          u1.login('jay','123');
          u2.login('tom','456');
          console.log(u1.address,u2.address,u1.gender,u2.gender);
          ```

        - 获取原型对象：

          ```js
          User.prototype;   
          u1.__proto__;
          User.prototype === u1.__proto__ //true
          //可以通过原型名访问原型对象或者使用实例名访问原型对象
          ```

      - 原型链

        - 原型链是**JavaScript中对象继承属性和方法的一种方式**。具体介绍如下：

          ```
          原型链是JavaScript中对象继承属性和方法的一种方式。当访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，它会通过原型链去它的原型对象中查找，如果原型对象也没有，会继续在其原型的原型中查找，这样逐级向上，直到找到属性或方法或者达到原型链的末端。
          ```

          原型对象本身也是一个对象，它也可以使用\__proto__访问它的原型对象，类似于：

          ```js
          u1.__proto__.__proto__
          ```

        - 原型链的成员访问：

          - 实例对象可以访问其原型内的成员和其原型链上所有原型对象内的成员

          ```js
          User.toString()
          ```

      - 浏览器环境navigator分析

        <img src="D:\BaiduNetdiskDownload\七期\day11-js语法实操06\imgs\Snipaste_2024-07-11_09-24-19.png" alt="Snipaste_2024-07-11_09-24-19" style="zoom:75%;" />

        - 思考：navigator对象的原型对象是谁？
          - navigator.__proto__ === Navigator（原型）
          - navigator.__proto__ === Navigator.prototype（原型对象）

      #### 3. Object对象常用成员

      ```js
      //判断对象类型typeof和Object.prototype.toString.call（Object原型对象）
      console.log('数字1',typeof 1);
      console.log("字符串1",typeof "1");
      console.log('空对象{}',typeof {});
      console.log('布尔true',typeof true);
      console.log('空数组[]',typeof []);
      console.log('null空',typeof null);
      console.log('undefined',typeof undefined);
      console.log('函数function (){}',typeof function (){});
      //发现null和空数组[]类型都是object类型（无法区分具体类型）
      console.log("=================================")
      console.log(Object.prototype.toString.call(1));
      console.log(Object.prototype.toString.call("1"));
      console.log(Object.prototype.toString.call({}));
      console.log(Object.prototype.toString.call(true));
      console.log(Object.prototype.toString.call([]));
      console.log(Object.prototype.toString.call(null));
      console.log(Object.prototype.toString.call(undefined));
      console.log(Object.prototype.toString.call(function () {}));
      
      //创建新对象，设置其原型对象为window（在node环境下可能需要伪装浏览器环境下的对象）
      a = Object.create(window)
      a.__proto__ === window //true
      
      //判断对象自身属性中是否具有指定的属性
      function func(){
          this.name="bobo";
          this.getAge=function(){}
      };
      f = new func();
      f.hasOwnProperty('name'); //true
      f.hasOwnProperty('getAge'); //true
      f.hasOwnProperty('toString'); //false
      
      //获取指定对象上一个自有属性对应的属性描述符
      Object.getOwnPropertyDescriptor(f,'name');
      //获取指定对象上所有属性对应的属性描述符
      Object.getOwnPropertyDescriptors(f);
      /*
      	属性描述符是一组用于精确定义和描述对象属性的特性的集合(属性描述符也是一个对象)。通过属性描述符，开发者可以指定一个属性是否可被修改、删除、枚举或者通过特定的函数来获取和设置其值。
      */
      
      
      //获取实例对象的原型对象
      Object.getPrototypeOf(f);
      Object.getPrototypeOf(f) === f.__proto__ ;//true
      
      //设置一个指定的对象的原型(可以对一个已经存在的对象重新设置其原型对象)
      Object.setPrototypeOf(f,Object.__proto__) //f对象的原型对象设置成了window的原型对象
      
      
      //defineProperty直接在一个对象上定义一个新属性，然后可指定新属性的属性描述的，并返回此对象。
      let User = {
          "name":"小明",
      }//创建一个User对象
      //给对象添加两个成员
      User.age = 10;
      User["age"] = 20;
      //给对象定义一个新属性且设置其属性描述符（属性描述符可分为：数据描述符和存取描述符）此时使用数据描述符。
      	//参数1：对象。参数2：属性名。参数3：属性描述的
      Object.defineProperty(User, "height", {
          enumerable:true, //该属性是否可遍历
          configurable:true,//该属性是否可配置:决定该属性是否可以被删除或修改其属性描述符。
          value:160, //属性的值
          writable:false //该属性的值是否可以通过赋值运算符改变
      });
      //对象属性遍历,如果某个属性的文件描述符中的enumerable:false则无法遍历出该属性
      for (const userKey in User) {
          console.log(userKey);
      }
      //存取描述符
      let Stu = {
          "name":"小红",
      }//创建一个Stu对象
      let temp = null;//临时变量
      //给Stu对象定义一个新属性score，且设置其属性描述符
      Object.defineProperty(Stu, "score", {
          enumerable:true,
          configurable:true,
          get:function (){// 当获取属性值是调用
              console.log("正在获取值");
              return temp;
          },
          set:function (value){// 当对属性进行赋值操作时调用
              console.log("正在设置值");
              temp = value;
          }
      });
      console.log(User.score);
      User.score = 100;
      console.log(User.score);
      /*
      	属性描述符注意事项：属性描述符分为两类：数据描述符和存取描述符。数据描述符包含value、writable、enumerable和configurable这些属性。存取描述符包含get、set、enumerable和configurable。两者不能混用，即一个描述符如果是数据描述符就不能包含get或set，反之亦然。
      */
      ```

      

      ## 二. hook插件  

      #### 1.概念

      在JavaScript中，hook是一种能够**拦截和修改函数或方法行为的技术**。通过使用hook，开发者可以在现有的函数执行前、执行后或者替换函数的实现逻辑。hook目的是找到函数入口以及一些参数变化，便于分析js逻辑。

      #### 2.hook的作用：

      1. **增强代码的可扩展性**：Hook技术允许开发者在不修改原始代码的情况下，增加或修改功能，使得代码更加灵活和可扩展。
      2. **减少代码的侵入性**：使用hook可以在不改变原始代码的前提下增加新功能，这减少了对原始代码的侵入，使得添加的功能更容易被管理和维护。
      3. **便于调试和问题定位**：利用hook技术可以在函数执行前后插入调试信息，帮助开发者更好地理解程序执行流程和定位问题源头。

      #### 3. hook基本使用

      ##### 3.1 函数的hook

      - 定义函数

      ```js
      // 定义函数
      function add(a,b){
          console.log("add方法正在执行");
          return a+b;
      }
      ```

      - 保存原函数，目的是为了不修改原函数内部的实现

      ```js
      _add = add;
      ```

      - 对add函数进行hook(进行相关的日志输出)
        - hook的位置必须是加载完需要hook的函数（原函数）后

      ```js
      add = function(a,b){
      	console.log("原函数调用前, 参数：", a, b);
          let result = _add(a,b)
          console.log("原函数调用后, 结果：", result);
          return result;
      }
      ```

      - 调用函数

      ```js
      add(1,2)
      ```

      ##### 3.2 对象属性的hook

      ```js
      //1、创建一个对象
      let user = {
          "name": "波波",
      };
      
      //2、保存原属性
      _name = user.name;
      
      //3、对象属性的hook
      //defineProperty函数用来重新定义对象的属性。
      //参数1：对象。参数2：属性。参数3：属性描述符
      Object.defineProperty(user, "name",{
          get(){ // 获取属性值的时候执行
              console.log("正在获取属性值");
              return _name;
          },
          set(value){ // 设置属性值的时候执行
               console.log("正在设置属性值:", value);
               _name = value;
          }
      });
      
      //4、获取属性和设置属性操作
      console.log(user.name)
      user.name = 'Jay'
      console.log(user.name)
      ```

      如果对象没有/不存在的属性可以被hook吗？

      ```js
      //1、创建一个对象
      let user = {
          "name": "波波",
      };
      
      //2、保存原属性
      _age = 18;
      
      //3、对象属性age的hook
      Object.defineProperty(user, "age",{
          get(){ 
              console.log("正在获取属性值");
              return _age;
          },
          set(value){ 
               console.log("正在设置属性值:", value);
               _age = value;
          }
      });
      
      //4、获取属性和设置属性操作
      console.log(user.age)
      user.age = 20
      console.log(user.age)
      ```

      ##### 3.3 浏览器环境下atob函数的hook

      atob函数是浏览器环境自带的用来对base64数据进行解编码。接下来，使用对atob函数进行hook。

      - 编写hook操作：

      ```js
      _atob = atob;//保存原函数
      
      atob = function (str){
          console.log("正在执行atob方法, 参数:", str);
          let result = _atob(str);
          console.log("正在执行atob方法, 返回值:", result);
          return result;
      }
      ```

      - hook时机：在浏览器页面加载出来之前进行hook

        - 在一个空白页面打开浏览器开发者工具

        - 开启js的事件监听器

          <img src="D:\BaiduNetdiskDownload\七期\day11-js语法实操06\imgs\Snipaste_2024-07-06_09-52-59.png" alt="Snipaste_2024-07-06_09-52-59" style="zoom:50%;" />

        - 在Sources中的Snippets代码段中新增hook代码片段，然后运行

          <img src="D:\BaiduNetdiskDownload\七期\day11-js语法实操06\imgs\Snipaste_2024-07-06_10-07-34.png" alt="Snipaste_2024-07-06_10-07-34" style="zoom:50%;" />

      - 查看hook运行，监控atob函数的执行

        - 取消事件监听器中的Script，因为此时已经成功对atob函数进行了hook（不可刷新页面）

          <img src="D:\BaiduNetdiskDownload\七期\day11-js语法实操06\imgs\Snipaste_2024-07-06_10-17-16.png" alt="Snipaste_2024-07-06_10-17-16" style="zoom: 50%;" />

      3.4 浏览器环境下cookie的hook

      - 操作步骤如步骤：3.3

      ```js
      _cookie = document.cookie;
      Object.defineProperty(document, "cookie", {
          get(){
              console.log("正在获取cookie:", _cookie);
              return _cookie;
          },
          set(value){
              console.log("正在设置cookie:", value);
              _cookie = value;
          }
      });
      ```

      ##### 3.4 hook检测与破解检测

      一些网站会严格检测该网站中的先关函数或者属性是否被一些别有用心的人进行hook。那么检测方式是什么呢？我们又该如何破解该种检测呢？

      - toString() 检测法

        - atob原函数的toString() 结果为：

          <img src="D:\BaiduNetdiskDownload\七期\day11-js语法实操06\imgs\Snipaste_2024-07-06_10-48-42.png" alt="Snipaste_2024-07-06_10-48-42" style="zoom:75%;" />

        - atob被hook后的toString() 结果为：

          <img src="D:\BaiduNetdiskDownload\七期\day11-js语法实操06\imgs\Snipaste_2024-07-06_10-51-28.png" alt="Snipaste_2024-07-06_10-51-28" style="zoom:75%;" />

        - 结果：两个atob的toString返回的结果是不一样的。

        ```
        什么是native？
        	- 在js中，一些内置函数如toString或者atob等函数的函数实现会被显示为[native code]，而不是显示实现的具体代码。这样的操作对于提高代码的安全性和封装性有一定的作用。
        ```

      - toString() 检测法的破解

        - 在hook中重写atob函数的toStirng方法：

        ```js
        _atob = atob;//保存原函数
        
        atob = function (str){
            console.log("正在执行atob方法, 参数:", str);
            let result = _atob(str);
            console.log("正在执行atob方法, 返回值:", result);
            return result;
        }
        //重写atob函数的toString方法
        atob.toString = function(){
            return 'function atob() { [native code] }'
        }
        ```

      - 原型链上的toString()检测法

        ```js
        Function.prototype.toString.call(atob)
        //调用函数原型对象中的toString进行的检测，而不是atob实例对象的toString了。
        ```

      - 原型链上的toString()检测法的破解

        - 在hook中重写原型链上的toString()方法：

        ```js
        _atob = atob;//保存原函数
        
        atob = function (str){
            console.log("正在执行atob方法, 参数:", str);
            let result = _atob(str);
            console.log("正在执行atob方法, 返回值:", result);
            return result;
        }
        //重写原型链上的toString方法
        Function.prototype.toString = function(){
            return 'function ${this.name}() { [native code] }'
        }//this.name就是toString的调用者的名字,比如Location.toString，则this.name就是Location，如果将this.name直接换成atob的话，则以后任何调用者调用toString的话，则返回的function后面的名字就都是atob了。也就是如果Location.toString()返回的也是：function atob() { [native code] }
        ```

      

      ## 一. JavaScript前置知识点

      ### 1. js中创建对象的方法

      ```js
      let a = {'name':'bobo'}; //方法1
      
      function B(){this.name="bobo"};
      let b = new B(); //方法2
      
      console.log(a);
      console.log(b);
      ```

      #### 1.1 特别的window对象

      使用 window 对象可以访问客户端其他对象，这种关系构成浏览器对象模型，window 对象代表根节点，浏览器对象关系的关系如图所示，每个对象说明如下。

      - window：客户端 JavaScript 顶层对象。每当 <body> 或 <frameset> 标签出现时，window 对象就会被自动创建。
      - navigator：包含客户端有关浏览器信息。
      - screen：包含客户端屏幕的信息。
      - history：包含浏览器窗口访问过的 URL 信息。
      - location：包含当前网页文档的 URL 信息。
      - document：包含整个 HTML 文档，可被用来访问文档内容及其所有页面元素。

      window对象树：https://www.processon.com/view/link/66497878cfe67e27899b9e70?cid=66488d812b49420be3452fa3

      ### 2. 原型链

      #### 2.1 何为原型链

      原型链是**JavaScript中对象继承属性和方法的一种方式**。具体介绍如下：

      ```
      原型链是JavaScript中对象继承属性和方法的一种方式。当访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，它会通过原型链去它的原型对象中查找，如果原型对象也没有，会继续在其原型的原型中查找，这样逐级向上，直到找到属性或方法或者达到原型链的末端。
      ```

      #### 2.2 原型链相关概念

      - 原型

        - 概念：原型本身其实是一个function函数(构造函数)，可以将其理解成Python中的class类。

        - 创建一个原型：

          ```js
          function User(name,pwd){
              this.userName = name;
              this.pwd = pwd;
              this.regist = function(){
                  console.log(this.name+"在注册")
              }
          }
          ```

        - 注意：

          ```js
          /*
          	如下代码是使用字面量方式创建对象时，直接使用花括号{}来定义对象的属性和方法。这种方式创建的对象是一个简单的键值对集合，没有原型链和构造函数。
          */
          let a = {'name':'bobo'};
          
          /*
          	通过定义一个构造函数B()，然后使用new关键字来创建一个新的对象实例。这种方式创建的对象可以继承构造函数的原型属性和方法，从而实现面向对象编程的特性。
          */
          function B(){
              this.name="bobo";
          }
          let b = new B();
          
          ```

          

      - 实例对象

        - 概念：通过new关键字创建的对象（调用构造函数）称为实例对象。

        - 创建实例对象：

          ```js
          let u1 = new User('jay','123');
          let u2 = new User('tom','456');
          ```

      - 原型对象

        - 概念：原型对象用于存储所有实例对象共享的属性和方法，以减少每个实例对象重复存储相同属性和方法的开销。

        - 原型对象存储所有实例对象共享的属性和方法

          ```js
          //类似于类属性
          User.prototype.address = "BJ";
          User.prototype.gender = "male";
          //类似于类方法
          User.prototype.login = function login(username, password){
              console.log(`${username}在登录`);
          }
          
          //实例对象共享原型对象存储的内容
          u1.login('jay','123');
          u2.login('tom','456');
          console.log(u1.address,u2.address,u1.gender,u2.gender);
          ```

        - 获取原型对象：

          ```js
          User.prototype;   
          u1.__proto__;
          User.prototype === u1.__proto__ //true
          //可以通过原型名访问原型对象或者使用实例名访问原型对象
          ```

      - 原型链

        - 原型链是**JavaScript中对象继承属性和方法的一种方式**。具体介绍如下：

          ```
          原型链是JavaScript中对象继承属性和方法的一种方式。当访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，它会通过原型链去它的原型对象中查找，如果原型对象也没有，会继续在其原型的原型中查找，这样逐级向上，直到找到属性或方法或者达到原型链的末端。
          ```

          原型对象本身也是一个对象，它也可以使用\__proto__访问它的原型对象，类似于：

          ```js
          u1.__proto__.__proto__
          ```

        - 原型链的成员访问：

          - 实例对象可以访问其原型内的成员和其原型链上所有原型对象内的成员

          ```js
          User.toString()
          ```

      - 浏览器环境navigator分析

        <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-11_09-24-19.png" alt="Snipaste_2024-07-11_09-24-19" style="zoom:75%;" />

        - 思考：navigator对象的原型对象是谁？
          - navigator.__proto__ === Navigator（原型）
          - navigator.__proto__ === Navigator.prototype（原型对象）

      #### 3. Object对象常用成员

      ```js
      //判断对象类型typeof和Object.prototype.toString.call（Object原型对象）
      console.log('数字1',typeof 1);
      console.log("字符串1",typeof "1");
      console.log('空对象{}',typeof {});
      console.log('布尔true',typeof true);
      console.log('空数组[]',typeof []);
      console.log('null空',typeof null);
      console.log('undefined',typeof undefined);
      console.log('函数function (){}',typeof function (){});
      //发现null和空数组[]类型都是object类型（无法区分具体类型）
      console.log("=================================")
      console.log(Object.prototype.toString.call(1));
      console.log(Object.prototype.toString.call("1"));
      console.log(Object.prototype.toString.call({}));
      console.log(Object.prototype.toString.call(true));
      console.log(Object.prototype.toString.call([]));
      console.log(Object.prototype.toString.call(null));
      console.log(Object.prototype.toString.call(undefined));
      console.log(Object.prototype.toString.call(function () {}));
      
      //创建新对象，设置其原型对象为window（在node环境下可能需要伪装浏览器环境下的对象）
      a = Object.create(window)
      a.__proto__ === window //true
      
      //判断对象自身属性中是否具有指定的属性
      function func(){
          this.name="bobo";
          this.getAge=function(){}
      };
      f = new func();
      f.hasOwnProperty('name'); //true
      f.hasOwnProperty('getAge'); //true
      f.hasOwnProperty('toString'); //false
      
      //获取指定对象上一个自有属性对应的属性描述符
      Object.getOwnPropertyDescriptor(f,'name');
      //获取指定对象上所有属性对应的属性描述符
      Object.getOwnPropertyDescriptors(f);
      /*
      	属性描述符是一组用于精确定义和描述对象属性的特性的集合(属性描述符也是一个对象)。通过属性描述符，开发者可以指定一个属性是否可被修改、删除、枚举或者通过特定的函数来获取和设置其值。
      */
      
      
      //获取实例对象的原型对象
      Object.getPrototypeOf(f);
      Object.getPrototypeOf(f) === f.__proto__ ;//true
      
      //设置一个指定的对象的原型(可以对一个已经存在的对象重新设置其原型对象)
      Object.setPrototypeOf(f,Object.__proto__) //f对象的原型对象设置成了window的原型对象
      
      
      //defineProperty直接在一个对象上定义一个新属性，然后可指定新属性的属性描述的，并返回此对象。
      let User = {
          "name":"小明",
      }//创建一个User对象
      //给对象添加两个成员
      User.age = 10;
      User["age"] = 20;
      //给对象定义一个新属性且设置其属性描述符（属性描述符可分为：数据描述符和存取描述符）此时使用数据描述符。
      	//参数1：对象。参数2：属性名。参数3：属性描述的
      Object.defineProperty(User, "height", {
          enumerable:true, //该属性是否可遍历
          configurable:true,//该属性是否可配置:决定该属性是否可以被删除或修改其属性描述符。
          value:160, //属性的值
          writable:false //该属性的值是否可以通过赋值运算符改变
      });
      //对象属性遍历,如果某个属性的文件描述符中的enumerable:false则无法遍历出该属性
      for (const userKey in User) {
          console.log(userKey);
      }
      //存取描述符
      let Stu = {
          "name":"小红",
      }//创建一个Stu对象
      let temp = null;//临时变量
      //给Stu对象定义一个新属性score，且设置其属性描述符
      Object.defineProperty(Stu, "score", {
          enumerable:true,
          configurable:true,
          get:function (){// 当获取属性值是调用
              console.log("正在获取值");
              return temp;
          },
          set:function (value){// 当对属性进行赋值操作时调用
              console.log("正在设置值");
              temp = value;
          }
      });
      console.log(User.score);
      User.score = 100;
      console.log(User.score);
      /*
      	属性描述符注意事项：属性描述符分为两类：数据描述符和存取描述符。数据描述符包含value、writable、enumerable和configurable这些属性。存取描述符包含get、set、enumerable和configurable。两者不能混用，即一个描述符如果是数据描述符就不能包含get或set，反之亦然。
      */
      ```

      

      ## 二. hook插件  

      #### 1.概念

      在JavaScript中，hook是一种能够**拦截和修改函数或方法行为的技术**。通过使用hook，开发者可以在现有的函数执行前、执行后或者替换函数的实现逻辑。hook目的是找到函数入口以及一些参数变化，便于分析js逻辑。

      #### 2.hook的作用：

      1. **增强代码的可扩展性**：Hook技术允许开发者在不修改原始代码的情况下，增加或修改功能，使得代码更加灵活和可扩展。
      2. **减少代码的侵入性**：使用hook可以在不改变原始代码的前提下增加新功能，这减少了对原始代码的侵入，使得添加的功能更容易被管理和维护。
      3. **便于调试和问题定位**：利用hook技术可以在函数执行前后插入调试信息，帮助开发者更好地理解程序执行流程和定位问题源头。

      #### 3. hook基本使用

      ##### 3.1 函数的hook

      - 定义函数

      ```js
      // 定义函数
      function add(a,b){
          console.log("add方法正在执行");
          return a+b;
      }
      ```

      - 保存原函数，目的是为了不修改原函数内部的实现

      ```js
      _add = add;
      ```

      - 对add函数进行hook(进行相关的日志输出)
        - hook的位置必须是加载完需要hook的函数（原函数）后

      ```js
      add = function(a,b){
      	console.log("原函数调用前, 参数：", a, b);
          let result = _add(a,b)
          console.log("原函数调用后, 结果：", result);
          return result;
      }
      ```

      - 调用函数

      ```js
      add(1,2)
      ```

      ##### 3.2 对象属性的hook

      ```js
      //1、创建一个对象
      let user = {
          "name": "波波",
      };
      
      //2、保存原属性
      _name = user.name;
      
      //3、对象属性的hook
      //defineProperty函数用来重新定义对象的属性。
      //参数1：对象。参数2：属性。参数3：属性描述符
      Object.defineProperty(user, "name",{
          get(){ // 获取属性值的时候执行
              console.log("正在获取属性值");
              return _name;
          },
          set(value){ // 设置属性值的时候执行
               console.log("正在设置属性值:", value);
               _name = value;
          }
      });
      
      //4、获取属性和设置属性操作
      console.log(user.name)
      user.name = 'Jay'
      console.log(user.name)
      ```

      如果对象没有/不存在的属性可以被hook吗？

      ```js
      //1、创建一个对象
      let user = {
          "name": "波波",
      };
      
      //2、保存原属性
      _age = 18;
      
      //3、对象属性age的hook
      Object.defineProperty(user, "age",{
          get(){ 
              console.log("正在获取属性值");
              return _age;
          },
          set(value){ 
               console.log("正在设置属性值:", value);
               _age = value;
          }
      });
      
      //4、获取属性和设置属性操作
      console.log(user.age)
      user.age = 20
      console.log(user.age)
      ```

      ##### 3.3 浏览器环境下atob函数的hook

      atob函数是浏览器环境自带的用来对base64数据进行解编码。接下来，使用对atob函数进行hook。

      - 编写hook操作：

      ```js
          _atob = atob;//保存原函数
      
          atob = function (str){
              console.log("正在执行atob方法, 参数:", str);
              let result = _atob(str);
              console.log("正在执行atob方法, 返回值:", result);
              return result;
          }
      ```

      - hook时机：在浏览器页面加载出来之前进行hook

        - 1.在一个空白页面打开浏览器开发者工具

        - 2.开启js的事件监听器

          <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_09-52-59.png" alt="Snipaste_2024-07-06_09-52-59" style="zoom:50%;" />

        - 3.访问百度页面，会有断点停留

        - 4.在Sources中的Snippets代码段中新增hook代码片段，打上断点，然后运行

          ![Snipaste_2024-07-28_09-13-38](C:\Users\Administrator\Desktop\逆向\js基础到进阶Bobo\4.JavaScript进阶\imgs\Snipaste_2024-07-28_09-13-38.png)

      - 5.查看hook运行，监控atob函数的执行

        - 取消事件监听器中的Script，因为此时已经成功对atob函数进行了hook（不可刷新页面）

          <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_10-17-16.png" alt="Snipaste_2024-07-06_10-17-16" style="zoom: 50%;" />

      3.4 浏览器环境下cookie的hook

      - 操作步骤如步骤：3.3

      ```js
      _cookie = document.cookie;
      Object.defineProperty(document,'cookie',{
          get(){
              console.log("正在获取cookie:", _cookie);
              return _cookie;
          },
          set(value){
              console.log("正在设置cookie:", value);
              _cookie = value;
          }
      });
      ```

      ##### 3.4 hook检测与破解检测

      一些网站会严格检测该网站中的先关函数或者属性是否被一些别有用心的人进行hook。那么检测方式是什么呢？我们又该如何破解该种检测呢？

      - toString() 检测法

        - atob原函数的toString() 结果为：

          <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_10-48-42.png" alt="Snipaste_2024-07-06_10-48-42" style="zoom:75%;" />

        - atob被hook后的toString() 结果为：

          <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_10-51-28.png" alt="Snipaste_2024-07-06_10-51-28" style="zoom:75%;" />

        - 结果：两个atob的toString返回的结果是不一样的。

        ```
        什么是native？
        	- 在js中，一些内置函数如toString或者atob等函数的函数实现会被显示为[native code]，而不是显示实现的具体代码。这样的操作对于提高代码的安全性和封装性有一定的作用。
        ```

      - toString() 检测法的破解

        - 在hook中重写atob函数的toStirng方法：

        ```js
        _atob = atob;//保存原函数
        
        atob = function (str){
            console.log("正在执行atob方法, 参数:", str);
            let result = _atob(str);
            console.log("正在执行atob方法, 返回值:", result);
            return result;
        }
        //重写atob函数的toString方法
        atob.toString = function(){
            return 'function atob() { [native code] }'
        }
        ```

      - 原型链上的toString()检测法

        ```js
        Function.prototype.toString.call(atob)
        //调用函数原型对象中的toString进行的检测，而不是atob实例对象的toString了。
        ```

      - 原型链上的toString()检测法的破解

        - 在hook中重写原型链上的toString()方法：

        ```js
        _atob = atob;//保存原函数
        
        atob = function (str){
            console.log("正在执行atob方法, 参数:", str);
            let result = _atob(str);
            console.log("正在执行atob方法, 返回值:", result);
            return result;
        }
        //重写原型链上的toString方法
        Function.prototype.toString = function(){
            return `function ${this.name}() { [native code] }`
        }//this.name就是toString的调用者的名字,比如Location.toString，则this.name就是Location，如果将this.name直接换成atob的话，则以后任何调用者调用toString的话，则返回的function后面的名字就都是atob了。也就是如果Location.toString()返回的也是：function atob() { [native code] }
        ```

      

      

      

      



## 一. JavaScript前置知识点

### 1. js中创建对象的方法

```js
let a = {'name':'bobo'}; //方法1

function B(){this.name="bobo"};
let b = new B(); //方法2

console.log(a);
console.log(b);
```

#### 1.1 特别的window对象

使用 window 对象可以访问客户端其他对象，这种关系构成浏览器对象模型，window 对象代表根节点，浏览器对象关系的关系如图所示，每个对象说明如下。

- window：客户端 JavaScript 顶层对象。每当 <body> 或 <frameset> 标签出现时，window 对象就会被自动创建。
- navigator：包含客户端有关浏览器信息。
- screen：包含客户端屏幕的信息。
- history：包含浏览器窗口访问过的 URL 信息。
- location：包含当前网页文档的 URL 信息。
- document：包含整个 HTML 文档，可被用来访问文档内容及其所有页面元素。

window对象树：https://www.processon.com/view/link/66497878cfe67e27899b9e70?cid=66488d812b49420be3452fa3

### 2. 原型链

#### 2.1 何为原型链

原型链是**JavaScript中对象继承属性和方法的一种方式**。具体介绍如下：

```
原型链是JavaScript中对象继承属性和方法的一种方式。当访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，它会通过原型链去它的原型对象中查找，如果原型对象也没有，会继续在其原型的原型中查找，这样逐级向上，直到找到属性或方法或者达到原型链的末端。
```

#### 2.2 原型链相关概念

- 原型

  - 概念：原型本身其实是一个function函数(构造函数)，可以将其理解成Python中的class类。

  - 创建一个原型：

    ```js
    function User(name,pwd){
        this.userName = name;
        this.pwd = pwd;
        this.regist = function(){
            console.log(this.name+"在注册")
        }
    }
    ```

  - 注意：

    ```js
    /*
    	如下代码是使用字面量方式创建对象时，直接使用花括号{}来定义对象的属性和方法。这种方式创建的对象是一个简单的键值对集合，没有原型链和构造函数。
    */
    let a = {'name':'bobo'};
    
    /*
    	通过定义一个构造函数B()，然后使用new关键字来创建一个新的对象实例。这种方式创建的对象可以继承构造函数的原型属性和方法，从而实现面向对象编程的特性。
    */
    function B(){
        this.name="bobo";
    }
    let b = new B();
    
    ```

    

- 实例对象

  - 概念：通过new关键字创建的对象（调用构造函数）称为实例对象。

  - 创建实例对象：

    ```js
    let u1 = new User('jay','123');
    let u2 = new User('tom','456');
    ```

- 原型对象

  - 概念：原型对象用于存储所有实例对象共享的属性和方法，以减少每个实例对象重复存储相同属性和方法的开销。

  - 原型对象存储所有实例对象共享的属性和方法

    ```js
    //类似于类属性
    User.prototype.address = "BJ";
    User.prototype.gender = "male";
    //类似于类方法
    User.prototype.login = function login(username, password){
        console.log(`${username}在登录`);
    }
    
    //实例对象共享原型对象存储的内容
    u1.login('jay','123');
    u2.login('tom','456');
    console.log(u1.address,u2.address,u1.gender,u2.gender);
    ```

  - 获取原型对象：

    ```js
    User.prototype;   
    u1.__proto__;
    User.prototype === u1.__proto__ //true
    //可以通过原型名访问原型对象或者使用实例名访问原型对象
    ```

- 原型链

  - 原型链是**JavaScript中对象继承属性和方法的一种方式**。具体介绍如下：

    ```
    原型链是JavaScript中对象继承属性和方法的一种方式。当访问一个对象的属性或方法时，如果该对象本身没有这个属性或方法，它会通过原型链去它的原型对象中查找，如果原型对象也没有，会继续在其原型的原型中查找，这样逐级向上，直到找到属性或方法或者达到原型链的末端。
    ```

    原型对象本身也是一个对象，它也可以使用\__proto__访问它的原型对象，类似于：

    ```js
    u1.__proto__.__proto__
    ```

  - 原型链的成员访问：

    - 实例对象可以访问其原型内的成员和其原型链上所有原型对象内的成员

    ```js
    User.toString()
    ```

- 浏览器环境navigator分析

  <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-11_09-24-19.png" alt="Snipaste_2024-07-11_09-24-19" style="zoom:75%;" />

  - 思考：navigator对象的原型对象是谁？
    - navigator.__proto__ === Navigator（原型）
    - navigator.__proto__ === Navigator.prototype（原型对象）

#### 3. Object对象常用成员

```js
//判断对象类型typeof和Object.prototype.toString.call（Object原型对象）
console.log('数字1',typeof 1);
console.log("字符串1",typeof "1");
console.log('空对象{}',typeof {});
console.log('布尔true',typeof true);
console.log('空数组[]',typeof []);
console.log('null空',typeof null);
console.log('undefined',typeof undefined);
console.log('函数function (){}',typeof function (){});
//发现null和空数组[]类型都是object类型（无法区分具体类型）
console.log("=================================")
console.log(Object.prototype.toString.call(1));
console.log(Object.prototype.toString.call("1"));
console.log(Object.prototype.toString.call({}));
console.log(Object.prototype.toString.call(true));
console.log(Object.prototype.toString.call([]));
console.log(Object.prototype.toString.call(null));
console.log(Object.prototype.toString.call(undefined));
console.log(Object.prototype.toString.call(function () {}));

//创建新对象，设置其原型对象为window（在node环境下可能需要伪装浏览器环境下的对象）
a = Object.create(window)
a.__proto__ === window //true

//判断对象自身属性中是否具有指定的属性
function func(){
    this.name="bobo";
    this.getAge=function(){}
};
f = new func();
f.hasOwnProperty('name'); //true
f.hasOwnProperty('getAge'); //true
f.hasOwnProperty('toString'); //false

//获取指定对象上一个自有属性对应的属性描述符
Object.getOwnPropertyDescriptor(f,'name');
//获取指定对象上所有属性对应的属性描述符
Object.getOwnPropertyDescriptors(f);
/*
	属性描述符是一组用于精确定义和描述对象属性的特性的集合(属性描述符也是一个对象)。通过属性描述符，开发者可以指定一个属性是否可被修改、删除、枚举或者通过特定的函数来获取和设置其值。
*/


//获取实例对象的原型对象
Object.getPrototypeOf(f);
Object.getPrototypeOf(f) === f.__proto__ ;//true

//设置一个指定的对象的原型(可以对一个已经存在的对象重新设置其原型对象)
Object.setPrototypeOf(f,Object.__proto__) //f对象的原型对象设置成了window的原型对象


//defineProperty直接在一个对象上定义一个新属性，然后可指定新属性的属性描述的，并返回此对象。
let User = {
    "name":"小明",
}//创建一个User对象
//给对象添加两个成员
User.age = 10;
User["age"] = 20;
//给对象定义一个新属性且设置其属性描述符（属性描述符可分为：数据描述符和存取描述符）此时使用数据描述符。
	//参数1：对象。参数2：属性名。参数3：属性描述的
Object.defineProperty(User, "height", {
    enumerable:true, //该属性是否可遍历
    configurable:true,//该属性是否可配置:决定该属性是否可以被删除或修改其属性描述符。
    value:160, //属性的值
    writable:false //该属性的值是否可以通过赋值运算符改变
});
//对象属性遍历,如果某个属性的文件描述符中的enumerable:false则无法遍历出该属性
for (const userKey in User) {
    console.log(userKey);
}
//存取描述符
let Stu = {
    "name":"小红",
}//创建一个Stu对象
let temp = null;//临时变量
//给Stu对象定义一个新属性score，且设置其属性描述符
Object.defineProperty(Stu, "score", {
    enumerable:true,
    configurable:true,
    get:function (){// 当获取属性值是调用
        console.log("正在获取值");
        return temp;
    },
    set:function (value){// 当对属性进行赋值操作时调用
        console.log("正在设置值");
        temp = value;
    }
});
console.log(User.score);
User.score = 100;
console.log(User.score);
/*
	属性描述符注意事项：属性描述符分为两类：数据描述符和存取描述符。数据描述符包含value、writable、enumerable和configurable这些属性。存取描述符包含get、set、enumerable和configurable。两者不能混用，即一个描述符如果是数据描述符就不能包含get或set，反之亦然。
*/
```



## 二. hook插件  

#### 1.概念

在JavaScript中，hook是一种能够**拦截和修改函数或方法行为的技术**。通过使用hook，开发者可以在现有的函数执行前、执行后或者替换函数的实现逻辑。hook目的是找到函数入口以及一些参数变化，便于分析js逻辑。

#### 2.hook的作用：

1. **增强代码的可扩展性**：Hook技术允许开发者在不修改原始代码的情况下，增加或修改功能，使得代码更加灵活和可扩展。
2. **减少代码的侵入性**：使用hook可以在不改变原始代码的前提下增加新功能，这减少了对原始代码的侵入，使得添加的功能更容易被管理和维护。
3. **便于调试和问题定位**：利用hook技术可以在函数执行前后插入调试信息，帮助开发者更好地理解程序执行流程和定位问题源头。

#### 3. hook基本使用

##### 3.1 函数的hook

- 定义函数

```js
// 定义函数
function add(a,b){
    console.log("add方法正在执行");
    return a+b;
}
```

- 保存原函数，目的是为了不修改原函数内部的实现

```js
_add = add;
```

- 对add函数进行hook(进行相关的日志输出)
  - hook的位置必须是加载完需要hook的函数（原函数）后

```js
add = function(a,b){
	console.log("原函数调用前, 参数：", a, b);
    let result = _add(a,b)
    console.log("原函数调用后, 结果：", result);
    return result;
}
```

- 调用函数

```js
add(1,2)
```

##### 3.2 对象属性的hook

```js
//1、创建一个对象
let user = {
    "name": "波波",
};

//2、保存原属性
_name = user.name;

//3、对象属性的hook
//defineProperty函数用来重新定义对象的属性。
//参数1：对象。参数2：属性。参数3：属性描述符
Object.defineProperty(user, "name",{
    get(){ // 获取属性值的时候执行
        console.log("正在获取属性值");
        return _name;
    },
    set(value){ // 设置属性值的时候执行
         console.log("正在设置属性值:", value);
         _name = value;
    }
});

//4、获取属性和设置属性操作
console.log(user.name)
user.name = 'Jay'
console.log(user.name)
```

如果对象没有/不存在的属性可以被hook吗？

```js
//1、创建一个对象
let user = {
    "name": "波波",
};

//2、保存原属性
_age = 18;

//3、对象属性age的hook
Object.defineProperty(user, "age",{
    get(){ 
        console.log("正在获取属性值");
        return _age;
    },
    set(value){ 
         console.log("正在设置属性值:", value);
         _age = value;
    }
});

//4、获取属性和设置属性操作
console.log(user.age)
user.age = 20
console.log(user.age)
```

##### 3.3 浏览器环境下atob函数的hook

atob函数是浏览器环境自带的用来对base64数据进行解编码。接下来，使用对atob函数进行hook。

- 编写hook操作：

```js
    _atob = atob;//保存原函数

    atob = function (str){
        console.log("正在执行atob方法, 参数:", str);
        let result = _atob(str);
        console.log("正在执行atob方法, 返回值:", result);
        return result;
    }
```

- hook时机：在浏览器页面加载出来之前进行hook

  - 1.在一个空白页面打开浏览器开发者工具

  - 2.开启js的事件监听器

    <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_09-52-59.png" alt="Snipaste_2024-07-06_09-52-59" style="zoom:50%;" />

  - 3.访问百度页面，会有断点停留

  - 4.在Sources中的Snippets代码段中新增hook代码片段，打上断点，然后运行

    ![Snipaste_2024-07-28_09-13-38](C:\Users\Administrator\Desktop\逆向\js基础到进阶Bobo\4.JavaScript进阶\imgs\Snipaste_2024-07-28_09-13-38.png)

- 5.查看hook运行，监控atob函数的执行

  - 取消事件监听器中的Script，因为此时已经成功对atob函数进行了hook（不可刷新页面）

    <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_10-17-16.png" alt="Snipaste_2024-07-06_10-17-16" style="zoom: 50%;" />

3.4 浏览器环境下cookie的hook

- 操作步骤如步骤：3.3

```js
_cookie = document.cookie;
Object.defineProperty(document,'cookie',{
    get(){
        console.log("正在获取cookie:", _cookie);
        return _cookie;
    },
    set(value){
        console.log("正在设置cookie:", value);
        _cookie = value;
    }
});
```

##### 3.4 hook检测与破解检测

一些网站会严格检测该网站中的先关函数或者属性是否被一些别有用心的人进行hook。那么检测方式是什么呢？我们又该如何破解该种检测呢？

- toString() 检测法

  - atob原函数的toString() 结果为：

    <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_10-48-42.png" alt="Snipaste_2024-07-06_10-48-42" style="zoom:75%;" />

  - atob被hook后的toString() 结果为：

    <img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_10-51-28.png" alt="Snipaste_2024-07-06_10-51-28" style="zoom:75%;" />

  - 结果：两个atob的toString返回的结果是不一样的。

  ```
  什么是native？
  	- 在js中，一些内置函数如toString或者atob等函数的函数实现会被显示为[native code]，而不是显示实现的具体代码。这样的操作对于提高代码的安全性和封装性有一定的作用。
  ```

- toString() 检测法的破解

  - 在hook中重写atob函数的toStirng方法：

  ```js
  _atob = atob;//保存原函数
  
  atob = function (str){
      console.log("正在执行atob方法, 参数:", str);
      let result = _atob(str);
      console.log("正在执行atob方法, 返回值:", result);
      return result;
  }
  //重写atob函数的toString方法
  atob.toString = function(){
      return 'function atob() { [native code] }'
  }
  ```

- 原型链上的toString()检测法

  ```js
  Function.prototype.toString.call(atob)
  //调用函数原型对象中的toString进行的检测，而不是atob实例对象的toString了。
  ```

- 原型链上的toString()检测法的破解

  - 在hook中重写原型链上的toString()方法：

  ```js
  _atob = atob;//保存原函数
  
  atob = function (str){
      console.log("正在执行atob方法, 参数:", str);
      let result = _atob(str);
      console.log("正在执行atob方法, 返回值:", result);
      return result;
  }
  //重写原型链上的toString方法
  Function.prototype.toString = function(){
      return `function ${this.name}() { [native code] }`
  }//this.name就是toString的调用者的名字,比如Location.toString，则this.name就是Location，如果将this.name直接换成atob的话，则以后任何调用者调用toString的话，则返回的function后面的名字就都是atob了。也就是如果Location.toString()返回的也是：function atob() { [native code] }
  ```







## 三. proxy代理机制

#### 1. 概念

JavaScript中的Proxy是一种内置对象，它允许你在访问或操作对象之前拦截和自定义底层操作的行为。通过使用Proxy，你可以修改对象的默认行为，添加额外的逻辑或进行验证，以实现更高级的操作和控制。

Proxy对象包装了另一个对象（目标对象），并允许你定义一个处理程序（handler）来拦截对目标对象的操作。处理程序是一个带有特定方法的对象，这些方法被称为"捕获器"（traps），它们会在执行相应的操作时被调用。

#### 2. 代理操作

- 给User对象设置代理，监控该对象 “已有/存在” 属性值的相关操作

```js
//创建user对象
var User = {
    username: "bobo",
    age: 20
}
//创建代理对象，Proxy的参数1：被代理对象。参数2：处理器
User = new Proxy(User, {
    //target:被代理对象。p:属性。receiver：代理后的对象就是User
    get(target, p, receiver) {
        console.log(`获取属性${p}操作`)
        //返回代理对象的p属性值
        return Reflect.get(target, p);
    },
    set(target, p, value, receiver) {
        console.log(`设置属性${p}操作`)
        Reflect.set(target, p, value);
    }
});

console.log(User.username);
console.log(User.age);
User.username = "Jay"
User.age = 18
console.log(User.username);
console.log(User.age);
```

- 给User对象设置代理，监控该对象 “不存在” 属性值的相关操作

```js
//创建user对象
var User = {
    username: "bobo",
    age: 20
}

User = new Proxy(User, {
    get(target, p, receiver) {
        console.log(`获取属性${p}操作`)
        return Reflect.get(target, p);
    },
    set(target, p, value, receiver) {
        console.log(`设置属性${p}操作`)
        Reflect.set(target, p, value);
    }
});

console.log(User.username);
console.log(User.age);
console.log(User.address);
```

​	输出结果为：<img src="D:\BaiduNetdiskDownload\七期\day12-js语法实操07\imgs\Snipaste_2024-07-06_12-43-54.png" alt="Snipaste_2024-07-06_12-43-54" style="zoom:75%;" />说明该对象中不存在address这个属性值，则需要给该对象进行address属性的补充。同理可以作用在逆向的补环境中，例如：对window对象进行代理时，发现window的xxx属性返回的是空，则需要在逆向js代码中的window对象中补上xxx属性即可。

#### 3. 属性描述符

- getOwnPropertyDescriptor对象属性的获取

有些时候，一些网站的js中是通过属性描述符的方式获取一个对象的属性值。如下所示：

```js
var Stu = {
    "name":"小明"
};
//通过属性描述符获取Stu对象中的name属性值
console.log(Object.getOwnPropertyDescriptor(Stu, "name").value);
```

那么，这个时候我们该如何通过代理去监控通过属性描述符对属性进行的操作行为呢？（拦截获取属性描述符）

```js
//创建user对象
var User = {
    username: "bobo",
    age: 20,
    address:"Bj"
}

User = new Proxy(User, {
    get(target, p, receiver) {
        console.log(`获取属性${p}操作`)
        return Reflect.get(target, p);
    },
    set(target, p, value, receiver) {
        console.log(`设置属性${p}操作`)
        Reflect.set(target, p, value);
    },
    getOwnPropertyDescriptor(target, p){
        let result;
        result = Reflect.getOwnPropertyDescriptor(target, p);
        console.log(`通过属性描述符获取属性${p}操作`)
        return result;
    }
});

//测试
console.log(User.username);
User.age = 30;
console.log(Object.getOwnPropertyDescriptor(User, "address").value);
```

- defineProperty对象属性的定义

有些时候，一些网站的js中是通过属性描述符的方式对属性值的定义。如下所示：

```js
var Person = {};

// 通过属性描述符给Person对象定义一个name属性
Object.defineProperty(Person, 'name', {
      value: 'Bobo',
      writable: true, // 允许修改属性值
      enumerable: true, // 允许枚举属性
      configurable: true // 允许删除或修改属性描述符
    }
);
console.log(Person)

```

那么，这个时候我们该如何通过代理去监控通过属性描述符对属性进行的定义行为呢？(拦截属性定义)

```js
var Person = {};

Person = new Proxy(Person, {
    get(target, p, receiver) {
        console.log(`获取属性${p}操作`)
        return Reflect.get(target, p);
    },
    set(target, p, value, receiver) {
        console.log(`设置属性${p}操作`)
        Reflect.set(target, p, value);
    },
    getOwnPropertyDescriptor(target, p){
        let result;
        result = Reflect.getOwnPropertyDescriptor(target, p);
        console.log(`通过属性描述符获取属性${p}操作`)
        return result;
    },
    defineProperty: function (target, p, descriptor){
        console.log(`通过属性描述符设置属性${p}操作`)
        let result;
        result = Reflect.defineProperty(target, p, descriptor); 
        return result;
    }
    
});

// 通过属性描述符给Person对象定义一个name属性
Object.defineProperty(Person, 'name', {
      value: 'Bobo',
      writable: true, // 允许修改属性值
      enumerable: true, // 允许枚举属性
      configurable: true // 允许删除或修改属性描述符
    }
);
console.log(Person)

```

#### 4. 函数调用拦截监控

拦截监控指定函数的调用

```js
add = function(a,b){
    console.log("add函数正在被调用");
    return a+b;
}
add = new Proxy(add, {
    apply:function (target, thisArg, argList){
        // target: 函数对象。thisArg: 调用函数的this指针。argList:函数参数数组
        let result;
        result = Reflect.apply(target, thisArg, argList);
        console.log(`${target.name}函数被调用，参数为:${argList}`);
        return result;
	}
});
add(1,2);
```

#### 5. 对象构造方法拦截监控

拦截new关键字。基于new创建的对象就是在调用该对象的构造方法。

```js
function Animal(){
    
}
Animal = new Proxy(Animal, {
	construct:function (target, argArray, newTarget) {
    //target: 函数对象。argArray: 参数列表。newTarget: 代理后的对象
    let result = Reflect.construct(target, argArray, newTarget);
        
    console.log(`${target.name}对象被创建`);
    return result;
	}
});
animal = new Animal();
```







