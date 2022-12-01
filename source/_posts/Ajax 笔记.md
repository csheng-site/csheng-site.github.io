---
title: 'Ajax 笔记'
date: 2022-11-06 21:56:43
categories: JavaScript
description: Asynchronous Javascript And XML（异步JavaScript和XML），使用Ajax技术网页应用能够快速地将增量更新呈现在用户界面上
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/ajax.png
---

> AJAX 是`异步的 JavaScript 和 XML`（Asynchronous JavaScript And XML）。
> 简单点说，就是使用 `XMLHttpRequest` 对象与服务器通信。它可以使用 JSON，XML，HTML 和 text 文本等格式发送和接收数据。
> AJAX 最吸引人的就是它的“`异步`”特性，也就是说它可以在`不重新刷新页面`的情况下与服务器通信，交换数据，或更新页面。

{% btn 'https://developer.mozilla.org/zh-CN/docs/Web/Guide/AJAX/Getting_Started',AJAX详细介绍,far fa-hand-point-right,block orange center larger %}
{% btn 'http://www.axios-js.com/zh-cn/docs/',axios中文网,far fa-hand-point-right,block blue center larger %}

| 使用 Ajax 请求数据的 5 种方式          | 描述                                             |
| --------------------- | -------------------------------------------------- |
|  POST  |  向服务器`新增`数据  |
|  GET  |  从服务器`获取`数据  |
|  DELETE  |  `删除`服务器上的数据  |
|  PUT  |  更新服务器上的数据（侧重于`完整更新`：例如更新用户的完整信息）  |
|  PATCH  |  更新服务器上的数据（侧重于`部分更新`：例如只更新用户的手机号）  |

axios 网络请求库（简化`ajax`的调用）

```javascript
<script src='https://cdn.jsdelivr.net/npm/axios@1.1.2/dist/axios.min.js'></script>
```

## 初体验-天气预报

```javascript
document.querySelector('.ipt').addEventListener('keyup', function (e) {
	// 限制仅回车触发
	if (e.keyCode !== 13) return;
	const city = this.value.trim();
	// 内容非空判断
	if (!city) return alert('请输入查询的城市');
	// ajax 前后端交互
	axios({
		url: `http://ajax-api.itheima.net/api/city?pname=${city}`,
		method: 'get',
	}).then(res => {
		if (res.data.status === 1002) {
			alert('城市名有误,请检查');
		} else {
			alert(res.data.data.ganmao);
		}
	});
});
```

## GET 和 POST 请求

```javascript
/* ****************** GET请求 ******************** */
axios({
	url: '接口地址', // 无参数
	method: 'get', // get可以省略
});
axios({
	url: '接口地址?key=value&key2=value2', // 有参数,参数拼接在URL中
	method: 'get',
});
axios({
	url: '接口地址',
	method: 'get',
	// 有参数,通过params传递会自动转为上面的格式
	params: { key1: 'value1', key2: 'value2' },
});
/* ****************** POST 请求 ******************** */
axios({
	url: '接口地址',
	method: 'post',
	data: { key: 'value', key2: 'value2' },
});
```

### HTTP状态码
| 状态码  | 状态码描述            | 说明                                               |
| ------- | --------------------- | -------------------------------------------------- |
| **200** | OK                    | 请求成功。                                         |
| 201     | Created               | 资源在服务器端已成功创建。                         |
| 304     | Not Modified          | 资源在客户端被缓存，响应体中不包含任何资源内容！   |
| **400** | Bad Request           | 客户端的请求方式、或请求参数有误导致的请求失败！   |
| **401** | Unauthorized          | 客户端的用户身份认证未通过，导致的此次请求失败！   |
| **404** | Not Found             | 客户端请求的资源地址错误，导致服务器无法找到资源！ |
| 500     | Internal Server Error | 服务器内部错误，导致的本次请求失败！               |



### 新闻列表📒
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%96%B0%E9%97%BB%E5%88%97%E8%A1%A8%E6%A1%88%E4%BE%8B.gif)
```javascript
axios({
	url: 'http://ajax-api.itheima.net/api/news',
	method: 'get',
}).then(({ data: { data } }) => {
	document.querySelector('#news-list').innerHTML = data
		.map(({ cmtcount, img, source, time, title }) => {
			return `<div class="news-item">
                    <img class="thumb" src="${img}" alt="" />
                    <div class="right-box">
                      <h1 class="title">${title}</h1>
                      <div class="footer">
                        <div>
                          <span>${source}</span>&nbsp;&nbsp;
                          <span>${time}</span>
                        </div>
                        <span>评论数：${cmtcount}</span>
                      </div>
                    </div>
                  </div>`;
		})
		.join('');
});
```

### 用户登录📒
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E7%94%A8%E6%88%B7%E7%99%BB%E5%BD%95%E6%A1%88%E4%BE%8B.gif)
```javascript
			// 请求方式 POST
			// 地址 http://www.liulongbin.top:3009/api/login
			// 参数:  username 用户名     password  密码  ==> 请求体参数，写在data中
			document.querySelector('#btnLogin').addEventListener('click', function () {
				let username = document.querySelector('#username').value;
				let password = document.querySelector('#password').value;

				axios({
					url: 'http://ajax-api.itheima.net/api/login',
					method: 'POST',
					data: { username, password },
				}).then(({ data: res }) => {
					if (res.code === 200) {
						alert('恭喜你，登录成功了，稍后跳转去首页');
					} else {
						alert(res.msg);
					}
				});
			});
		</script>
```

### 聊天机器人📒
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E8%81%8A%E5%A4%A9%E6%9C%BA%E5%99%A8%E4%BA%BA%E6%A1%88%E4%BE%8B.gif)
```javascript
			const input_sub = document.querySelector('.input_sub');
			const input_txt = document.querySelector('.input_txt');
			const talk_list = document.querySelector('.talk_list');

			function creatLi(txt, position) {
				let newLi = document.createElement('li');
				newLi.className = position;
				newLi.innerHTML = `
              <img src="img/person${position == 'right_word' ? '02' : '01'}.png" />
              <span>${txt}</span>`;
				talk_list.appendChild(newLi);
				resetui();
				input_txt.value = '';
			}

			input_sub.addEventListener('click', function () {
				const content = input_txt.value;
				creatLi(content, 'right_word');

				// 发起请求
				axios({
					url: 'http://ajax-api.itheima.net/api/robot',
					method: 'GET',
					params: { spoken: content },
				}).then(({ data: { data } }) => {
					let content = data.info.text;
					creatLi(content, 'left_word');
				});
			});

			input_txt.addEventListener('keyup', function (e) {
				const value = this.value.trim();
				if (e.keyCode !== 13) return;
				if (!value) return alert('请在输入框输入内容');
				input_sub.click();
			});
```

## form 表单 & 文件上传
### form 表单

```html
		<form action="http://hmajax.itheima.net/api/books" method="get">
			<label><span>用户名</span><input type="text" name="username" /></label>
			<label><span>密码</span><input type="password" name="password" /></label>
			<button type="submit">提交</button>
		</form>
```
**须知：**
➤ 表单的三个组成部分  分别是：<u>`表单标签、表单域、表单按钮`</u>
➤ `<form>` 标签最重要的 3 个属性分别是 <u>`action、method 和 enctype`</u>
➤ 注意：每个表单域必须包含 `name` 属性，否则用户填写的信息无法被采集到！
➤ type 属性的默认值就是 `submit`，因此 type="submit" 可以省略不写

**缺点：**
➤ 提交表单数据的时候，`整个页面会发生跳转`，跳转到了 action 属性所指向的 url 地址，用户无法停留在当前的页面，导致体验很差

**解决方案：**
➤ `<form>` 表单只负责`采集数据`；
➤ `Ajax` 负责`将数据提交到服务器`。

❶ {% label 使用form-serialize插件收集表单数据 pink %}
➤ `serialize(表单元素，选项)`：可以获取指定表单中，拥有`name属性的表单元素的value值`，返回相应的值
➤ 选项说明：
	- { hash: `true` }：返回的是`对象 {key:value}`
	- { hash: `false` }: 返回的是 `bookname=aaa&author=bbb&publisher=ccc&creater=rrr`
➤ 重点前提：表单元素`一定要有name属性`，且属性值一定要参照后台接口文档的说明

```html
		<script src="https://cdn.jsdelivr.net/npm/axios@1.1.2/dist/axios.min.js"></script>
		<script src="./lib/form-serialize.js"></script>
		<script>
			document.querySelector('button').addEventListener('click', function (e) {
				e.preventDefault();

				// {username: '111', password: '222'}
				let data1 = serialize(document.querySelector('form'), { hash: true }); 
				// 'username=111&password=222'
				let data2 = serialize(document.querySelector('form'), { hash: false }); 

				// 发起网络请求
			});
		</script>
```
**缺点：**
form-serialze插件：并不能获取到文件数据
意味着，在有文件数据的场景下，这个插件力不从心
以后选择
1。`常规表单（输入框，下拉列表，文本域）元素`的数据：`插件 + formdata`
2。有`文件(file)数据`：只能使用`formdata`
3。如果是`状态值(checked)`：单独进行数据的获取

❷ {% label FormData的使用 pink %}

| api |  说明       |   
| ----------------------- | -------------- |   
| `append`('key', 'value'); |  向对象中`追加`数据       |   
| `set`('key', 'value');    |  `修改`对象中的数据       |   
| `delete` ('key');         |  从对象中`删除`数据       |   
| `get`('key')              |  `获取`指定key的`一项`数据  |
| `getAll`('key')           |  `获取`指定key的`全部`数据  |
| `forEach`()               |  `遍历`对象中的数据       |


```javascript
			document.querySelector('button').addEventListener('click', function (e) {
				e.preventDefault();
				
				// 获取对象
				let formData = new FormData(document.querySelector('form')); // 对象

				// 发起网络请求
			});
```


总结
➤ 为什么serialize方法无法获取文件数据：
答：因为`serialize方法只能获取普通的字符串值`，而`文件是二进制数据`
➤ 为什么formdata可以获取文件数据？
答：在formdata看来，所有数据都是二进制数据，而`formdata就是用来收集二进制数据的`
➤ 为什么控制台打印formdata没有看到具体收集的数据？
答：因为`控制台只能输出字符串或者普通的对象数据`，而不是直接输出二进制
➤ 那怎么办呢？
答：使用`formdata提供的内置的api`进行操作
➤ 那些api可以做这个事情呀？例如：forEach
注：后期接口不一定支持formdata

如果后台不支持 formData ，要进行转换成对象
```javascript
				// 获取对象
				let formData = new FormData(document.querySelector('form')); // 对象

				let obj = {};
				formData.forEach((value, key) => {
					obj[key] = value;
				});
				console.log(obj);
```

### 文件上传
多图片上传
```javascript
			document.querySelector('[type="file"]').addEventListener('change', function () {
				// 或者[...this.files]也行
				Array.from(this.files).forEach((value, key) => {
					const formdata = new FormData();
					formdata.append('avatar', value);
					axios({
						url: 'http://ajax-api.itheima.net/api/file',
						method: 'post',
						data: formdata,
					}).then(res => {
						console.log(res);
					});
				});
			});
```

头像上传
```javascript
			document.querySelector('#iptFile').addEventListener('change', function () {
				const formdata = new FormData();
				formdata.append('avatar', this.files[0]);
				axios({
					url: 'http://ajax-api.itheima.net/api/file',
					method: 'post',
					data: formdata,
				}).then(res => {
					document.querySelector('.thumb').src = res.data.data.url;
				});
			});
```

本地预览
```javascript
			document.querySelector('#iptFile').addEventListener('change', function () {
				// 获取file对象
				let myFile = this.files[0];
				// 基于文件对象生成一个内存路径
				let myUrl = URL.createObjectURL(myFile);
				// 更换背景图片
				document.querySelector('.thumb').src = myUrl;
			});
```

### 图书管理📒
```javascript
window.addEventListener('load', function () {
	// 全局配置请求根路径
	axios.defaults.baseURL = 'http://hmajax.itheima.net/api';

	// 渲染数据
	let tbody = document.querySelector('tbody');
	function init() {
		axios.get('/books', { params: { creater: '陈升' } }).then(({ data: res }) => {
			tbody.innerHTML = res.data
				.map((value) => {
					let { id, bookname, author, publisher } = value;

					return `<tr>
                            <th scope="row">${id}</th>
                            <td>${bookname}</td>
                            <td>${author}</td>
                            <td>${publisher}</td>
                            <td>
                                <button data-id=${id} type="button" class="btn btn-link btn-sm btn-delete">删除</button>
                                <button data-id=${id} type="button" class="btn btn-link btn-sm btn-update">编辑</button>
                            </td>
                        </tr>`;
				})
				.join('');
		});
	}
	init();

	// 确认添加
	let addModal = new bootstrap.Modal(document.querySelector('#addModal'));
	let addForm = document.querySelector('#addForm');
	document.querySelector('#addBtn').addEventListener('click', function () {
		let data = serialize(addForm, { hash: true });
		axios.post('/books', { ...data, creater: '陈升' }).then((res) => {
			addModal.hide();
			addForm.reset();
			if (res.status == '201') {
				alert(res.data.message);
				init();
			}
		});
	});

	let editModal = new bootstrap.Modal(document.querySelector('#editModal'));
	let editForm = document.querySelector('#editForm');
	tbody.addEventListener('click', function (e) {
		let id = e.target.dataset.id;

		// 删除
		if (e.target.classList.contains('btn-delete')) {
			axios.delete(`/books/${id}`, { params: { creater: '陈升' } }).then((res) => {
				if (res.status == '204') {
					alert('删除成功');
					init();
				}
			});
		}

		// 编辑
		if (e.target.classList.contains('btn-update')) {
			editModal.show();
			axios.get(`/books/${id}`, { params: { creater: '陈升' } }).then(({ data: res }) => {
				for (let key in res.data) {
					editForm.querySelector(`[name=${key}]`).value = res.data[key];
				}
			});
		}
	});

	// 确认编辑
	document.querySelector('#editBtn').addEventListener('click', function () {
		let data = serialize(editForm, { hash: true });
		axios.put(`/books/${data.id}`, data).then((res) => {
			editModal.hide();
			editForm.reset();
			if (res.status == '200') {
				alert(res.data.message);
				init();
			}
		});
	});
});
```

## 原生ajax
{% btn 'https://developer.mozilla.org/zh-CN/docs/Web/Guide/AJAX/Getting_Started',ajax详解,far fa-hand-point-right,block blue center larger %}

### 基本用法
```javascript
				// 步骤一、创建一个异步对象
				const xhr = new XMLHttpRequest();
				// 步骤二、设置请求方式和请求地址
				xhr.open('post', 'http://hmajax.itheima.net/api/books');
				// 步骤三：设置请求头：对参数编码,如果没有正确的编码格式，参数后台无法正确的接收。
				// post需要，get不需要
				xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
				// 步骤四、发起请求
				xhr.send();
				// 步骤五、接受响应
				xhr.addEventListener('load', function () {
					console.log(JSON.parse(xhr.response));
				});
```

### get请求
```javascript
				let xhr = new XMLHttpRequest();
                
				// get 无参请求
				xhr.open('get', 'http://hmajax.itheima.net/api/books');
				// get 带参请求 【注意：如果get方式有参数，那么参数只能在url后面拼接】
				xhr.open('get', `http://hmajax.itheima.net/api/robot?spoken=${ipt.value}`); // 地址?参数=值&参数=值
				xhr.open('get', `http://hmajax.itheima.net/api/computer/2058`); // 地址/值

				xhr.send();
				xhr.addEventListener('load', function () {
					console.log(JSON.parse(xhr.response));
				});
```

### post请求
```javascript
				let xhr = new XMLHttpRequest();
				xhr.open('post', 'http://hmajax.itheima.net/api/books');

				// post 请求都需要手动进行编码的设置

				// 情况一：参数为key=value&key=value格式：设置格式 Content-type为：application/x-www-form-urlencoded
				xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
				let data = serialize(document.querySelector('#addForm'), { hash: false });
				xhr.send(data);

				// 情况二：参数是对象格式：设置格式 Content-type为：application/json
				xhr.setRequestHeader('Content-Type', 'application/json');
				let data = serialize(document.querySelector('#addForm'), { hash: true });
				xhr.send(JSON.stringify(data));

				// 情况三：参数是formdata对象格式：它的编码格式为：Content-Type:multipart/form-data
				// 浏览器会自动设置请求头，不用我们手动设置
				let formdata = new FormData(document.querySelector('#addForm'));
				xhr.send(formdata);

				xhr.addEventListener('load', function () {
					console.log(JSON.parse(xhr.response));
				});
```

## Promise
{% btn 'https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise',Promise详解,far fa-hand-point-right,block blue center larger %}

**🞇 概念:**
promise是什么：它是一种`解决回调地狱`的处理方式，现在成为ES6的一个标准
promise如何创建：是内置的构造函数
调用 Promise 的时候需要传入一个函数做为参数
这个函数也有两个参数--函数
resolve：操作成功所调用的函数，操作结果可以以参数的形式返回
reject：操作失败所调用的函数，错误信息可以参数的形式返回

**🞇 使用步骤:**
```javascript
// 1. 创建 并传入回调函数
const p = new Promise((resolve,reject)=>{
    // 内部一般封装异步的操作
    // 成功执行 resolve
    // 失败执行 reject
})
// 2. 使用
p.then(res=>{}) // resolve的值
.catch(err=>{}) // reject的值
```

### axios封装
```javascript
function exchange(obj) {
	let arr = [];
	for (let key in obj) {
		arr.push(`${key}=${obj[key]}`);
	}
	return arr.join('&');
}

function axios({ url, method, params, data }) {
	if (!url) throw new Error('参数不能为空');
	method = method.toLowerCase() || 'get';

	// 如果为 get 带参请求
	if (method === 'get' && params && params instanceof Object) {
		url = url + '?' + exchange(params);
	}

	const xhr = new XMLHttpRequest();
	xhr.open(method, url);

	if (method === 'get') {
		xhr.send();
	} else if (method === 'post') {
		if (data instanceof FormData) {
			xhr.send(data);
		} else if (typeof data === 'string') {
			xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
			xhr.send(data);
		} else if (data instanceof Object) {
			xhr.setRequestHeader('Content-Type', 'application/json');
			xhr.send(JSON.stringify(data));
		}
	}

	return new Promise((resolve, reject) => {
		xhr.addEventListener('load', function () {
			resolve(JSON.parse(xhr.response));
		});

		xhr.addEventListener('error', function () {
			reject(JSON.parse(xhr.response));
		});
	});
}
```

### Promise静态方法
```javascript
function createPro(num) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve(num);
		}, num * 1000);
	});
}

let p1 = createPro(1);
let p2 = createPro(2);
let p3 = createPro(3);
let p4 = createPro(4);

// Promise.all：执行所有传入的promise对象，全部执行完毕之后再统一返回结果(将所有promise的执行结果综合在一起) -- 结果是一个数组
// all：返回的也是一个promise
Promise.all([p1, p2, p3, p4]).then((res) => {
	console.log(res);
}).catch((err) => {
	console.log(err);
});

// Promise.race:执行传入的对象，谁第一个执行完毕就立刻结束
Promise.race([p1, p2, p3, p4]).then(res => {
	console.log(res);
})
```

### 分类导航📒
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%88%86%E7%B1%BB%E5%AF%BC%E8%88%AA%E6%A1%88%E4%BE%8B.gif)

```javascript
	axios
		.get('http://ajax-api.itheima.net/api/category/top')
		.then((res) => {
			const list = res.data.data;
			const proArr = list.map((item) => axios.get('http://ajax-api.itheima.net/api/category/sub', { params: { id: item.id } }));
			return Promise.all(proArr);
		})
		.then((res) => {
			document.querySelector('.top').innerHTML = res
				.map((item) => {
					let subHtml = item.data.data.children
						.map((sub) => {
							return `
                                <li>
                                    <a href="javascript:;">
                                        <span>${sub.name}</span>
                                        <img src="${sub.picture}" alt="" />
                                    </a>
                                </li>`;
						})
						.join('');

					return `
                        <li>
                            <a href="javascript:;">${item.data.data.name}</a>
                            <ul class="sub">
                                ${subHtml}
                            </ul>
                        </li>`;
				})
				.join('');
		});
```

### async函数

{% btn 'https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function',async函数介绍,far fa-hand-point-right,block blue center larger %}

```js
			// 创建一个promise -- 封装
			function createPro(num) {
				return new Promise((resolve, reject) => {
					setTimeout(() => {
						resolve('当前的值为' + num);
					}, num * 1000);
				});
			}

			// 将函数标记为async函数：async指异步，说明这个函数中会执行异步操作
			async function doSomthing() {
				// await:会等待当前await后面的函数执行完毕，再继续后续的代码执行
				// await可以将函数的then中的回调参数返回
				// await:可以以同步方式调用异步代码
				// await is only仅仅 valid有效 in在 async functions函数：await仅仅在async函数中使用，意味着我们需要将await所在函数标记为async函数
				// 同步方法：往往有返回值，可以直接接收返回值
				// 异步方法：没有返回值，需要传入回调函数，以回调函数的参数接收返回值
				// await:要求后面的函数返回promise,它只能处理promise对象
				let res = await createPro(1);
				console.log(res);

				let res1 = await createPro(2);
				console.log(res1);

				let res2 = await createPro(3);
				console.log(res2);
			}

			doSomthing();
```

### 个人设置 📒
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E4%B8%AA%E4%BA%BA%E8%AE%BE%E7%BD%AE%E6%A1%88%E4%BE%8B.png)

### 英雄管理 📒
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E8%8B%B1%E9%9B%84%E7%AE%A1%E7%90%86%E6%A1%88%E4%BE%8B.png)

{% btn 'http://210.21.98.31:6003/upload/ajaxapidoc/index.html',英雄管理接口文档,far fa-hand-point-right,block green center larger %}

```js
// 渲染
let tbody = document.querySelector('#tbody');
async function init(value) {
	let {
		data: { data: heroList },
	} = await axios.get('/getHeroList', { params: { heroName: value } });
	tbody.innerHTML = heroList
		.map((item, index) => {
			let { gender, id, img, name } = item;
			return `<tr>
					<td>${index + 1}</td>
					<td>${name}</td>
					<td>${gender}</td>
					<td>
						<img src="${img}" alt="" />
					</td>
					<td>
						<button data-id="${id}" type="button" class="btn btn-info editBtn">编辑</button>
						<button data-id="${id}" type="button" class="btn btn-warning delBtn">删除</button>
					</td>
				</tr>`;
		})
		.join('');
}
init();

// 查询
let btn_search = document.querySelector('#btn_search');
let hname = document.querySelector('#hname');
btn_search.addEventListener('click', function () {
	init(hname.value);
});

// 委托事件（删除/编辑）
let modalTitle = document.querySelector('.modal-title');
tbody.addEventListener('click', async function (e) {
	let id = e.target.dataset.id;
	// 删除
	if (e.target.classList.contains('delBtn')) {
		let res = await axios.get('/delHeroById', { params: { id } });
		alert(res.data.msg);
		init();
	}
	// 编辑
	if (e.target.classList.contains('editBtn')) {
		$('#modal').modal('show');
		modalTitle.innerHTML = '编辑英雄';
		// 数据回填
		let {
			data: { data: info },
		} = await axios.get('/getHeroById', { params: { id } });
		document.querySelector('#modal [name="name"]').value = info.name;
		document.querySelector('#modal [name="gender"]').value = info.gender;
		document.querySelector('#modal [name="img"]').value = info.img;
		document.querySelector('#modal [name="id"]').value = info.id;
		document.querySelector('#modal .edituserimg').src = info.img;
	}
});

// 添加英雄
let openDialog = document.querySelector('.openDialog');
let edituserimg = document.querySelector('.edituserimg');
openDialog.addEventListener('click', function () {
	$('#modal').modal('show');
	modalTitle.innerHTML = '添加英雄';
	// 重置
	document.querySelector('#modal [name="name"]').value = '';
	document.querySelector('#modal [name="gender"]').value = '男';
	document.querySelector('#modal [name="img"]').value = '';
	edituserimg.src = './images/default.jpg';
	addUserImg.value = null;
});

// 上传头像
let addUserImg = this.document.querySelector('#addUserImg');
addUserImg.addEventListener('change', async function () {
	if (this.files.length > 0) {
		let formdata = new FormData();
		formdata.append('file_data', this.files[0]);
		let res = await axios.post('/uploadFile', formdata);
		alert(res.data.msg);
		document.querySelector('#modal [name="img"]').value = res.data.src;
		edituserimg.src = res.data.src;
		document.querySelector('#addUserImg').value = '';
	}
});

// 确认编辑或添加
let btnAdd = document.querySelector('.btnAdd');
let form = document.querySelector('form');
btnAdd.addEventListener('click', async function () {
	let data = serialize(form, { hash: false });

	// 确认添加
	if (modalTitle.innerHTML == '添加英雄') {
		let res = await axios.post('/addHero', data);
		alert(res.data.msg);
		$('#modal').modal('hide');
		init();
	}
	// 确认编辑
	else {
		let res = await axios.post('/updateHero', data);
		alert(res.data.msg);
		$('#modal').modal('hide');
		init();
	}
});
```

