# JavaScript 编程指南

------

## 2 JavaScript 可维护约定

### 2.1 变量

**变量的声明必须使用关键字(var,let,cosnt,并正确使用关键字)，避免隐式声明导致的全局变量**

### 2.1.1 变量先声明，后使用

说明：“先声明，后使用”，即在代码或者函数执行前必须有声明。

```javascript
var a = 1, // 直接量始化在前
    b = a, // 需要引用其它变量的变量在后
    c;    // 

function max() {
    var max = a;
    max = max > b ? max : b; // max=> 1
    c = c || 6;
    d = d || 5;
    max = max > c ? max : c; // max=> 6
    max = max > d ? max : d; // d=5,而不是期望的9. max=> 6
    console.log('max:' + max); // 6
}

max();

var d = 9;
```

### 2.1.2 全局变量的创建和使用

```javascript
// 不推荐
myglobal = "hello"; // 隐式的全局变量
console.log(myglobal); // "hello"
console.log(window.myglobal); // "hello"
console.log(window["myglobal"]); // "hello"
console.log(this.myglobal); // "hello"

// 推荐，声明了一个全局变量
var MyGlobal = "hello"; 
```

**一些导致隐式全局变量的写法**

```javascript
// 不推荐
function sum(x, y) {
    // 反模式：隐式全局变量
    result = x + y;
    return result;
}

// 推荐
function sum(x, y) {
    var result = x + y;
    return result;
}

// 反模式，下面的代码中a是局部变量，b是全局变量
function foo() {
    var a = b = 0;
    // ...
}

// 推荐
function foo() {
    var a, b;
    // ...
    a = b = 0; // 两个都是本地变量
}
```

**忘记var时的副作用**

隐式创建的全局变量和显式定义的全局变量之间有着细微的差别，就是通过`delete`来删除它们的时候表现不一致。

- 通过`var`创建的全局变量（在任何函数体之外创建的变量）不能被删除。
- 没有用`var`创建的隐式全局变量（不考虑函数内的情况）可以被删除。

也就是说，隐式全局变量并不算是真正的变量，但它们却是全局对象的属性。属性是可以通过`delete`运算符删除的，而变量不可以被删除：

```javascript
    // 定义三个全局变量
    var global_var = 1;
    global_novar = 2; // 反模式
    (function () {
        global_fromfunc = 3; // 反模式
    }());

    // 尝试删除
    delete global_var; // false
    delete global_novar; // true
    delete global_fromfunc; // true

    // 测试删除结果
    typeof global_var; // "number"
    typeof global_novar; // "undefined"
    typeof global_fromfunc; // "undefined"
```

在ES5严格模式中，给未声明的变量赋值会报错（比如这段代码中提到的两个反模式）。

**不使用this来访问全局对象**

对于很多人而言，this往往超出其驾驭能力。

```javascript
var global = (function () {
    return this;
}()),
    MyApp = {
    name: 'JavaScript Guide',
    version: '0.0.1'
};

function print() {
    console.log(this.MyApp.name); // 不推荐
    console.log(global.MyApp.name); // 推荐
}
```

### 2.1.3 使用单var模式

```javascript
/* 推荐 */
function func() {
    var a = 1,
        b = 2,
        sum = a + b,
        myobject = {},
        i,
        j;
    // 函数体…
}

/* ES6 */
function funcEs6(){
      let a = 1,
        b = 2,
        sum = a + b,
        myobject = {},
        i,
        j;
    // 函数体…
}

// 对于ES6,下列情况尽量使用let来声明变量
function func(){
  // 函数内
}

// 条件块内
if (true) {
  let x = 'hello';
}

// for 循环
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```



#### for 循环

```javascript
// 非最优的循环方式,当myArray引用的是DOM集合时，每次计算耗时的
for (var i = 0; i < myArray.length; i++) {
    // 访问myArray[i]…
}

// 至少这样写
for (var i = 0, max = myArray.length; i < max; i++) {
    // 访问myArray[i]…
}

// 优秀是一种习惯，结合单var模式，代码可以这样写
function looper() {
    // ES6可以用 let 替换 var
    var i = 0,
        max,
        myArray = [];
    // …
    for (i = 0, max = myArray.length; i < max; i++) {
        // 访问myArray[i]…
    }
}
```

**变量最少，速度最快**
关于这种for模式还有两种变化的形式，做了少量改进，原因有二：

减少一个变量（没有max）
减量循环至0，这种方式速度更快，因为和零比较要比和非零数字或数组长度比较要高效的多
第一种变化形式是：

```javascript
var i,
    myArray = [];
for (i = myArray.length; i--;) {
    // 访问myArray[i]…
}
```

第二种变化形式用到了while循环：

```javascript
var myArray = [],
    i = myArray.length;
while (i--) {
    // 访问myArray[i]…
}
```

**注意：以上这两种情况都是逆序的，如果对顺序有严格的需求的话，可以先将数组reverse()再遍历**

```javascript
var myArray = [1,2,3],
    i = myArray.length;

myArray.reverse();
while (i--) {
    // 访问myArray[i]…
}
```

#### for-in 循环

```javascript
// 对象
var man = {
    hands: 2,
    legs: 2,
    heads: 1
};

// 在代码的另一个地方给所有的对象添加了一个方法
if (typeof Object.prototype.clone === "undefined") {
    Object.prototype.clone = function () {};
}
```

使用**hasOwnProperty()**方法过滤来自原型链中继承来的属性
严格说来，省略hasOwnProperty()并不是一个错误。根据具体的任务以及你对代码的自信程度，你可以省略掉它以提高一些程序执行效率。但当你对当前要遍历的对象不确定的时候，添加hasOwnProperty()则更加保险些。

```javascript
var i;
for (i in man) {
    if (Object.prototype.hasOwnProperty.call(man, i)) { // 过滤
        console.log(i, ":", man[i]);
    }
}
// 或是
var i,
    hasOwn = Object.prototype.hasOwnProperty;
for (i in man) {
    if (hasOwn.call(man, i)) { // 过滤
        console.log(i, ":", man[i]);
    }
}
```

#### 避免扩展内置类型

我们可以扩充构造函数的`prototype`属性来为构造函数增加功能，这个特性非常强大，但有时会强大到超过我们的掌控。

给内置构造函数如`Object()`、`Array()`、`Function()`扩充原型看起来非常诱人，但这种做法会严重降低代码的可维护性，因为它会让你的代码变得难以预测。对于那些基于你的代码来做开发的开发者来说，他们更希望使用原生的JavaScript方法来保持代码的一致性，而不愿意使用你所添加的方法。

另外，如果将属性添加至原型中，很可能导致原型上的属性在那些不使用`hasOwnProperty()`做过滤的循环中被遍历出来，从而造成混乱。

#### switch模式

你可以通过下面这种模式来增强`switch`语句的可读性和健壮性：

```javascript
    var inspect_me = 0,
        result = '';
    switch (inspect_me) {
    case 0:
        result = "zero";
        break;
    case 1:
        result = "one";
        break;
    default:
        result = "unknown";
    }
```

这个简单的例子所遵循的风格约定如下：

- 每个`case`和`switch`对齐（这里不考虑花括号相关的缩进规则）。
- 每个`case`中的代码整齐缩进。
- 每个`case`都以`break`作为结束。
- 避免连续执行多个case语句块（省略break时），如果你坚持认为连续执行多个`case`语句块是最好的方法，请务必补充文档说明，对于其他人来说，会觉得这种情况是错误的写法。
- 以`default`结束整个`switch`，以确保即便是在找不到匹配项时也有合理的结果。

#### 避免隐式类型转换

在JavaScript对变量进行比较时会有一些隐式的数据类型转换。比如诸如`false == 0`或`"" == 0`之类的比较都返回`true`。

为了避免隐式类型转换对程序造成干扰，推荐使用`===`和`!==`运算符，它们除了比较值还会比较类型：

```javascript
    var zero = 0;
    if (zero === false) {
        // 不会执行，因为zero是0，不是false
    }
    // 反模式
    if (zero == false) {
        // 代码块会执行…
    }
```

有一种观点认为当`==`够用的时候就不必使用`===`。比如，当你知道`typeof`的返回值是一个字符串，就不必使用全等运算符。但JSLint却要求使用全等运算符，这无疑会提高代码风格的一致性，并减少了阅读代码时的思考量（“这里使用`==`是故意的还是无意的？”）。

#### 避免使用eval()

当你想使用`eval()`的时候，不要忘了那句话“`eval()` is evil”（`eval()`是魔鬼）。这个函数的参数是一个字符串，它会将传入的字符串作为JavaScript代码执行。如果用来解决问题的代码是事先知道的（在运行之前），则没有理由使用`eval()`。如果需要在运行时动态生成并执行代码，那一般都会有更好的方式达到同样的目的，而非一定要使用`eval()`。例如，访问动态属性时可以使用方括号：

```javascript
    // 反模式
    var property = "name";
    alert(eval("obj." + property));
    // 更好的方式
    var property = "name";
    alert(obj[property]);
```

`eval()`还有安全隐患，因为你有可能会运行一些被干扰过的代码（比如一段来自于网络的代码）。这是一种在处理Ajax请求所返回的JSON数据时比较常见的反模式。这种情况下最好使用浏览器的内置方法来解析JSON数据，以确保代码的安全性和数据的合法性。如果浏览器不支持`JSON.parse()`，你可以使用JSON.org所提供的库。

值得一提的是，多数情况下，给`setInterval()`、`setTimeout()`和`Function()`构造函数传入字符串的情形和`eval()`类似，这种用法也是应当避免的，因为这些情形中JavaScript最终还是会执行传入的字符串参数：

```javascript
    // 反模式
    setTimeout("myFunc()", 1000);
    setTimeout("myFunc(1, 2, 3)", 1000);
    // 更好的方式
    setTimeout(myFunc, 1000);
    setTimeout(function () {
        myFunc(1, 2, 3);
    }, 1000);
```

`new Function()`的用法和`eval()`非常类似，应当特别注意。这种构造函数的方式很强大，但经常会被误用。如果你不得不使用`eval()`，你可以尝试用`new Function()`来代替。这有一个潜在的好处，在`new Function()`中运行的代码会在一个局部函数作用域内执行，因此源码中所有用`var`定义的变量不会自动变成全局变量。还有一种方法可以避免`eval()`中定义的变量被转换为全局变量，即是将`eval()`包装在一个即时函数内（详细内容请参见第四章）。

看一下这个例子，这里只有`un`成为全局变量污染了全局命名空间：

```javascript
    console.log(typeof un);// "undefined"
    console.log(typeof deux); // "undefined"
    console.log(typeof trois); // "undefined"

    var jsstring = "var un = 1; console.log(un);";
    eval(jsstring); // 打印出 "1"

    jsstring = "var deux = 2; console.log(deux);";
    new Function(jsstring)(); // 打印出 "2"

    jsstring = "var trois = 3; console.log(trois);";
    (function () {
        eval(jsstring);
    }()); // 打印出 "3"

    console.log(typeof un); // "number"
    console.log(typeof deux); // "undefined"
    console.log(typeof trois); // "undefined"
```

`eval()`和`Function()`构造函数还有一个区别，就是`eval()`可以修改作用域链，而`Function`更像是一个沙箱。不管在什么地方执行`Function()`，它都只能看到全局作用域。因此它不会太严重的污染局部变量。在下面的示例代码中，`eval()`可以访问并修改其作用域之外的变量，而`Function()`则不能（注意，使用`Function()`和`new Function()`是完全一样的）。

```javascript
    (function () {
        var local = 1;
        eval("local = 3; console.log(local)"); // 打印出 3
        console.log(local); // 打印出 3
    }());

    (function () {
        var local = 1;
        Function("console.log(typeof local);")(); // 打印出 undefined
    }());
```

#### 使用parseInt()进行数字转换

你可以使用`parseInt()`将字符串转换为数字。函数的第二个参数是进制参数，这个参数应该被指定，但却通常被省略。当字符串以0为前缀时转换就会出问题，例如，在表单中输入日期的一个字段。ECMAScript3中以0为前缀的字符串会被当作八进制数处理，这一点在ES5中已经有了改变。为了避免转换类型不一致而导致的意外结果，应当总是指定第二个参数：

```javascript
    var month = "06",
        year = "09";
    month = parseInt(month, 10);
    year = parseInt(year, 10);
```

在这个例子中，如果省略掉parseInt的第二个参数，比如`parseInt(year)`，返回的值是0，因为“09”被认为是八进制数（等价于`parseInt(year,8)`），但09是非法的八进制数。

字符串转换为数字还有两种方法：

```javascript
    +"08" // 结果为8
    Number("08") // 结果为8
```

这两种方法要比`parseInt()`更快一些，因为顾名思义`parseInt()`是一种“解析”而不是简单的“转换”。但当你期望将“08 hello”这类字符串转换为数字，则必须使用`parseInt()`，其他方法都会返回NaN。

#### 代码规范

确立并遵守代码规范非常重要，这会让你的代码风格一致、可预测，并且可读性更强。团队新成员通过学习代码规范可以很快进入开发状态，并写出让团队其他成员易于理解的代码。

在开源社区和邮件组中关于编代风格的争论一直不断。（比如关于代码缩进，用tab还是空格？）因此，如果你打算在团队内推行某种编码规范时，要做好应对各种反对意见的心理准备，而且要吸取各种意见。确定并遵守代码规范非常重要，任何一种规范都可以，这甚至比代码规范中的具体约定是怎么样的还要重要。

#### 缩进

代码如果没有缩进就几乎不能读了，而不一致的缩进会使情况更加糟糕，因为它看上去像是遵守了规范，但真正读起来却没那么顺利。因此规范地使用缩进非常重要。

有些开发者喜欢使用tab缩进，因为每个人都可以根据自己的喜好来调整tab缩进的空格数，有些人则喜欢使用空格缩进，通常是四个空格。这都无所谓，只要团队每个人都遵守同一个规范即可，本书中所有的示例代码都采用四个空格的缩进写法，这也是JSLint所推荐的。（译注：电子版中看到的是用tab缩进，本译文也保留使用tab缩进。）

那么到底什么时候应该缩进呢？规则很简单，花括号里的内容应当缩进，包括函数体、循环（`do`、`while`、`for`和`for-in`）体、`if`语句、`switch`语句和对象字面量里的属性。下面的代码展示了如何正确地使用缩进：

```javascript
    function outer(a, b) {
        var c = 1,
            d = 2,
            inner;
        if (a > b) {
            inner = function () {
                return {
                    r: c - d
                };
            };
        } else {
            inner = function () {
                return {
                    r: c + d
                };
            };
        }
        return inner;
    }
```

#### 花括号

在特定的语句中应当总是使用花括号，即便是在可省略花括号的情况下也应当如此。从技术角度讲，如果`if`或`for`中只有一个语句，花括号是可以省略的，但最好还是不要省略，这会让你的代码更加工整一致而且易于修改。

假设有这样一段代码，`for`循环中只有一条语句，你可以省略掉这里的花括号，而且不会有语法错误：

```javascript
    // 不好的方式
    for (var i = 0; i < 10; i += 1)
        alert(i);
```

但如果过了一段时间，你给这个循环添加了另一行代码会怎样？

```javascript
    // 不好的方式
    for (var i = 0; i < 10; i += 1)
        alert(i);
        alert(i + " is " + (i % 2 ? "odd" : "even"));
```

第二个`alert`实际上在循环体之外，但这里的缩进会让你迷惑。从长远考虑最好还是写上花括号，即便是在只有一个语句的语句块中也应如此：

```javascript
    // 更好的方式
    for (var i = 0; i < 10; i += 1) {
        alert(i);
    }
```

同理，if条件句也应当如此：

```javascript
    // 不好的方式
    if (true)
        alert(1);
    else
        alert(2);

    // 更好的是
    if (true) {
        alert(1);
    } else {
        alert(2);
    }
```

#### 左花括号的位置

开发人员对于左大括号的位置有着不同的偏好，在同一行呢还是在下一行？

```javascript
    if (true) {
        alert("It's TRUE!");
    }
```

或者：

```javascript
    if (true)
    {
        alert("It's TRUE!");
    }
```

在这个例子中，这个问题只是个人偏好问题。但有时候花括号位置的不同会影响程序的执行，因为JavaScript会“自动插入分号”。JavaScript对行尾是否有分号并没有要求，它会自动将分号补全。因此，当函数的`return`语句返回了一个对象字面量，而对象的左花括号和`return`又不在同一行时，程序的执行就和预期的不同了：

```javascript
    // 警告：返回值和预期的不同
    function func() {
        return
        {
            name: "Batman"
        };
    }
```

可以看出程序作者的意图是返回一个包含了`name`属性的对象，但实际情况不是这样。因为return后会填补一个分号，函数的返回值就是undefined。这段代码等价于：

```javascript
    // 警告：返回值和预期的不同
    function func() {
        return undefined;
        // 下面的代码不会运行…
        {
            name: "Batman"
        };
    }
```

总结一下好的写法，在特写的语句中总是使用花括号，并且总是将左花括号与上一条语句放在同一行：

```javascript
    function func() {
        return {
            name: "Batman"
        };
    }
```

> 关于分号也值得注意：和花括号一样，应当总是使用分号，尽管在JavaScript解析代码时会补全行末省略的分号，但严格遵守这条规则，可以让代码更加严谨，同时可以避免前面例子中所出现的歧义。

\####　空格

空格的使用同样有助于改善代码的可读性和一致性。

适合使用空格的地方包括：

- for循环中的分号之后，比如`for (var i = 0; i < 10; i += 1) {...}`
- for循环中初始化多个变量，比如`for (var i = 0, max = 10; i < max; i += 1) {...}`
- 用于分隔数组元素的逗号之后，比如`var a = [1, 2, 3];`
- 对象属性后的逗号以及名值对之间的冒号之后，比如`var o = {a: 1, b: 2};`
- 函数参数中，比如`myFunc(a, b, c)`
- 函数声明的花括号之前，比如`function myFunc() {}`
- 匿名函数表达式`function`之后，比如`var myFunc = function () {};`

另外，我们推荐在运算符和操作数之间也添加空格。也就是说在`+`、`-`、`*`、`=`、`<`、`>`、`<=`、`>=`、`===`、`!==`、`&&`、`||`、`+=`符号前后都添加空格。

```javascript
    // 适当且一致的空格给代码留了“呼吸空间”，使代码更易读
    var d = 0,
        a = b + 1;
    if (a && b && c) {
        d = a % c;
        a += d;
    }

    // 反模式，缺少或者不正确的空格使得代码不易读
    var d= 0,
        a =b+1;
    if (a&& b&&c) {
        d=a %c;
        a+= d;
    }
```

最后，还应当注意，最好在花括号旁边添加空格：

- 在函数、`if-else`语句、循环、对象字面量的左花括号之前补充空格
- 在右花括号和`else`或者`while`之间补充空格

> 垂直空白的使用经常被我们忽略，你可以使用空行来将代码单元分隔开，就像文学作品中使用段落进行分隔一样。

#### 命名规范

下面是一些建议的命名规范，你可以原样采用，也可以根据自己的喜好作调整。同样，遵循规范要比规范本身是什么样更加重要。

#### 构造函数命名中的大小写

JavaScript中没有类，但有构造函数，构造函数使用Pascal命名，如Person(),：

```javascript
var adam = new Person(); // 正确的写法 
var adan = new person(); // 错误的写法
```

#### 单词分隔

当你的变量名或函数名中含有多个单词时，单词之间的分隔也应当遵循统一的规范。最常见的是“驼峰式”（camel case）命名，单词都是小写，每个单词的首字母是大写。

对于构造函数，可以使用Pascal命名，比如`MyConstructor()`，对于函数和方法，可以采用“小驼峰式”（lower camel case）命名，比如`myFunction()`、`calculateArea()`和`getFirstName()`。

#### 其他命名风格

有时开发人员使用命名规范来弥补或代替语言特性的不足。

比如，JavaScript中无法定义常量（尽管有一些内置常量比如`Number.MAX_VALUE`），所以开发者都采用了一种命名规范，对于那些程序运行周期内不会更改的变量使用全大写字母来命名。比如：

```javascript
    // 常量，请勿修改
    var PI = 3.14,
        MAX_WIDTH = 800;
```

除了使用大写字母的命名方式之外，还有另一种命名规范：全局变量全大写。这种命名方式和“减少全局变量”的约定相辅相成，并让全局变量很容易辨认。

除了常量和全局变量的命名规范，这里讨论另外一种命名规范，即私有变量的命名。尽管在JavaScript是可以实现真正的私有变量的，但开发人员更喜欢在私有成员或方法名之前加上下划线前缀，比如下面的例子：

```javascript
    var person = {
        getName: function () {
            return this._getFirst() + ' ' + this._getLast();
        },
        _getFirst: function () {
            // ...
        },
        _getLast: function () {
            // ...
        }
    };
```

在这个例子中，`getName()`是一个公有方法，是确定的API的一部分，而`_getFirst()`和`_getLast()`则是私有方法。尽管这两个方法本质上和公有方法没有区别，但在方法名前加下划线前缀就是为了告知用户不要直接使用这两个私有方法，因为不能保证它们在下一个版本中还能正常工作。JSLint会对私有方法作检查，除非设置了JSLint的`nomen`选项为`false`。

下面介绍一些`_private`风格写法的变种：

- 在名字尾部添加下划线以表明私有，比如`name_`和`getElements_()`
- 使用一个下划线前缀表明受保护的属性`_protected`，用两个下划线前缀表明私有属性`__private`
- 在Firefox中实现了一些非标准的内置属性，这些属性在开头和结束都有两个下划线，比如`__proto__`和`__parent__`

#### 写注释

在写代码时，即便你认为你的代码不会被别人读到，也应该写好注释。因为当你对一个问题非常熟悉时，你会非常明白这些代码的作用，但当过了几个星期后再来读这段代码时，则需要绞尽脑汁的回想这些代码在干什么。

你不必对那些浅显易懂的代码写过多的注释，比如每个变量、每一行都写注释。但你应该对所有的函数、它们的参数和返回值进行注释，除此之外，对于那些值得注意的或是比较怪异的算法和技术也应当写好注释。对于其他阅读你代码的人来说，注释就是一种提示，只要阅读注释、函数名和参数，就算不读其它部分的代码也能大概理解程序的逻辑。比如，这里有五六行代码完成了某个功能，如果有一行描述这段代码功能的注释，读程序的人就不必再去关注代码的实现细节了。代码注释的写法并没有硬性规定，但有些代码片段（比如正则表达式）需要比代码本身更多的注释。

> 过时的注释会造成误导，这比不写注释还要糟糕。保持注释的状态为最新的习惯非常重要，尽管对很多人来说这很难做到。

在下一小节我们会讲到，利用注释可以自动生成文档。

#### 写API文档

很多人都觉得写文档是一件很枯燥而且吃力不讨好的事情，但实际情况并不是这样。我们可以通过代码注释自动生成文档，这样就不用再去专门写文档了。很多人觉得这是一个不错的点子，因为根据某些关键字和特定的格式自动生成可阅读的参考手册本身就是“某种编程”。

最早利用注释生成API文档的工具诞生自Java业界，这个工具名叫“javadoc”，和Java SDK（软件开发工具包）一起提供，但这个创意迅速被其他语言借鉴。JavaScript领域有两个非常优秀的开源工具，它们是JSDoc Toolkit（[http://code.google.com/p/jsdoc-toolkit/](http://code.google.com/p/jsdoc-toolkit/)）和YUIDoc（[http://yuilibrary.com/projects/yuidoc](http://yuilibrary.com/projects/yuidoc)）。

生成API文档的过程：

- 以特定的格式来写代码
- 运行工具来对代码和注释进行解析
- 发布工具运行的结果，通常是HTML页面

这种语法包括十几种标签（tag），写法类似于：

```javascript
/**
 * @tag value
 */
```

比如这里有一个函数`reverse()`，可以对字符串进行反序操作。它的参数和返回值都是字符串。给它补充注释如下：

```javascript
/**
* Reverse a string
*
* @param {String} input String to reverse
* @return {String} The reversed string
*/
var reverse = function (input) {
    // ...
    return output;
};
```

如你所见，`@param`是用来说明输入参数的标签，`@return`是用来说明返回值的标签，文档生成工具最终会将这种带注释的源代码解析成HTML文档。

#### 示例：YUIDoc

YUIDoc的初衷是为YUI（Yahoo! User Interface）库生成文档，但其实它也可以应用于任何项目。为了更充分的使用YUIDoc，你需要学习它的注释规范，比如模块和类的写法。（尽管在JavaScript中其实是没有类的概念的）。

让我们看一个用YUIDoc生成文档的完整例子。

图2-1展示了最终生成的文档的样子，你可以根据项目需要定制HTML模板，让生成的文档更加友好和个性化。

这里提供了在线的demo，请参照[http://jspatterns.com/book/2/](http://jspatterns.com/book/2/)。

这个例子中所有的应用作为一个模块（myapp）放在一个文件里（app.js），后续的章节会更详细的介绍模块，现在只需知道可以用一个YUIDoc的标签来表示模块即可。

图2-1 YUIDoc生成的文档

[![YUIDoc生成的文档](https://github.com/codingEcho/frontend/issues/Figure/chapter2/2-1.jpg)](https://github.com/codingEcho/frontend/issues/Figure/chapter2/2-1.jpg)

`app.js`的开始部分：

```javascript
    /**
     * My JavaScript application
     *
     * @module myapp
     */
```

然后定义了一个空对象作为模块的命名空间：

```javascript
    var MYAPP = {};
```

紧接着定义了一个包含两个方法的对象`math_stuff`，这两个方法分别是`sum()`和`multi()`：

```javascript
    /**
    * A math utility
    * @namespace MYAPP
    * @class math_stuff
    */
    MYAPP.math_stuff = {
        /**
        * Sums two numbers
        *
        * @method sum
        * @param {Number} a First number
        * @param {Number} b The second number
        * @return {Number} The sum of the two inputs
        */
        sum: function (a, b) {
            return a + b;
        },

        /**
        * Multiplies two numbers
        *
        * @method multi
        * @param {Number} a First number
        * @param {Number} b The second number
        * @return {Number} The two inputs multiplied
        */
        multi: function (a, b) {
            return a * b;
        }
    };
```

这样就完成了第一个“类”的定义，注意以下标签：

- `@namespace`

  包含对象的全局引用

- `@class`

  代表一个对象或构造函数（JavaScript中没有类）

- `@method`

  定义对象的方法，并指定方法的名称

- `@param`

  列出函数需要的参数，参数的类型放在一对花括号内，后面跟参数名和描述

- `@return`

  和[@param](https://github.com/param)类似，用以描述方法的返回值，可以不带名字

我们来实现第二个“类”，使用一个构造函数，并给这个构造函数的原型添加一个方法，看看YUIDoc在面对不同的对象创建方式时是如何工作的：

```javascript
    /**
    * Constructs Person objects
    * @class Person
    * @constructor
    * @namespace MYAPP
    * @param {String} first First name
    * @param {String} last Last name
    */
    MYAPP.Person = function (first, last) {
        /**
        * Name of the person
        * @property first_name
        * @type String
        */
        this.first_name = first;
        /**
        * Last (family) name of the person
        * @property last_name
        * @type String
        */
        this.last_name = last;
    };
    /**
    * Returns the name of the person object
    *
    * @method getName
    * @return {String} The name of the person
    */
    MYAPP.Person.prototype.getName = function () {
        return this.first_name + ' ' + this.last_name;
    };
```

在图2-1中可以看到生成的文档中`Person`构造函数的生成结果，值得注意的部分是：

- `@constructor` 说明这个“类”其实是一个构造函数
- `@prototype` 和 `@type` 用来描述对象的属性

YUIDoc工具是与语言无关的，只解析注释块，而不是JavaScript代码。它的缺点是必须要在注释中指定属性、参数和方法的名字，比如，`@property first_name`。好处是一旦你熟练掌握YUIDoc，就可以用它对任何语言源码生成文档。

#### 编写易读的代码

这种编写注释块来生成API文档的做法可不仅仅是为了偷懒，它还有另外一个作用，就是通过回头重看代码来提高代码质量。

随便一个作者或者编辑都会告诉你“编辑非常重要”，甚至是写一本好书或好文章最最重要的步骤。将想法落实在纸上形成草稿只是第一步，草稿确实可以给读者提供不少信息，但往往还不是重点最明晰、结构最合理、最符合阅读习惯的呈现形式。

编程也是同样的道理，当你坐下来解决一个问题的时候，这时的解决方案只是一种“草案”，尽管能正常工作，但是不是最优的方法呢？是不是可读性好、易于理解、可维护和更新？假设当你过一段时间后再来回头看你的代码，一定会发现很多需要改进的地方，比如需要重新组织代码或删掉多余的内容等等。这实际上就是在“整理”你的代码了，可以很大程度上提高你的代码质量。但事实却不那么如愿，我们常常承受着高强度的工作，根本没有时间来整理代码，因此通过代码注释来写文档其实是个不错的机会。

你往往会在写注释文档的时候发现很多问题，也会重新思考代码中的不合理之处，比如，某个方法中的第三个参数比第二个参数更常用，第二个参数多数情况下取值为`true`，因此就需要对这个方法进行适当的改造和包装。

写出易读的代码（或API），是指写代码时要有让别人能轻易读懂的意识。带着这个意识，你就需要不断思考采用更好的方法来解决手头的问题。

说回“草稿”的问题，也算是“抱佛脚”的权宜之计，一眼看上去是有点“草”，不过至少是有用的，特别是当你处理的是一个关键项目时（比如人命关天时）。一个合适的思路是，你应当始终扔掉你所给出的第一个解决方案，虽然它是可以正常工作的，但毕竟是一个草稿，是一种仅用于验证解决问题可行性的方案。事实上，第二个方案往往会更好，因为这时你对问题的理解会更加透彻。在产生第二个方案的过程中，不要允许自己去复制粘贴之前的代码，这有助于阻止自己投机取巧利用之前的捷径，最后产生不完美的方案。

#### 同事评审（Peer Reviews）

另外一种可以提高代码质量的方法是组织相互评审。同事评审可以用一些工具辅助，可以很正式很规范，也是一种开发流程中值得提倡的步骤。你可能觉得没有时间去作代码互相评审，没关系，你可以让坐在你旁边的同事读一下你的代码，或者和她（译注：注意是“她”而不是“他”）一起过一遍你的代码。

同样，当你在写API文档或者其他文档的时候，同事评审能让你的产出物更加清晰，因为你写的文档是本来就是让别人读的，你得让别人通过文档知道你所做的东西。

同事评审是一种很好的实践，不仅仅是因为它能让代码变得更好，更重要的是，在评审的过程中，评审人和代码作者通过分享和讨论，两人都能取长补短、相互促进。

如果你的团队只有你一个开发人员，找不出第二个人能给你作代码评审，这也没关系。你可以通过将你的代码片段开源，或把有意思的代码片段贴在博客中，让全世界的人为你评审。

另外一个很好的实践是使用版本管理工具（CVS、SVN或Git），一旦有人修改并提交了代码，就会发邮件通知组内成员。虽然大部分邮件都进入了垃圾箱，但总是会碰巧有人在工作间隙看到你所提交的代码，并对代码做出一些评价。

#### 发布时的代码压缩（Minify）

这里所说的代码压缩（Minify）是指去除JavaScript代码中的空格、注释以及其他不必要的部分，用以减少JavaScript文件的体积，降低网络带宽消耗。我们通常使用压缩工具来进行压缩，比如YUICompressor（Yahoo!）或Closure Compiler（Google），这可以减少页面加载时间。压缩用于发布的的脚本是很重要的，压缩后的文件体积能减少至原来的一半以下。

下面这段代码是压缩后的样子（这段代码是YUI2库中的事件模块）：

```
YAHOO.util.CustomEvent=function(D,C,B,A){this.type=D;this.scope=C||window;this.silent
=B;this.signature=A||YAHOO.util.CustomEvent.LIST;this.subscribers=[];if(!this.silent)
{}var E="_YUICEOnSubscribe";if(D!==E){this.subscribeEvent=new
YAHOO.util.CustomEvent(E,this,true);}...

```

除了去除空格、空行和注释之外，压缩工具还能缩短命名的长度（在保证代码安全的前提下），比如这段代码中的参数`A`、`B`、`C`、`D`。压缩工具只会重命名局部变量，因为更改全局变量会破坏代码的逻辑，这也是要尽量使用局部变量的原因。如果你使用的全局变量是对DOM节点的引用，而且程序中多次用到，那么最好将它赋值给一个局部变量，这样能提高查找速度，代码也会运行的更快，此外还能提高压缩比、加快下载速度。

补充说明一下，Goolge Closure Compiler还会为了更高的压缩比对全局变量进行压缩（在“高级”模式中），这是很危险的，且对编程规范的要求非常苛刻。

对用于生产环境的脚本做压缩是非常重要的步骤，因为它能提升页面性能，但你应当将这个过程交给工具来完成。千万不要试图手写“压缩好的”代码，你应当在编写代码时坚持使用语义化的变量命名，并保留足够的空格、缩进和注释。你写的代码是需要被人阅读的，所以应当将注意力放在代码可读性和可维护性上，将代码压缩的工作交给工具去完成。

#### 运行JSLint

在上一章我们已经介绍了JSLint，本章中也提到了数次。到现在你应该已经相信用JSLint检查你的代码是一种好的编程模式了。

JSLint的检查点都有哪些呢？它会对本章讨论过的一些模式（单`var`模式、`parseInt()`的第二个参数、总是使用花括号）做检查。JSLint还包括其他方面的检查：

- 不可达代码（译注：指永远不可能运行的代码）
- 变量在声明之前被使用
- 不安全的UTF字符
- 使用`void`、`with`或者`eval`
- 无法正确解析的正则表达式

JSLint是基于JavaScript实现的（它自己的代码是可以通过JSLint检查的），它提供了在线工具，也可以下载使用，可以运行于很多种平台的JavaScript解析器。你可以将源码下载后在本地运行，支持的环境包括WSH（Windows Scripting Host，Windows）、JSC（JavaScriptCore，MacOSX）或Rhino（Mozilla开发的JavaScript引擎）。

将JSLint下载后和你的代码编辑器配置在一起是个很不错的主意，这样每次你保存代码的时候都会自动执行代码检查。（为它配置一个快捷键也很有用）。

#### 小结

- 减少全局对象，最好每个应用只有一个全局对象
- 函数都使用单`var`模式来定义，这样可以将所有的变量放在同一个地方声明，同时可以避免“声明提前”给程序逻辑带来的影响
- `for`循环、`for-in`循环、`switch`语句、“避免使用`eval()`”、不要扩充内置原型
- 遵守统一的编码规范（在任何必要的时候保持空格、缩进、花括号和分号）和命名规范（构造函数、普通函数和变量）。

本章还讨论了其他一些和代码本身无关的实践，这些实践和编码过程紧密相关，包括写注释、写API文档、组织同事评审、不要试图去手动“压缩”（minify）代码而牺牲代码可读性、坚持使用JSLint来对代码进行检查。

*Revision: 2015.10.29*