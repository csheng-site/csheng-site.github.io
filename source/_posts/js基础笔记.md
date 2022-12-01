---
title: js基础笔记
date: 2022-10-07 22:36:50
categories: JavaScript
description: 摘录学习JavaScript过程中遇到的重点，以方便后面回来查阅。
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/JavaScript.png
---
> JS基础学习的是 {% label ECMAScript pink %}（JavaScript语法）

# 变量
## 变量命名规范
{% note success no-icon %}
- 由`字母(A-Za-z)、数字(0-9)、下划线(_)、美元符号( $ )`组成
- 严格区分大小写
- 不能以数字开头
- 不能是关键字、保留字
- 变量名必须有意义 
- 驼峰命名法
{% endnote %}

## JS的数据类型
- 基本(简单)数据类型
  > - `Number`【NaN属于里面的一种】, 
  > - `String`：其中的模板字符串需要掌握 `${变量名}测试文本`(⭐)
  > - `Boolean`
  > - `Undefined`：未赋值（声明了变量但没有赋值，变量默认值就是undefined）
  > - `Null`：未定义（还没有创建对象，内存地址还没有分配）
- 引用(复杂)数据类型
  > - `函数funciton`
  > - `数组array`
  > - `对象object`

扩展：NaN 不会和任何一个值相等
```javascript
			console.log(NaN == NaN); // false
			console.log(NaN === NaN); // false
			console.log(NaN != NaN); // true
```

## 判断数据类型
### typeof() 和 isNaN()
- typeof 
```javascript
			let str = 'jack', num = 20, boolean = true, undefined = undefined, null = null, 
			arr = ['写代码', '调bug', '写文档'],fn = function () {},obj = {};

			console.log(typeof str); // string
			console.log(typeof num); // number
			console.log(typeof boolean); // boolean
			console.log(typeof undefined); // undefined
			console.log(typeof null); // object

			console.log(typeof arr); // object
			console.log(typeof fn); // function
			console.log(typeof obj); // object

			/*
				总结：typeof 可以判断除了 null 以外的基本数据类型
				     但只能判断对象类型中的 function，其它都为 object
			*/
```

- isNaN() ：用来判断非数字
```javascript
			// isNaN()会尝试把它转换为数值，比如字符串或布尔值，任何不能转换为数值的值都会导致这个函数返回 true
			console.log(isNaN(NaN)); // true
			console.log(isNaN(10)); // false，10 是数值
			console.log(isNaN('10')); // false，可以转换为数值10
			console.log(isNaN('blue')); // true，不可以转换为数值
			console.log(isNaN(true)); // false，可以转换为数值1
```


### 扩展
> isNaN 和 Number.isNaN 函数的区别？

```javascript
			// 函数 Number.isNaN 会首先判断传入参数是否为数字，
			// 如果是数字再继续判断是否为 NaN，不会进行数据类型的转换，这种方法对于 NaN 的判断更为准确。
			console.log(Number.isNaN(NaN)); //true
			console.log(Number.isNaN('10')); //false
			console.log(Number.isNaN(10)); //false
			console.log(Number.isNaN('blue')); //false
```
二者对比
```javascript
			// 对于字符串blue，
			// 本质上我们是想判断传入的参数是否为NaN，但是用 isNaN 函数，判断结果为真，
			// 所以说 isNaN函数判断 NaN 不够准确。
			console.log(Number.isNaN('blue')); //false
			console.log(isNaN('blue')); //true
```

## 变量类型转换
{% note success no-icon %}
- 字符型
  1. 变量.toString()
  2. String(变量)
  3. 变量 + '' (隐式转换)【推荐】
- 数字型
  1. parseInt(变量): 得到整数
  2. parseFloat(变量): 得到浮点数
  3. Number(变量)
  4. 变量 - * / 运算 (隐式转换)
```javascript
// - * / %: 会将两边的数据转换为数值
// +: 写在变量的前面会将变量转为数值【+号作为正号解析】
let num1 = prompt('请输入第一个数：');
let num2 = prompt('请输入第二个数：');
let sum = +num1 + +num2;
console.log(sum);
```
- 布尔型
  - Boolean(变量)
  > 转换 boolean 值为 false 的有： 0  '' "" null undefined NaN
{% endnote %}

# 运算符
## 算术运算符 
- +
- -
- *
- /
- % ： 求余数：5 % 3 = 2
  
## 赋值运算符 
```javascript
			age += 5; // 等价于 age = age + 5
			age -= 5; // 等价于 age = age - 5
			age *= 5; // 等价于 age = age * 5
			age /= 5; // 等价于 age = age / 5
			age %= 5; // 等价于 age = age % 5
```

## 一元运算符 
```javascript
			let age = 10;
			// ++放在变量的后面，是先使用这个变量，在下一次再使用这个变量时就已经加好了
			age++; //  age += 1
			age--; //  age -= 1
			// ++在变量的前面，先执行自增或自减，再使用这个变量
			++age;
			--age;
```

## 比较运算符 
>  \>、\<、\>=、\<=
>  ==   ===   !==


1.比较的时候，如果全部都是字符串，则转换为ASCII码；
  > '9'< 'z'

2.如果数值和字符串比较，会将比较的内容转换为number，但是字符是不能转换为number，得到的结果为`NaN`，而NaN不能和任意的数据进行比较，所以比较的结果都为false
  > 999 > 'a'

隐式转换
[你好](https://blog.csdn.net/weixin_47947930/article/details/106467804)

## 逻辑运算符
- `逻辑与（ && ）`:如果第一个表达式为假，直接返回false,如果第一个为真，才会去执行第二个,返回第二个的值
- `逻辑或（ || ）`:如果第一个为真，就返回第一个值，后面的不再执行，如果第一个表达式为假，那么就执行第二个表达式，如果第二个也为假，那么就执行第三个，如果没有第三个，就为false
- `逻辑非（ ! ）`

# 分支&循环
{% note success no-icon %}
- 单分支: if(){}
- 双分支: if(){ } else{ }
- 多分支: if(){ } else if{ } else{ }
- 三元表达式: 条件表达式 ？成立之后的操作 :  不成立之后的操作
{% endnote %}
{% note success no-icon %}
- if... else ...
- while
- switch
- for
{% endnote %}

## 九九乘法表	
```css
			span {
				display: inline-block;
				width: 100px;
				text-align: center;
				border: 1px solid #000;
				padding: 10px 0;
				margin: 2px;
			}
```
```javascript
			for (let row = 1; row <= 9; row++) {
				for (let col = 1; col < row + 1; col++) {
					document.write(`<span>${col} * ${row} = ${col * row}</span>`);
				}
				document.write('<br/>');
			}
```

## ATM案例
```javascript
			let choice = '';
			let total = 1000;
			while (choice !== '4') {
				choice = prompt(`请选择您的操作：\n 1.取款\n 2.存款\n 3.查看余额\n 4.退出`);
				if (choice === '1') {
					let qu = prompt('请输入取款金额');
					if (qu > 0 && qu <= total) {
						total -= qu;
					} else {
						alert('输入的金额不合理');
					}
				} else if (choice === '2') {
					let cun = +prompt('请输入存款金额');
					if (cun > 0) {
						total += cun;
					} else {
						alert('输入的金额不合理');
					}
				} else if (choice === '3') {
					alert(`余额为 ${total}`);
				} else if (choice === '4') {
					alert('退出');
				} else {
					alert('输入不正确');
				}
			}
```
 
# 数组 
```javascript
			// 1. 利用 new 创建数组
			var arr = new Array();
			// 2. 利用数组字面量创建数组
			var arr = [];
```
## 筛选数组
{% tabs filter_array %}
<!-- tab 方法① -->
```javascript
			var arr = [2, 0, 6, 1, 77, 0, 52, 0, 25, 7];
			var newArr = [];
			var j = 0;                                            

			for (var i = 0; i < arr.length; i++) {
				if (arr[i] >= 10) {
					newArr[j] = arr[i];
					j++;
				}
			}

			console.log(newArr);
```
<!-- endtab -->

<!-- tab 方法② (推荐) -->
```javascript
			var arr = [2, 0, 6, 1, 77, 0, 52, 0, 25, 7];
			var newArr = [];

			for (var i = 0; i < arr.length; i++) {
				if (arr[i] >= 10) {
					newArr[newArr.length] = arr[i];
				}
			}

			console.log(newArr);
```
<!-- endtab -->
{% endtabs %}

## 数组去重

## 翻转数组
```javascript
			var arr = ['red', 'green', 'blue', 'pink', 'purple', 'hotpink'];
			var newArr = [];

			for (var i = arr.length - 1; i >= 0; i--) {
				newArr[newArr.length] = arr[i];
			}

			console.log(newArr);
```

## 冒泡排序(⭐)
```javascript
			var arr = [4, 1, 2, 3, 5];

			for (var i = 0; i <= arr.length - 1; i++) {
				// 外循环负责趟数
				for (var j = 0; j <= arr.length - i - 1; j++) {
					// 内循环负责每一趟的交换次数
					if (arr[j] < arr[j + 1]) {
						var temp = arr[j];
						arr[j] = arr[j + 1];
						arr[j + 1] = temp;
					}
				}
			}

			console.log(arr);
```
## 【案例】根据数据生成柱状图
```css
			* {
				padding: 0;
				margin: 0;
			}
			.box {
				width: 800px;
				height: 500px;
				margin: 100px auto;
				border-left: 1px pink solid;
				border-bottom: 1px pink solid;
				display: flex;
				justify-content: space-around;
				align-items: flex-end;
			}
			.box > div {
				width: 50px;

				background-color: pink;
				position: relative;
			}
			.box > div > p {
				left: 0;
				top: -20px;
				position: absolute;
			}
			.box > div > span {
				width: 100px;
				left: 0;
				bottom: -20px;
				position: absolute;
			}
```
```javascript
			let datas = [];
			for (let i = 0; i < 4; i++) {
				datas.push(prompt(`请输入第${i + 1}个季度的值`));
			}

			let htmlStr = `<div class="box">`;
			// 遍历拼接生成div结构
			for (let i = 0; i < datas.length; i++) {
				htmlStr += `
				<div style='height:${datas[i]}px'>
					<p>${datas[i]}</p>
					<span>第${i + 1}季度</span>
                </div>`;
			}
			htmlStr += `</div>`;
			document.write(htmlStr);
```

---

# 函数
> 含义：封装了一段可以{% label 被重复执行调用 %}的 {% label 代码块 pink %}
> 目的：**让大量代码重复使用**

```javascript
			// 2 种声明方式

			// 1. 利用函数关键字自定义函数(命名函数)
			function fn() {}
			fn();

			// 2. 函数表达式(匿名函数)
			// var 变量名 = function() {};
			var fun = function (aru) {
				console.log('我是函数表达式');
				console.log(aru);
			};
			fun('pink老师');

			// (1) fun是变量名 不是函数名
			// (2) 函数表达式声明方式跟声明变量差不多，只不过变量里面存的是值 而 函数表达式里面存的是函数
			// (3) 函数表达式也可以进行传递参数
```

## arguments
```javascript
			// 只有函数才有 arguments 对象  而且是每个函数都内置好了这个arguments
			function fn() {
				// console.log(arguments); // 里面存储了所有传递过来的实参  arguments = [1,2,3]
				// console.log(arguments.length);
				// console.log(arguments[2]);
				// 我们可以按照数组的方式遍历arguments
				for (var i = 0; i < arguments.length; i++) {
					console.log(arguments[i]);
				}
			}
			fn(1, 2, 3);
			fn(1, 2, 3, 4, 5);
			// 伪数组 并不是真正意义上的数组
			// 1. 具有数组的 length 属性
			// 2. 按照索引的方式进行存储的
			// 3. 它没有真正数组的一些方法 pop()  push() 等等
```
应用arguments**求任意个数的最大值**
```javascript
			function getMax() {
				var max = arguments[0];
				for (var i = 1; i < arguments.length; i++) {
					if (arguments[i] > max) {
						max = arguments[i];
					}
				}
				return max;
			}
			console.log(getMax(1, 6, 3, 4, 5));
```

## 函数案例
### 翻转数组
```javascript
			function reverse(arr) {
				var newArr = [];
				for (var i = arr.length - 1; i >= 0; i--) {
					newArr[newArr.length] = arr[i];
				}
				return newArr;
			}
			var arr = reverse([1, 2, 3, 4, 5]);
			console.log(arr);
```

### 冒泡排序(⭐)
```javascript
			function sort(arr) {
				for (var i = 0; i < arr.length - 1; i++) {
					for (var j = 0; j < arr.length - i - 1; j++) {
						if (arr[j] < arr[j + 1]) {
							var temp = arr[j];
							arr[j] = arr[j + 1];
							arr[j + 1] = temp;
						}
					}
				}
				return arr;
			}

			var arr = sort([1, 2, 4, 5]);
			console.log(arr);
```

### 判断闰年
```javascript
			function isRunYear(year) {
				// 如果是闰年我们返回 true  否则 返回 false
				var flag = false;
				if ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0) {
					flag = true;
				}
				return flag;
			}
			console.log(isRunYear(2000));
			console.log(isRunYear(1999));
```

### 输出2月份天数
```javascript
			function backDay() {
				var year = prompt('请您输入年份：');
				if (isRunYear(year)) {
					alert('当前年份是闰年，二月份有29天');
				} else {
					alert('当前年份是平年，二月份有28天');
				}
			}
			backDay();

			function isRunYear(year) {
				var flag = false;
				if ((year % 4 == 0 && year % 100 != 0) || year % 400 == 0) {
					flag = true;
				}
				return flag;
			}
```

# 作用域
> 含义：代码名字（变量）在某个范围内起作用和效果 
> 目的：为了提高程序的可靠性更重要的是减少命名冲突

---

# 对象
{% note success no-icon %}
创建对象的3种方法
1. 利用【对象字面量】创建对象
```javascript
let obj = { uname: '张三疯', age: 18, sex: '男' };
```
2. 利用【new Object】创建对象
```javascript
let obj = new Object();
	obj.uname = '张三疯';
	obj.age = 18;
	obj.sex = '男';
```
3. 利用【构造函数】创建对象
```javascript
function Star(uname, age, sex) {
	this.name = uname;
	this.age = age;
	this.sex = sex;
	this.sing = function (sang) {
		console.log(sang);
	};
}
var ldh = new Star('刘德华', 18, '男'); // 调用函数返回的是一个对象
console.log(ldh.name);
console.log(ldh['sex']);
ldh.sing('冰雨');
```
遍历对象
```javascript
			for (var k in obj) {
				console.log(k); // 属性名
				console.log(obj[k]); // 属性值
			}
```
{% endnote %}

## Math对象
{% note success no-icon %}
- Math.PI: 圆周率
- Math.max(): 求最大值
- Math.min(): 求最小值
- Math.abs(): 求绝对值
- Math.floor(): 向下取整
- Math.ceil(): 向上取整
- Math.round(): 四舍五入【注：例：-1.5四舍五入，往大的取，=-1】
- Math.pow(r,N): 获取指定数r的N次幂
- Math.random() 
```javascript
			// random(): 返回一个随机的小数 0<=x<1
			console.log(Math.random()); // 比如 0.5804600372622006

			// 获取两个数之间的随机整数（包括2个整数）
			function getRandom(min, max) {
				return Math.floor(Math.random() * (max - min + 1)) + min;
			}
```

{% btn 'https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math',MDN详细Math对象,far fa-hand-point-right,block orange center larger %}
{% endnote %}

### 案例：猜数游戏
```javascript
let rdmNum = Math.floor(Math.random() * 10) + 1;
let iptNum;

while (1) {
	iptNum = +prompt('请输入一个数字');
	if (iptNum > rdmNum) {
		alert('数字猜大了,继续猜');
	} else if (iptNum < rdmNum) {
		alert('数字猜小了,继续猜');
	} else if (iptNum == rdmNum) {
		alert('恭喜你，猜对了！');
		break;
	} else {
		alert('请输入数字');
	}
}
```

## 日期对象
{% note success no-icon %}
Date 对象是一个构造函数，所以我们需要{% label 实例化 pink %}后才能使用。如果括号里面有时间，就返回参数里面的时间。
```javascript
			let date = new Date(); // Tue Oct 11 2022 22:28:00 GMT+0800 (中国标准时间)，当前时间
			let date1 = new Date(2022, 10, 1); // Fri Nov 01 2022 00:00:00 GMT+0800 (中国标准时间) (月份大了一个月)
			let date2 = new Date('2022-10-1'); // 这里是10月，字符串是我们经常用的
			let date3 = new Date('2022/10/1'); 
			let date4 = new Date('2022-10-1 8:8:8'); 
```

### 案例：输出当前时间
```javascript
let date = new Date();

// 年月日
let year = date.getFullYear();
let month = date.getMonth() + 1;
let dates = date.getDate();
let day = date.getDay();
let dayArr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];

// 时分秒
let h = date.getHours();
h = h < 10 ? '0' + h : h;
let m = date.getMinutes();
m = m < 10 ? '0' + m : m;
let s = date.getSeconds();
s = s < 10 ? '0' + s : s;

// 今天是2022年10月16日 星期日 17:56:05
document.write(`今天是${year}年${month}月${dates}日 ${dayArr[day]} ${h}:${m}:${s}`);
```

### 案例：获取刷新时间
```javascript
			function getTime() {
				let date = new Date();

				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();
				let hour = date.getHours();
				let minute = date.getMinutes();
				let second = date.getSeconds();

				return `${year}-${month}-${day} ${hour}:${minute}:${second}`;
			}

			let h1 = document.querySelector('h1');
			h1.innerHTML = getTime();
			setInterval(function () {
				h1.innerHTML = getTime();
			}, 1000);
```

获取日期的总的毫秒数(时间戳)
```javascript
			let date = new Date();
			
			// 方法1.通过 valueOf() 或 getTime()
			console.log(date.valueOf());
			console.log(date.getTime());

			// 方法2.简单且最常用的写法
			let date1 = +new Date();
			console.log(date1);

			// 方法3.H5新增的，有兼容性问题
			console.log(Date.now());
```

### 案例：倒计时
```javascript
			function countDown(time) {
				let nowTime = +new Date();
				let inputTime = +new Date(time);
				let times = (inputTime - nowTime) / 1000; // 总秒数
				
				// 1天=24小时，1小时=60分钟 1分钟=60秒
				let d = parseInt(times / 60 / 60 / 24);
				d = d < 10 ? '0' + d : d;
				let h = parseInt((times / 60 / 60) % 24);
				h = h < 10 ? '0' + h : h;
				let m = parseInt((times / 60) % 60);
				m = m < 10 ? '0' + m : m;
				let s = parseInt(times % 60);
				s = s < 10 ? '0' + s : s;

				return `${d}天${h}时${m}分${s}秒`;
			}
			console.log(countDown('2022-12-12 11:11:11'));
```
{% endnote %}

## 数组对象
### 创建数组对象
{% note success no-icon %}
- 字面量方式
```javascript
			let arr = [1, 2, 3];
```
- new Array()
```javascript
			let arr1 = new Array(); // 创建了一个空的数组
			let arr2 = new Array(2); // 这个2 表示 数组的长度为 2  里面有2个空的数组元素
			let arr3 = new Array(2, 3); // 等价于 [2,3]  这样写表示 里面有2个数组元素 是 2和3
```
{% endnote %}

### 检查是否为数组
{% note success no-icon %}
- {% label instanceof pink %}：判断一个对象是否属于某种类型
```javascript
			let arr = [];
			console.log(arr instanceof Array);
```
- {% label Array.isArray(参数) pink %}：判断一个对象是否为数组，H5新增的方法，ie9以上版本支持
```javascript
			let arr = [];
			console.log(Array.isArray(arr));
```
{% endnote %}
### 添加/删除数组元素
```javascript
let arr = [1, 2, 3];
// 1.push()：在数组的末尾添加一个或多个数组元素, 返回值是新数组的长度，原数组也会发生变化
arr.push(4);
// 2.unshift()：在数组的前面添加一个或多个数组元素, 返回值是新数组的长度，原数组也会发生变化
arr.unshift(4);
// 3.pop()：删除数组最后一个元素，一次只能删除一个元素，没有参数，返回值是删除的元素，原数组也会发生变化
arr.pop();
// 4.shift()：删除数组第一个元素，一次只能删除一个元素，没有参数，返回值是删除的元素，原数组也会发生变化
arr.shift();
// 5.splice(起始索引,数量)：可同时实现pop和shift的功能
arr.splice();
// 6.slice(起始索引,数量)：可同时实现pop和shift的功能
arr.slice();
```
{% note info simple %}
拓展：splice()和slice()的区别？
答：slice(): 不影响原数组，会返回所选择的元素。
   splice(): 会影响原数组，返回被删除的元素。
{% endnote %}

### 数组排序
```javascript
			// 1.翻转数组
			// reverse():颠倒数组中元素的顺序，无参数，会修改原数组，返回新数组
			let arr = ['pink', 'red', 'blue'];
			arr.reverse();
			console.log(arr);

			// 2.数组排序（冒泡排序）
			// sort():对数组的元素进行排序，会修改原数组，返回新数组
			let arr1 = [13, 4, 77, 1, 7];
			arr1.sort(function (a, b) {
				// return a - b; // 升序
				return b - a; // 降序
			});
			console.log(arr1);
```	
### 数组索引方法
{% note success no-icon %}
```javascript
			// indexOf(): 数组中查找给定元素的第一个索引，如果存在返回索引号，如果不存在返回-1
			let arr = ['red', 'green', 'blue', 'pink', 'blue'];
			console.log(arr.indexOf('blue')); // 2
			console.log(arr.indexOf('yellow')); // 不存在返回-1
			// lastIndexOf(): 在数组中的最后一个的索引，如果存在返回索引号，如果不存在返回-1
			console.log(arr.lastIndexOf('blue')); // 4
```
案例：数组去重
```javascript
			function unique(arr) {
				let newArr = [];
				for (let i = 0; i < arr.length; i++) {
					if (newArr.indexOf(arr[i]) === -1) {
						newArr.push(arr[i]);
					}
				}
				return newArr;
			}

			console.log(unique([1, 3, 2, 2, 4, 4, 2]));
```
{% endnote %}
### 数组转换为字符串
```javascript
			// 1. toString(): 把数组转换为字符串，逗号分隔每一项
			let arr = [1, 2, 3];
			console.log(arr.toString()); // 1,2,3

			// 2. join('分隔符'): 把数组所有的元素转换为一个字符串
			let arr1 = ['green', 'blue', 'pink'];
			console.log(arr1.join()); // green,blue,pink
			console.log(arr1.join('-')); // green-blue-pink
```

## 字符串对象
> 字符串的{% label 不可变 pink %}: 
> 指的是里面的值不可变，虽然看上去可以改变内容，但其实是地址变了，内存中新开辟了一个内存空间。
### 根据字符返回位置
```javascript
			// 根据字符返回位置  str.indexOf('要查找的字符', [起始的位置])
			let str = '改革春风吹满地，春天来了';
			console.log(str.indexOf('春'));
			console.log(str.indexOf('春', 3)); // 从索引号是 3的位置开始往后查找
```
案例：{% label 查找字符串中每个字符出现的次数 %}: 
```javascript
let str = 'sadasdasdasdasdassdfd';
let obj = {}; // {s:1,a:2,d:3}

for (let i = 0; i < str.length; i++) {
	let key = str[i];

	if (obj[key]) {
		obj[key]++;
	} else {
		obj[key] = 1;
	}
}

for (let key in obj) {
	console.log(`${key}出现的次数为${obj[key]}`);
}
```
### 根据位置返回字符
```javascript
			let str = 'andy';

			// 1. charAt(index): 根据位置返回字符
			console.log(str.charAt(3)); // 'd'
			// 遍历所有的字符
			for (let i = 0; i < str.length; i++) {
				console.log(str.charAt(i)); // a n d y
			}
			
			// 2. charCodeAt(index): 返回相应索引号的字符ASCII值 目的： 判断用户按下了那个键
			console.log(str.charCodeAt(0)); // 97

			// 3. str[index]: H5 新增的
			console.log(str[0]); // a
```
案例：{% label 统计出现最多的字符和次数 %}: 
```javascript
			let str = 'abcoefoxyozzopp';
			let o = {};
			for (let i = 0; i < str.length; i++) {
				let chars = str.charAt(i); // 取出单个字符
				if (o[chars]) {
					o[chars]++;
				} else {
					o[chars] = 1;
				}
			}

			console.log(o); // {a: 1, b: 1, c: 1, o: 4, e: 1, …}

			// 遍历对象，筛选出现次数最多的字符
			let max = 0;
			let ch = '';
			for (let k in o) {
				if (o[k] > max) {
					max = o[k];
					ch = k;
				}
			}
			console.log(max);
			console.log(`最多的字符是${ch}`);
```
### 字符串操作方法
{% note success no-icon %}
```javascript
			let str = 'csheng';

			// 1.concat(str1,[str2],[str3]): 拼接字符串，等效于+，+更常用
			console.log(str.concat('isBoy'));

			// 2.substr('截取的起始位置','截取几个字符')
			console.log(str.substr(2));
			console.log(str.substr(2, 3));

			// 3.replace('被替换的字符', '替换为的字符')  它只会替换第一个字符
			console.log(str.replace('c', 'f')); // fshengcsheng
			// 有一个字符串 'abcoefoxyozzopp'  要求把里面所有的 o 替换为 *
			let str1 = 'abcoefoxyozzopp';
			while (str1.indexOf('o') !== -1) {
				str1 = str1.replace('o', '*');
			}
			console.log(str1);

			// 4.split('分隔符'): 字符转换为数组（前面我们学过 join 把数组转换为字符串）
			var str2 = 'red, pink, blue';
			console.log(str2.split(','));

			// 5.转换大写
			console.log(str.toUpperCase()); // CSHENG
			// 6.转换小写
			console.log(str.toLowerCase()); // csheng
```
案例：给定一个字符串，如：“abaasdffggghhjjkkgfddsssss3444343”，问题如下：

{% endnote %}

# 简单类型与复杂类型(拓展)
简单类型
![简单类型](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E7%AE%80%E5%8D%95%E7%B1%BB%E5%9E%8B.png)
复杂类型
![复杂类型](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%A4%8D%E6%9D%82%E7%B1%BB%E5%9E%8B%EF%BC%88%E4%B8%80%EF%BC%89.png)
![复杂类型](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%A4%8D%E6%9D%82%E7%B1%BB%E5%9E%8B%EF%BC%88%E4%BA%8C%EF%BC%89.png)

