# 编写可维护的JavaScript

## 1 代码书写

1.1 缩进层级
1.2 语句结尾
1.3 行的长度
1.4 换行
1.5 空行

- 添加适当的空行
- 在方法之间
- 在方法中的局部变量（`local variable`）和第一条语句之间
- 在多行或单行注释之前
- 在方法内的逻辑片段之间插入空行，提高可读性

1.6 命名
1.6.1 变量和函数
对于函数命名来说，第一个单词一般是动词，常见的动词约定如下

`can`

函数返回一个布尔值

```javascript
function canSubmitData(){
  var flag = false;
  // ...
  return flag;
}
```

`has`

函数返回一个布尔值

```javascript
function hasClass(className){
  var isHasClass = false;
  // ...
  return isHasClass;
}
```

`is`

函数返回一个布尔值

```javascript
function isString(obj){
  return typeof(obj) === 'string';
}
```

`get`

函数返回一个非布尔值

`set`

函数用来保存一个值

```javascript
if (isEnabled()) {
    setName( "Jane");
}

if (getName() === "Jane") {
    doSomething();
}
```

1.6.2 常量

用大写字母和下划线来命名约定。

```javascript
var MAX_COUNT = 10;
var URL = "http://www.echo.me";
// 在ES6中(可以使用const关键字)
const MAX_COUNT = 10;
const URL = "http://www.echo.me";
```

1.6.3 构造函数

构造函数应遵循Pascal命名规范。（区分`Pascal Case`和` Camel Case`）

```javascript
// 构造函数
function Person(name) {
    this.name = name;
}

// 原型方法
Person.prototype.sayName = function () {
    console.log( this.name);
};

var me = new Person("Jane");
```

1.7 直接量
1.7.1 字符串
无论使用单引号(`''`)还是双引号(`""`)括住字符串，在代码中务必统一代码风格。

字符串的换行

```javascript
// bad
var longString = "Here's the story of man \
named Jane." ;

// good
var longString = "Here's the story of man " +
"named Jane.";
```

1.7.2 数值

整数请勿添加小数点，如果是小数请勿省略小数点前面和后面部分。

```javascript
var count = 10;

var price = 10.0;

var price = 10.00;
```

1.7.3 null

`null`是个一个特殊值，但我们常常误解它，将它和`undefined`搞混淆，下列场景中应该使用null:

- 用来初始化一个变量，这个变量可能赋值为一个对象。
- 用来和一个已经初始化的变量比较，这个变量可以是也可以不是一个对象。
- 当函数的参数期望值是对象时，用作参数传入。
- 当单数的返回值期望是对象时，用作返回值传出。

不应该使用`null`的场景：

- 不要使用`null`来检测是否传入了某个参数。

- 不要使用`null`来检测一个未初始化的变量。

  ```javascript
    //good
    var person = null;

    //good
    function getPerson() {
        if (condition) {
            return new Person( "Jane");
        } else {
            return null;
        }
    }

    //good
    var person = getPerson();
    if (person !== null) {
        doSomething();
    }

    //bad:用来和未初始化的变量比较
    var person;
    if (person != null) {
        doSomething();
    }
   //bad:检测是否传入了参数
    function doSomething(arg1, arg2, arg3, arg4) {
        if (arg4 != null) {
            doSomethingElse();
        }
    }
  ```

1.7.4 `undefined`

`undefined` 是一个特殊值，我们常将它和`null`混淆；

让人困惑的地方：

- `null == undefined; // true`
- `typeof`运算无论是值为`undfined`的变量还是未声明的变量，其都返回`"undefined"`

```javascript
// #1 bad
var person;
console.log(person === undefined);

// #2 foo未被声明
var person;
console.log( typeof person);
console.log( typeof foo);
```

避免使用undefined，确保只有在一种情况下`typeof`运算才会返回`"undefined"`;

```javascript
// good
var person = null;
console.log(person === null); // true

(typeof null) // 返回"object"
```

1.7.5 对象直接量

直接量可以高效完成非直接量写法相同的任务，非直接量写法语法看起来要更为复杂。
```javascript
    // bad
    var book = new Object();
    book.title = "Maintainable JavaScript";
    book.author = "Zakas";
    // good
    var book = {
        title: "Maintainable JavaScript",
        author: "Zakas"
    };
```

1.7.6 数组直接量

```javascript
    // bad
    var colors = new Array("red", "yellow", "blue", "white");
    var numbers = new Array(1, 2, 3, 4);
    
	// good
    var colors = ["red", "yellow", "blue", "white"];
    var numbers = [1, 2, 3, 4];
```

## 2  JavaScript 注释

### 2.1 行注释

- 独占一行的注释，用来解释下一行代码。这行注释之前总有一个空行，且缩进层级和下一行代码保持一致。
- 在代码行的尾部的注释。代码结束到注释之间至少有个一缩紧。注释（包括之前的代码部分）不应到超过单行最大字符数显示，如果超过了，就将这条注释放置于当前代码行的上方。
- 被注释掉的打断代码（很多编辑器都可以批量注释掉多行代码）。

### 2.2 多行注释

### 2.3 使用注释

添加注释的一般原则：需要让代码变得更清晰时添加注释。

- 难于理解的代码
- 可能被认为错误的代码
- 浏览器特性hack
- 文档注释

### 3 JavaScript 语句

### 3.3 switch语句

```javascript
switch (condition) {
    case "one":
        //...
        break;
    case "two":
        //...
        break;
    case "three":
        //...
        break;
    case "four":
        //...
        break;
    default:
        //...
}
```

另外一种格式：case和switch左对齐

#### 3.3.2 case的连续执行

大多数人认为是可取的，只要程序逻辑清晰即可

#### 3.3.3 default

*  观点1：无论何时都应该保留default

*  观点2：省略没有默认行为的default，并添加注释

   ```javascript
   switch (condition) {
    case "one":
        //...
        break;
    case "two":
        //...
        break;
        //no default
   }
   ```

### 3.4 with语句

 **禁止使用`with`语句！！！**

### 3.5 for

```javascript
/* break */
var values = [1, 2, 3, 4, 5, 6, 7],
    i,
    len;
for (i = 0, len = values.length; i < len; i++) {

    if (i == 2) {
        break; // 迭代不会继续
    }
    console.log (values[i]);

}

/* continue */
var values = [1, 2, 3, 4, 5, 6, 7],
    i,
    len;

for (i = 0, len = values.length; i < len; i++) {
    if (i == 2) {
        continue; // 跳过本次循环
    }
    console.log(values[i]);
}


/* Crockford的编程规范不允许使用continue，而是主张使用条件语句代替 */
var values = [1, 2, 3, 4, 5, 6, 7],
    i,
    len;

for (i = 0, len = values.length; i < len; i++) {
    if (i !== 2) {
        process(values[i]);
    }
}
```

3.6 for-in

用`hasOwnProperty()`方法过滤出实例属性

```javascript
var prop;
for (prop in object) {
    // 若想要查找原型链，则省略if条件语句
    if (object.hasOwnProperty(prop)) {
        console.log( "Property name is " + prop);
        console.log( "Property value is " + object[prop]);
    }
}
```

**不要用for-in来遍历数组**

```javascript
var values = [1, 2, 3, 4, 5, 6, 7],
    i;
for (i in values) {
    process(values[i]);
}
```



## 4 变量函数和运算符

### 4.1 变量声明

- 变量先声明再使用
- JavaScript没有块级变量声明(ES6中的class也只是语法糖)

```javascript
// 通常的写法
function doSomethingWithItems(items) {
    for (var i = 0, len = itmes.length; i < len; i++) {
        doSomething(items[i]);
    }
}

// 建议的写法
function doSomethingWithItems(items) {
    var i,
        len;
    for (i = 0, len = items.length; i < len; i++) {
        doSomething(items[i]);
    }
}
```

- 建议总是将局部变量的定义作为函数内第一条语句；
-  并且使用单var模式。

```javascript
function doSomethingWithItems(items) {
    var value = 10,
        result = value + 10,
        i,
        len;
    
    for (i = 0, len = items.length; i < len; i++) {
        doSomething(items[i]);
    }
}
```

**为了保持成本最低，推荐合并var 语句！！！**

### 4.2 函数声明

```javascript
// bad
doSomething();
function doSomething() {
    console.log("Hello world!");
}

// good (先定义，后使用)
function doSomething() {
    console.log("Hello world!");
}
doSomething();
```

### 4.3 函数的间隔

```javascript
function func(){
  // ...
}

function func1(){
  // ...
}

var Tools = {
  getWidth:function(){
    // ...
  },
  getSomethind:function(){
    // ...
  }
};
```

### 4.4 立即调用的函数

```javascript
// bad
var value = function () {
    // code
    return {
        message: "Hi"
    };

}();

// good
var value = ( function () {
    // code
    return {
        message: "Hi"
    }
}());
```

### 4.5 严格模式

```javascript
/* 1# */
// bad - 全局的严格模式

"use strict";

function doSomething() {
    //code
}

/* 2# */
// bad
function doSomething() {
    "use strict";
     // code
}

/* 3# */
// good
(function () {

    "use strict";
  
    function doSomething() {
        //code
    }
    
    function doSomethingElse() {
        //code
    }

})();
```

### 4.6 相等

`== !=` 和 `=== !==`

(1)如果一个布尔值和数字比较，布尔值会首先转换位数字，然后进行比较。
false 值变为0、true变为1.

```javascript
// 数字1和true
console.log(1 == true); // true

// 数字0和false
console.log(0 == false); // true

// 数字2和false
console.log(2 == true); // false
```

(2)如果其中一个值是对象而另一个不是，则会首先调用对象的`valueOf()`方法，得到原始类型值再进行比较。如果没有定义`valueOf()`，则在调用`toString()`。之后的比较操作就和上文提到的多类型值比较的情形了一样。

```javascript
// 数字5和字符串5
console.log(5 == "5"); // true
console.log(5 === "5"); // false

// 数字25和16进制字符串25
console.log(25 == "0x19"); // true
console.log(25 === "0x19"); // false

// 数字1和true
console.log(1 == true); // true
console.log(1 === true); // false

// 数字0和false
console.log(0 == false); // true
console.log(0 === false); // false

// 数字2和true
console.log(2 == true); // false
console.log(2 === true); // false
```



## 6 避免使用全局变量

### 6.1 全局变量

```javascript
var color = "red";

function sayColor() {
    console.log(color);
}

console.log(window.color); // "red"
console.log(typeof window.sayColor); // "function"
```

6.1.1 命名冲突

```javascript
function sayColor() {
    console.log(color); // 不好的做法,color从哪里来的？？
}
```

6.1.2 代码脆弱性
6.1.3 难以测试

### 6.2 意外的全局变量

在处理老代码时要非常小心的使用严格模式，对于新代码最好使用严格模式来避免意外的全局变量，
同时一些常见的编码错误也能在严格模式中被捕捉到。

### 6.3 单全局变量方式

```javascript
function Book(title) {

    this.title = title;
    this.page = 1;

}

Book.prototype.turnPage = function (direction) {

    this.page += direction;

}

var Chapter1 = new Book( "Introction to Style GuideLines");

var Chapter2 = new Book( "Basic Formatting");

var Chapter3 = new Book( "Comments");

// 使用单全局变量

var MaintainableJS = {};

MaintainableJS.Book = function (title) {

    this.title = title;
    this.page = 1;

};

MaintainableJS.Book.turnPage = function (direction) {

    this.page = direction;

};

MaintainableJS.Chapter1 = new MaintainableJS.Book( "Introction to Style GuideLines");

MaintainableJS.Chapter2 = new Book("Basic Formatting");

MaintainableJS.Chapter3 = new Book("Comments");
```

#### 6.3.1 命名空间

```javascript
var ZakasBooks = {};

//编写可维护JavaScript的命名空间
ZakasBooks.MaintainableJavaScript = {};

//高性能JavaScript的mingmingkongjian
ZakasBooks.HighPerformanceJavaScript = {};

var YourGlobal = {

    namespace: function (ns) {
        var parts = ns.split( "."),
            object = this,
            i,
            len;
        for (i = 0, len = parts.length; i < len; i++) {
            if (!object[parts[i]]) {
                object[parts[i]] = {};
            }
            object = object[parts[i]];
        }
        return object;
    }

};

YourGlobal.namespace("Books.MaintainableJavaScript");

YourGlobal.Books.MaintainableJavaScript.author = "Nicholas c. Zakas";

YourGlobal.namespace("Books.HighPerformaceJavaScript");

console.log(YourGlobal.Books.MaintainableJavaScript.author);

YourGlobal.namespace("Bookes").ANewBook = {};

// good- 拆分应用逻辑

var MyApplication = {

    dandleClick: function (event) {
        this.showPooup(event);
    },
    
    showPopup: function (event) {
        var popup = document.getElementById( "popup");
        popup.style.left = event.clientX + "px";
        popup.style.top = event.dandleClick + "px";
        popup.className = "reveal";
    }

};

addEventListener(element, "click", function (event) {

    MyApplication.handleClick(event);

});

// good
var MyApplication = {

    dandleClick: function (event) {
        this.showPooup(event.clientX, event.clientY);
    },
    
    showPopup: function (x, y) {
        var popup = document.getElementById( "popup");
        popup.style.left = x + "px";
        popup.style.top = y + "px";
        popup.className = "reveal";
    }

};

addEventListener(element, "click", function (event) {

    MyApplication.handleClick(event);

});

//good #3
//good
var MyApplication = {

    dandleClick: function (event) {
        /// <summary>
        /// 事件处理程序
        /// </summary>
        /// <param name="event"></param>
    
        //假设事件支持DOM Level2
        event.prventDefault();
        event.stopPropagation();
    
        //传入应用逻辑
        this.showPooup(event.clientX, event.clientY);
    },
    
    showPopup: function (x, y) {
        /// <summary>
        /// 应用逻辑
        /// </summary>
        /// <param name="x"></param>
        /// <param name="y"></param>
        var popup = document.getElementById( "popup");
        popup.style.left = x + "px";
        popup.style.top = y + "px";
        popup.className = "reveal";
    }

};

addEventListener(element, "click", function (event) {
    MyApplication.handleClick(event);
});
```



## 8 避免空比较

```javascript
var Controller = {
    process: function (items) {
        if (items !== null) {
           // 不好的写法
            itmes.sort();
            items.forEach( function (items) {
                //执行一些逻辑
            });
        }
    }
};
```

### 8.1 检测原始值

5种原始类型:**字符串、数字、布尔值、null、undefined**

如果希望一个值是`字符串、数字、布尔值、undefined`最佳选择是`typeof`运算符

- 对于字符串,typeof返回"string"
- 对于数字,typeof返回"number"
- 对于布尔值,typeof返回"boolean"
- 对于undefined,typeof返回"undefined"
- 对于null,typeof返回"object"(低效的判断方法，建议直接使用===和!==)

typeof运算符的独特之处在于，将其应用于一个未声明的变量也不会报错。

简单的和null比较通常不会包含足够的信息以判断值的类型是否合法，但有一个例外，如果所期望的值真的是null,
则可以直接和null比较，这时应当使用` === `或者` !== `来和`null`进行比较

```javascript
var element = document.getElementById( "my-div");
if (element !== null) { //null是可以预见的输出
    element.className = "found";
}
```

### 8.2 检测引用值

引用值也称作对象(object), 在JavaScript中除了原始值之外的值都是引用,
`Object、Array、Date、Error`;

```javascript
console.log(typeof {}); // "object"
console.log(typeof []); // "object"
console.log(typeof new Date()); // "object"
console.log(typeof new RegExp()); // "object"
console.log(typeof null); // "object"
```

**使用instanceof来检测引用类型**

```javascript
// 检测日期date
if (value instanceof Date) {
    console.log(value.getFullYear());
}

// 检测正则表达式
if (value instanceof RegExp) {
    if (value.test(anotherValue)) {
        console.log( "Matches");
    }
}

// 检测Error
if (value instanceof Error) {
    throw value;
}

// IE8及更早版本的IE
console.log(typeof document.getElementById); // "object"
console.log(typeof document.createAttribute); // "object"
console.log(typeof document.getElementsByTagName); // "object"

//采用鸭式辨型的方法检测数组
function isArray(value) {
    return typeof value.sort === "function";
}

//Kangax的解决方案

function isArray(value) {

    return Object.prototype.toString.call(value) === [ "object Array"];

}

function isJSON(value) {

    return Object.prototype.toString.call(value) === [ "object JSON"];

}

// 兼容的做法
function isArray(value) {
    if (typeof Array.isArray === "function") {
        return Array.isArray(value);
    } else {
        return Object.prototype.toString.call(value) === ["object Array"];
    }
}

```



#### 8.3 检测属性

```javascript
//bad:检测假值
if (object[propertyName]) {
    //code...
}

//bad:和null比较
if (object[propertyName] != null) {
    //code...
}

//bad:和undefined比较
if (object[propertyName] != undefined) {
    //code...
}
```

上面的代码里的每个判断，实际上是通过给定的名字来检查属性的值，而非判断
给定的名字所指的属性是否存在，因为当属性值为假(false)时结果会出错，
比如0、""(empty string)、false、null和undefined

**用in运算符判断属性是否存在**

``` javascript
var obj = {
    count: 0,
    related: null
};

// 好的写法
if ( "count" in obj) {
    // 这里的代码会执行
    console.log("count is in obj");
}

// 不好的写法:检测假值
if (obj[ "count"]) {
    //这里的代码不会执行
}

// 好的写法
if ( "related" in obj) {
    // 这里的代码会执行
}

if (obj[ "releated"] != null) {
    // 这里代码不会执行
}

if (obj.length) {
    console.log("nothing");
} else {
    console.log("obj.length is :" + obj.length); // obj.length is :undefined
}
```

如果你只想检查实例对象的某个属性是否存在则使用Object.hasOwnProperty()方法,
需要注意的是:
IE8及更早版本中,DON对象并非继承自Object，所以不包含该方法

```javascript
// 对于所有非DOM对象来说，这是好的写法
if (obj.hasOwnProperty( "related")) {
    // 执行这里的代码
}

// 如果你不确定是否为DOM对象，则这样来写
if ("hasOwnProperty" in obj && obj.hasOwnProperty( "related")) {
    // 执行这里的代码
}
```

因为存在IE8以及更早版本的IE，在判断实例对象时候存在时，NC更倾向于使用in运算符，只有在需要判断实例属性时才会使用到hasOwnProperty()

不管你什么时候需要检测属性的存在性，请使用 in 运算符或者hasOwnProperty(),这样做可以避免很多Bug

## 10 抛出自定义错误

```javascript
// bad
throw "message";

// 如果愿意，你可以跑出任何类型的数据类型
throw { name: "Jane" };
throw true;
throw 12345;
throw new Date();

// 如果没有通过try-catch语句捕获，抛出任何值都将引发一个错误
function getDivs(element) {
    return element.getElementsByTagName( "div");
}

function getDivs(element) {
    if (element && element.getElementsByTagName) {
        return element.getElementsByTagName( "div");
    } else {
        throw new Error("getDivs():Argument must be a DOM element.");
    }
}

// 何时抛出错误???

// 不好的写法：检查了太多的错误
function addClass(element, className) {
    if (!element || typeof element.className != "string") {
        throw new Error("addClass():first argument must be a DOM element");
    }
    if (typeof className != "string") {
        throw new Error("addClass():second argument must be a string");
    }
    element.className += " " + className;
}

// 好的写法
function addClass() {
    if (!element || typeof element.className != "string") {
        throw new Error("addClass():first argument must be a DOM element");
    }
    element.className += " " + className;
}
```

**抛出错误的最佳地方是在工具函数中（JavaScript类库）**

**抛出错误很好的经验法则:**

- 一旦修复了一个很难调试的错误, 尝试增加一两个自定义错误。当再次发生错误时，这将有助于更容易地解决问题。
- 如果正在编写代码，思考一下：我希望某些事情不会发生，如果发生，我的代码回一团糟。这时,如果[某些事情]发生,就跑出一个错误
- 如果正在编写的代码别人（不知道是谁）也会使用，思考一下他们使用的方式，在特定的情况下抛出错误。
- 目的不是防止错误，而是在错误发生时能更加容易地调试。

```javascript
// 10.5 try-catch语句
try {
    somethingThatMightCauseAnError();
} catch (e) {
    handleError(ex);
}


try{
    somethingThatMightCauseAnError();
} catch (ex) {
    handleError(ex);
} finally {
    continueDoingOtherStuff();
}

// finally块工作起来有点复杂（微妙）,如果try中有return语句，
// 实际上它必须等到finally块中代码执行后才能返回,所以finally其实不太常用

// 不好的用法
try {
    somethingThatMightCauseAnError();
} catch (ex) {
    //donothing
}
```

### 10.6 错误类型

- Error —— 所有错误的基本类型。实际上引擎从来不会抛出该类型的错误。
- EvalError —— 通过eval()函数执行代码发生错误时抛出。
- RangeError —— 一个数字超出它的边界时抛出;
- ReferenceError —— 期望对象不存在时抛出
- SyntaxError —— 给eval()函数传递的代码中有语法错误时抛出
- TypeError —— 变量不是期望的类型时抛出。例如,new 10 或 "prop" in true
- URIError —— 给encodeURI、encodeURIComponent、decodeURI、decodeURIComponent等函数传递非法的URI字符串时抛出

```javascript
try {
    // 有些代码引发了错误
} catch (e) {
    if (e instanceof TypeError) {
        //hadle TypeError
    } else if (e instanceof ReferenceError) {
        //handle ReferenceError
    } else {
        //other
    }
}
```



### 11.3 基于对象的继承

``` javascript
var person = {
    name: "Jane",
    sayName: function () {
        console.log( this.name);
    }
};

var myPerson = Object.create(person);
myPerson.sayName(); // Jane
myPerson.sayName = function () {
    console.log("Anonymous");
};
myPerson.sayName(); // Anonymous
person.sayName(); // Jane

/* Object.create()指定第二个参数,该参数对象中的属性和方法将添加到新创建的实例中。 */
var otherPerson = Object.create(person, {
    name: {
        value: "Greg"
    }
});

otherPerson.sayName(); // Greg
person.sayName(); //Jane
```

 一旦以这种方式创建了一个新对象，该新对象完全可以随意修改.(新增、覆盖、删除（或者阻止它们的访问）)

#### 11.3.2 基于类型的继承

```javascript
function CustomError(message) {
    this.message = message;
}

CustomError.prototype = new Error(); // CustomError继承自Error

var error = new CustomError( "Something bad happend");
console.log(error instanceof Error); // true
console.log(error instanceof CustomError); // true
console.log(CustomError instanceof Error); // false

function Person(name) {
    this.name = name;
}

function Author(name) {
    Person.call(this, name); // 继承构造器
}

Author.prototype = new Person();
```

对比基于对象的继承，基于类型的继承在创建新对象时更加灵活。
定义了一个【烈性】可以让你创建多个实例对象，所有的对象都是继承自一个通用的超类。

新的类型应该明确定义需要使用的属性和方法，它们与超类中的应该完全不同。

#### 11.3.3 门面模式

```javascript
function DOMWrapper(element) {
    this.element = element;
}

DOMWrapper.prototype.addClass = function (className) {
    element.className += " " + className;
}

DOMWrapper.prototype.remove = function () {
    this.element.parentNode.removeChild( this.element);
}

// 用法
var wrapper = new DOMWrapper(document.getElementById( "my-div"));
// 添加一个className
wrapper.addClass("selected");
// 删除元素
wrapper.remove();
```

门面实现一个特定接口，让一个对象看起来像另一个对象，就称作一个适配器。
门面和适配器唯一的不同时前者在创建接口，而后者实现已存在的接口。

### 11.5 阻止修改

三种锁定级别（IE9、FF4+,Safari5+,Chrome，Opera12+）：

-  防止扩展（禁止对象“添加”属性和方法，但已存在的属性和方法是可以修改和删除的）
-  密封（类似“防止扩展”,而且禁止为对象“删除”已存在的方法和属性）
-  冻结（类似密，而且禁止为对象“修改”已存在的属性和方法）

```javascript
// Object.preventExtensions()
// Object.isExtensible()

var person = {
    name:"Jane"
};

// 防止扩展
Object.preventExtensions(person);
console.log(Object.isExtensible(person)); // false
person.age = 25; // 正常情况悄悄地失败，除非在strict模式下抛出错误

// 密封
Object.seal(person);
console.log(Object.isExtensible(person)); //false
console.log(Object.isSealed(person)); // true
delete person.name; // 正常情况瞧瞧地失败，除非在strict模式下抛出错误
person.age = 25; //同上

// 冻结
Object.freeze(person);
console.log(Object.isExtensible(person)); // false
console.log(Object.isSealed(person)); // true
console.log(Object.isFrozen(person)); // true
person.name = "Jane"; // 正常情况瞧瞧地失败，除非在strict模式下抛出错误
person.age = 25; // 同上
delete person.name; // 同上
```



## 12 浏览器嗅探

### 12.1 User-Agent 检测

```javascript
// 不好的做法
if (navigator.userAgent.indexOf( "MSIE") > -1) {
    //IE
} else {
    // not IE
}
```

### 12.2 特性检测

```javascript
// bad
if (navigator.userAgent.indexOf( "MSIE 7">-1)) {
    //do some thing
}

// good
if (document.getElementById) {
    //do some thing
}

// 合理的requestAnimationFrame()特性检测如下
function setAnimation(callback) {
    if (window.requestAnimationFrame) { //  标准
        return requestAnimationFrame(callback);
    } else if (window.mozRequestAnimationFrame) { //FireFox
        return mozRequestAnimationFrame(callback);
    } else if (webkitRequestAnimationFrame) { // webkit
        return webkitRequetsAnimationFrame(callback);
    } else if (window.oRequestAnimationFrame) { // Opera
        return oRequestAnimationFrame(callback);
    } else if (window.msRequestAnimationFrame) { // IE
        return msRequestAnimationFrame(callback);
    } else {
        return setTimeout(callback, 0);
    }
}
```

### 12.3 避免特性推断

```javascript
// 不好的写法 使用特性推断

function getById(id) {
    var element = null;
  
    if (document.getElementsByTagName) { // DOM
        element = document.getElementById(id);
    } else if (window.ActiveXObject) {   // IE
        element = document.all(id);
    } else {// Netscape <=4
        element = document.layers[id];
    }
   
    return element;
}
```

该函数是最糟糕的特性判断，其中做出了如下几个推断：

1. 如果document.getElementsByTagName()存在，则document.getElementById()也存在。
   实际上，这个架设是从一个DOM方法的存在推断出所有方法都存在。

2. 如果window.ActiveXObject存在，则document.all也存在。这个推断基本上断定window.ActiveXObject仅仅存在
   于IE,且document.all也仅存在于IE，所以如果判断一个存在，其他的也必定存在。
   实际上,Opera的一些版本也支持document.all()。

3. 如果这些推断都不成立，则一定是Netscape Navigator 4或者更早的版本。这砍死正确，但极其不严格

### 12.4 避免浏览器推断

```javascript
// 不好的写法
if (document.all) { // IE
    id = document.uniqueID;
} else {
    id = Math.random;
}

// bad
var isIE = !!document.all;

// bad
var isIE = !!document.all && document.uniqueID;
```



# 第三部分

利用自动化构建系统构建你的JavaScript的优点：

1. 你本地的源代码不必同生产环境保持一致，所以你可以任意组织你
   的代码结构而不必担心在服务器上使用的代码是否需要优化。

2. 静态分析可以自动发现错误

3. 在部署之前有多种方式处理JavaScript，比如文件的连接和压缩。

4. 通过自动化测试可以很容易地发现问题。

5. 很方便的自动部署到生产环境。

6. 你可以轻松快速地重新执行常见任务。

使用这样的自动化系统也会带来一些弊端

1. 开发者在开发环境每次改动后可能都需要在本地重新构建。一些习惯了改完
   代码就去刷新浏览器的开发人员会很难适应这一步。

2. 被部署到生产的代码看起来并不像我们在开发环境编辑的代码，就是得我们很
   难定位线上的Bug。

3. 技术水平偏低的开发人员使用构建系统可能会遇到问题。

13 文件和目录结构

13.1 最佳实践

```javascript
// 一个文件只包含一个对象

//相关的文件使用目录分组

//保持第三方代码的独立

//确定创建位置

//保持测试代码的完整性
```

13.2 基本结构

```javascript
//JavaScript

//--build （用来防止最终构建后的文件，立项情况下这个目录不应该提交）

//--src （用来放置所有的源文件，包括用来进行文件分组的子目录）

//--test或tests （用来防止所有的测试文件。通常包含一些同源代码目录一一对应的子目录或文件）
```