# webpack 介绍

- [webpack官网](http://webpack.github.io/)
- [webpack文档](http://webpack.github.io/docs/)
- [webpack介绍](http://webpack.github.io/docs/what-is-webpack.html)
- [webpack教程](http://webpack.github.io/docs/tutorials/getting-started/)

## webpack 简单教程
从本教程中，你将学会：
- 如何安装webpack
- 如何使用webpack
- 如何使用加载器
- 如何使用webpack 开发服务器

### 安装webpack
使用node.js安装，在命令行中输入以下命令来进行全局安装：

`npm install webpack -g`

安装完成后你就可以像使用`npm`一样来使用`webpack`命令。

### 设置编译

在一个空目录中添加创建一个entry.js 和 index.html文件:
以Windows系统为例，目录路径为`D:\Dev\React\webpackstart`

**entry.js**
```javascript
document.writeln('hello webpack!');
```

**index.html**
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>SETUP THE COMPILATION</title>
</head>
<body>
    <script src="bundle.js" charset="utf-8" type="text/javascript"></script>
</body>
</html>
```

打开命令行，切换到当前目录` cd /d D:\Dev\React\webpackstart`，运行以下命令进行打包：

`webpack ./entry.js bundle.js`

得到以下结果：

```shell
Hash: 85699777ef8f322c7070
Version: webpack 1.13.0
Time: 102ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.42 kB       0  [emitted]  main
   [0] ./entry.js 33 bytes {0} [built]
```
在浏览器中查看`index.html` 

### 引入第二个脚本文件

在当前目录中添加`content.js`文件
```javascript
    module.exports='hello webpack from content.js!';
```

修改`entry.js`：

```javascript
document.writeln('hello webpack!');
document.writeln(require('./content.js'));
```
在DOS命令行中重新编译（打包）：
`webpack ./entry.js bundle.js`

浏览器中查看index.html

### 第一个webpack加载器

如我我们想添加一个CSS文件到我们的APP中：
但是webpack只能处理原生的JavaScript，所以我们需要`css-loader`来处理CSS文件，同时也需要引入
`style-loader`来应用CSS文件中的样式。

运行`npm install css-loader style-loader`来安装加载器（局部安装）

在当前目录中添加一个文件夹css（`D:\Dev\React\webpackstart\css`）,在css文件夹中添加样式文件style.css
```css
body{
    background-color:#ff6a00;
}
```

修改entry.js:
```javascript
//document.writeln('hello webpack!');
require('!style!css!./css/style.css');
document.writeln(require('./content.js'));
```

重新编译（打包）后查看index.html。
[更多的加载器](http://webpack.github.io/docs/list-of-loaders.html);

### 使用webpack.config.js

添加文件`webpack.config.js`

```javascript
module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname + "/build/",
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style!css" }
        ]
    }
};
```

修改index.html文件：
```html
<script src="./build/bundle.js" charset="utf-8" type="text/javascript"></script>
```
添加配置文件之后，只需执行`webpack`命令进行编译，完成之后再浏览器中看到一样的效果。


### 更直观的输出

随着项目的不断增大，就需要更长的时间来进行编译。可以通过以下命令来实现类进度条且带有颜色：

`webpack --progress --colors`

### 监视模式

每次重新编译是不是很麻烦呢，我们可以通过`--watch`参数来实现自动编译：

`webpack --progress --colors --watch`

执行该命令之后，只要命令行窗口不关闭，相关文件一旦改动就会自动进行重新编译。

### 开发服务器

`npm install webpack-dev-server -g`
`webpack-dev-server --progress --colors`

webpack开发服务器使用8080端口 [localhost:8080](localhost:8080) 绑定了一个express服务器，

which serves your static assets as well as the bundle (compiled automatically). 
It automatically updates the browser page when a bundle is recompiled (SockJS).
Open [http://localhost:8080/webpack-dev-server/bundle]( http://localhost:8080/webpack-dev-server/bundle) in your browser.

The dev server uses webpack’s watch mode. It also prevents webpack from emitting the resulting files to disk.
Instead it keeps and serves the resulting files from memory.
