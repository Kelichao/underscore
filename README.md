# underscore模板使用介绍



> 底线库（\_）的相关用法包含常用函数，模板引擎

- [底线库模板解读](https://github.com/hanzichi/underscore-analysis)
- [这里推荐一个模板Mustache.js ](https://github.com/janl/mustache.js#readme)

## HTML模板介绍

```html
<!--type需要修改，以及增加一个id -->
<script type="text/underscoreTemplat" id="underscoreTemp1">
	//innerHTML
</script>
```

```js
var html = document.getElementById("underscoreTemp1").innerHTML// 取数据直接取
```

![image](https://cloud.githubusercontent.com/assets/18028533/20421233/9e8a9996-ad9d-11e6-9272-79c7a4ca15c4.png);

## 底线库模板语法

> 底线库模板逻辑非常的清新，结构就跟原生的js语法一模一样，而且不用对html标签的特殊包裹

<table>
	<tr>
               <td colspan=2>模板语法</td></tr>
	<tr>
		<td><% ... %></td>
		<td>填js语句，可以很灵活的执行</td>
	</tr>
	<tr>
		<td>&lt;script&gt;</td>
		<td>填写普通的标签不需要添加多余的标签</td>
	</tr>
	<tr>
		<td><%= ... %></td>
		<td>填写需要渲染的数值</td>
	</tr>
		<tr>
		<td><% print(...); %></td>
		<td>作用与上面=形式一样，用于渲染并输出内容</td>
	</tr>
	<tr>
		<td><%- ... %></td>
		<td>输出带标签形式的html（预编译）</td>
	</tr>
</table>

## 模板的编译分为三步
```js
// 拿到模板
var html = "hello: <%= name %>"
// 编译模板
var compiled = _.template(html);
// 执行编译
console.log(compiled({name: 'moe'}));
```
## 模板的基本使用办法
```html
<script type="text/underscoreTemplat" id="underscoreTemp1">
	<% _.each(viewList, function(value, key) { %>
		<div><%= value %></div>
	<% }); %>
</script>
<script type="text/javascript">
var html = document.getElementById("underscoreTemp1").innerHTML;

var compiled = _.template(html);

// 注意，这里传进去是一整个对象，可以自己外围包一层
// each在取viewList是整个data里面拿对应的键值对
var data = {
	viewList: {
		1:"a",
		2:"b",
		3:"c"
		}
	};

 #$("body").append(compiled(data));
</script> 
```
![image](https://cloud.githubusercontent.com/assets/18028533/20422762/44496930-ada7-11e6-9450-6aeb78cf3c09.png)

## 快捷模板写法(推荐)

```js
<script type="text/underscoreTemplat" id="underscoreTemp1">
	{{ _.each(viewList, function(value, key) { }}
		<div>{{= value }}</div>
	{{ }); }}
</script>
<script type="text/javascript">
var html = document.getElementById("underscoreTemp1").innerHTML;

// 设置Mustache.js类型的模板语法
// 这句话需要写在_.template()方法之前
_.templateSettings = {
  interpolate: /\{\{=([\s\S]+?)\}\}/g,
  evaluate: /\{\{([\s\S]+?)\}\}/g,
  escape: /\{\{-([\s\S]+?)\}\}/g
};

// 注意，这里传进去是一整个对象，可以自己外围包一层
// each在取viewList是整个data里面拿对应的键值对
var data = {
	viewList: {
		1:"a",
		2:"b",
		3:"c"
		}
	};

// 将提取模板与数据渲染写在一起
var template = _.template(html,data);

$("body").append(template);
```
- 由于有些老版本的库可能不支持直接编写，所以推荐分两步进行。

## with 指定当前变量
```js
// 默认的, template 通过 with 语句来取得 data 所有的值.
// 当然, 您也可以在 variable 设置里指定一个变量名. 这样能显著提升模板的渲染速度.

_.template("Using 'with': <%= data.answer %>", {variable: 'data'})({answer: 'no'});
//=> "Using 'with': no"
```
