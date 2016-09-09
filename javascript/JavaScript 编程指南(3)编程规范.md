# JavaScript 编程指南

---

## 3 JavaScript 编程规范

### 3.1 代码风格

#### 3.1.1 缩进层级
可以使用IDE自带的格式化功能，但是有的IDE格式化后的代码也不好看，如Eclipse（一般不会有人用它来写JavaScript）。

Visual Studio Code 会把单行代码后面的分号`;`干掉

#### 3.1.2 语句的结尾
- 一般要求一个语句独占一行
- 语句的结尾必须用分号“`;`”

#### 3.1.3 行的长度
**每行的长度不应过长**

#### 3.1.4 换行
**注意什么时候该换行，什么时候不该换行**
#### 3.1.5 空行
添加适当的空行

- 在方法之间
- 在方法中的局部变量和第一条语句之间
- 在多行或单行注释之前
- 在方法内的逻辑片段之间插入空行，提高可读性
- 在return之前（方法仅有一句return除外）

```javascript
function getArray() {
	return []; // return前无需空行
}

/*
 * 这是一个多行注释，
 * 之前有个空行。
 */

// 这是单行注释，之前有个空行
function getElement() {
	var $target = $('#id'), // 用jQuery id 选择器
		$element; // [空行(下方)：局部变量和第一条语句之间]

	if ($target.length) {
		$element = $target.find('.css-class');
	}
	
	return $element || null; // [return和其它语句之间]
}

```

### 3.2 使用直接量

#### 3.2.1 字符串
**无论是用单引号还是双引号扩住字符串，在代码风格中务必统一代码的风格。**
**个人推荐：**本人习惯使用单引号，原因可能和大多数人一样：HTML中属性一般使用双引号，所以JavaScript中使用单引号，直接拷贝HTML就可以使用了；

一些只能使用单引号的地方：

- **在HTML中如果要给元素属性赋值JSON必须用外单、内双;**
```html
	<input type="hidden" name="datauserInfo" id="userInfo" value='{"name":"mike","age":12}' />

	<script type="text/javascript">
		(function () {
			function printUserInfo() {
				var hiddenUserInfo = document.getElementById('userInfo'),
					userInfo = JSON.parse(hiddenUserInfo.value);

				console.dir(userInfo);
			}

			printUserInfo();

		}());
	</script>
```

**字符串换行**

```javascript
// 不好的写法
var longString = 'Here's the story of man \
named Jane.' ;

// 好的写法
var longString = 'Here's the story of man ' +
'named Jane.';

// ES6增加了反引号(``),尽量使用反引号
// bad
const a = "foobar";
const b = 'foo' + a + 'bar';

// acceptable
const c = `foobar`;

// good
const a = 'foobar';
const b = `foo${a}bar`;
const c = 'foobar';
```

#### 3.2.2 数值

整数请勿添加小数点，如果是小数请勿省略小数点前面和后面部分。

```javascript
var count = 10;
var price = 10.0;
var price = 10.00;
```

#### 3.2.3 `null`

null是个一个特殊值，但我们常常误解它，将它和undefined搞混淆，下列场景中**`应该`**使用null:

- 用来初始化一个变量，这个变量可能赋值为一个对象。
- 用来和一个已经初始化的变量比较，这个变量可以是也可以不是一个对象。
- 当函数的参数期望值是对象时，用作参数传入。
- 当函数的返回值期望是对象时，用作返回值传出。

**`不应该`**使用null的场景：

- 不要使用null来检测是否传入了某个参数。
- 不要使用null来检测一个未初始化的变量。

```javascript
// good
var person = null;

// good
function getPerson() {
	if (condition) {
		return new Person('Jane');
	} else {
		return null;
	}
}

// good
var person = getPerson();
if (person !== null) {
	doSomething();
}

// bad:用来和未初始化的变量比较
var person;
if (person != null) {
	doSomething();
}

// bad:检测是否传入了参数
function doSomething(arg1, arg2, arg3, arg4) {
	if (arg4 != null) {
		doSomethingElse();
	}
}
```

#### 3.2.4 undefined

`undefined` 是一个特殊值，我们常将它和`null`混淆；

让人困惑的地方：

	1. null==undefined; // true
	2. typeof运算无论是值为undfined的变量还是未声明的变量，其都返回'undefined'

```javascript
// bad
var person;
console.log(person === undefined); // true

// foo未被声明
var person;
console.log( typeof person);  // 'undefined'
console.log( typeof foo); // 'undefined'
```

避免使用`undefined`，确保只有在一种情况下`typeof`运算才会返回`'undefined'`;
```javascript
// good
var person = null;
console.log(person === null); //true
```
**注意：**`(typeof null)`返回`'object'`

#### 3.2.5 对象直接量

直接量可以高效完成非直接量写法相同的任务，非直接量写法语法看起来要更为复杂。
```javascript
// bad
var book = new Object();
book.title = 'Maintainable JavaScript';
book.author = 'Zakas';

// good
var book = {
	title: 'Maintainable JavaScript',
	author: 'Zakas'
};
```

#### 3.2.6 数组直接量
**使用数组直接量而不是构造函数**
```javascript
// bad
var colors = new Array('red', 'yellow', 'blue', 'white'),
	numbers = new Array(1, 2, 3, 4);

// good
var colors = ['red', 'yellow', 'blue', 'white'],
	numbers = [1, 2, 3, 4];
```

### 3.3 注释


#### 3.3.1 行注释

- 独占一行的注释，用来解释下一行代码。这行注释之前总有一个空行，且缩进层级和下一行代码保持一致。
- 在代码行的尾部的注释。代码结束到注释之间至少有个一缩紧。注释（包括之前的代码部分）不应到超过单行最大字符数显示，如果超过了，就将这条注释放置于当前代码行的上方。
- 被注释掉的打断代码（很多编辑器都可以批量注释掉多行代码）。

#### 3.3.2 多行注释

```javascript
/*
 * 这是一个多行注释，
 * 之前有个空行。
 */

```
#### 3.3.3 使用注释
添加注释的一般原则：需要让代码变得更清晰时添加注释。

- 难于理解的代码
- 可能被认为错误的代码
- 浏览器特性hack
- 文档注释

### 3.4 JavaScript语句

#### 3.4.1 块语句必须使用花括号
包括：

- if
- for
- while
- do...while
- try...catch...finally

```javascript
// Bad
if (condition)
	doSomething();

// Bad
if (condition) doSomething();

// Bad
if (condition) { doSomething(); }

// Good
if (condition) {
	doSomething();
}
```

#### 3.4.2 括号对齐方式

```javascript
// 首选使用（和Java类似）
if (condition) {
	doSomething();
} else {
	doSomethingElse();
}

// 微软风格，没什么问题，但是为了风格一致性，不推荐使用！
if (condition)
{
	doSomething();
}
else
{
	doSomethingElse();
}
```

#### 3.4.3 switch 语句
- 允许多个case匹配同一个代码块（确保你不是忘记写了`break;`），最好加注释说明；
- 推荐保留`default`，谁也不敢保证代码万无一失。

```javascript
switch (condition) {
	case 'one':
		//...
		break;
	// 当值为'two'或 '二'时,做相同的处理
	case 'two':
	case '二':
		//...
		break;
	default:
		throw '发现非期望的值：' + condition;
}
```

#### 3.4.5 with 语句
**`禁止`使用`with`语句（JavaScript大神可以忽略）**

#### 3.4.6 for 语句

- **`break`：结束循环**
```javascript
var values = [1, 2, 3, 4, 5, 6, 7],
	i,
	len;
for (i = 0, len = values.length; i < len; i++) {
	if (i === 2) {
		break; // 迭代不会继续
	}
	console.log (values[i]);
}
// 以上代码只输出： 1 2 ，不再往下执行
```

- **`continue`：跳过本次循环，往下执行**

```javascript
var values = [1, 2, 3, 4, 5, 6, 7],
	i,
	len;
for (i = 0, len = values.length; i < len; i++) {
	if (i === 2) {
		continue; // 跳过本次循环
	}
	console.log(values[i]);
}
// 不输出索引为2的元素，即：3

// Crockford 的编程规范不允许使用continue，而是主张使用条件语句代替
for (i = 0, len = values.length; i < len; i++) {
	if (i !== 2) {
		console.log(values[i]);
	}
}

```

#### 3.4.7 for-in 语句

```javascript
// 用hasOwnProperty()方法过滤出实例属性
var prop;
for (prop in object) {
	// 若想要查找原型链，则省略if条件语句
	if (object.hasOwnProperty(prop)) {
		console.log('Property name is ' + prop);
		console.log('Property value is ' + object[prop]);
	}
}

// 不要用for-in来遍历数组
var values = [1, 2, 3, 4, 5, 6, 7],
	i;
// bad
for (i in values) {
	console.log(values[i]);
}
```

### 3.5 变量函数和运算符
#### 3.5.1 变量声明

- **变量先声明再使用**
- **JavaScript没有块级变量声明（在ES6之前）**
```javascript
function doSomethingWithItems(items) {
	for (var i = 0, len = itmes.length; i < len; i++) {
		doSomething(items[i]);
	}
}

function doSomethingWithItems(items) {
	var i,
		len;

	for (i = 0, len = items.length; i < len; i++) {
		doSomething(items[i]);
	}
}
```

**最佳实践：**

- 建议总是将局部变量的定义作为函数内第一条语句;
- 并且使用`单var模式`(**为了保持成本最低，推荐合并 `var` 语句**)
```javascript
function doSomethingWithItems(items) {
	var value = 10,
		result = value + 10,
		i,
		len;

	function doSomething(item) {
		// do something
	}

	for (i = 0, len = items.length; i < len; i++) {
		doSomething(items[i]);
	}
}
```
#### 3.5.2 函数声明

```javascript
// bad：调用代码在前，定义代码在后
doSomething();

function doSomething() {
	console.log('Hello world!');
}



// good：先定义，后使用
function doSomething() {
	console.log('Hello world!');
}

doSomething();
```

#### 3.5.3 函数调用的间隔
```javascript
// good
doSomething(itme);

// bad：多一个空格，断片了吧！手贱了吧！
doSomething (item);
```

#### 3.5.4 即时调用的函数

```javascript

// bad
var value = function () {
	// code

	return {
		message: 'Hi'
	};
}();

// good
var value = (function () {
	//code

	return {
		message: 'Hi'
	}
}());
```
#### 3.5.5 严格模式
- 不要使用全局的严格模式
- 也不必每个函数都写严格模式
```javascript
// bad：全局的严格模式
'use strict';
function doSomething() {
	//code
}

// bad
function doSomething() {
	'use strict';
	//code
}

// good
(function () {
	'use strict';

	function doSomething() {
		//code
	}

	function doSomethingElse() {
		//code
	}
})();
```

#### 3.5.6 相等

**掌握：== != 和 === !==**

如果一个布尔值和数字比较，布尔值会首先转换位数字，然后进行比较。
false 值变为0、true变为1.
```javascript
//数字1和true
console.log(1 == true); // true

//数字0和false
console.log(0 == false); // true

//数字2和false
console.log(2 == true); // false
```


如果其中一个值是对象而另一个不是，则会首先调用对象的valueOf()方法，
得到原始类型值再进行比较。如果没有定义valueOf()，则在调用toString()。
之后的比较操作就和上文提到的多类型值比较的情形了一样。

```javascript
// 数字5和字符串5
console.log(5 == '5'); // true
console.log(5 === '5'); // false

// 数字25和16进制字符串25
console.log(25 == '0x19'); // true
console.log(25 === '0x19'); // false

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

### 3.6 避免使用全局变量
```javascript
var color = 'red';

function sayColor() {
	console.log(color); // 不好的做法,color从哪里来的？？
}

console.log(window.color); // 'red'

console.log(typeof window.sayColor); // 'function'
```
**全局变量带来的问题**

- 命名冲突
- 代码脆弱性
- 难以测试

#### 3.6.1 意外的全局变量

在处理老代码时要非常小心的使用严格模式，对于新代码最好使用严格模式来避免意外的全局变量，
同时一些常见的编码错误也能在严格模式中被捕捉到。

#### 3.6.2 单全局变量方式
```javascript
function Book(title) {
	this.title = title;
	this.page = 1;
}

Book.prototype.turnPage = function (direction) {
	this.page += direction;
}

var Chapter1 = new Book( 'Introction to Style GuideLines');
var Chapter2 = new Book( 'Basic Formatting');
var Chapter3 = new Book( 'Comments');

// 使用单全局变量

var MaintainableJS = {};

MaintainableJS.Book = function (title) {
	this.title = title;
	this.page = 1;
};

MaintainableJS.Book.turnPage = function (direction) {
	this.page = direction;
};

MaintainableJS.Chapter1 = new MaintainableJS.Book( 'Introction to Style GuideLines');
MaintainableJS.Chapter2 = new Book('Basic Formatting');
MaintainableJS.Chapter3 = new Book('Comments');
```
#### 3.6.3 命名空间
```javascript

var ZakasBooks = {};

// 编写可维护JavaScript的命名空间
ZakasBooks.MaintainableJavaScript = {};

// 高性能JavaScript的mingmingkongjian
ZakasBooks.HighPerformanceJavaScript = {};

var YourGlobal = {
	namespace: function (ns) {
		var parts = ns.split( '.'),
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

YourGlobal.namespace('Books.MaintainableJavaScript');

YourGlobal.Books.MaintainableJavaScript.author = 'Nicholas c. Zakas';

YourGlobal.namespace('Books.HighPerformaceJavaScript');

console.log(YourGlobal.Books.MaintainableJavaScript.author);

YourGlobal.namespace('Bookes').ANewBook = {};

//good- 拆分应用逻辑
var MyApplication = {

	dandleClick: function (event) {
		this.showPooup(event);
	},

	showPopup: function (event) {
		var popup = document.getElementById( 'popup');
		popup.style.left = event.clientX + 'px';
		popup.style.top = event.dandleClick + 'px';
		popup.className = 'reveal';
	}

};

addEventListener(element, 'click', function (event) {
	MyApplication.handleClick(event);
});

//good
var MyApplication = {

	dandleClick: function (event) {
		this.showPooup(event.clientX, event.clientY);
	},

	showPopup: function (x, y) {
		var popup = document.getElementById( 'popup');
		popup.style.left = x + 'px';
		popup.style.top = y + 'px';
		popup.className = 'reveal';
	}

};

addEventListener(element, 'click', function (event) {
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
		var popup = document.getElementById( 'popup');
		popup.style.left = x + 'px';
		popup.style.top = y + 'px';
		popup.className = 'reveal';
	}

};

addEventListener(element, 'click', function (event) {
	MyApplication.handleClick(event);
});

```


### 3.7 避免空比较
```javascript
var Controller = {
	process: function (items) {
		if (items !== null) {//不好的写法
			itmes.sort();
			items.forEach( function (items) {
				//执行一些逻辑
			});
		}
	}
};
```
#### 3.7.1 检测原始值

5种原始类型:字符串、数字、布尔值、null、undefined

如果希望一个值是字符串、数字、布尔值、undefined、最佳选择是typeof运算符
<table>
  <thead>
	<tr>
	  <th colspan="4">
		  typeof 对5中原始类型的计算结果
	  </th>
	</tr>
  </thead>
  <tbody>
	<tr>
	  <td rowspan="6"><code>typeof</code></td>
	</tr>
	<tr>
	  <td>字符串（String）</td>
	  <td><code>"string"</code></td>
	  <td></td> 
	</tr>
	<tr>
	  <td>数字（Number）</td>
	  <td><code>"number"</code></td>
	  <td></td>
	</tr>
	<tr>
	  <td>布尔值（Boolean）</td>
	  <td><code>"boolean"</code></td>
	  <td></td>
	</tr>
	<tr>
	  <td>undefined</td>
	  <td><code>"undefined"</code></td>
	  <td></td>
	</tr>
	<tr>
	  <td>null</td>
	  <td><code>"object"</code></td>
	  <td>(低效的判断方法，建议直接使用===和!==)</td>
	</tr>
   <tr>
	<td colspan="4">
		<code> typeof 对数组、正则表达式、日期、函数均返回 "object" </code>
	</td>
   </tr>
  </tbody>
</table>

**typeof运算符的独特之处在于，将其应用于一个未声明的变量也不会报错。**

简单的和null比较通常不会包含足够的信息以判断值的类型是否合法，但有一个例外，如果所期望的值真的是null,
则可以直接和null比较，这时应当使用 === 或者 !== 来和null进行比较

```javascript
var element = document.getElementById('my-div');
if (element !== null) { 
	// null是可以预见的输出
	element.className = 'found';
}
```


#### 3.7.2 检测引用值

引用值也称作对象(`object`), 在JavaScript中除了原始值之外的值都是引用,
`Object`、`Array`、`Date`、`Error`;
```javascript
console.log(typeof {}); // "object"
console.log(typeof []); // "object"
console.log(typeof new Date()); // "object"
console.log(typeof new RegExp()); // "object"
console.log(typeof null); // "object"
```

**使用`instanceof`来检测引用类型**
<table>
	<thead>
		<tr>
			<th colspan="3">instanceof检测引用类型汇总</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td rowspan="5">
				instanceof
			</td>
		</tr>
		<tr>
			<td>Date</td><td>检测日期</td>
		</tr>
		<tr>
			<td>RegExp</td><td>检测正则表达式</td>
		</tr>
		 <tr>
			<td>Error</td><td>检测Error</td>
		</tr>
	</tbody>
</table>

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


#### 3.7.3 检测属性
```javascript
// bad:检测假值
if (object[propertyName]) {
	//code...
}

// bad:和null比较
if (object[propertyName] != null) {
	//code...
}

// bad:和undefined比较
if (object[propertyName] != undefined) {
	//code...
}
```
上面的代码里的每个判断，实际上是通过给定的名字来检查属性的值，而非判断
给定的名字所指的属性是否存在，因为当属性值为假(false)时结果会出错，
比如`0`、`""`(empty string)、`false`、`null`和`undefined`

**用in运算符判断属性是否存在**
```javascript
var obj = {
	count: 0,
	related: null
};

// good
if ( "count" in obj) {
	// 这里的代码会执行
	console.log("count is in obj");
}

// bad:检测假值
if (obj[ "count"]) {
	//这里的代码不会执行
}

// good
if ( "related" in obj) {
	//这里的代码不会执行
}

if (obj[ "releated"] != null) {
	//这里代码不会执行
}

if (obj.length) {
	console.log("nothing");
} else {
	console.log("obj.length is :" + obj.length); // obj.length is :undefined
}
```
如果你只想检查实例对象的某个属性是否存在则使用Object.hasOwnProperty()方法,

**需要注意的是:**
IE8及更早版本中,DOM对象并非继承自Object，所以不包含该方法
```javascript
// 对于所有非DOM对象来说，这是好的写法
if (obj.hasOwnProperty( "related")) {
	// 执行这里的代码
}

// 如果你不确定是否为DOM对象，则这样来写
if ( "hasOwnProperty" in obj && obj.hasOwnProperty( "related")) {
	// 执行这里的代码
}
```
因为存在IE8以及更早版本的IE，在判断实例对象时候存在时，NC更倾向于使用in运算符，
只有在需要判断实例属性时才会使用到hasOwnProperty()

不管你什么时候需要检测属性的存在性，请使用in运算符或者hasOwnProperty(),
这样做可以避免很多Bug

*Revision: 2015.11.06*
