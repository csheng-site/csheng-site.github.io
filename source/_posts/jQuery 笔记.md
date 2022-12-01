---
title: jQuery笔记
date: 2022-11-21 12:00:00
categories: JavaScript
description: jQuery 是一个高效、精简并且功能丰富的 JavaScript 工具库。jQuery的宗旨：“写的更少，做得更多！”
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/jQuery.jpeg
--- 

<div class="btn-center">
{% btn 'https://jquery.com/',jQuery官网,far fa-hand-point-right,blue larger %}
{% btn 'https://www.jq22.com/chm/jquery/',jQuery手册,far fa-hand-point-right,green larger %}
</div>

# jQuery简介和使用
**jQuery简介**：
- jQuery 是一个高效、精简并且功能丰富的 {% label 'JavaScript 工具库' pink %}。
- 它提供的 API 易于使用且兼容众多浏览器，这让诸如 HTML 文档遍历和操作、事件处理、动画和 Ajax 操作更加简单。
- 目前超过90%的网站都使用了jQuery库，
- jQuery的宗旨：{% label '写的更少，做得更多！' red %}

**CDN地址**：
```html
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.1/jquery.min.js"></script>
```

**jQuery快速使用**：
```html
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.1/jquery.min.js"></script>
<script>
	$(function () {
		// 请将jQuery代码书写在这里 ...
	});
</script>
```
# jQuery核心函数
## 选择器
### 基本选择器
```js
标签选择器：$('div')
id选择器：$('#btn')
class选择器：$('.red')
通配符选择器：$('.content *') 
并集选择器：$('p, button')
交集选择器：$('p.red')
```

### 层级选择器
```js
子代选择器：$('ul>span')
后代选择器：$('ul span')
兄弟选择器：
    $('#box+li'): 选中id为box的下一个兄弟li
    $('#box~li'): 选中id为box的后边的兄弟li
```

### 过滤选择器
```js
基本筛选器：
    $('tr:even')/$('tr:odd') 比如：实现隔行变色
    $('tr:first') 比如：让表格的第一行变色
    $('tr:last') 比如：让表格的最后一行变色
    $('tr:lt(2)')/$('tr:gt(2)') 比如：让下标（从0开始）小于/大于2的行变色
    $('tr:eq(2)')/$('tr:not(tr:eq(2))') 比如：让下标（从0开始）等于/不等于2的行变色
内容筛选器：
    $('td:contains("男")') 比如：让内容为“男”的单元格变色
    $('td:has(span)') 比如：让内容为span标签的单元格变色
    $('td:empty')/$('td:parent') 比如：让内容为空/不为空的单元格变色
属性筛选器：
    $('[name]') 比如：查找 name 属性的标签元素
    $('[name="id"]')/$('[name!="id"]') 比如：查找 name 属性为 id / 不为 id 的标签元素
    $('[name|="id"]') 或 $('[name^="id"]') 比如：查找 name 属性 为id 或者以 id开头 的标签元素
    $('[name$="id"]') 比如：查找 name 属性 以 id 结尾 的标签元素
    $('[name*="id"]') 比如：查找 name 属性 包含一个给定的子字符串id 的标签元素
    $('[name~="id"]') 比如：查找 name 属性 包含一个给定值为id 的标签元素
    $('[name="id"][class="my_id"]') 比如：查找 name 属性 为 id 并且 class 属性 为 my_id 的标签元素
可见性筛选器：
子元素筛选器：
```

### 表单选择器
```markdown
表单类型选择器
表单状态选择器
```



2.2、工具
2.2.1、$.each方法
方法描述：一个通用的迭代函数，它可以用来无缝迭代对象和数组。数组和类似数组的对象通过一个长度属性（如一个函数的参数对象）来迭代数字索引，从0到length - 1，其他对象通过其属性名进行迭代。

需求描述：给定一个数组，使用$.each方法进行遍历输出

var arr = [10, 90, 20, 80, 30, 70, 40, 60, 50];
$.each(arr, function (index, element) {
    console.log(index, element);
});
1
2
3
4


需求描述：给定一个对象，使用$.each方法进行遍历输出

var obj = {
    name: 'Tom',
    age: 28,
    speak: function () {}
};
$.each(obj, function (key, value) {
    console.log(key, value);
});
1
2
3
4
5
6
7
8


2.2.2、$.trim方法
方法描述：去掉字符串起始和结尾的空格。

需求描述：给定一个字符串，去掉该字符串的前后空格

var str = '    hello    ';
console.log($.trim(str));
1
2


2.2.3、$.type方法
方法描述：确定JavaScript 对象的类型。

需求描述：给定一个对象，输出该对象的类型

var str = '    hello    ';
console.log($.type(str));
1
2


2.2.4、$.isArray方法
方法描述：用来测试指定对象是否为一个数组。

需求描述：给定一个对象，输出该对象是不是数组类型

var arr = [10, 90, 20, 80, 30, 70, 40, 60, 50];
console.log($.isArray(arr));
1
2


2.2.5、$.isFunction方法
方法描述：用来测试指定对象是否为一个函数。

需求描述：给定一个对象，输出该对象是不是函数类型

var fun = function () {
    console.log("hello");
};
console.log($.isFunction(fun));
1
2
3
4


2.3、ajax
2.3.1、$.ajax方法
方法描述：执行一个异步的HTTP的请求。

需求描述：执行一个异步的HTTP GET请求，从服务器加载数据。

$.ajax({
    url: '/user/login',
    type: 'get',
    data: {
        username: 'zhangsan',
        password: '123456'
    },
    dataType: 'text',
    success: function (response) {
        alert(response);
    },
    error: function (response) {
        alert(response);
    }
});
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
需求描述：执行一个异步的HTTP POST请求，从服务器加载数据。

$.ajax({
    url: '/user/login',
    type: 'post',
    data: {
        username: 'zhangsan',
        password: '123456'
    },
    dataType: 'text',
    success: function (response) {
        alert(response);
    },
    error: function (response) {
        alert(response);
    }
});
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
2.3.2、$.get方法
方法描述：使用一个HTTP GET请求从服务器加载数据。

这是一个ajax功能的缩写，这相当于:

$.ajax({
    url: url,
    data: data,
    success: success,
    dataType: dataType
});
1
2
3
4
5
6
$.get('/user/login', {username: 'zhangsan', password: '123456'}, function (response) {
    alert(response);
});
1
2
3
2.3.3、$.post方法
方法描述：使用一个HTTP POST请求从服务器加载数据。

这是一个ajax功能的缩写，这相当于:

$.ajax({
    url: url,
    data: data,
    success: success,
    dataType: dataType
});
1
2
3
4
5
6
$.post('/user/login', {username: 'zhangsan', password: '123456'}, function (response) {
    alert(response);
});
1
2
3
第三章 jQuery核心对象
3.1、属性
3.1.1、属性
3.1.1.1、attr()
方法描述：专门操作属性值为非布尔值的属性，该方法读写一体。

需求描述：设置p标签的title属性为”我是attr修改后的段落标题“

<p id="content" title="我是段落标题">我是段落</p>
1
$('#content').attr('title', '我是attr修改后的段落标题');
1


需求描述：读取p标签的title属性并输出

<p id="content" title="我是段落标题">我是段落</p>
1
console.log($('#content').attr('title'));
1


3.1.1.2、prop()
方法描述：专门操作属性值为布尔值的属性，该方法读写一体。

需求描述：设置复选框的状态为选中状态

<input type="checkbox">复选框
1
$(':checkbox').prop('checked', 'true');
1


需求描述：读取复选框的选中状态并输出

<input type="checkbox" checked>复选框
1
console.log($(':checkbox').prop('checked'));
1


3.1.1.3、val()
方法描述：该方法主要用于获取表单元素的值和设置表单元素的值，该方法读写一体。

需求描述：设置文本框的值为”123456“

<input type="text">
1
$(':text').val('123456');
1


需求描述：读取文本框的值并输出

<input type="text" value="123456">
1
console.log($(':text').val());
1


3.1.2、样式
3.1.2.1、css()
方法描述：获取匹配元素集合中的第一个元素的样式属性的计算值或设置每个匹配元素的一个或多个CSS属性。

需求描述：设置div的背景颜色为红色，字体颜色为白色

<div>我是div</div>
1
$('div').css({
    'background': 'red',
    'color': 'white'
});
1
2
3
4


需求描述：获取div的背景颜色和字体颜色并输出

<div style="background: red;color: white">我是div</div>
1
console.log($('div').css('background'));
console.log($('div').css('color'));
1
2


3.1.2.2、addClass()
方法描述：为每个匹配的元素添加指定的样式类名。

需求描述：为所有的li添加样式”beauty“

.beauty {
    font-weight: bold;
    font-size: 18px;
    color: coral;
}
1
2
3
4
5
<ul>
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
    <li>列表项4</li>
</ul>
1
2
3
4
5
6
$('li').addClass('beauty');
1


3.1.2.3、removeClass()
方法描述：移除集合中每个匹配元素上一个，多个或全部样式。

需求描述：为所有的li移除样式”beauty“

.beauty {
    font-weight: bold;
    font-size: 18px;
    color: coral;
}
1
2
3
4
5
<ul>
    <li class="beauty">列表项1</li>
    <li class="beauty">列表项2</li>
    <li class="beauty">列表项3</li>
    <li class="beauty">列表项4</li>
</ul>
1
2
3
4
5
6
$('li').removeClass('beauty');
1


3.1.2.4、hasClass()
方法描述：确定任何一个匹配元素是否有被分配给定的样式类。

需求描述：判断p标签是否包含”beauty“的样式

.beauty {
    font-weight: bold;
    font-size: 18px;
    color: coral;
}
1
2
3
4
5
<p class="beauty"></p>
1
console.log($('p').hasClass('beauty'));
1


3.1.2.5、toggleClass()
方法描述：为匹配的元素集合中的每个元素上添加或删除一个或多个样式类，取决于这个样式类是否存在。

注意：如果存在（不存在）就删除（添加）一个样式类

需求描述：当单击按钮的时候，隐藏div，再次单击按钮的时候，显示div

.hide {
    width: 100px;
    height: 100px;
    display: none;
}
1
2
3
4
5
<button>按钮</button>
<div>我是div</div>
1
2
$('button').click(function () {
    $('div').toggleClass('hide');
});
1
2
3


3.1.3、尺寸
3.1.3.1、width()
方法描述：获取内容元素width的值。

3.1.3.2、height()
方法描述：获取内容元素height的值。

3.1.3.3、innerWidth()
方法描述：获取内容元素width+padding的值。

3.1.3.4、innerHeight()
方法描述：获取内容元素height+padding的值。

3.1.3.5、outerWidth()
方法描述：outerWidth(false/true)，获取内容元素width+padding+border的值，如果是true再加上margin的值。

3.1.3.6、outerHeight()
方法描述：outerHeight(false/true)，获取内容元素height+padding+border的值，如果是true再加上margin的值。

3.1.3.7、综合演示
需求描述：创建按一个div，获取以上六种值

.box {
    margin: 30px;
    padding: 20px;
    border: 10px;
    width: 100px;
    height: 100px;
    background: coral;
}
1
2
3
4
5
6
7
8
<div class="box"></div>
1
var $box = $('.box');
console.log($box.width(), $box.height());// 100 100
console.log($box.innerWidth(), $box.innerHeight());// 140 140
console.log($box.outerWidth(), $box.outerHeight());// 160 160
console.log($box.outerWidth(true), $box.outerHeight(true));// 220 220
1
2
3
4
5
3.1.4、位置
3.1.4.1、offset()
方法描述：相对页面左上角的坐标。

需求描述：获取div相对页面左上角的坐标

.box {
    width: 100px;
    height: 100px;
    background: coral;
}
1
2
3
4
5
<div class="box"></div>
1
var offset = $('.box').offset();
console.log(offset.left, offset.top);
1
2


3.1.4.2、position()
方法描述：相对于父元素左上角的坐标。

需求描述：获取div相对于父元素左上角的坐标

.box-container {
    width: 300px;
    height: 300px;
    background: pink;
    position: absolute;
    left: 20px;
    top: 20px;
}

.box {
    width: 100px;
    height: 100px;
    background: coral;
    position: absolute;
    left: 20px;
    top: 20px;
}
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
<div class="box-container">
    <div class="box"></div>
</div>
1
2
3
var offset = $('.box').position();
console.log(offset.left, offset.top);
1
2


3.1.4.3、scrollLeft()
方法描述：读取/设置滚动条的X坐标，该方法读写合一。

读取页面滚动条的Y坐标(兼容chrome和IE)

var scrollLeft = $(document.body).scrollLeft()+$(document.documentElement).scrollLeft();
1
设置页面滚动条滚动到指定位置(兼容chrome和IE)

$('body,html').scrollLeft(60);
1
需求描述：设置页面的宽度为2000px，设置滚动条的X轴坐标为300px，要求兼容各种浏览器

$('body').css('width', '2000px');
$('html,body').scrollLeft(300);
1
2
3.1.4.4、scrollTop()
方法描述：读取/设置滚动条的Y坐标，该方法读写合一。

读取页面滚动条的Y坐标(兼容chrome和IE)

var scrollTop = $(document.body).scrollTop()+$(document.documentElement).scrollTop();
1
设置页面滚动条滚动到指定位置(兼容chrome和IE)

$('body,html').scrollTop(60);
1
需求描述：设置页面的高度为2000px，设置滚动条的Y轴坐标为300px，要求兼容各种浏览器

$('body').css('height', '2000px');
$('html,body').scrollTop(300);
1
2
3.2、操作
3.2.1、DOM内部插入
3.2.1.1、text()
方法描述：设置/获取元素的文本内容，该方法读写合一。

需求描述：设置p段落标签的内容为“我是段落”

<p></p>
1
$('p').text('我是段落');
1


需求描述：获取p段落标签的内容并输出

<p>我是段落</p>
1
console.log($('p').text());
1


3.2.1.2、html()
方法描述：设置/获取元素的html内容，该方法读写合一。

需求描述：设置ul列表标签的li列表项

<ul></ul>
1
var li = '<li>我是列表项</li>';
$('ul').html(li);
1
2


需求描述：获取ul列表中的列表项并输出

<ul><li>我是列表项</li></ul>
1
console.log($('ul').html());
1


3.2.1.3、append()
方法描述：向当前匹配的所有元素内部的最后面插入指定内容。

需求描述：为当前的ul向后添加一个列表项

<ul>
    <li>我是列表项1</li>
    <li>我是列表项2</li>
</ul>
1
2
3
4
var last = '<li>我是最后一个列表项</li>';
$('ul').append(last);
1
2


3.2.1.4、appendTo()
方法描述：将指定的内容追加到当前匹配的所有元素的最后面。

需求描述：为当前的ul向后添加一个列表项

<ul>
    <li>我是列表项1</li>
    <li>我是列表项2</li>
</ul>
1
2
3
4
var last = '<li>我是最后一个列表项</li>';
$(last).appendTo($('ul'));
1
2


3.2.1.5、prepend()
方法描述：向当前匹配的所有元素内部的最前面插入指定内容。

需求描述：为当前的ul向前添加一个列表项

<ul>
    <li>我是列表项1</li>
    <li>我是列表项2</li>
</ul>
1
2
3
4
var first = '<li>我是第一个列表项</li>';
$('ul').prepend(first);
1
2


3.2.1.6、prependTo()
方法描述：将指定的内容追加到当前匹配的所有元素的最前面。

需求描述：为当前的ul向前添加一个列表项

<ul>
    <li>我是列表项1</li>
    <li>我是列表项2</li>
</ul>
1
2
3
4
var first = '<li>我是第一个列表项</li>';
$(first).prependTo($('ul'));
1
2


3.2.2、DOM外部插入
3.2.2.1、after()
方法描述：在匹配元素集合中的每个元素后面插入参数所指定的内容，作为其兄弟节点。

需求描述：在div的后边插入一个段落

<div>我是div</div>
1
var after = '<p>我是段落</p>';
$('div').after(after);
1
2


3.2.2.2、before()
方法描述：在匹配元素集合中的每个元素前边插入参数所指定的内容，作为其兄弟节点。

需求描述：在div的前边插入一个段落

<div>我是div</div>
1
var before = '<p>我是段落</p>';
$('div').before(before);
1
2


3.2.2.3、insertAfter()
方法描述：.after()和.insertAfter() 实现同样的功能。主要的不同是语法，特别是插入内容和目标的位置。 对于 .after()，选择表达式在函数的前面，参数是将要插入的内容。对于 .insertAfter()，刚好相反，内容在方法前面，它将被放在参数里元素的后面。

需求描述：在div的后边插入一个段落

<div>我是div</div>
1
var content = '<p>我是段落</p>';
$(content).insertAfter($('div'));
1
2


3.2.2.4、insertBefore()
方法描述：.before()和.insertBefore()实现同样的功能。主要的不同是语法，特别是插入内容和目标的位置。 对于 .before()，选择表达式在函数前面，参数是将要插入的内容。对于 .insertBefore()，刚好相反，内容在方法前面，它将被放在参数里元素的前面。

需求描述：在div的前边插入一个段落

<div>我是div</div>
1
var content = '<p>我是段落</p>';
$(content).insertBefore($('div'));
1
2


3.2.3、DOM移除
3.2.3.1、empty()
方法描述：删除所有匹配元素的子元素。

需求描述：将ul列表下所有的子节点全部移除

<ul>
    <li>列表项1</li>
    <p>我是段落1</p>
    <li>列表项2</li>
    <p>我是段落2</p>
    <li>列表项3</li>
</ul>
1
2
3
4
5
6
7
$('ul').empty();
1


3.2.3.2、remove()
方法描述：删除所有匹配的元素。

注意：同时移除元素上的事件及 jQuery 数据

需求描述：将ul列表下所有的p子节点全部移除

<ul>
    <li>列表项1</li>
    <p>我是段落1</p>
    <li>列表项2</li>
    <p>我是段落2</p>
    <li>列表项3</li>
</ul>
1
2
3
4
5
6
7
$('ul>p').remove();
1


3.2.4、DOM替换
3.2.4.1、replaceWith()
方法介绍：用提供的内容替换集合中所有匹配的元素并且返回被删除元素的集合。

需求描述：将ul下的所有li替换为p标签

<ul>
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
</ul>
1
2
3
4
5
$('ul>li').replaceWith('<p>我是段落</p>');
1


3.2.4.2、replaceAll()
方法介绍：.replaceAll()和.replaceWith()功能类似，但是目标和源相反。

需求描述：将ul下的所有li替换为p标签

<ul>
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
</ul>
1
2
3
4
5
$('<p>我是段落</p>').replaceAll($('ul>li'));
1


3.2.5、DOM拷贝
3.2.5.1、clone()
方法描述：创建一个匹配的元素集合的深度拷贝副本。如果传入一个true，则表示是否会复制元素上的事件处理函数，从jQuery 1.4开始，元素数据也会被复制。

需求描述：为ul列表创建一个深克隆并追加到body后

<ul>
    <li>列表项1</li>
    <li>列表项2</li>
    <li>列表项3</li>
</ul>
1
2
3
4
5
var ul = $('#ul').clone();
$('body').append(ul);
1
2


3.2.6、DOM遍历
3.2.6.1、parent()
方法描述：获取集合中每个匹配元素的父元素，可以提供一个可选的选择器来进行筛选。

需求描述：输出id为two的li的父元素

<ul>
    <p>我是段落1</p>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
    <p>我是段落2</p>
</ul>
1
2
3
4
5
6
7
console.log($('#two').parent()[0]);
1


3.2.6.2、children()
方法描述：获取集合中每个匹配元素的子元素，可以提供一个可选的选择器来进行筛选。

需求描述：输出ul下的所有子元素

<ul>
    <p>我是段落1</p>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
    <p>我是段落2</p>
</ul>
1
2
3
4
5
6
7
var childrens = $('ul').children();
for (var i = 0; i < childrens.length; i++) {
    console.log(childrens[i]);
}
1
2
3
4


3.2.6.3、prev()
方法描述：获取集合中每个匹配元素紧邻的前一个兄弟元素，可以提供一个可选的选择器来进行筛选。

需求描述：获取id为two元素的前一个兄弟元素并输出

<ul>
    <p>我是段落1</p>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
    <p>我是段落2</p>
</ul>
1
2
3
4
5
6
7
console.log($('#two').prev()[0]);
1


3.2.6.4、prevAll()
方法描述：获得集合中每个匹配元素的所有前面的兄弟元素，可以提供一个可选的选择器来进行筛选。

需求描述：获取id为two元素的前边所有的兄弟元素并输出

<ul>
    <p>我是段落1</p>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
    <p>我是段落2</p>
</ul>
1
2
3
4
5
6
7
var prevs = $('#two').prevAll();
for (var i = 0; i < prevs.length; i++) {
    console.log(prevs[i]);
}
1
2
3
4


3.2.6.5、next()
方法描述：获取集合中每个匹配元素紧邻的后一个兄弟元素，可以提供一个可选的选择器来进行筛选。

需求描述：获取id为two元素的后一个兄弟元素并输出

<ul>
    <p>我是段落1</p>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
    <p>我是段落2</p>
</ul>
1
2
3
4
5
6
7
console.log($('#two').next()[0]);
1


3.2.6.6、nextAll()
方法描述：获得集合中每个匹配元素的所有后面的兄弟元素，可以提供一个可选的选择器来进行筛选。

需求描述：获取id为two元素的后边所有的兄弟元素并输出

<ul>
    <p>我是段落1</p>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
    <p>我是段落2</p>
</ul>
1
2
3
4
5
6
7
var nexts = $('#two').nextAll();
for (var i = 0; i < nexts.length; i++) {
    console.log(nexts[i]);
}
1
2
3
4


3.2.6.7、siblings()
方法描述：获得集合中每个匹配元素的兄弟元素，可以提供一个可选的选择器来进行筛选。

需求描述：获取id为two元素的所有兄弟元素并输出

<ul>
    <p>我是段落1</p>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
    <p>我是段落2</p>
</ul>
1
2
3
4
5
6
7
var siblings = $('#two').siblings();
for (var i = 0; i < siblings.length; i++) {
    console.log(siblings[i]);
}
1
2
3
4


3.3、遍历
3.3.1、遍历
3.3.1.1、each()
方法描述：遍历一个jQuery对象，为每个匹配元素执行一个函数。

需求描述：获取每一个li元素并把每一个li元素的标签及内容输出

<ul>
    <p>我是段落1</p>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
    <p>我是段落2</p>
</ul>
1
2
3
4
5
6
7
$('li').each(function (index, element) {
    console.log(index, element);
});
1
2
3


3.3.2、筛选
3.3.2.1、first()
方法描述：获取匹配元素集合中第一个元素。

需求描述：设置ul下第一个li的背景为红色

<ul>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
</ul>
1
2
3
4
5
$('ul>li').first().css('background', 'red');
1


3.3.2.2、last()
方法描述：获取匹配元素集合中最后一个元素。

需求描述：设置ul下最后一个li的背景为红色

<ul>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
</ul>
1
2
3
4
5
$('ul>li').last().css('background', 'red');
1


3.3.2.3、eq()
方法描述：获取匹配元素集合中指定位置索引的一个元素。

注意：索引下标从0开始

需求描述：设置ul下第二个li的背景为红色

<ul>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
</ul>
1
2
3
4
5
$('ul>li').eq(1).css('background', 'red');
1


3.3.2.4、not()
方法描述：从匹配的元素集合中移除指定的元素。

需求描述：设置ul下li标签的背景为红色，排除第二个li

<ul>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
</ul>
1
2
3
4
5
var two = $('ul>li').eq(1);
$('ul>li').not(two).css('background', 'red');
1
2


3.3.2.5、filter()
方法描述：筛选元素集合中匹配表达式或通过传递函数测试的元素集合。



需求描述：查找所有id为two的li标签设置其背景为红色

<ul>
    <li>列表项1</li>
    <li id="two">列表项2</li>
    <li>列表项3</li>
</ul>
1
2
3
4
5
$('ul>li').filter('[id=two]').css('background', 'red');
1


3.4、事件
3.4.1、浏览器事件
3.4.1.1、resize()
方法描述：为 JavaScript 的 “resize” 事件绑定一个处理函数，或者触发元素上的该事件。

需求描述：当浏览器窗口的尺寸改变时，控制台输出“浏览器尺寸改变了”

$(window).resize(function () {
    console.log('浏览器尺寸改变了');
});
1
2
3


3.4.1.2、scroll()
方法描述：为 JavaScript 的 “scroll” 事件绑定一个处理函数，或者触发元素上的该事件。

需求描述：当浏览器窗口的滚动条滚动时，控制台输出“浏览器滚动条改变了”

body {
    height: 2000px;
}
1
2
3
$(window).scroll(function () {
    console.log('浏览器滚动条改变了');
});
1
2
3


3.4.2、事件绑定
3.4.2.1、on()
方法描述：在选定的元素上绑定一个或多个事件处理函数。

需求描述：为按钮添加单击事件，当按钮单击的时候，向控制台输出“按钮被单击了”

<button>按钮</button>
1
$('button').on('click',function () {
    console.log('按钮被单击了');
});
1
2
3


3.4.2.2、off()
方法描述： 移除一个事件处理函数。

需求描述：为按钮添加单击事件，然后再解绑，这时候你在点击按钮看看是不是不会输出信息了

<button>按钮</button>
1
$('button').on('click',function () {
    console.log('按钮被单击了');
});

$('button').off('click');
1
2
3
4
5


3.4.3、事件委托
3.4.3.1、delegate()
方法描述：设置事件委托。

需求描述：为ul下的所有li添加单击事件，要求将该单击事件委托给ul，当单击li时，所对应的li背景变为红色

<ul>
    <li>1111</li>
    <li>2222</li>
    <li>3333</li>
    <li>4444</li>
</ul>
1
2
3
4
5
6
$('ul').delegate('li', 'click', function () {
    this.style.background = 'red';
});
1
2
3


3.4.3.2、undelegate()
方法描述：移除事件委托。

需求描述：要求移除上一节中设置的事件委托，然后在分别点击li进行验证是否移除事件委托

<ul>
    <li>1111</li>
    <li>2222</li>
    <li>3333</li>
    <li>4444</li>
</ul>
1
2
3
4
5
6
// 设置事件委托
$('ul').delegate('li', 'click', function () {
    this.style.background = 'red';
});

// 移除事件委托
$('ul').undelegate('click');
1
2
3
4
5
6
7


3.4.4、事件对象
对象属性名称	对象属性描述
event.currentTarget	当前执行的DOM元素。
event.target	触发事件的DOM元素。
event.pageX	相对于页面的左上角。
event.pageY	相对于页面的顶部。
event.clientX	相对于视口的左上角。
event.clientY	相对于视口的顶部。
event.offsetX	相对于事件元素左上角。
event.offsetY	相对于事件元素顶部。
event.key	键盘的按键信息。
event.preventDefault()	阻止事件默认行为。
event.stopPropagation()	防止事件向外冒泡。
3.4.5、表单事件
3.4.5.1、focus()
方法描述：当获取焦点时触发所绑定的函数。

需求描述：当文本框获取焦点时，设置其背景为红色

<input type="text">
1
$(':text').focus(function () {
    $(this).css('background', 'red');
});
1
2
3


3.4.5.2、blur()
方法描述：当失去焦点时触发所绑定的函数。

需求描述：当文本框获取焦点时，设置其背景为红色，当文本框失去焦点时，设置其背景为绿色

<input type="text">
1
$(':text').focus(function () {
    $(this).css('background', 'red');
});
$(':text').blur(function () {
    $(this).css('background', 'green');
});
1
2
3
4
5
6


3.4.5.3、change()
方法描述：当内容改变时触发所绑定的函数。

需求描述：当文本框内容改变时，就向控制台输出当前文本框的内容

<input type="text">
1
$(':text').change(function () {
    console.log($(this).val());
});
1
2
3


需求描述：当选择框的内容改变时，就向控制台输出当前选中项的内容

<select>
    <option>河北</option>
    <option>河南</option>
    <option>上海</option>
    <option>北京</option>
    <option>广东</option>
</select>
1
2
3
4
5
6
7
$('select').change(function () {
    console.log($(this).val());
});
1
2
3


3.4.5.4、select()
方法描述：当内容选择时触发所绑定的函数。

需求描述：当文本框的内容被选择时，就向控制台输出当前文本框的内容

<input type="text" value="123456">
1
$('input').select(function () {
    console.log($(this).val());
});
1
2
3


3.4.5.5、submit()
方法描述：当表单提交时触发所绑定的函数。

需求描述：当表单提交的时候，弹出对话框“表单提交了”

<form action="#">
    <input type="submit">
</form>
1
2
3
$('form').submit(function () {
    alert('表单提交了');
});
1
2
3


3.4.6、鼠标事件
3.4.6.1、click()
方法描述：当鼠标单击时调用所绑定的函数。

需求描述：为按钮绑定一个单击函数，然后点击按钮，在控制台输出“按钮被单击了”

<button>按钮</button>
1
$('button').click(function () {
    console.log('按钮被单击了');
});
1
2
3


3.4.6.2、dblclick()
方法描述：当鼠标双击时调用所绑定的函数。

需求描述：为按钮绑定一个双击函数，然后双击按钮，在控制台输出“按钮被单击了”

<button>按钮</button>
1
$('button').dblclick(function () {
    console.log('按钮被双击了');
});
1
2
3


3.4.6.3、mousedown()
方法描述：当鼠标左键按下的时候调用所绑定的函数。

需求描述：当鼠标左键按下的时候，控制台输出“鼠标左键按下”

<button>按钮</button>
1
$('button').mousedown(function () {
    console.log('鼠标左键按下');
});
1
2
3


3.4.6.4、mouseup()
方法描述：当鼠标左键松开的时候调用所绑定的函数。

需求描述：当鼠标左键松开的时候，控制台输出“鼠标左键松开”

<button>按钮</button>
1
$('button').mouseup(function () {
    console.log('鼠标左键松开');
});
1
2
3


3.4.6.5、mouseenter()
方法描述：当鼠标进入目标元素的时候调用所绑定的函数。

需求描述：创建两个div，当鼠标移到外层div的时候，向控制台输出“mouse enter”

.outer {
    width: 200px;
    height: 200px;
    background: coral;
}

.inner {
    width: 100px;
    height: 100px;
    background: pink;
}
1
2
3
4
5
6
7
8
9
10
11
<div class="outer">
    <div class="inner"></div>
</div>
1
2
3
$('.outer').mouseenter(function () {
    console.log('mouse enter');
});
1
2
3


3.4.6.6、mouseleave()
方法描述：当鼠标离开目标元素的时候调用所绑定的函数。

需求描述：创建两个div，当鼠标移出外层div的时候，向控制台输出“mouse leave”

.outer {
    width: 200px;
    height: 200px;
    background: coral;
}

.inner {
    width: 100px;
    height: 100px;
    background: pink;
}
1
2
3
4
5
6
7
8
9
10
11
<div class="outer">
    <div class="inner"></div>
</div>
1
2
3
$('.outer').mouseleave(function () {
    console.log('mouse leave');
});
1
2
3


3.4.6.7、mouseover()
方法描述：当鼠标进入目标元素的时候调用所绑定的函数。

注意：mouseenter事件和mouseover的不同之处是事件的冒泡的方式。mouseenter 事件只会在绑定它的元素上被调用，而不会在后代节点上被触发。

需求描述：创建两个div，当鼠标移到外层div的时候，向控制台输出“mouse over”，鼠标移到内层div的时候，向控制台输出“mouse over”，当鼠标移到外层div的时候，向控制台输出“mouse over”

.outer {
    width: 200px;
    height: 200px;
    background: coral;
}

.inner {
    width: 100px;
    height: 100px;
    background: pink;
}
1
2
3
4
5
6
7
8
9
10
11
<div class="outer">
    <div class="inner"></div>
</div>
1
2
3
$('.outer').mouseover(function () {
    console.log('mouse over');
});
1
2
3


3.4.6.8、mouseout()
方法描述：当鼠标离开目标元素的时候调用所绑定的函数。

注意：mouseleave事件和mouseout的不同之处是事件的冒泡的方式。mouseleave 事件只会在绑定它的元素上被调用，而不会在后代节点上被触发。

需求描述：创建两个div，当鼠标移出外层div的时候，向控制台输出“mouse out”

.outer {
    width: 200px;
    height: 200px;
    background: coral;
}

.inner {
    width: 100px;
    height: 100px;
    background: pink;
}
1
2
3
4
5
6
7
8
9
10
11
<div class="outer">
    <div class="inner"></div>
</div>
1
2
3
$('.outer').mouseout(function () {
    console.log('mouse out');
});
1
2
3


3.4.6.9、mousemove()
方法描述：当鼠标指针在元素内移动时，mousemove事件就会被触发，任何HTML元素都可以接受此事件。

需求描述：鼠标在div内移动，获取当前鼠标相对div的位置坐标

.outer {
    width: 200px;
    height: 200px;
    background: black;
    position: absolute;
    left: 20px;
    top: 20px;
}
1
2
3
4
5
6
7
8
<div class="outer"></div>
1
$('.outer').mousemove(function (event) {
    console.log(event.offsetX, event.offsetY);
});
1
2
3


3.4.6.10、hover()
方法描述：.hover()方法是同时绑定 mouseenter和 mouseleave事件。

需求描述：当鼠标进入div设置背景为红色，当鼠标移出div设置背景为绿色，默认背景为黑色

.outer {
    width: 200px;
    height: 200px;
    background: black;
}
1
2
3
4
5
<div class="outer"></div>
1
$('.outer').hover(function () {
    $(this).css('background', 'red');
}, function () {
    $(this).css('background', 'green');
});
1
2
3
4
5


3.4.7、键盘事件
3.4.7.1、keydown()
方法描述：当键盘按键按下的时候调用绑定的函数。

需求描述：当键盘按键按下的时候，输出当前的按键

<input type="text">
1
$(':text').keydown(function (event) {
    console.log(event.key);
});
1
2
3


3.4.7.2、keyup()
方法描述：当键盘按键松开的时候调用绑定的函数。

需求描述：当键盘按键松开的时候，输出当前的按键

<input type="text">
1
$(':text').keyup(function (event) {
    console.log(event.key);
});
1
2
3


3.4.7.3、keypress()
方法描述：keypress与keydown类似，当键盘按键按下的时候调用绑定的函数。

需求描述：当键盘按键按下的时候，输出当前的按键

<input type="text">
1
$(':text').keypress(function (event) {
    console.log(event.key);
});
1
2
3


3.5、动画
3.5.1、基础
3.5.1.1、hide()
方法描述：隐藏元素。

需求描述：创建一个显示div，然后隐藏该元素

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: block;
}
1
2
3
4
5
6
<div class="box"></div>
1
$('.box').hide();
1


3.5.1.2、show()
方法描述：显示元素。

需求描述：创建一个隐藏div，然后显示该元素

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: none;
}
1
2
3
4
5
6
<div class="box"></div>
1
$('.box').show();
1


3.5.1.3、toggle()
方法描述：如果隐藏就设置为显示，如果显示就设置为隐藏。

需求描述：创建一个按钮，控制div显示和隐藏

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: block;
}
1
2
3
4
5
6
<button>隐藏/显示</button>
<div class="box"></div>
1
2
$('button').click(function () {
    $('.box').toggle();
});
1
2
3


3.5.2、渐变
3.5.2.1、fadeIn()
方法描述：通过淡入的方式显示匹配元素。

需求描述：创建一个按钮，控制div缓慢淡入

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: none;
}
1
2
3
4
5
6
<button>淡入</button>
<div class="box"></div>
1
2
$('button').click(function () {
    $('.box').fadeIn('slow');
});
1
2
3


3.5.2.2、fadeOut()
方法描述：通过淡出的方式隐藏匹配元素。

需求描述：创建一个按钮，控制div缓慢淡出

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: block;
}
1
2
3
4
5
6
<button>淡出</button>
<div class="box"></div>
1
2
$('button').click(function () {
    $('.box').fadeOut('slow');
});
1
2
3


3.5.2.3、fadeToggle()
方法描述：用淡入淡出动画显示或隐藏一个匹配元素。

需求描述：创建一个按钮，控制div淡入和淡出

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: block;
}
1
2
3
4
5
6
<button>淡出/淡入</button>
<div class="box"></div>
1
2
$('button').click(function () {
    $('.box').fadeToggle('slow');
});
1
2
3


3.5.3、滑动
3.5.3.1、slideDown()
方法描述：用滑动动画显示一个匹配元素。

需求描述：创建一个按钮，控制div滑动显示

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: none;
}
1
2
3
4
5
6
<button>滑动显示</button>
<div class="box"></div>
1
2
$('button').click(function () {
    $('.box').slideDown();
});
1
2
3


3.5.3.2、slideUp()
方法描述：用滑动动画隐藏一个匹配元素。

需求描述：创建一个按钮，控制div滑动隐藏

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: block;
}
1
2
3
4
5
6
<button>滑动隐藏</button>
<div class="box"></div>
1
2
$('button').click(function () {
    $('.box').slideUp();
});
1
2
3


3.5.3.3、slideToggle()
方法描述：用滑动动画显示或隐藏一个匹配元素。

需求描述：创建一个按钮，控制div滑动显示和滑动隐藏

.box {
    width: 200px;
    height: 200px;
    background: coral;
    display: block;
}
1
2
3
4
5
6
<button>滑动隐藏/滑动显示</button>
<div class="box"></div>
1
2
$('button').click(function () {
    $('.box').slideToggle();
});
1
2
3


3.5.4、自定义
3.5.4.1、animate()
方法描述：根据一组 CSS 属性，执行自定义动画，并且支持链式调用。

需求描述：创建一个div，默认div宽高100px，背景颜色为黑色，先让div宽度变为200px，再让div高度变为200px

.box {
    width: 100px;
    height: 100px;
    background: black;
}
1
2
3
4
5
<button>自定义动画</button>
<div class="box"></div>
1
2
$('.box')
.animate({
    width: '200'
})
.animate({
    height: '200',
});
1
2
3
4
5
6
7


3.5.4.2、stop()
方法描述：停止匹配元素当前正在运行的动画。