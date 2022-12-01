---
title: js高阶笔记
date: 2022-10-27 01:07:04
categories: JavaScript
description: 原型，闭包，创建模式与继承，线程等等
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/js%E9%AB%98%E7%BA%A7.png
---

# 作用域
## 局部作用域
> 分为 {% label 函数作用域 pink %} 和 {% label 块作用域 pink %}

1. 函数作用域
> 1. 函数内部声明的变量，在函数外部无法被访问
> 2. 函数的参数也是函数内部的局部变量
> 3. 不同函数内部声明的变量无法互相访问
> 4. 函数执行完毕后，函数内部的变量实际被清空了

2. 块作用域
> 1. `let` 声明的变量会产生块作用域，`var` 不会产生块作用域
> 2. `const` 声明的常量也会产生块作用域
> 3. 不同代码块之间的变量无法互相访问
> 4. 推荐使用 `let` 或 `const`


## 全局作用域
> 1. 为 `window` 对象动态添加的属性默认也是全局的，不推荐！
> 2. 函数中未使用任何关键字声明的变量为全局变量，不推荐！！！
> 3. 尽可能少的声明全局变量，防止全局变量被污染

## 作用域链
```javascript
			// 全局作用域
			let a = 1;
			let b = 2;
			// 局部作用域
			function fn1() {
				let a = 1;
				// 局部作用域
				function fn2() {
					a = 2;
					console.log(a); // 2
				}
				fn2();
			}
			fn1();
```
{% note success no-icon flat %}
作用域链：本质上是底层的{% label 变量查找机制 pink %}。
- 在函数被执行时，会{% label 优先查找当前 pink %}函数作用域中查找变量
- 如果当前作用域查找不到则会依次{% label 逐级查找父级作用域 pink %}直到全局作用域

总结：
1. 嵌套关系的作用域串联起来形成了作用域链
2. 相同作用域链中按着从小到大的规则查找变量
3. 子作用域能够访问父作用域，父级作用域无法访问子级作用域
{% endnote %}

## 闭包(⭐)
{% btn 'https://www.bilibili.com/video/BV1iE411q7Qd/?spm_id_from=333.337.search-card.all.click&vd_source=51dc0d76d16e5d6eb2c55984ed42ac56',蛋老师闭包详解,far fa-hand-point-right,block orange center larger %}

闭包是什么：一个函数A包含一个函数B，函数B使用了函数A中声明的变量 -- 就叫闭包
闭包要解决什么问题：如何在函数A外使用函数A里面声明的变量
如何解决：返回函数B

## 变量提升

# 函数进阶
## 函数提升
## 函数参数
### 动态参数(`arguments`):
{% label 伪数组 pink %}，只存在于函数中
作用是{% label 动态获取函数的实参 blue %}
可以通过for循环依次得到传递过来的实参
```javascript
// 求所有参数的和
function getSum() {
    let sum = 0;
    for (let i = 0; i < arguments.length; i++) {
        sum += arguments[i];
    }
    return sum;
}

let result = getSum(10, 20, 30, 40, 50, 60, 70);
console.log(result);
```
### 剩余参数(...)
{% label 真数组 pink %}，`...剩余运算符`：{% label 允许将不确定数量的参数做为一个数组整体存储 blue %}
开发中，还是提倡多用 {% label 剩余参数 red %}
```javascript
			function getSum(...arr) {
				console.log(arr); // [1, 2, 3, 4, 5, 6, 7, 8, 9] 

				let sum = 0;
				for (let i = 0; i < arr.length; i++) {
					sum += arr[i];
				}

				return sum;
			}

			let result = getSum(1, 2, 3, 4, 5, 6, 7, 8, 9);
			console.log(result);
```
...还可以作为`展开运算符`，{% label 将一个数组进行展开，不会修改原数组 blue %}
典型运用场景： {% label 求数组最大值(最小值)、合并数组 purple %}等
```javascript
// 求数组最大值(最小值)
const arr = [1, 2, 3, 4, 5];
console.log(...arr); // 1 2 3 4 5
console.log(Math.min(...arr)); // 1
console.log(Math.max(...arr)); // 5
```

```javascript
// 合并数组
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const arr3 = [...arr1, ...arr2];
console.log(arr3); // [1, 2, 3, 4, 5, 6]
```


## 箭头函数(⭐)
this
{% note success no-icon %}
在箭头函数出现之前，每一个新函数根据它是被如何调用的来定义这个函数的this值

箭头函数不会创建自己的this，它只会从自己的作用域链的上一层沿用this
{% endnote %}

```javascript
			console.log(this); // window

			const sayHi = function () {
				console.log(this); // window调用的，所以函数内部this指向window
			};
			const sayHi = () => {
				console.log(this); // window调用的，所以函数内部this指向window
			};

			const user = {
				name: '陈升',
				eat: function () {
					console.log(this); // user
					const fn = () => {
						console.log(this); // user，与eat中的this一致	
					};
					fn();
				},
				sayHi: () => {
					console.log(this); // window
				},
			};
			user.eat();
			user.sayHi();
```

```javascript
			btn.addEventListener('click', function () {
				console.log(this); // btn ✔ 
			});

			btn.addEventListener('click', () => {
				console.log(this); // window ✖
			});
```
{% note danger flat %}
总结：DOM事件回调函数为了简便，还是不太建议使用箭头函数
{% endnote %}

# 解构赋值
{% note success no-icon %}
解构赋值是一种快速为变量赋值的简洁语法，本质上仍然是为变量赋值。
{% endnote %}

## 数组解构
{% note success no-icon %}
将数组的单元值快速**批量赋值**给一系列变量的简洁语法
{% endnote %}

```javascript
const [a, b, c] = [1, 2, 3];
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```
典型运用场景： {% label 交换2个变量 purple %}
```javascript
let a = 1;
let b = 2;
[b, a] = [a, b];
console.log(a, b); // 2 1
/* ------------------------------------------------- */
let arr = [1, 3, 2, 5, 4];
for (let i = 0; i < arr.length; i++) {
	for (let j = 0; j < arr.length - i - 1; j++) {
		if (arr[j] > arr[j + 1]) {
			[arr[j + 1], arr[j]] = [arr[j], arr[j + 1]];
		}
	}
}
console.log(arr);
```

## 对象解构
{% note success no-icon %}
将**对象属性和方法**快速批量赋值给一系列变量的简洁语法
{% endnote %}

```javascript
const user = { name: '陈升', age: 18 };
const { name, age } = user;
// const { name:uname, age } = user; 提取变量并修改变量名
console.log(name); // 陈升
console.log(age); // 18
/* ---------------------------- */
const students = [
	{ name: '陈升', age: 18 },
];
const [{ name, age }] = students;
console.log(name); // 陈升
console.log(age); // 18
```

{% label 多级对象解构 pink %}
```javascript
const people = [
	{
		name: '陈升',
		family: {
			father: '康爸',
			mother: '英妈',
			brother: '啸哥',
		},
		age: 18,
	},
];

const [{name, family: { father, mother, brother }, age}] = people;
console.log(name);
console.log(father);
console.log(mother);
console.log(brother);
```
模拟后台获取的数据处理
```javascript
// 后台传递过来的数据
const msg = {
	code: 200,
	msg: '获取新闻列表成功',
	data: [
		{ id: 1, title: '三大运营商收入下降', count: 56 },
		{ id: 2, title: '三大运营商收入下降', count: 56 },
		{ id: 3, title: '三大运营商收入下降', count: 56 },
	],
};

// 需求1：只需获取对象里面的data数据
const { data } = msg;

// 需求2：将上面对象只选出data数据，传递给另外一个函数
function render({ data }) {
	console.log(data);
}
render(msg);

// 需求3：为了防止msg里面的data名字混淆，把渲染函数里面的数据吗改为myData
function render({ data: myData }) {
	console.log(myData);
}
render(msg);
```
## 综合案例：价格筛选

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E4%BB%B7%E6%A0%BC%E7%AD%9B%E9%80%89%E6%A1%88%E4%BE%8B.png)
```html
		<div class="filter">
			<a data-index="1" href="javascript:;">0-100元</a>
			<a data-index="2" href="javascript:;">100-300元</a>
			<a data-index="3" href="javascript:;">300元以上</a>
			<a href="javascript:;">全部区间</a>
		</div>

		<div class="list">
			<!-- <div class="item">
				<img src="" alt="">
				<p class="name">大师监制龙泉青瓷茶叶罐</p>
				<p class="price">139.00</p>
			</div> -->
		</div>
```
```css
* {
	padding: 0;
	margin: 0;
	box-sizing: border-box;
}
.filter {
	display: flex;
	width: 990px;
	margin: 0 auto;
	padding: 50px 30px;
}
.filter a {
	text-decoration: none;
	padding: 10px 20px;
	background-color: #f5f5f5;
	color: #666;
	margin-right: 20px;
}
.filter a:active,
.filter a:focus {
	background-color: #05943c;
	color: #fff;
}
.list {
	display: flex;
	flex-wrap: wrap;
	width: 990px;
	margin: 0 auto;
}
.item {
	width: 240px;
	margin-left: 10px;
	margin-bottom: 20px;
	padding: 20px 30px;
	transition: all 0.5s;
}
.item:nth-child(4n) {
	margin-left: 0;
}
.item img {
	width: 100%;
}
.item:hover {
	box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.2);
	cursor: pointer;
	transform: translate(0, -4px);
}
.item .name {
	font-size: 18px;
	color: #666;
	margin-bottom: 10px;
}
.item .price {
	font-size: 22px;
	color: firebrick;
}
.item .price::before {
	content: '￥';
	font-size: 14px;
}
```
```javascript
const goodsList = [
	{ id: '4001172', name: '称心如意手摇咖啡磨豆机咖啡豆研磨机', price: '289.00', picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg' },
	{ id: '4001594', name: '日式黑陶功夫茶组双侧把茶具礼盒装', price: '288.00', picture: 'https://yanxuan-item.nosdn.127.net/3346b7b92f9563c7a7e24c7ead883f18.jpg' },
	{ id: '4001009', name: '竹制干泡茶盘正方形沥水茶台品茶盘', price: '109.00', picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png' },
	{ id: '4001874', name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器', price: '488.00', picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png' },
	{ id: '4001649', name: '大师监制龙泉青瓷茶叶罐', price: '139.00', picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png' },
	{ id: '3997185', name: '与众不同的口感汝瓷白酒杯套组1壶4杯', price: '108.00', picture: 'https://yanxuan-item.nosdn.127.net/8e21c794dfd3a4e8573273ddae50bce2.jpg' },
	{ id: '3997403', name: '手工吹制更厚实白酒杯壶套装6壶6杯', price: '100.00', picture: 'https://yanxuan-item.nosdn.127.net/af2371a65f60bce152a61fc22745ff3f.jpg' },
	{ id: '3998274', name: '德国百年工艺高端水晶玻璃红酒杯2支装', price: '139.00', picture: 'https://yanxuan-item.nosdn.127.net/8896b897b3ec6639bbd1134d66b9715c.jpg' },
];

// 渲染数据
function render(arr) {
	let htmlStr = '';
	arr.forEach(({ name, price, picture }) => {
		htmlStr += `<div class="item">
                    <img src="${picture}" alt="" />
                    <p class="name">${name}</p>
                    <p class="price">${price}</p>
                </div>`;
	});
	document.querySelector('.list').innerHTML = htmlStr;
}
render(goodsList);

// 筛选数据
document.querySelector('.filter').addEventListener('click', function (e) {
	let {
		localName,
		dataset: { index },
	} = e.target;

	if (localName === 'a') {
		let arr = goodsList;
		if (index === '1') {
			arr = goodsList.filter((value) => value.price >= 0 && value.price <= 100);
		} else if (index === '2') {
			arr = goodsList.filter((value) => value.price > 100 && value.price <= 300);
		} else if (index === '3') {
			arr = goodsList.filter((value) => value.price > 300);
		}
		render(arr);
	}
});
```

{% btn 'https://ks.wjx.top/vj/wOeHuEk.aspx',课后测试题,far fa-hand-point-right,block green right larger %}

---
# 深入对象
## 创建对象的3种方式
利用`对象字面量`创建对象
```javascript
let obj = { name: '陈升', age: 18 };
```
利用 `new Object` 创建对象
```javascript
			// let obj = new Object({ name: '陈升', age: 18 });
			let obj = new Object();
			obj.name = '陈升';
			obj.age = 18;
			console.log(obj);
```
利用`构造函数`创建对象
```javascript
			function Person(name, age) {
				this.name = name;
				this.age = age;
			}
			const Csheng = new Person('陈升', 18);
```
## 构造函数
作用：{% label 快速创建多个类似的对象 blue %}。
```javascript
			// 1. 创建构造函数
			function Pig(name, age, gender) {
				this.name = name;
				this.age = age;
				this.gender = gender;
			}
			// 2. new 关键字调用函数
			// 接受创建的对象
			const Peppa = new Pig('佩奇', 6, '女');
			const George = new Pig('乔治', 3, '男');
```

约定：
它们的命名以`大写字母`开头。 
它们只能由 "`new`" 操作符来执行。

说明：
1.使用 new 关键字调用函数的行为被称为实例化
2.实例化构造函数时没有参数时可以省略 ()
3.构造函数内部无需写return，返回值即为新创建的对象
4.构造函数内部的 return 返回的`值无效`，return 返回的`引用类型会覆盖`
5.new Object（）   new Date（） 也是实例化构造函数

实例化执行过程
1.创建新空对象
2.构造函数this指向新对象
3.执行构造函数代码，修改this，添加新的属性
4.返回新对象

{% label 实例成员 pink %}

1. 通过构造函数创建的对象称为实例对象，`实例对象的属性和方法`即为实例成员
2. 为构造函数传入参数，动态创建结构相同但值不同的对象
3. 构造函数创建的实例对象彼此独立互不影响。
```javascript
			// 构造函数
			function Person() {
				// 构造函数内部的 this 就是实例对象
				// 实例对象中动态添加属性
				this.name = '小明';
				// 实例对象动态添加方法
				this.sayHi = function () {
					console.log('大家好~');
				};
			}

			// 实例化，p1是实例对象
			// p1 实际就是构造函数内部的 this
			const p1 = new Person();
			console.log(p1.name); // 访问实例属性
			p1.sayHi(); // 调用实例方法
```

{% label 静态成员 pink %}

1.`构造函数的属性和方法`被称为静态成员
2.一般公共特征的属性或方法静态成员设置为静态成员
3.静态成员方法中的 this 指向构造函数本身
```javascript
			// 构造函数
			function Person(name, age) {
				// 省略实例成员
			}
			// 静态属性
			Person.eyes = 2;
			// 静态方法
			Person.walk = function () {
				console.log('人都会走路');
				// this 指向 Person
				console.log(this.eyes);
			};
```

案例：{% label 打印多个实例化商品数据 blue %}
```javascript
			function Goods(name, price, count) {
				this.name = name;
				this.price = price;
				this.count = count;
				this.showInfo = function () {
					console.log(this.name, this.price, this.count);
				};
			}

			let xiaoMi = new Goods('小米', 1999, 20);
			let huaWei = new Goods('华为', 3999, 59);
			let vivo = new Goods('vivo', 1888, 100);
			
			xiaoMi.showInfo();
			huaWei.showInfo();
			vivo.showInfo();
```

---
# 内置构造函数
## Object
```javascript
const user = new Object({ name: '小明' , age: 15 })
// 推荐使用 字面量 方式声明对象，而不是 Object 构造函数
```
三个常用静态方法（只有构造函数Object可以调用的）
{% label Object.keys blue %} 获取对象中所有属性（键），并且返回的是一个数组
```javascript
			const arr = Object.keys(user);
			console.log(arr); // ['name', 'age']
```
{% label Object.values blue %} 获取对象中所有属性值，并且返回的是一个数组
```javascript
			const arr = Object.values(user);
			console.log(arr); // ['小明', 15]
```
{% label Object.assign blue %} 常用于对象拷贝
```javascript
			// 拷贝对象：把 user 拷贝给 obj
			const obj = {};
			Object.assign(obj, user);
			console.log(obj); // {name: '小明', age: 15}
			// 给 user 新增属性
			Object.assign(user, { gender: '男' });
			console.log(user); // {name: '小明', age: 15, gender: '男'}
```

## Array
```javascript
const arr = new Array(3, 5);
// 创建数组建议使用 字面量 创建，不用 Array 构造函数创建
```
{% label 常见实例方法 blue %}

| 方法 | 作用 | 说明 |
| :----:| :----: | :---- |
| forEach | 遍历数组 | 不返回，用于不改变值，经常用于查找打印输出值 |
| filter | 过滤数组 | 筛选数组元素，并生成新数组 |
| map | 迭代数组 | 返回新数组，新数组里面的元素是处理之后的值，经常用于处理数据 |
| reduce | 累计器 | 返回函数累计处理的结果，经常用于`求和`等 |

`join`：数组元素**拼接为字符串**，返回字符串
`find`：查找元素，返回符合测试条件的第一个数组元素值，如果没有符合条件的则返回undefined
`every`：检测数组所有元素是否都符合指定条件，如果所有元素都通过检测返回true，否则返回false
`some`：检测数组中的元素是否满足指定条件，如果数组中有元素满足条件返回true，否则返回false
`concat`：**合并两个数组**，返回生成新数组
`sort`：对原数组单元值**排序**
`splice`：**删除或替换**原数组单元
`reverse`：**反转**数组
`findIndex`：**查找元素的索引值**

数组常见方法-伪数组转换为真数组
静态方法 `Array.from()` 

{% label 求和运算 blue %}
```javascript
			const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
			const count = arr.reduce((prev, item) => prev + item);
			console.log(count); // 45
```
案例：员工涨薪计算成本（给每个员工涨薪30%）
```javascript
			const arr = [
				{ name: '张三', salary: 10000 },
				{ name: '李四', salary: 10000 },
				{ name: '王五', salary: 10000 },
			];
			const money = arr.reduce((prev, item) => prev + item.salary * 0.3, 0);
			console.log(money);
```
练习：const spec =  { size: '40cm*40cm' , color: '黑色'}
请将size和color里面的值拼接为字符串之后，写到div标签里面，处理成："40cm*40cm/黑色" 的格式
```javascript
			const spec = { size: '40cm*40cm', color: '黑色' };
			console.log(Object.values(spec).join('/')); 
```


## String
实例属性：
`length`：用来获取字符串的长度
常见实例方法：
`split`：('分隔符')用来将字符串拆分成数组
`substring`：(需要截取的第一个字符的索引[,结束的索引号])用于字符串截取
`startsWith`：(检测字符串[，检测位置索引号])检测是否以某字符开头
`includes`：(搜索的字符串[，检测位置索引号]）判断一个字符串是否包含在另一个字符串中，根据情况返回true/false
`toUppercase`：用于将字母转换成大写
`toLowercase`：用于将就转换成小写
`indexof`：检测是否包含某字符
`endswith`：检测是否以某字符结尾
`replace`：用于替换字符串，支持正则匹配
`match`：用于查找字符串，支持正则匹配

## Number
toFixed()：设置保留小数位的长度
```javascript
			const price = 12.345;
			// 保留2位小数，四舍五入
			console.log(price.toFixed(2)); // 12.35
```

## 综合案例：购物车展示
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E8%B4%AD%E7%89%A9%E8%BD%A6%E5%B1%95%E7%A4%BA%E6%A1%88%E4%BE%8B.png)

需求：
根据后台提供的数据，渲染购物车页面

分析业务模块：
①：渲染图片、标题、颜色、价格、赠品等数据
②：单价和小计模块
③：总价模块

分析业务模块：
①：把整体的结构直接生成然后渲染到大盒子.list 里面
②：哪个方法可以遍历的同时还有返回值  map 方法
③：最后计算总价模块，哪个方法可以求和？ reduce 方法

```html
    <div class="list">
            <!-- <div class="item">
                <img src="https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg" alt="">
                <p class="name">
                    称心如意手摇咖啡磨豆机咖啡豆研磨机
                    <span class="tag">【赠品】10优惠券</span>
                </p>
                <p class="spec">白色/10寸</p>
                <p class="price">289.90</p>
                <p class="count">x2</p>
                <p class="sub-total">579.80</p>
            </div> -->
    </div>

    <div class="total">
        <div>合计：<span class="amount">1000.00</span></div>
    </div>
```
```css
* {
	padding: 0;
	margin: 0;
	box-sizing: border-box;
}
.list {
	width: 990px;
	margin: 100px auto 0;
}
.item {
	display: flex;
	padding: 15px;
	border-top: 1px solid #e4e4e4;
}
.item img {
	width: 80px;
	height: 80px;
	margin-right: 10px;
}
.item .name {
	font-size: 18px;
	margin-right: 10px;
	color: #333;
	flex: 2;
}
.item .name .tag {
	display: block;
	padding: 2px;
	font-size: 12px;
	color: #999;
}
.item .spec {
	flex: 2;
	color: #888;
	font-size: 14px;
}
.item .price,
.item .sub-total {
	flex: 1;
	font-size: 18px;
	color: firebrick;
}
.item .count {
	flex: 1;
	color: #aaa;
}
.item .price::before,
.item .sub-total::before,
.amount::before {
	content: '￥';
	font-size: 12px;
}
.item:hover {
	cursor: pointer;
	background-color: #f5f5f5;
}
.total {
	display: flex;
	justify-content: flex-end;
	width: 990px;
	margin: 0 auto;
	border-top: 1px solid #e4e4e4;
	padding: 20px;
}
.total .amount {
	font-size: 18px;
	color: firebrick;
	font-weight: bold;
	margin-right: 50px;
}
```

```javascript
let list = document.querySelector('.list');
let amount = document.querySelector('.amount');

const goodsList = [
	{ id: '4001172', name: '称心如意手摇咖啡磨豆机咖啡豆研磨机', price: 289.9, picture: 'https://yanxuan-item.nosdn.127.net/84a59ff9c58a77032564e61f716846d6.jpg', count: 2, spec: { color: '白色' } },
	{ id: '4001009', name: '竹制干泡茶盘正方形沥水茶台品茶盘', price: 109.8, picture: 'https://yanxuan-item.nosdn.127.net/2d942d6bc94f1e230763e1a5a3b379e1.png', count: 3, spec: { size: '40cm*40cm', color: '黑色' } },
	{ id: '4001874', name: '古法温酒汝瓷酒具套装白酒杯莲花温酒器', price: 488, picture: 'https://yanxuan-item.nosdn.127.net/44e51622800e4fceb6bee8e616da85fd.png', count: 1, spec: { color: '青色', sum: '一大四小' } },
	{ id: '4001649', name: '大师监制龙泉青瓷茶叶罐', price: 139, picture: 'https://yanxuan-item.nosdn.127.net/4356c9fc150753775fe56b465314f1eb.png', count: 1, spec: { size: '小号', color: '紫色' }, gift: '50g茶叶,清洗球,宝马, 奔驰' },
];

// 渲染数据（通过map处理数组每一项）
let arr = goodsList.map((value) => {
	// 将 spec: { color: '青色', sum: '一大四小' } 处理成 "青色/一大四小"
	let spec = Object.values(value.spec).join('/');
	// 处理 赠品 gift (可能有可能没有)
	let gift = value.gift
		? value.gift
				.split(',')
				.map((v) => `<span class="tag">【赠品】${v}</span>`)
				.join('')
		: '';

	return `<div class="item">
                <img src="${value.picture}" alt="">
                <p class="name">
                    ${value.name}
                    ${gift}
                </p>
                <p class="spec">${spec}</p>
                <p class="price">${value.price}</p>
                <p class="count">${value.count}</p>
                <p class="sub-total">${value.price * value.count}</p>
            </div>`;
});
list.innerHTML = arr.join('');

// 计算合计
amount.innerHTML = goodsList.reduce((total, value) => total + value.price * value.count, 0).toFixed(2);
```


{% btn 'https://ks.wjx.top/vj/QG54lSj.aspx',课后测试题,far fa-hand-point-right,block green right larger %}

# 面向对象
![构造函数&原型对象&实例对象的三角关系](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0%20&%20%E5%8E%9F%E5%9E%8B%E5%AF%B9%E8%B1%A1%20&%20%E5%AE%9E%E4%BE%8B%E5%AF%B9%E8%B1%A1%E7%9A%84%E4%B8%89%E8%A7%92%E5%85%B3%E7%B3%BB.png)
```javascript
			function Person(name, age) {
				this.name = name;
				this.age = age;
				// 问题：会为每个对象添加单独的sing方法，其实没有必要，因为行为一致，这里反复创建，浪费内存
				this.eat = function () {
					console.log('吃饭');
				};
			}
			const father = new Person('爸爸', 48);
			const mother = new Person('妈妈', 45);
			console.log(father.eat === mother.eat); // false
```
解决方法：使用下面的`原型 prototype`

## 原型
目标：能够利用原型对象实现方法共享
- 构造函数通过原型分配的函数是所有对象所`共享`的。
- JavaScript 规定，`每一个构造函数都有一个 prototype 属性`，指向另一个对象，所以我们也称为原型对象
- 这个对象可以挂载函数，对象实例化不会多次创建原型上函数，节约内存
- `我们可以把那些不变的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些方法`。
- `构造函数和原型对象中的this 都指向 实例化的对象`

我们会将公共的成员(方法)添加到 原型 中
- 什么是原型：系统会在定义构造函数之后，为构造函数关联一个默认的对象，这个对象就是构造函数的原型
- 原型如何操作：原型是一个对象，所以可以像操作对象一样操作原型
- 如何获取到原型：通过构造函数 的 prototype属性可以获取到函数的原型对象，如 Person.prototype
- 原型如何使用:通过构造函数的对象直接访问原型中的成员，也只有当前构造函数所创建的对象才能使用原型中的成员

```javascript
			function Person(name, age) {
				this.name = name;
				this.age = age;
			}
			// 将 eat 方法添加到 Person 构造函数的原型上
			Person.prototype.eat = function () {
				console.log('吃饭');
			};

			const father = new Person('爸爸', 48);
			father.eat();
			const mother = new Person('妈妈', 45);
			mother.eat();
			console.log(father.eat === mother.eat); // true
```

总结：
原型是什么？  {% hideInline '一个对象，我们也称为 prototype 为原型对象',查看答案,#FF7242,#fff %}
原型的作用是什么？ {% hideInline '共享方法。可以把那些不变的方法，直接定义在 prototype 对象上',查看答案,#FF7242,#fff %}
构造函数和原型里面的this指向谁？ {% hideInline '实例化的对象',查看答案,#FF7242,#fff %}

{% label 给数组扩展方法 blue %}
```javascript
// 求最大值
Array.prototype.max = function () {
	// this 指向方法的调用者，就是数组[1,2,3]
	return Math.max(...this);
};
console.log([1, 2, 3].max()); // 3
// 求和
Array.prototype.sum = function () {
	return this.reduce((prev, item) => prev + item, 0);
};
console.log([1, 2, 3].sum()); // 6
```

## constructor 属性
**存放位置**：`每个原型对象里面都有个constructor 属性`（constructor 构造函数）
**作用**：该属性指向该原型对象的`构造函数`
**使用场景**：
如果有多个对象的方法，我们可以给原型对象采取对象形式赋值.
但是这样就会覆盖构造函数原型对象原来的内容，这样修改后的原型对象 constructor  就不再指向当前构造函数了
此时，我们可以在修改后的原型对象中，添加一个 `constructor` 指向原来的构造函数。

```javascript
function Star(name) {
	this.name = name;
}
Star.prototype = {
	// 手动利用 constructor 指向 Star 构造函数
	constructor: Star,
	sing: function () {
		console.log('唱歌');
	},
	dance: function () {
		console.log('跳舞');
	},
};
console.log(Star.prototype.constructor); // 指向 Star
```

## 对象原型
`对象都会有一个属性 __proto__`  指向构造函数的 prototype 原型对象，之所以我们对象可以使用构造函数 prototype 原型对象的属性和方法，就是因为对象有__proto__原型的存在。
注意：
- __proto__是JS非标准属性
- [[prototype]]和__proto__意义相同
- 用来表明当前实例对象指向哪个原型对象prototype
- __proto__对象原型里面也有一个 constructor属性，`指向创建该实例对象的构造函数`

## 原型继承
继承是面向对象编程的另一个特征，通过继承进一步提升代码封装的程度，JavaScript 中大多是借助`原型对象`实现继承的特性。
```javascript
function Man() {
	this.head = 1;
	this.eyes = 2;
	this.legs = 2;
	this.say = function () {};
	this.eat = function () {};
}
const father = new Man();

function Woman() {
	this.head = 1;
	this.eyes = 2;
	this.legs = 2;
	this.say = function () {};
	this.eat = function () {};
}
const mother = new Woman();
```
继承写法：
真正做这个案例，我们的思路应该是先考虑大的，后考虑小的
1.人类共有的属性和方法有那些，然后做个构造函数，进行封装，一般公共属性写到构造函数内部，公共方法，挂载到构造函数原型身上。
2.男人继承人类的属性和方法，之后创建自己独有的属性和方法
3.女人同理

```javascript
// 人类公共的属性和方法
function Person() {
	this.head = 1;
	this.eyes = 2;
	this.legs = 2;
	this.say = function () {};
	this.eat = function () {};
}

// 男人
function Man() {}
// 用 new Person() 替换刚才的固定对象
Man.prototype = new Person();
// 注意让原型里面的 constructor 重新指回 Man 找自己的爸爸
Man.prototype.constructor = Man;
const father = new Man();
Man.prototype.smoking = function () {};
console.log(father);

// 女人
function Woman() {
	this.baby = function () {};
}
Woman.prototype = new Person();
Woman.prototype.constructor = Woman;
const mother = new Woman();
console.log(mother);
```

## 原型链
> 基于原型对象的继承使得不同构造函数的原型对象关联在一起，并且这种关联的关系是一种链状的结构，我们将原型对象的链状结构关系称为原型链

查找规则
❶ 当访问一个对象的属性（包括方法）时，首先查找这个`对象自身`有没有该属性。
❷ 如果没有就查找它的原型（也就是 __proto__指向的 `prototype 原型对象`）
❸ 如果还没有就查找原型对象的原型（`Object的原型对象`）
❹ 依此类推一直找到 Object 为止（`null`）
❺ __proto__对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线
❻ 可以使用  instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

## 综合案例：消息提示对象封装
目的： 练习面向对象写插件（模态框）
需求：![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%B6%88%E6%81%AF%E6%8F%90%E7%A4%BA%E5%AF%B9%E8%B1%A1%E5%B0%81%E8%A3%85.gif)

分析需求：
❶ 定义模态框 Modal 构造函数，用来创建对象
❷ 模态框具备 打开功能 open 方法 （按钮点击可以打开模态框）
❸ 模态框 具备关闭功能  close 方法  
问：
open 和 close 方法 写到哪里？
`构造函数的原型对象上，共享方法`
`所以可以分为三个模块，构造函数，open方法，close方法`

步骤：
❶ Modal 构造函数 制作
       -  需要的公共属性：  标题（title）、提示信息内容（message）  可以设置默认参数
       -  在页面中创建模态框
           (1)  创建div标签可以命名为：modalBox
           (2)  div标签的类名为 modal 
           (3)  标签内部添加 基本结构，并填入相关数据
❷ open方法
       - 写到构造函数的原型对象身上
       - 把刚才创建的modalBox 添加到 页面 body 标签中
       -  open 打开的本质就是 把创建标签添加到页面中
       -  点击按钮， 实例化对象，传入对应的参数，并执行 open 方法
❸ close方法
       - 写到构造函数的原型对象身上
       - 把刚才创建的modalBox 从页面 body 标签中 删除  
       - 需要注意，x 删除按钮绑定事件，要写到open里面添加
          因为open是往页面中添加元素，同时顺便绑定事件
```html
		<button id="delete">删除</button>
		<button id="login">登录</button>

		<!-- <div class="modal">
			<div class="header">温馨提示 <i>x</i></div>
			<div class="body">您没有删除权限操作</div>
		</div> -->
```
```css
.modal {
	position: fixed;
	left: 50%;
	top: 50%;
	transform: translate(-50%, -50%);
	z-index: 999;
	width: 300px;
	min-height: 100px;
	background-color: #fff;
	box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
	border-radius: 4px;
}
.modal .header {
    position: relative;
	line-height: 40px;
	padding: 0 10px;
	font-size: 20px;
}
.modal .header i {
    position: absolute;
    right: 15px;
    top: -2px;
	font-style: normal;
	color: #999;
    cursor: pointer;
}
.modal .body{
    text-align: center;
    padding: 10px;
}
```
```javascript
			function MyModal(title, msg) {
				this.title = title;
				this.msg = msg;
				// 添加方法：创建模态框
				this.modal = document.createElement('div');
				this.modal.className = 'modal';
				this.modal.innerHTML = `<div class="header">${title} <i>x</i></div>
                                        <div class="body">${msg}</div>`;
			}
			MyModal.prototype.open = function () {
				document.body.appendChild(this.modal);
				const _this = this;
				document.querySelector('i').addEventListener('click', function () {
					_this.close();
				});
			};
			MyModal.prototype.close = function () {
				document.body.removeChild(this.modal);
			};

			document.querySelector('#delete').addEventListener('click', function () {
				let modal = new MyModal('温馨提示', '您没有删除权限操作');
				modal.open();
				setTimeout(() => {
					modal.close();
				}, 3000);
			});
			document.querySelector('#login').addEventListener('click', function () {
				let modal = new MyModal('恭喜您', '您已登陆成功');
				modal.open();
			});
```

{% btn 'https://ks.wjx.top/vj/rTd1xoS.aspx',课后测试题,far fa-hand-point-right,block green right larger %}

# 高阶技巧
## 深浅拷贝
![对象直接赋值的弊端](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%AF%B9%E8%B1%A1%E7%9B%B4%E6%8E%A5%E8%B5%8B%E5%80%BC%E7%9A%84%E5%BC%8A%E7%AB%AF.png)
<b><i>浅拷贝和深拷贝都是只针对`引用类型`</i></b>
❶ {% label 浅拷贝 pink %} 拷贝的是`地址`
1.拷贝 **对象**：{% label 'Object. assign ( ) 或 展开运算符 { ...obj }' green %}  
2.拷贝**数组**：{% label 'Array. prototype. concat ( ) 或 [ ...arr ]' green %}  

![浅拷贝的2种方法](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%B5%85%E6%8B%B7%E8%B4%9D%E7%9A%84%202%20%E7%A7%8D%E6%96%B9%E6%B3%95.png)

![多层浅拷贝的弊端](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%A4%9A%E5%B1%82%E6%B5%85%E6%8B%B7%E8%B4%9D%E7%9A%84%E5%BC%8A%E7%AB%AF.png)

❓直接赋值和浅拷贝有什么区别？
➤ 直接赋值的方法，只要是对象，都会相互影响，因为是直接拷贝对象栈里面的`地址`
➤ 浅拷贝如果是`一层`对象，不相互影响，如果出现`多层`对象拷贝还会相互影响
❓浅拷贝怎么理解？
➤ 拷贝对象之后，里面的属性值是`简单数据类型`直接拷贝`值`
➤ 如果属性值是`引用数据类型`则拷贝的是`地址`

❷ {% label 深拷贝 pink %} 拷贝的是`对象`
1.{% label 递归 green %}
2.{% label lodash里的cloneDeep方法 green %}
3.{% label JSON.stringify() green %}

> 函数递归:
> `如果一个函数在内部可以调用其本身，那么这个函数就是递归函数`
➤ 简单理解:函数内部自己调用自己, 这个函数就是递归函数
➤ 递归函数的作用和循环效果类似
➤ 由于递归很容易发生“栈溢出”错误（stack overflow），所以`必须要加退出条件 return`
```javascript
			function getSum(n) {
				if (n == 1) {
					return 1;
				} else {
					return getSum(n - 1) + n;
				}
			}
			let sum = getSum(100);
```

![实现深拷贝的三个方法](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%AE%9E%E7%8E%B0%E6%B7%B1%E6%8B%B7%E8%B4%9D%E7%9A%84%E4%B8%89%E4%B8%AA%E6%96%B9%E6%B3%95.png)

## 异常处理
了解 JavaScript 中程序异常处理的方法，提升代码运行的健壮性。 
> 异常处理是指预估代码执行过程中可能发生的错误，然后最大程度的避免错误的发生导致整个程序无法继续运行


1.{% label 'throw 抛出异常' pink %}
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/throw%20%E6%8A%9B%E5%87%BA%E5%BC%82%E5%B8%B8.png)

2.{% label 'try...catch 捕获错误信息' pink %}
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/try...catch%20%E6%8D%95%E8%8E%B7%E9%94%99%E8%AF%AF%E4%BF%A1%E6%81%AF.png)

3.{% label debugger pink %}

## 处理this（⭐）
改变this的3个方法：
➤ `call()`
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/call().png)

➤ `apply()`
![求数组最大值的2个方法](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/apply().png)

❓call和apply的区别是？
➤ 都是调用函数，都能改变this指向
➤ 参数不一样，apply传递的必须是`数组`

➤ `bind()`（⭐）
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/bind().png)
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/call&apply&bind%E7%9A%84%E5%8C%BA%E5%88%AB.png)

## 性能优化
### 节流（throttle）
> 指连续触发事件，但是在 n 秒中只执行一次函数

开发使用场景：小米轮播图点击效果、鼠标移动、页面尺寸缩放resize、滚动条滚动

![利用节流来处理-鼠标滑过盒子显示文字](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%88%A9%E7%94%A8%E8%8A%82%E6%B5%81%E6%9D%A5%E5%A4%84%E7%90%86%20-%20%E9%BC%A0%E6%A0%87%E6%BB%91%E8%BF%87%E7%9B%92%E5%AD%90%E6%98%BE%E7%A4%BA%E6%96%87%E5%AD%97.png)

### 防抖（debounce）
> 指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间

开发使用场景：搜索框防抖

![利用防抖来处理-鼠标滑过盒子显示文字](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%88%A9%E7%94%A8%E9%98%B2%E6%8A%96%E6%9D%A5%E5%A4%84%E7%90%86%20-%20%E9%BC%A0%E6%A0%87%E6%BB%91%E8%BF%87%E7%9B%92%E5%AD%90%E6%98%BE%E7%A4%BA%E6%96%87%E5%AD%97.png)

Lodash 库 实现节流和防抖

![Lodash 库 实现节流和防抖](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Lodash%20%E5%BA%93%20%E5%AE%9E%E7%8E%B0%E8%8A%82%E6%B5%81%E5%92%8C%E9%98%B2%E6%8A%96.png)

## 综合案例-记录视频播放位置
```html
		<div class="container">
			<div class="header">
				<a href="http://pip.itcast.cn">
					<img src="https://pip.itcast.cn/img/logo_v3.29b9ba72.png" alt="" />
				</a>
			</div>
			<div class="video">
				<video src="https://v.itheima.net/LapADhV6.mp4" controls></video>
			</div>
			<div class="elevator">
				<a href="javascript:;" data-ref="video">视频介绍</a>
				<a href="javascript:;" data-ref="intro">课程简介</a>
				<a href="javascript:;" data-ref="outline">评论列表</a>
			</div>
		</div>
		<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
		<script src="./js/index.js"></script>
```
```css
* {
	padding: 0;
	margin: 0;
	box-sizing: border-box;
}
.container {
	width: 1200px;
	margin: 0 auto;
}
.video video {
	width: 100%;
	padding: 20px 0;
}
.elevator {
	position: fixed;
	top: 280px;
	right: 20px;
	z-index: 999;
	background: #fff;
	border: 1px solid #e4e4e4;
	width: 60px;
}
.elevator a {
	display: block;
	padding: 10px;
	text-decoration: none;
	text-align: center;
	color: #999;
}
.elevator a.active {
	color: #1286ff;
}
.outline {
	padding-bottom: 300px;
}
```
```javascript
const video = document.querySelector('video');

// timeupdate：视频播放的时候持续触发
video.addEventListener(
	'timeupdate',
	_.throttle(() => {
		localStorage.setItem('currentTime', video.currentTime);
	}, 1000)
);

// loadeddata：打开页面触发事件，就从本地存储里面取出记录的时间，赋值给 video.currentTime
video.addEventListener('loadeddata', function () {
	video.currentTime = localStorage.getItem('currentTime') || 0;
});
```

{% btn 'https://ks.wjx.top/vj/QARCGSJ.aspx',课后测试题,far fa-hand-point-right,block green right larger %}
