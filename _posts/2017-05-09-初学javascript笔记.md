
---
layout:     post
title:      初学javascript笔记
subtitle:   初学javascript笔记
date:       2017-01-05
author:     BY annie dong
header-img: img/post-bg.jpg
catalog: true
tags:
    - javascript
---
javascript实现者Netscape、Mozilla基金会
它的解释器被称为JavaScript引擎，为浏览器的一部分，广泛用于客户端的脚本语言，最早是在HTML（标准通用标记语言下的一个应用）网页上使用，用来给HTML网页增加动态功能。

---
### javascript的组成
ECMAScript，描述了该语言的语法和基本对象。
文档对象模型（DOM），描述处理网页内容的方法和接口。
浏览器对象模型（BOM），描述与浏览器进行交互的方法和接口。

### 特点
* 是一种解释性脚本语言（代码不进行预编译）。
* 主要用来向HTML（标准通用标记语言下的一个应用）页面添加交互行为。
* 可以直接嵌入HTML页面，但写成单独的js文件有利于结构和行为的分离。
* 跨平台特性，在绝大多数浏览器的支持下，可以在多种平台下运行（如Windows、Linux、Mac、Android、iOS等）。

### 用途
* 嵌入动态文本于HTML页面。
* 对浏览器事件做出响应。 
* 读写HTML元素。
* 在数据被提交到服务器之前验证数据。
* 检测访客的浏览器信息。
* 控制cookies，包括创建和修改等。 
* 基于Node.js技术进行服务器端编程。 

```
document.write("<h1>This is a heading</h1>");
document.write("<p>This is a paragraph</p>");

<button type="button" onclick="alert('Welcome!')">点击这里</button>

x=document.getElementById("demo")  //查找元素
x.innerHTML="Hello JavaScript";    //改变内容

x=document.getElementById("demo")  //找到元素
x.style.color="#ff0000";           //改变样式

if isNaN(x) {alert("Not Numeric")};

<script> 和 </script> 会告诉 JavaScript 在何处开始和结束
如需使用外部文件，请在 <script> 标签的 "src" 属性中设置该 .js 文件
//document.getElementById("myH1").innerHTML="Welcome to my Homepage”;  注释
document.getElementById("myP").innerHTML="This is my first paragraph.";

/* 多行注释
document.getElementById("myH1").innerHTML="Welcome to my Homepage";
document.getElementById("myP").innerHTML="This is my first paragraph.";
*/
```

### JAVAScript Grammar

* 数据类型:  字符串、数字、布尔、数组、对象（hash）、Null、Undefined
JavaScript 中的几乎所有事务都是对象：字符串、数字、数组、日期、函数
创建person 对象，并赋予4个属性：
```
var person=new Object()
person.first_name = ‘annie'
person.last_name = ‘dong'
person.age = 10
person.sex = ‘female'
```

* 变量
局部，全局
向未声明的 JavaScript 变量来分配值为全局变量

* 函数： function f(arg1, arg2) {…...}

* ==  等于  x==8 为 false
* === 全等（值和类型）  x===5 为 true；x==="5" 为 false

* For/In 循环
JavaScript for/in 语句循环遍历对象的属性

* 异常捕获
```
try
  {
  //在这里运行代码
  }
catch(err)
  {
  //在这里处理错误
  }
```

* math算数
```
round() 四舍五入
random() 取0-10的随机数
floor()取小数的整数部分
```


* 正则表达式RegExp
```
var patt=new RegExp("e");
patt.test(’test’)  => true/false
patt.exec(’test’) => ‘e'
patt.compile("d”) 改变检索模式，也可以添加或删除第二个参数
```

```
window.open() - 打开新窗口
window.close() - 关闭当前窗口
window.moveTo() - 移动当前窗口
window.resizeTo() - 调整当前窗口的尺寸

Window Location
window.location 对象在编写时可不使用 window 这个前缀。


location.hostname 返回 web 主机的域名
location.pathname 返回当前页面的路径和文件名
location.port 返回 web 主机的端口 （80 或 443）
location.protocol 返回所使用的 web 协议（http:// 或 https://）
location.href

Window History
window.history 对象在编写时可不使用 window 这个前缀。

history.back() - 与在浏览器点击后退按钮相同
history.forward() - 与在浏览器中点击按钮向前相同

alert("文本")
prompt(‘文本’， ‘默认值’)
confirm('文本')

navigator.appCodeName
navigator.appName
navigator.appVersion
navigator.cookieEnabled
navigator.platform
navigator.userAgent
navigator.systemLanguage
```


* 计时
```
var t=setTimeout("javascript语句",毫秒) 未来的某时执行代码
clearTimeout() 取消setTimeout()
```



### JQUERY
```
$(this) 当前 HTML 元素
$("p")  所有 <p> 元素
$("p.intro")  所有 class="intro" 的 <p> 元素
$(".intro") 所有 class="intro" 的元素
$("#intro") id="intro" 的元素
$("ul li:first")  每个 <ul> 的第一个 <li> 元素
$("[href$='.jpg']") 所有带有以 ".jpg" 结尾的属性值的 href 属性
$("div#intro .head")  id="intro" 的 <div> 元素中的所有 class="head" 的元素


 attr()，html(), text(), val()提供回调函数。回调函数由两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值
remove() - 删除被选元素（及其子元素）
empty() - 从被选元素中删除子元素
addClass() - 向被选元素添加一个或多个类
removeClass() - 从被选元素删除一个或多个类
toggleClass() - 对被选元素进行添加/删除类的切换操作
css() - 设置或返回样式属性
width() 方法设置或返回元素的宽度（不包括内边距、边框或外边距）。
height() 方法设置或返回元素的高度（不包括内边距、边框或外边距）。
innerWidth() 方法返回元素的宽度（包括内边距）。
innerHeight() 方法返回元素的高度（包括内边距）。
outerWidth() 方法返回元素的宽度（包括内边距和边框）。
outerHeight() 方法返回元素的高度（包括内边距和边框）。
parent() 方法返回被选元素的直接父元素。
parents() 方法返回被选元素的所有祖先元素，它一路向上直到文档的根元素 (<html>)。
parentsUntil() 方法返回介于两个给定元素之间的所有祖先元素。
children() 方法返回被选元素的所有直接子元素。
find() 方法返回被选元素的后代元素，一路向下直到最后一个后代。
siblings() 方法返回被选元素的所有同胞元素。
next() 方法返回被选元素的下一个同胞元素。
nextAll() 方法返回被选元素的所有跟随的同胞元素。
nextUntil() 方法返回介于两个给定参数之间的所有跟随的同胞元素。
prev(), prevAll() 以及 prevUntil() 方法的工作方式与上面的方法类似
first() 方法返回被选元素的首个元素。
last() 方法返回被选元素的最后一个元素。
eq() 方法返回被选元素中带有指定索引号的元素。
filter() 方法允许您规定一个标准。不匹配这个标准的元素会被从集合中删除，匹配的元素会被返回。
not() 方法返回不匹配标准的所有元素。


$.ajax()回调函数可以设置不同的参数：
responseTxt - 包含调用成功时的结果内容
statusTXT - 包含调用的状态
xhr - 包含 XMLHttpRequest 对象

$.get(), $.post() 回调函数第一个回调参数存有被请求页面的内容，第二个回调参数存有请求的状态
```


* click(), bind(),live(),delegate()区别：

1、click()与bind()

　　1).click()
　　　　在jqeury事件处理API中，bind()是其API基础。click(),mouseover(),mousermove等来处理事件，真正起作用的是bind()。而这些方法都只是辅助作用（别名函数），简化使用。他们都只有一个参数(触发事件时执行的回调函数).
　　2).bind()
　　　　bind()能更好的控制事件处理函数中的处理过程，且它可以一次绑定多个事件（事件名作为第一个参数，多个事件用空格分开，eg：bind('click contains',function(){})  鼠标左右点击事件 ）
所有实用bind()或者click(),mouserover()绑定的事件都可以使用unbind()方法解除绑定

2、live()
　　与bind()作用基本一样。
　　最重要区别：live()可以将事件绑定到当前和将来的元素(eg:为id=zy元素绑定点击事件，而当你用js动态生成一个节点并插入到dom文档结构中时，如果你是用bind()绑定的，怎么新插入的节点将不会有该bind绑定事件。而live()则可以);
　　缺点: 无法用于链式结构。
　　　　　　eg:  $('.class').live('click',function(){  })     正确写法
　　　　　　　　 $('.class').find('span').live('click',function(){  })   错误写法  无效
　　live()绑定的事件可用 die()方法解除绑定。

3、delegate()
　　与live()作用基本一样，但是解决live缺点。它通过将选择器内的上下文作为第一个参数来解决live问题（也就是delegate得第一个参数你可以当作是一个选择元素所用）。
eg. $('.class').delegate('span','mouseover',fucntion(){  })    为class为class的元素下的所有span标签绑定mouseover事件。
　　通过delegate()绑定的事件可通过undelegate()方法解除处理函数的绑定。

