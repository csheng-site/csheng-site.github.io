---
title: Web APIs 笔记
date: 2022-10-14 13:06:00
categories: JavaScript
description: 摘录学习JavaScript过程中遇到的重点，以方便后面回来查阅。
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Web%20APIs.png
---
> Web APIs学习的是 {% label DOM（页面文档对象模型） pink %}和 {% label BOM（浏览器对象模型） pink %} ，主要学习{% label 页面交互功能 orange %}
> 因为 Web API 很多，所以我们将这个阶段称为 Web APIs

{% btn 'https://developer.mozilla.org/zh-CN/docs/Web/API',MDN详细API,far fa-hand-point-right,block orange center larger %}

# DOM
> 文档对象模型（Document Object Model，简称 DOM）
> 用来操作页面，主要操作页面元素

{% note success no-icon flat %}
`DOM对象`：
浏览器会将html结构生成dom树，同时生成对应的dom对象
我们的任务就获取这些dom对象， 则时进行相应的操作
{% endnote %}

## 获取元素
- 根据{% label ID pink %}获取
{% note success no-icon flat %}
document.getElementById('id');
{% endnote %}

- 根据{% label 标签名 pink %}获取
{% note success no-icon flat %}
document.getElementsByTagName('标签名');  // 返回带有指定标签名的对象集合，以伪数组的形式存储
element.getElementsByTagName('标签名');  // 父元素必须是指定的单个元素
// 如果获取不到元素,则返回为空的伪数组(因为获取不到对象)
{% endnote %}

- 通过{% label HTML5新增 pink %}的方法获取
{% note success no-icon flat %}
- document.getElementsByClassName('类名'); // 根据类名获得某些元素集合
- document.querySelector('选择器'); // 返回指定选择器的第一个元素对象
- document.querySelectorAll('选择器'); // 返回指定选择器的所有元素对象集合
  > `拓展：两个方法的差异`
  > querySelector只能取到一个，querySelectorAll可以获取满足条件的所有元素
  > querySelector返回一个dom元素，可以直接操作，querySelectorAll返回一个伪数组，需要遍历之后 再进行操作
  > querySelector如果没有获取到则返回null，querySelectorAll如果没有取到则返回一个空的伪数组
{% endnote %}

- {% label 特殊元素 pink %}获取
{% note success no-icon flat %}
- document.body; // 返回body元素对象
- document.documentElement; // 返回html元素对象
{% endnote %}

## 为元素设置内容
> 针对双标签而言
{% note success no-icon flat %}
- innerText：覆盖标签的原始内容，不会解析标签，只获取文本内容
- innerHTML：覆盖标签的原始内容，会解析标签，会获取完整的html结构
{% endnote %}

## 操作元素属性
> 语法：`元素.属性 = '值'`，只适用于内置属性
	
{% note success no-icon flat %}
- 常规属性操作：`img.src = '路径'`
- 操作行内样式：`元素.style.样式属性名 = '值'`
- 操作样式类名className：`元素.className = '样式类名'`
  > 特点：会覆盖元素之前的类样式
  > 使用场景：确定只有一个类名样式
- 操作样式列表classList
  - add：`元素.classList.add('样式1','样式2'.....)`
    > 为元素添加新样式
  - remove：`元素.classList.remove('样式1','样式2'.....)`
    > 为元素移除样式
  - toggle：`元素.classList.toggle('样式')`
    > 为元素切换样式（没有就添加，有就移除）
  - contains：`元素.classList.contains('样式')`
    > 判断元素是否拥有指定名称的样式（有就返回true，否则返回false）
- 表单元素的属性操作
  > checked, disabled, selected等
{% endnote %}

## 定时器
> 作用：每隔一段时间重复做某个事件
> setInterval() ：【set:设定+Interval:间隔】
> - let timeId = setInterval(function(),1000)
> - clearInterval(标识)

案例：倒计时
```html
		<input type="button" value="倒计时5秒钟" />

		<script>
			let count = 5;
			let btn = document.querySelector('input');

			btn.addEventListener('click', function () {
				this.disabled = true;
				let timerId = setInterval(() => {
					count--;
					this.value = `倒计时${count}秒钟`;
					if (count === 0) {
						count = 5;
						this.disabled = false;
						clearInterval(timerId);
						this.value = '倒计时5秒钟';
					}
				}, 1000);
			});
		</script>
```

## 轮播图
```html
		<div class="box">
			<img src="./images/b01.jpg" alt="" />
			<p>第1张图片</p>
		</div>

		<script>
			let index = 1;
			let img = document.querySelector('img');
			let p = document.querySelector('p');

			setInterval(function () {
				index++;
				if (index > 9) index = 1;
				img.src = `./images/b0${index}.jpg`;
				p.innerText = `第${index}张图片`;
			}, 1000);
		</script>
```

## 自定义属性操作
- 获取元素的属性值
  > element.getAttribute('属性')  
  > element.dataset.属性（H5新增）【注：自定义属性里面如果有多个-链接的单词，获取要采用驼峰命名法】
- 设置元素属性值
  > element.setAttribute('属性', '值');
- 移除属性
  > element.removeAttribute('index')

## tab栏切换
```css
			* {
				padding: 0;
				margin: 0;
			}
			li {
				list-style: none;
			}
			.tab {
				width: 978px;
				margin: 100px auto;
			}
			.tab_list {
				height: 39px;
				border: 1px solid #ccc;
				background-color: #f1f1f1;
			}
			.tab_list li {
				float: left;
				height: 39px;
				line-height: 39px;
				padding: 0 20px;
				cursor: pointer;
			}
			.tab_list .current {
				background-color: #c81623;
				color: #fff;
			}
			.tab_con .item {
				display: none;
			}
```
```html
		<div class="tab">
			<div class="tab_list">
				<ul>
					<li class="current">商品介绍</li>
					<li>规格与包装</li>
					<li>售后保障</li>
					<li>商品评价（50000）</li>
					<li>手机社区</li>
				</ul>
			</div>
			<div class="tab_con">
				<div class="item" style="display: block">商品介绍模块内容</div>
				<div class="item">规格与包装模块内容</div>
				<div class="item">售后保障模块内容</div>
				<div class="item">商品评价（50000）模块内容</div>
				<div class="item">手机社区模块内容</div>
			</div>
		</div>

		<script>
			let lis = document.querySelectorAll('.tab_list li');
			let items = document.querySelectorAll('.item');

			for (let i = 0; i < lis.length; i++) {
				lis[i].setAttribute('data-index', i); // 设置索引号
				lis[i].addEventListener('click', function () {
					for (let i = 0; i < lis.length; i++) {
						lis[i].className = '';
					}
					this.className = 'current';

					let index = this.getAttribute('data-index'); // 获取索引号
					for (let i = 0; i < items.length; i++) {
						items[i].style.display = 'none';
					}
					items[index].style.display = 'block';
				});
			}
		</script>
```

## 事件高级
### 常用的鼠标事件
| 鼠标事件 | 触发条件 |
| :-----| :----- |
| click | 单击鼠标左键 |
| contextmenu | 单击鼠标右键 |
| dblclick | 双击鼠标左键 |
| mouseenter | 鼠标移至元素范围内触发一次 |
| mouseleave | 鼠标移出元素范围外触发一次 |
| mouseover | 鼠标移至元素或其子元素内，可能触发多次 |
| mouseout | 鼠标移出元素，或者移至其子元素内，可能触发多次 |
| mouseup | 鼠标弹起 |
| mousedown | 鼠标按下 |
| focus | 获取鼠标焦点 |
| blur | 失去鼠标焦点 |

| 鼠标事件对象 | 说明 |
| :-----| :----- |
| e.clientX / e.clientY | 返回鼠标相对于浏览器窗口可视区的X/Y坐标 |
| e.pageX / e.pageY | 返回鼠标相对于文档页面的X/Y坐标 |
| e.screenX / e.screenY | 返回鼠标相对于电脑屏幕的X/Y坐标 |

### 常用的键盘事件
| 键盘事件 | 触发条件 |
| :-----| :----- |
| keyup | 某个键盘按键被松开时 |
| keydown | 某个键盘按键被按下时 |
| keypress | 某个键盘按键被按下时(不识别功能键) |

执行顺序：`keydown -> keypress -> keyup`

| 键盘事件对象 | 说明 |
| :-----| :----- |
| keyCode | 返回该键的ASCII值 |

## 节点操作
{% note success no-icon flat %}
```javascript
			// 父节点
			// 得到的是离元素最近的父级节点(亲爸爸) 如果找不到父节点就返回为 null
			console.log(div.parentNode); 

			// 子节点
			//（1）childNodes：获取所有的子节点（包含元素节点、文本节点等等）
			console.log(ul.childNodes); ☒ 了解即可
			//（2）children 获取所有的子元素节点 也是我们实际开发常用的
			console.log(ul.children); ☑ 推荐

			// 兄弟节点
			// nextElementSibling 得到下一个兄弟元素节点
			// previousElementSibling 得到上一个兄弟元素节点
			console.log(div.nextElementSibling);
			console.log(div.previousElementSibling);
```

```javascript
// 创建节点
let li = document.creatElement('li');

// 添加节点
ul.appendChild(li); // 插到父元素 ul 的最后
ul.insertBefore(li, ul.children[0]); // 将 li 插到 ul 第一个子元素的前面
// 如果位置参数找不到（null），insertBefore会默认完成appendCChild的功能

// 删除节点
ul.removeChild(ul.children[0]); // 通过父容器删除直接子元素
ele.parentNode.parentNode.remove(); // 删除元素本身

// 克隆节点
// node.cloneNode(): 括号为空或者里面是false 浅拷贝 只复制标签不复制里面的内容
// node.cloneNode(true): 括号为true 深拷贝 复制标签复制里面的内容
let li = ul.children[0].cloneNode(true);
```
{% endnote %}

## 事件捕获和事件冒泡
> 事件冒泡：当一个元素触发事件后，会依次向上调用所有父级元素的同名事件
> 事件捕获：从DOM的根元素开始去执行对应的事件 (从外到里)

```javascript
DOM.addEventListener('click',function(){},[是否使用捕获机制])
e.stopPropagation(); // 阻止事件传播(捕获或冒泡)
e.preventDefault(); // 阻止默认行为
```

# BOM
> **浏览器对象模型**（Browser Object Model，简称 BOM）

![BOM](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/BOM.png)

## window 对象
document

location
```javascript
location.href: 获取完整的 URL 地址，对其赋值时用于地址的跳转
location.search: 获取地址中携带的参数，符号 ？后面部分
location.hash: 获取地址中的沙希值，符号 # 后面部分
location.reload: 用来刷新当前页面，传入参数 true 时表示强制刷新
```

history
```javascript
history.back(): 后退
history.go(-1): 后退
history.go(1): 前进一步.
history.forward()
```

navgator

## 本地存储
![本地存储](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%9C%AC%E5%9C%B0%E5%AD%98%E5%82%A8.png)

```javascript
				// 添加本地存储 (只能存储字符串类型的数据)
				localStorage.setItem('字符串键', '字符串值');

				// 获取本地存储 (如果字符串键没有存在，返回null,如果存在就返回对应的值)
				let result = localStorage.getItem('字符串键');

				// 删除本地存储 (如果键不存在，则也不会报错)
				localStorage.removeItem('字符串键');

				// 清除浏览器中存储的所有本地存储
				localStorage.clear();
```
**JSON的转换**
```javascript
			set.addEventListener('click', function () {
				let obj = { name: 'jack', age: 20 };
				localStorage.setItem('userinfo', JSON.stringify(obj));
			});

			get.addEventListener('click', function () {
				let result = localStorage.getItem('userinfo');
				let obj = JSON.parse(result);
				console.log(obj, typeof obj);
			});
```