# JavaScript 编程规范

---

- 添加了React Native 的一些命名规范

## 1 JavaScript 命名约定

### 1.1 大小写约定


**大小写样式**
下列术语描述了标识符的不同大小写形式。

**`Pascal 大小写`**
别名：“大驼峰式”（upper camel case）
将标识符的首字母和后面连接的每个单词的首字母都大写。 可以对三字符或更多字符的标识符使用 Pascal 大小写。 例如：
`BackColor`

**`Camel`**
别名：“小驼峰式”（lower camel case）
标识符的首字母小写，而每个后面连接的单词的首字母都大写。 例如：
`backColor`

**`大写`**
标识符中的所有字母都大写。 例如：
`IO`

**`小写`**
标识符中的所有字母都小写。例如：
`color`

#### 1.1.1 基本大小写规则

如果标识符由多个单词组成，请不要在各单词之间使用分隔符，如下划线（“_”）或连字符（“-”）等。 而应使用大小写来指示每个单词的开头。
<table>
  <thead>
    <tr>
      <th colspan="3">
          标识符大小写规则汇总
      </th>
    </tr>
    <tr>
      <th>标识符</th>
      <th>Case</th>
      <th>示例</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>局部变量</td>
      <td>Camel</td>
      <td><code>arg,jsonArg</code></td>
    </tr>
    <tr>
      <td>全局变量</td>
      <td>Pascal、大写</td>
      <td><code>Goods,GoodsDetail,GD,MyAPP</code></td>
    </tr>
    <tr>
      <td>常量</td>
      <td>大写、<code>大写+下划线</code></td>
      <td><code>PI,AJAXURL,MAX_VALUE</code></td>
    </tr>
    <tr>
      <td>方法</td>
      <td>Camel</td>
      <td><code>getName(),getGoodsGallery()</code></td>
    </tr>
    <tr>
      <td>参数</td>
      <td>Camel</td>
      <td><code>imgWidth,imgHeight</code></td>
    </tr>
    <tr>
      <td>构造函数</td>
      <td>Pascal</td>
      <td><code>Student(),MyConstructor()</code></td>
    </tr>
  </tbody>
</table>

详细描述如下：

**局部变量**

局部变量采用Camel大小写。
```javascript
function func() {
    var json; // Camel
    var jsonArg; // Camel
}
```

**全局变量**

全局变量采用大写或者Pascal。
```javascript
var GOODS = {}, // 单个单词采用大写
    GoodsDetail = {}, // 多个单词采用Pascal
    GD = GoodsDetail; // 多个单词的缩写采用全大写
```

**常量**

对于那些程序运行周期内不会更改的变量使用全大写字母来命名。

**特例：允许使用大写+下划线的方式来命名常量**
```javascript
// 推荐
var PI = 3.14, 
    AJAXURL = '/mobile/getCitys.action',
    MAX_VALUE = 100; // 大写字母和下划线命名
// 不推荐
var Max_Value = 100,
    max_value = 100;
```

**方法**

方法采用Camel大小写。
```javascript
// 正确的写法
var getGallery = function () {
};
// 或者
function getGallery() {
}

// 不推荐的写法
function GetGallery(){
}
```

**参数**
参数采用Camel大小写。
```javascript
function getGallery(imgWidth, imgHeight) {
}
```
**构造函数**
JavaScript构造函数采用Pascal大小写。
```javascript
function Student(name, age) {
    /// <summary>
    /// 学生构造函数
    /// </summary>
    /// <param name="name" type="String">姓名</param>
    /// <param name="age" type="Number">年龄</param>
    this.name = name;
    this.age = age;
}

var s1 = new Student('李鸣', 12),
    s2 = new Student('黄蕾', 23);
```

### 1.1.2 首字母缩写词的大小写规则
首字母缩写词是由术语或短语中各单词的首字母构成的单词。 例如，`HTML` 是 Hypertext Markup Language 的首字母缩写。 只有在公众广为认知和理解的情况下，才应在标识符中使用首字母缩写词。 首字母缩写词不同于缩写词，因为缩写词是一个单词的缩写。 例如，`ID` 是 `identifier` 的缩写。
**注意注意**

可在标识符中使用的两个缩写词是 `ID` 和 `OK`。 在采用 Pascal 大小写格式的标识符中，这两个缩写词的大小写形式应分别为 `Id` 和 `Ok`。 如果在采用大小写混合格式的标识符中将这两个缩写词用作首个单词，则它们的大小写形式应分别为 `id` 和 `ok`。

首字母缩写词的大小写取决于首字母缩写词的长度。 所有首字母缩写词都至少包含两个字符。 为方便说明准则，如果一个首字母缩写词只包含两个字符，则将其视为短型首字母缩写词。 包含三个或三个以上字符的首字母缩写词为长型首字母缩写词。

**下列准则为短型和长型首字母缩写词指定了正确的大小写规则。 标识符大小写规则优先于首字母缩写词大小写规则。**

两字符首字母缩写词的两个字符都要大写，但当首字母缩写词作为大小写混合格式的标识符的首个单词时例外。

- 例如，名为 `DBRate` 的属性是一个采用 Pascal 大小写格式的标识符，它使用短型首字母缩写词 (DB) 作为首个单词。
- 又如，名为 `ioChannel` 的参数是一个采用大小写混合格式的标识符，它使用短型首字母缩写词 (IO) 作为首个单词。

**包含三个或三个以上字符的首字母缩写词只有第一个字符大写，但当首字母缩写词作为大小写混合格式的标识符的首个单词时例外。**

例如，名为 `XmlWriter` 的类是一个采用 Pascal 大小写格式的标识符，它使用长型首字母缩写词作为首个单词。 又如，名为 `htmlReader` 的参数是一个采用大小写混合格式的标识符，它使用长型首字母缩写词作为首个单词。

如果任何首字母缩写词位于采用大小写混合格式的标识符开头，则无论该首字母缩写词的长度如何，都不大写其中的任何字符。

- 例如，名为 `xmlStream` 的参数是一个采用大小写混合格式的标识符，它使用长型首字母缩写词 (xml) 作为首个单词。 名为 `dbServerName` 的参数是一个采用大小写混合格式的标识符，它使用短型首字母缩写词 (db) 作为首个单词。

**组合词和常用术语的大小写规则**

不要将所谓的紧凑格式组合词中的每个单词都大写。 这种组合词是指写作一个单词的组合词，如`endpoint`。

- 例如，`hashtable` 是一个紧凑格式的组合词，应将其视为一个单词并相应地确定大小写。 如果采用 Pascal 大小写格式，则该组合词为 `Hashtable`；如果采用大小写混合格式，则该组合词为 `hashtable`。 若要确定某个单词是否是紧凑格式的组合词，请查阅最新的词典。

下表列出了不是紧凑格式组合词的一些常用术语。 术语先以 Pascal 大小写格式显示，后面的括号中的是其大小写混合格式。

- `BitFlag` (`bitFlag`)
- `FileName` (`fileName`)
- `LogOff` (`logOff`)
- `LogOn` (`logOn`)
- `SignIn` (`signIn`)
- `SignOut` (`signOut`)
- `UserName` (`userName`)
- `WhiteSpace` (`whiteSpace`)

### 1.2 通用命名约定

#### 1.2.1 变量和函数

**变量和函数的命名应该具有语义性。**
对于函数命名来说，第一个单词一般是动词，常见的动词约定如下：

<table>
  <thead>
    <tr>
      <th>动词</th>
      <th>适用函数范围</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>can</code></td>
      <td>函数返回一个布尔值，如：<code>canSubmit()</code></td>
    </tr>
       <tr>
      <td><code>has</code></td>
      <td>方法被调用时立即触发，如：<code>hasSubElement()</code></td>
    </tr>
       <tr>
      <td><code>can</code></td>
      <td>函数返回一个布尔值，如：<code>canSubmit()</code></td>
    </tr>
       <tr>
      <td><code>is</code></td>
      <td>函数返回一个布尔值，如：<code>canSubmit()</code></td>
    </tr>
       <tr>
      <td><code>get</code></td>
      <td>函数返回一个非布尔值，如：<code>getName()</code></td>
    </tr>
    <tr>
      <td><code>set</code></td>
      <td>函数用来保存一个值，如：<code>setName('李雷雷')</code></td>
    </tr>
  </tbody>
</table>

**详细示例如下：**

```javascript
function canSubmit() {
    /// <summary>
    /// 是否能提交
    /// </summary>
}

function hasSubElement() {
    /// <summary>
    /// 是否有子节点
    /// </summary>
}

function isEnabled() {
    /// <summary>
    /// XX是否可用
    /// </summary>
}

function Student(name, age) {
    /// <summary>
    /// 学生构造函数
    /// </summary>
    /// <param name="name" type="String">学生姓名</param>
    /// <param name="age" type="Number">学生年龄</param>
    // 在这里可以加更严谨的验证逻辑来判断参数是否正确
    this.name = name || '未知';
    this.age = age || -1;
}

Student.prototype.getName = function () {
    /// <summary>
    /// 返回姓名
    /// </summary>
    /// <returns type="String">学生姓名</returns>
    return this.name;
};

Student.prototype.setName = function (name) {
    /// <summary>
    /// 设置姓名
    /// </summary>
    /// <param name="name" type="String">学生姓名</param>
    this.name = name || '未知';
}


var student = new Student('李雷', 12);
student.setName('李雷雷');
console.log(student.getName());
```

### 1.3 命名空间的命名

大小写遵循Pascal，

```javascript
var HSH = {};

// 编写可维护JavaScript的命名空间
ZakasBooks.MaintainableJavaScript = {};

// 高性能JavaScript的mingmingkongjian
ZakasBooks.HighPerformanceJavaScript = {};

// 通用的命名空间函数“namespace”
var YourGlobal = {
    namespace: function (ns) {
        var parts = ns.split("."),
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

// 使用
YourGlobal.namespace("Books.MaintainableJavaScript");
YourGlobal.Books.MaintainableJavaScript.author = 'Zakas';
console.log(YourGlobal.Books.MaintainableJavaScript.author); //Zakas

```
### 1.4 文件命名

**文件的命名只允许包含小写字母、数值、连字符、点、和下划线。**
- 文件名应简短、也必须描述文件。
- 文件名与功能相对应、如：地址列表可以命名为`hsh.address.list.js`


#### 参考
- https://msdn.microsoft.com/zh-cn/library/ms229002.aspx
- https://github.com/TooBug/javascript.patterns/blob/master/chapter2.markdown

*Revision: 2015.11.02*

##  1.5 React Native JavaScript 命名规范

| 标识符  | 大小写（Case） | 示例                                       |
| ---- | --------- | ---------------------------------------- |
| 文件名  | Pascal    | `Home.js`、 ` PersonalSettings.js`        |
| 类名   | Pascal    | `Home` 、 `PersonalSettings`              |
| 常量   | 大写        | `UNCHECK_ICON` 、`CHECKED_ICON`(例外:styles) |
| 方法   | Camel     | `goBack()` 、`render()`                   |
| 局部变量 | Camel     | `json` 、`jsonArgs`                       |
| 参数   | Camel     | `function getGallery(imgWidth, imgHeight) {}` |
| 样式属性 | Camel     | `container`、`buttonAdd`、`buttonAddSmall` |
|      |           |                                          |

```javascript
/* ! Counter (c) 2016, HSH */
/*
 * @name: Counter.js
 * @description: Counter 组件
 * @author: xiongda,xiong2
 * @date: 16/8/24
 * @param:
 *
 */

import React, {
    Component,
    PropTypes,
} from 'react';
import {
    StyleSheet,
    Text,
    View,
    TouchableHighlight,
} from 'react-native';

/* ICON图片（常量）*/  
const UNCHECK_ICON = require('../img/ic_checkbox_false.png'),
    CHECKED_ICON = require('../img/ic_checkbox_true.png'),
    REDUCE_ICON = require('../img/ic_reduce.png'),
    INCREASE_ICON = require('../img/ic_increase.png');
  
/* 类名采用Pascal,文件名和类名相同 */
class Counter extends Component {
  	   /**
     * 打印内容
     * @param msg 内容
     */
    print(msg){
        console.log(msg);
    }

    /**
     * 获取当前时间
     * @returns {Date} Date 示例
     */
    getDateTimeNow(){
        return new Date();
    }

    /**
     * 渲染UI
     * @returns {XML}
     */
    render() {
        var {increment,incrementTwo, incrementIfOdd, incrementAsync, decrement, decrementAsync, counter} = this.props;
        return (
            <View style={styles.container}>
                <View style={styles.displayPanel}>
                    <Text style={styles.numberBlock}>{counter}</Text>
                    <Text style={styles.unitBlock}>/ times</Text>
                </View>
                <View style={[styles.controlPanel, styles.inline]}>
                    <TouchableHighlight onPress={increment} style={styles.buttonAddSmall} underlayColor={colors.add.bg}>
                        <Text style={[styles.text, styles.textColorAdd]}>+</Text>
                    </TouchableHighlight>
                    <TouchableHighlight onPress={decrement} style={styles.buttonMinusSmall}
                                        underlayColor={colors.minus.bg}>
                        <Text style={[styles.text, styles.textColorMinus]}>-</Text>
                    </TouchableHighlight>
                </View>
                <View style={styles.controlPanel}>
                    <TouchableHighlight onPress={incrementIfOdd} style={styles.buttonAdd} underlayColor={colors.add.bg}>
                        <Text style={[styles.text, styles.textColorAdd]}>Increment if odd</Text>
                    </TouchableHighlight>
                    <TouchableHighlight
                        onPress={() => incrementAsync()} style={styles.buttonAdd} underlayColor={colors.add.bg}>
                        <Text style={[styles.text, styles.textColorAdd]}>Increment async</Text>
                    </TouchableHighlight>
                    <TouchableHighlight
                        onPress={() => decrementAsync()} style={styles.buttonMinus} underlayColor={colors.minus.bg}>
                        <Text style={[styles.text, styles.textColorMinus]}>Decrement async</Text>
                    </TouchableHighlight>
                    <TouchableHighlight
                        onPress={incrementTwo} style={styles.buttonAdd} underlayColor={colors.add.bg}>
                        <Text style={[styles.text, styles.textColorAdd]}>Increment 2</Text>
                    </TouchableHighlight>
                </View>
            </View>
        );
    }
}

/**
 * 用于验证传入组件的 props
 */
Counter.propTypes = {
    increment: PropTypes.func.isRequired, // 传入的increment必须是函数且是必传属性
    incrementIfOdd: PropTypes.func.isRequired,
    incrementAsync: PropTypes.func.isRequired,
    decrementAsync: PropTypes.func.isRequired,
    decrement: PropTypes.func.isRequired,
    counter: PropTypes.number.isRequired
};

/**
 * 颜色(在此示例中，因为是单页应用，将这些配置)
 */
const colors = {
    background: {
        bg: '#F5FCFF'
    },
    add: {
        font: '#F69',
        border: '#F69',
        bg: '#FFC1D6'
    },
    minus: {
        font: '#6495ED',
        border: '#6495ED',
        bg: '#D0DFF9'
    }
};

/**
 * 样式(组件内样式允许用小写，如果是全局样式最好采用Pascal来与内部样式来区分开)
 */
const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: colors.background.bg,
    },
    displayPanel: {
        marginVertical: 30,
        flexDirection: 'row',
        alignItems: 'flex-end',
    },
    controlPanel: {
        marginVertical: 30
    },
    inline: {
        flexDirection: 'row',
        alignItems: 'flex-end',
    },
    numberBlock: {
        fontSize: 100,
        fontWeight: 'bold',
        textAlign: 'center',
        width: 200,
    },
    unitBlock: {
        fontSize: 16,
        fontWeight: 'bold',
        alignSelf: 'flex-end',
    },
    textColorAdd: {
        color: colors.add.font,
    },
    textColorMinus: {
        color: colors.minus.font,
    },
    text: {
        fontSize: 20,
        fontWeight: 'bold',
        textAlign: 'center',
    },
    buttonAddSmall: {
        justifyContent: 'center',
        alignItems: 'center',
        height: 30,
        margin: 10,
        borderRadius: 10,
        borderWidth: 2,
        borderColor: colors.add.border,
        width: 80,
    },
    buttonMinusSmall: {
        justifyContent: 'center',
        alignItems: 'center',
        height: 30,
        margin: 10,
        borderRadius: 10,
        borderWidth: 2,
        borderColor: colors.minus.border,
        width: 80,
    },
    buttonAdd: {
        justifyContent: 'center',
        alignItems: 'center',
        height: 30,
        margin: 10,
        borderRadius: 10,
        borderWidth: 2,
        borderColor: colors.add.border,
        paddingLeft: 10,
        paddingRight: 10
    },
    buttonMinus: {
        justifyContent: 'center',
        alignItems: 'center',
        height: 30,
        margin: 10,
        borderRadius: 10,
        borderWidth: 2,
        borderColor: colors.minus.border,
        paddingLeft: 10,
        paddingRight: 10
    }
});

/* 导出的组件名或其他对向用 Pascal */
export default Counter;

// index.io.js

/* 导入的变量都用大写 */
import Counter from 'Counter';

// 不推荐的写法：因为这样很容易与当前类的一些变量或者属性混淆
import counter from 'Counter'; 
```

