# JavaScript 模式

## 1. 基本技巧

- 减少使用全局变量，最理想的情况是一个应用程序仅用一个全局变量。
- 在每个函数中仅使用一个var变量声明，这有助于在一个地方查看所有变量,可以有限防范变量提升导致的错误。


- for循环、for-in循环、switch语句、"eval()是魔鬼"和不要扩充内置的原型

- 遵循一些编码规定（一致性的空格、缩进、在可选的时候都坚持使用大括号和分号）和命名约定（针对构造函数、函数和变量的命名约定）


### 1.1 for 循环

```javascript
  for ( var i = 0; i < myArray.length; i++) {
      //处理myArray[i]; // 效率低
  }

  /* (2.1) for模式中的两个变量引出了一些细微操作*/

  // 1.使用了最少的变量（而非最多）
  // 2.逐步减至0，这样通常更快，因为同0比较比同数组的长度比较或同非0数组比较效率更高

  /* #1 第一种做法 */
  var i, myArray = [];
  for (i = myArray.length; i--;) {
      //处理myArray[i];
  }

  /* #2使用 while */
  var myArray = [],
      i = myArray.length;
  while (i--) {
      // 处理myArray[i];
  }
```

### 1.2 forin

```javascript
/* (2.2)当遍历对象属性来过滤遇到原型链的属性时，使用hasOwnProperty()方法是非常重要的 */

//对象
var man = { hands: 2,legs: 2,hedds: 1};

//代码的其他部分
//将第一个方法添加到所有对象上
if (typeof Object.prototype.clone === "undefined") {
    Object.prototype.clone = function () {
    }
}

// 调用hasOwnProperty函数来过两次该原型的属性
for ( var i in man) {
    if (man.hasOwnProperty(i)) { //filter
        console.log(i, ":", man[i]);
	}
}

// 或者在Object.prototype中调用该函
for ( var i in man) {
if (Object.prototype.hasOwnProperty.call(man, i)) { //过滤
        console.log(i, ":", man[i]);
    }
}

// 或是
var i, hasOwn = Object.prototype.hasOwnProperty;
for (i in man) {
    if (hasOwn.call(man, i)) { //filter
        console.log(i, ":", man[i]);
    }
}
```

###  1.3 不要增加内置的原型方法

**以下情形例外可以增加自定义方法:**

- 提前定义未来ECMAScript版本或JavaScript的具体实现
- 检查自定义的属性或方法并未存在时
- 准确地使用文档记录下来，并和团队交流清楚
- ​
```javascript
/* 提前定义未来ECMAScript版本或JavaScript的具体实现 */
if ( typeof Object.prototype.myMethod !== "function") {
       Object.prototype.myMethod = function () {
           // 实现代码
       }
 }
```

### 1.4 switch 模式

**优点：**可读性及健壮性

### 1.5 避免使用隐式类型转换

**使用`===`代替`==`**

```javascript
// （使用===代替==）
var zero = 0;
if (zero === false) {
    //不执行
}

if (zero == false) {
    //执行
}
```

### 1.6 避免使用eval()

- eval()可能执行被篡改过的代码，效率低
- setInterval(),setTimeout(),function()等构造函数来传递函数类似

### 1.7 大括号`{}`的位置

```javascript
function myfunction() {
    return
    {
        name: "Jane"
    };
}
// 等同于
function myfunction() {
    return undefined;
    // 并未访问到接下来的代码...
    {
        name: "Jane"
    };
}
// 正确的做法
function myfunction() {
    return {
        name: "Jane"
    };
}
```

### 1.8 其它问题

- 空格：主要是改善代码的可读性和一致性（IDE格式化会为你解决）
- 命名约定:
  - 构造函数的首字母大写
  - 分隔单词，建议遵循驼峰式命名约定
  - 常量、全局变量、私有变量的命名
- 编写注释
- 编写API文档（JSDoc Toolkit、YUIDoc）
- 编写可读性强的代码、同行互查、精简代码、JSLint


```javascript
// 构造函数(首字母大写)
function Student(name , age){
  this.name = name || '';
  this.age = age || 0;
}

// 分隔单词，建议遵循驼峰式命名约定
function getStudentName(){
  return name;
}

```



##  2 字面量和构造函数

### 2.1 对象字面量

**使用字面量而不是构造函数**

```javascript
// 第一种方法：使用字面量
var car = { goes: "far" };

/*另一种方式——使用内置构造函数*/

// 警告:这是一个反模式
var car = new Object();
car.goes = "far";
```

### 2.2 自定义构造函数

```javascript
// 构造函数的返回值
var ObjectMaker = function () {
    this.name = "This is it";
    var that = {};
    that.name = "And that's that";
    return that;
};

var o = new ObjectMaker();

console.log(o.name);//"And that's that"

// 自调用构造函数
function Waffle() {
    if (!(this instanceof Waffle)) {
        return new Waffle();
    }
    this.tastes = "yummy";
}

Waffle.prototype.wantAnother = true;

var first = new Waffle(),
    second = new Waffle();

console.log(first.tastes); // "yummy"
console.log(second.tastes); // "yummy"
console.log(first.wantAnother); // true
console.log(second.wantAnother); // true

function ctor() {
    // ES5严格模式不支持arguments.callee
    if (!(this instanceof arguments.callee)) {
        return new arguments.callee();
    }
}
```

### 2.3 数组字面量

**使用数组字面量，而不是构造函数`new Array()`**

```javascript
// 反模式
var a = new Array( "green", "red", "blue");

// 完全相同的数组
var a = [ "green", "red", "blue"];
console.log(typeof a); // "object"
console.log(a.constructor === Array); // true

// 构造函数的特殊性
// 具有一个元素的数组
var a = [3];
conso.log(a.length); // 1
console.log(a[0]); // 3

var a = new Array(3);
console.log(a.length); // 3
console.log(typeof a[0]); // "undefined"

// 检查数组的性质 
if ( typeof Array.isArray === "undefined") {
    Array.isArray = function (arg) {
        return Object.prototype.toString.call(arg) === "[object Array]";
    }
}
```



##  3. 函数

- API模式，更好而且更简洁的接口：
  - 回调模式——将函数作为参数进行传递
  - 配置对象——有助于保持收到控制的函数的参数数量
  - 返回函数——一个函数的返回值是另一个函数
  - Curry化——当信息函数是基于现有函数，并且加上部分参数列表创建时

- 初始化模式

  （不污染全局命名空间的情况下，使用临时变量以一种更加整洁、结构化的方式执行初始化以及设置任务（当涉及Web网页和应用程序时是非常普遍的））

  - 即时函数——定义之后就立即执行
  - 即时对象初始化——匿名对象组织了初始化任务，提供了可被立即调用的方法
  - 初始化时分支——帮助分支代码在初始化代码执行过程中仅检测一次，这与以后在程序生命期内多次检测相反。

- 性能模式（加速代码运行）

  - 备忘模式——使用函数属性以便使得计算过的值无需计算
  - 自定义模式——以新的主体重写本身，以使得在第二次或以后调用时仅需执行更少的工作。

### 3.1 函数的提升

```javascript
function foo() {
    alert("global foo");
}

function bar() {
    alert("global bar");
}

function hoistMe() {
    console.log(typeof foo); // "function"
    console.log(typeof bar); // "undefined"
    
    foo(); // "local foo"
    bar(); // TypeError:bar is not function
    
    // 函数声明
    // 变量'foo'以及实现被提升
    function foo() {
        alert( "local foo");
    }
  
    // 函数表达式
    // 仅变量"bar"被提升
    // 函数实现未被提升
    var bar = function () {
        alert( "local bar");
    }
}
```

说明：函数的提升类似变量的提升，遵循**“先定义，后使用的原则”**一般不会出现错误;

### 3.2 回调函数模式

```javascript
var findNodes = function () {
    var i = 100000, // 大而繁重的循环
        nodes = [], // 存储该结果
        found; // 找到了下一个节点
    while (i--) {
        // ...这里是复杂的逻辑
        nodes.push(focus);
    }
  
    return nodes;
};

var hide = function (nodes) {
    var i = 0, max = nodes.length;
    for (; i < max; i++) {
        nodes[i].style.display = "none";
    }
};

// 执行该函数
hide(findNodes());
// 低效，hide()必须再次循环遍历有findNodes()所返回的数组节点
```

重构findeNodes()以接受一个回调函数

```javascript
var findNodes = function (callback) {
    var i = 100000,
        nodes = [],
        found;
  
    if (typeof callback !== "function") {
        callback = false;
    }
    
    while (i--) {
        // 这里是复杂的逻辑
        // 现在运行回调函数
        if (callback) {
            callback(found);
        }
        nodes.push(found);
    }
  
    return nodes;
};

// 重构hide(回调函数)
var hide = function (node) {
    node.style.display = "none";
};

// 找到指定节点，并在后续执行中将其隐藏起来
findNodes(hide);

// 传递一个匿名回调函数
findNodes(function (node) {
    node.style.display = "block";
})
```

**回调与作用域**

```javascript
var myapp = {};

myapp.color = "red";
myapp.paint = function (node) {
    node.style.color = this.color;
}

// 函数findNodes()执行以下语句
var findNodes = function (callback) {
    // ...
    if (typeof callback === "function") {
        callback(found);
    }
    // ...
}

// 若调用findNodes(myapp.paint)，它并不会按照预期哪样运行，这是由于this.color没有定义
```

** 解决方案 ** 

```javascript
findNodes(myapp.paint, myapp);
// ...
var findeNodes = function (callback, callback_obj) {
    //...
    if (typeof callback === "function") {
        callback.call(callback_obj, found);
    }
    //...
}

// 上述中可将方法作为字符串来传递（无需重复两次输入该对象的名称）
// findNodes(myapp.paint, myapp);
findeNodes("paint", myapp);
var findNodes = function (callback, callbackObj) {
    if (typeof callback === "string") {
        callback = callbackObj[callback];
    }
    //...
    if (typeof callback === "function") {
        callbackObj.call(callbackObj, found);
    }
    //...
}
```

**说明：**异步事件监听器和计时器(setTimeout(),setInterval())都运用了回调模式

### 3.3 返回函数

```javascript
var setup = function () {
    var count = 0;
    return function () {
        return (count += 1);
    };
};

//用法
var next = setup();
next(); // 1
next(); // 2
next(); // 3
```

### 3.4 自定义函数

```javascript
var scareMe = function () {
    alert("Boo!");
    // 重写了scareMe()
    scareMe = function () {
        alert( "Double boo!")
    };
};

// 使用自定义函数
scareMe(); // Boo!
scareMe(); // Double boo!

// 1.添加一个新的属性
scareMe.property = "properly";
// 2.赋值给另一个不同名称的变量
var prank = scareMe;
// 3.作为一个方法使用
var spooky = {
    boo: scareMe
};

// calling with a new name
prank(); //"Boo!"
prank(); //"Boo!"
console.log(prank.property); //"properly"

// 作为一个方法来调用
spooky.boo(); //"Boo!"
spooky.boo(); //"Boo!"
console.log(spooky.boo.property); //"properly"

// 使用自定义函数
scareMe(); //"Double boo!"
scareMe(); //"Double boo!"
console.log(scareMe.property); //undefined
```

### 3.5 即时函数

```javascript
// 保持洁净的全局作用域
(function () {
    var days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"],
        today = new Date(),
        msg = "Today is " + days[today.getDay()] + "," + today.getDate();
    console.log(msg);
}());

/* 1)即时函数的参数 */
(function (global) {
    //通过global访问全局变量
}(this));

/* 2)即时函数的返回值 */
var result = ( function () {
    return 2 + 2;
}());
// or
var result = function () {
    return 2 + 2;
}();

var getResult = ( function () {
    var res = 2 + 2;
    return function () {
        return res;
    };
}());

/* 3)定义一个在对象生存期内永远都不会改变的属性 */

var o = {
    message: (function () {
        var who = "me",
            what = "call";
        return what + " " + who;
    }()),
    getMsg: function () {
        return this.message;
    }
};

// 用法
o.getMsg();//"call me"
o.message;//"call me"

/* 4)组件化 */
// 文件module1.js中定义的模块module1
(function () {
    // 模块1的所有代码
}());

/* 5)即时对象初始化 */
({
    // 在这里可以定义设定值
    // 又称为配置常数
    maxwidth: 600,
    maxheight: 400,
    // 还可以定义一些实用的方法
    gimmeMax: function () {
        return this.maxwidth + "x" + this.maxheight;
    },
    // 初始化
    init: function () {
        console.log( this.gimmeMax());
        // 更多初始化任务
    }

}).init();

/*

两种方式都行
({...}.init());

({...}).init();

*/
```

## 3.6 初始化时分支

修改前的代码：

```javascript
var utils = {
    addListener: function (el, type, fn) {
        if (typeof window.addEventListener === "function") {
            el.addEventListener(type, fn, false);
        }
        else if (typeof document.attachEvent === "function") {//IE
            el.attachEvent( "on" + type, fn); //IE11 不在支持该方法
        }
        else {
            el[ "on" + type] = fn;
        }
    },
    removeListener: function (el, type, fn) {
        //几乎一样...
        if (typeof window.removeEventListener === "function") {
            el.removeEventListener(type, fn, false);
        }
        else if (typeof document.detachEvent === "function") {
            el.detachEvent( "on" + type, fn);
        }
        else {
            el[ "on" + type] = null;
        }
    }

};
```

重构后的代码：

```javascript
//接口
var utils = {
    addListener: null,
    removeListener: null
};

// 实现
if ( typeof window.addEventListener === "function") {
    utils.addListener = function (el, type, fn) {
        el.addEventListener(type, fn, false);
    };
    utils.removeListener = function (el, type, fn) {
        el.removeEventListener(type, fn, false);
    };
}
else if ( typeof document.attachEvent === "function") {
    //IE 11已经摒弃了attachEvent(这里只做演示用)
    utils.addListener = function (el, type, fn) {
        el.attachEvent( "on" + type, fn);
    };
    utils.removeListener = function (el, type, fn) {
        el.detachEvent( "on" + type, fn);
    }
}
else {
    utils.addListener = function (el, type, fn) {
        el["on" + type] = fn;
    };
    utils.removeListener = function (el, type, fn) {
        el["on" + type] = null;
    }
}
```

### 3.7 函数属性——备忘模式

函数的`length`属性

```javascript
// 函数length属性

function func(a, b, c) {
}
console.log(func.length); // 3(定义参数的个数)

var myFunc = function (param) {
    if (!myFunc.cache[param]) {
        var result = {};
        //...开销很大的操作...
        myFunc.cache[param] = result;
    }
  
    return myFunc.cache[param];
};
// 缓存存储
myFunc.cache = {};

var myFunc = function () {
    var cachekey = JSON.stringify(Array.prototype.slice.call(arguments)),
        result;
    if (!myFunc.cache[cachekey]) {
        result = {};
        //...开销很大的操作...
        myFunc.cache[cachekey] = result;
    }
  
    return myFunc.cache[cachekey];
};
// 缓存存储
myFunc.cache = {};


var myFunc = function (param) {
    var f = arguments.callee,
    result;
    if (!f.cache[param]) {
        result = {};
        //...开销很大的操作...
        f.cache[param] = result;
    }
    return f.cache[param];

};

// 缓存存储
myFunc.cache = {};
```

说明：如果函数的开销很大且需要反复计算的，可以使用该模式

### 3.8 配置对象 

```javascript
function addPerson() {
    //...
}

var conf = {
    userName: "batman",
    first: "Bruce",
    last: "Wayne"
};

addPerson(conf);
```

**优点:**

- 不需要记住众多的参数以及其顺序
- 可以安全忽略可选参数
- 更加易于阅读和维护
- 更加易于添加和删除参数

**缺点：**

- 需要记住参数名称
- 属性名称无法被压缩

说明：配置对象很有用，jQuery UI，jQgrid等插件都应用了该模式

### 3.9 Curry(柯里化)

#### (1) 函数应用

```javascript
var alien = {
    sayHi: function (who) {
        return "Hello" + (who ? "," + who : "");
    }
};

alien.sayHi("world");

sayHi.apply(alien, ["huamans"]);

sayHi.call(alien, "huamans"); //call是apply的语法糖

```

是函数理解并处理部分应用的过程就称为Curry过程（Currying）函数转换的过程

```javascript
// curry化的add()函数
// 接受部分参数列表
function add(x, y) {
    var oldx = x, oldy = y;
    if (typeof oldy === "undefined") {
        // 部分
        return function (newy) {
            return oldx + newy;
        };
    }
}

//测试
typeof add(5); // "function"
add(3)(4); // 7

// 创建并存储一个新函数
var add2000 = add(2000);
add2000(10);//2010
```

更为简单的版本

```javascript
// curry化的add()函数
// 接受部分参数列表
function add(x, y) {
    if (typeof y === "undefined") {
        return function (y) {
            return x + y;
        };
    }
    // 完全应用
    return x + y;
}

```

通用curry化函数示例(干货)

``` javascript
function schonfinkelize(fn) {
    var slice = Array.prototype.slice,
    stored_args = slice.call(arguments, 1); // 存储调用该方法后的参数
    return function () {
        var new_args = slice.call(arguments),
        args = stored_args.concat(new_args); // 合并到新参数
        return fn.apply(null, args);
    };
}
```

```javascript
//普通函数
function add(x, y) {
    return x + y;
}
// 将一个函数curry话以获得一个新的函数
var newadd = schonfinkelize(add, 5);
newadd(4); //9

// 另一种选择——直接调用新函数
schonfinkelize(add, 6)(7); // 13

// 转换函数schonfinkelize()并不局限一单个参数或者但不Curry化
// more demo
function add(a, b, c, d, e) {
    return a + b + c + d + e;
}
// 可运行于任意量的参数
schonfinkelize(add, 1, 2, 3)(5, 5);//16
// 两步curry化
var addOne = schonfinkelize(add, 1);
addOne(10, 10, 10, 10); // 输出41
var addSix = schonfinkelize(addOne, 2, 3);
addSix(5, 5); // 输出16
```



## 4. 对象创建模式

- 命名空间模式
- 私有属性和方法
- 模块模式
- 揭示模块模式
- 创建构造函数的模块
- 沙箱模式
- 静态成员
- 对象常量
- 链模式
- method()方法

### 4.1 命名空间模式

**有助于减少程序中所遇要的全局变量的数量，同时还有助于避免命名冲突或过长的名字前缀。**

```javascript
var MYAPP = {}; // 公约一般以全部大写来命名
// 构造函数
MYAPP.Parent = function () { };
MYAPP.Child = function () { };

// 变量
MYAPP.someVar = 1;

// 一个对象容器
MYAPP.modules = {};

// 嵌套对象
MYAPP.modules.module1 = {};
MYAPP.modules.module1.data = { a: 1, b: 2 };
MYAPP.modules.module2 = {};

/* =========通用命名空间函数======= */

// 不安全的代码
var MYAPP = {};
// 更好的代码风格
if ( typeof MYAPP === "undefined") {
    var MYAPP = {};
}

// 或者更简短的语句
var MYAPP = MYAPP || {};

/* 命名空间函数实现示例 */
var MYAPP = MYAPP || {};

MYAPP.namespace = function (nsString) {
    var parts = nsString.split( "."),
        parent = MYAPP,
        i;
  
    // 剥离最前面的冗余全局变量
    if (parts[0] === "MYAPP") {
        parts = parts.slice(1);
    }
    
    for (i = 0; i < parts.length; i++) {
        // 如果不存在就创建一个属性
        if (typeof parent[parts[i]] === "undefined") {
            parent[parts[i]] = {};
        }
        parent = parent[parts[i]];
    }
  
    return parent;
};

// 使用
var module2 = MYAPP.namespace( "MYAPP.modules.module2");
console.dir(MYAPP);

MYAPP.namespace("once.upon.a.time.there.was.this.long.nested.property");
console.dir(MYAPP);

```

**声明依赖关系**

```javascript
/* 声明依赖关系 */
var myFunction = function () {
    // 依赖
    var event = YAHOO.util.Event,
        dom = YAHOO.util.DOm;
  
    /*(类似C#中命名空间的别名)*/
    // 使用事件和DOM变量
    // 下面的函数

};

```

- 显示的依赖声明向您代码的用户表明了他们确定需要的特定脚本文件已经包含在该页面中；
- 在函数的顶部的前期声明可以使您很容易地发现并解析依赖；
- 解析局部变量 的速度总是要比解析全局变量要快，甚至比使用全局变量的嵌套属性(比如YAHOO.util.Dom)还要快；使用这种依赖声明模式时，全局符号解析仅会在函数中执行一次，在此后将会使用局部变量，这种解析速度也快得多；
- 类似于YUIComparessor和Google闭包编译器的这些高级小工具可以重命名局变量，但不会对全局变量进行重命名。


## 4.2 私有属性和方法

```javascript
/* 私有属性和方法 */

function Gadget() {
    // 私有成员
    var name = "iPod";
    // 公有函数(特权方法)
    this.getName = function () {
        return name;
    };
}

var toy = new Gadget();
console.log(toy.name); // undefined
console.log(toy.getName()); // "iPod"
```

（1）私有性失效

```javascript
function Gadget() {
    // 私有成员
    var specs = {
        screenWidth: 320,
        screenHeight: 480,
        color: "white"
    };
    
    // 公有函数
    this.getSpecs = function () {
        return specs;
    };

}
```

说明：当公有函数直接返回私有成员（数组、对象时），此时即发生了私有性失效

（2）对象字面量以及私有属性

```javascript
var myobj; //这将会是对象

(function () {

    // 私有成员
    var name = "Jane";
    // 实现公有部分
    // 注意，没有'var'修饰符
    myobj = {
        // 特权方法
        getName: function () {
            return name;
        }
    };

}());

console.log(myobj.getName()); // Jane

// 方式2
var myobj = ( function () {
    // 私有成员
    var name = "Jane";
    // 实现公有部分
    return {
        getName: function () {
            return name;
        }
    };
}());

console.log(myobj.getName());
// 说明：该例子称之为“模块模式”的基础框架
```

（3）原型和私有性

```javascript
function Gadget() {
    //私有成员
    var name = "IPad";
    //公有属性
    this.getName = function () {
        return name;
    };
}

Gadget.prototype = (function () {
    //私有成员
    var browser = "Mobile Webkit";
    //公有原型成员
    return {
        getBrowser: function () {
            return browser;
        }
    };
}());

var toy = new Gadget();
console.log(toy.getName());
console.log(toy.getBrowser());
```

（4）将私有方法揭示为公共方法

即：**将私有方法赋值给全局变量的方法**

```javascript
var myArray;

(function () {

    var astr = "[object Array]";
    // 私有方法
    function isArray(a) {
        return toString.call(a) === astr;
    }
    // 私有方法
    function indexOf(hayStack, needle) {
        var i = 0,
            max = hayStack.length;
        for (; i < max; i++) {
            if (hayStack[i] === needle) {
                return i;
            }
        }
        return -1;
    }
  
    // 将私有方法揭示为公共方法
    myArray = {
        isArray: isArray,
        indexOf: indexOf,
        inArray: indexOf
    };

}());

myArray.isArray([1, 2]);
myArray.isArray({ 0: 1 });
myArray.indexOf(["a", "b", "c"], "c");
myArray.inArray(["a", "b", "c"], "c");
// 公共方法myArray.indexOf() 发生意外不会影响私有方法indexOf()
myArray.indexOf = null;
myArray.inArray(["a", "b", "c"], "c");//2
```

### 4.3 模块模式

```javascript
MYAPP.namespace = function (nsString) {
    var parts = nsString.split( "."),
        parent = MYAPP,
        i;
  
    // 剥离最前面的冗余全局变量
    if (parts[0] === "MYAPP") {
        parts = parts.slice(1);
    }
    
    for (i = 0; i < parts.length; i++) {
        //如果不存在就创建一个属性
        if (typeof parent[parts[i]] === "undefined") {
            parent[parts[i]] = {};
        }
        parent = parent[parts[i]];
    }
  
    return parent;
};

// 1.建立命名空间
MYAPP.namespace("MYAPP.utilities.array");
// 2.定义模块
MYAPP.utilities.arry = (function () {
    return {
        // TODO
    };
}());

// 3.向公共接口添加一些方法
MYAPP.utilities.array = (function () {
    // 依赖
    var uobj = MYAPP.utilities.object,
        ulang = MYAPP.utilities.lang,
        //私有属性
        arrayString = "[object Array]",
        ops = Object.prototype.toLocaleString;
    
    // 私有方法
    // ...
    // var变量定义结束
    
    // 可选的一次性初始化过程
    // ...
    
    // 公有API
    return {
        inArray: function (neddle, haystack) {
            for ( var i = 0, max = haystack.length; i < max; i++) {
                if (haystack[i] === neddle) {
                    return true;
                }
            }
        },
        isArray: function (a) {
            return ops.call(a) === arrayString;
        }
        // ...更多方法和属性
    }

}());
```

### 4.4 揭示模块模式

```javascript
MYAPP.utilities.array = (function () {
        // 私有属性
    var arrayString = "[object Array]",
        ops = Object.prototype.toString,
        // 私有方法
        inArray = function (haystack, needle) {
            for ( var i = 0, max = haystack.length; i < max; i++) {
                if (haystack[i] === needle) {
                    return i;
                }
            }
            return -1;
        },
        isArray = function (a) {
            return ops.call(a) === arrayString;
        };
    // <<var变量定义结束
    
    // 揭示公有API
    return {
        isArray: isArray,
        indexOf: inArray
    }

}());
```

### 4.5 创建构造函数的模块

```javascript
MYAPP.namespace("MYAPP.utilities.Array");

MYAPP.utilities.Array = (function () {

    // 依赖
    var uobj = MYAPP.utilities.object,
           ulang = MYAPP.utilities.lang,
           // 私有属性和方法
           Constr;
  
    // var 变量定义结束
    
    // 可选的一次性初始化过程
    //...
    
    // 公有API——构造函数
    Constr = function (o) {
        this.elements = this.toArray(o);
    };
  
    // 公有API——原型
    Constr.prototype = {
        constructor: MYAPP.utilities.Array,
        version: "2.0",
        toArray: function (obj) {
            for ( var i = 0, a = [], len = obj.length; i < len; i++) {
                a[i] = obj[i];
            }
            return a;
        }
    };
    
    //返回要分配给新命名空间的构造函数
    return Constr;

}());

// 使用
var arr = new MYAPP.utilities.Array(obj);

/* =====将全局变量导入到模块中(加速解析)===== */
MYAPP.uitilities.module = (function (app, global) {
    // 引用全局对象
    // 以及现在被转换成局部变量的
    // 全局应用程序命名空间对象
}(MYAPP, this));
```



### 4.6 沙箱模式

概念：沙箱模式提供了一个可用于模块运行的环境，且不会对其他模块和个人沙箱造成任何影响

(1)全局构造函数Sandbox()

可以使用该构造函数创建对象并且还可以传递回调函数，它是代码的隔离沙箱运行环境。

```javascript
new Sandbox( function (box /* {object} */) {
    // box有您所需要的所有库函数，能够使代码正常运行
    // 你的代码写在这里...
});

Sandbox("dom" , "event" , function () {
    //使用DOM和事件来运行
    Sandbox("ajax", function (box) {
        //另一个沙箱化(sandboxed)的"box"对象
        //这里的"box"对象与函数外部的"box"并不相同
        //...
        //用Ajax来处理
    });
    // 这里没有Ajax模块
});

// 增加模块(dom、event、ajax)
Sandbox.modules = {};
Sandbox.modules.dom = function (box) {
    box.getElemet = function () { };
    box.getStyle = function () { };
    box.foo = "bar";
};

Sandbox.modules.event = function (box) {
    // 如果需要，就访问Sandbox原型，如下语句：
    // box.constructor.prototype.m="mmm"
    box.attachEvent = function () { };
    box.detachEvent = function () { };
};

Sandbox.modules.ajax = function (box) {
    box.makeRequest = function () { };
    box.getResponse = function () { };
};

// 实现构造函数
function Sandbox() {
    // 将参数转换成一个数组
    var args = Array.prototype.slice.call(arguments),
        //最后一个参数是回调函数
        callback = args.pop(),
        // 模块可以作为一个数组传递,或作为单独的参数传递
        modules = (args[0] && typeof args[0] === "string") ? args : args[0],
        i;
    // 确保该函数
    // 作为构造函数被调用
    if (!(this instanceof Sandbox)) {
        return new Sandbox(modules, callback);
    }
    // 需要项"this"添加属性
    this.a = 1;
    this.b = 2;
    // 现在向该核心"this"对象添加模块
    // 不指定模块名称或指定"*"都表示"使用所有模块"
    if (!modules || modules === "*") {
        modules = [];
        for (i in Sandbox.modules) {
            if (Sandbox.modules.hasOwnProperty(i)) {
                modules.push(i);
            }
        }
    }
    // 初始化所需的模块
    for (i = 0; i < modules.length; i += 1) {
        Sandbox.modules[modules[i]]( this);
    }
    // call the callback
    callback(this);

}

// 需要的任何原型属性
Sandbox.prototype = {
    name: "My Application",
    version: "1.0",
    getName: function () {
        return this.name;
    }
};
```

### 4.7 静态成员

(1)公有静态成员

```javascript
/* 构造函数 */
var Gadget = function () { };

// 静态方法
Gadget.isShiny = function () {
    return "you bet";
};

// 向该原型添加一个普通方法
Gadget.prototype.setPrice = function (price) {
    this.price = price;
};

// 调用静态方法
Gadget.isShiny(); // "you bet"
var iphone = new Gadget();
iphone.setPrice(500);
console.log(iphone.price); //500

// 静态方法与实例一起工作（指向静态方法）
Gadget.prototype.isShiny = Gadget.isShiny;
iphone.isShiny(); // "you bet"
```

（2）静态方法

```javascript
Gadget.isShiny = function () {
    // 这种方法总是可以运行
    var msg = "you bet";
    if (this instanceof Gadget) {
        // 非静态调用()
        msg += ",it costs $" + this.price + "|";
    }
  
    return msg;
};

// 向该原型添加一个普通方法
Gadget.prototype.isShiny = function () {
    return Gadget.isShiny.call( this);
};

// 静态调用
Gadget.isShiny(); "you bet"

// 实例调用
var a = new Gadget( "599.99");
a.isShiny();//"you bet,it costs $599.99|"
```

（3）私有静态成员

以同一个构造函数创建的所有对象共享该成员，构造函数外部不可访问该成员

```javascript
var Gadget = ( function () {
    // 静态变量/属性
    var counter = 0;
    // 返回该构造函数的新实现
    return function () {
        console.log(counter += 1);
    };

}()); // 立即执行

var g1 = new Gadget();
var g2 = new Gadget();
var g3 = new Gadget();

// 构造函数
var Gadget = ( function () {
    var counter = 0,
        NewGadget;
    // 这将成为新的构造函数的实现
    NewGadget = function () {
        counter += 1;
    };
    
    // 特权方法
    NewGadget.prototype.getLastId = function () {
        return counter;
    };
    
    // 覆盖该构造函数
    return NewGadget;

}());// 立即执行

var iphone = new Gadget();
iphone.getLastId(); // 1

var ipod = new Gadget();
ipod.getLastId(); // 2

var ipad = new Gadget();
ipad.getLastId(); // 3
```

### 4.8 对象常量

```javascript
var constant = ( function () {
    var constants = {},
        ownProp = Object.prototype.hasOwnProperty,
        allowed = {
            string: 1,
            number: 1,
            boolean:1
        },
        prefix = (Math.random() + "_").slice(2);
    return {
        isDefined: function (name) {
            /// <summary>
            /// 检测指定常量是否存在
            /// </summary>
            /// <param name="name">常量名</param>
            /// <returns type=""></returns>
            return ownProp.call(constants, prefix + name);
        },
        set: function (name, value) {
            /// <summary>
            /// 定义一个新常量
            /// </summary>
            /// <param name="name">常量名</param>
            /// <param name="value">常量值</param>
            /// <returns type="Boolean"></returns>
            if ( this.isDefined(name)) {
                return false;
            }
            if (!ownProp.call(allowed, typeof value)) {
                return false;
            }
            constants[prefix + name] = value;
            return true;
        },
        get: function (name) {
            /// <summary>
            /// 读取指定常量的值
            /// </summary>
            /// <param name="name">常量名</param>
            /// <returns type=""></returns>
            if ( this.isDefined(name)) {
                return constants[prefix + name];
            }
            return null;
        }
    };

}());

// 检查是否定义
constant.isDefined("maxwidth"); //false
//定义
constant.set("maxwidth", 489); //true
//再次检查
constant.isDefined("maxwidth"); //true
//视图重新定义
constant.set("maxwidth", 320); //false
//该值是否仍保持不变
constant.get("maxwidth");  //489
```

### 4.9 链模式

```javascript
var obj = {
    value: 1,
    increment: function () {
        this.value += 1;
        return this;
    },
    add:function(v) {
        this.value += v;
        return this;
    },
    shout: function () {
        alert( this.value);
        return this;
    }
};

//链方法调用
obj.increment().add(10).add(10).shout().increment().shout();

```

### 4.10 method() 方法

```javascript
if ( typeof Function.prototype.method!== "funcion" ) {
    Function.prototype.method = function (name, implementation) {
     /// <summary>
     /// 添加方法
     /// </summary>
     /// <param name="name">方法名</param>
     /// <param name="implementation">方法实现</param>
        this.prototype[name] = implementation;
        return this;
    };
}

// 使用
var Person = function (name) {
    this.name = name;
}.method("getName", function () {
    return this.name;
}).method("setName", function (name) {
    this.name = name;
}).method("setName", function () {
    this.name = "Jane";
});
```



