---
title: Nodejs 笔记
date: 2022-11-21 00:00:00
tags: 后端
categories: 后端
description: Node.js® 是一个开源、跨平台的 JavaScript 运行时环境。
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/nodejs.jpeg
toc_expand: false
--- 

<div class="btn-center">
{% btn 'https://nodejs.org/zh-cn/',Nodejs中文官网,far fa-hand-point-right,green larger %}
{% btn 'https://nodejs.org/dist/latest-v19.x/docs/api/',Nodejs核心模块,far fa-hand-point-right,green larger %}
</div>

# 介绍&使用
> Node.js 是一个基于 `Chrome V8` 引擎 的 JavaScript 运行时环境
> - chrome V8引擎： <u>在chrome浏览器用来解析和执行js代码的工具</u>；
> - <u>运行时：理解为一个容器，用来运行代码的环境</u>；这个环境<u>让JS有读写文件，操作数据库，开启web服务器等能力</u>
> - 注意：Node.js 只是`JS的服务端运行环境`，不是一门语言（`不需要学习新语言`）,而只需要学习它里面新的Api

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Node.js%20%E4%BD%9C%E7%94%A8.png)

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Node.js%20%E7%8E%AF%E5%A2%83%E4%B8%8E%E6%B5%8F%E8%A7%88%E5%99%A8%E7%8E%AF%E5%A2%83%E7%9A%84%E5%8C%BA%E5%88%AB.png)
- 在浏览器端：js由三部分组成：ECMAScript + BOM + DOM
- 在NoeJS端：由ECMAScript + 内置模块(fs, http, path等) + 第三方模块(别人开发的模块)
- `注意：NodeJS中没有DOM，也没有BOM，也没有window对象`。
- 浏览器是JS的前端运行环境，Node.js是JS的后端运行环境

**⭐总结**
- `浏览器`是JS的一个前端运行环境，`Node.js`是JS的后端运行环境。
- Chrome与NodeJS都是基于`V8`引擎执行JS的。
- Node.js与浏览器最大的不同点在于，Node.js可以让JS读写`文件`、操作`数据库`、开启`WEB服务器` 而浏览器可以让JS操作`DOM`和`BOM`。
- 所以我们学Node.js的`内置模块`、`自定义模块`、`第三方模块`以及学习npm工具来安装，卸载，发布第三方包，为后面学习Vue等框架打下基础。

**在Node环境下运行js代码**
```node
C:\Users\csheng\Desktop\test>node test.js
> 测试

alert(1)，node执行时时会报错，因为node环境中没有window不能操作dom
```

**学习常用的命令及按键**
```txt
node '某个js文件'            调用 node 程序，运用某个js文件
clear 或者 cls               清空界面
ls/list/dir                 查看列表（list）
cd 目录名                    进入到目录中去
cd ..                       返回上一级目录
cd \                        直接回到根目录
ctrl + C                      停止Node 程序
输入部分文件名后按下“Tab”键    补全文件名 或 目录名，多次 tab 会进行切换
↑↓ 上下箭头                   切换历史输入
```
---

# 文件读写fs
## 文件读取
```js
const fs = require('fs');

// ⭐异步读取
fs.readFile('./1.txt', 'utf-8', (err, data) => {
	if (err) {
		console.log('读取文件出错了:' + err);
	} else {
		console.log('读取成功:' + data);
	}
});

// ⭐同步读取
try {
	// 读取文件
	const fileData = fs.readFileSync('./test.txt', 'utf-8');
	// 读取图片
	const imgData = fs.readFileSync('./test.jpg');

	console.log('读取文件成功：' + fileData);
	console.log('读取图片成功：' + imgData);
} catch (error) {
	console.log('读取报错：' + error);
}
```

## 文件写入
```js
const fs = require('fs');

// ⭐异步写入
fs.writeFile('./1.txt', '我是异步写入的内容', 'utf-8', (err) => {
	if (err) {
		console.log('写入失败 :>> ', err);
	} else {
		console.log('写入成功');
	}
});

// ⭐同步写入
fs.writeFileSync('./2.txt', '我是同步写入的内容', 'utf-8');
console.log('写入成功');
```

## 综合案例📝
**案例一**: **往data.json文件添加新的内容**
```js
const fs = require('fs');
const data = fs.readFileSync('./db/data.json', 'utf-8');
const arr = JSON.parse(data);
arr.push({
	name: '小升',
	skill: '打羽毛球',
});
fs.writeFileSync('./db/data.json', JSON.stringify(arr, null, 2), 'utf-8');
```

**案例二**: **拷贝图片**
```js
// 描述：请使用fs模块的读写方法将3.jpg拷贝一份为4.jpg
const fs = require('fs');
// 读取
let res = fs.readFileSync('./3.jpg');
// 写入
fs.writeFileSync('./4.jpg', res);
```

---

# Http开启web服务器
## 实现Web服务器
```js
// 1）导入 http 模块
const http = require('http');
// 2）创建一个服务
let server = http.createServer((req, res) => {
	console.log('接收到了客户端请求');
	res.end('OK');
});
// 3）监听 8001 端口并启动 web 服务器等待客户端请求
server.listen(8001, () => {
	console.log('web服务器准备就绪：127.0.0.1:8001可以访问');
    // 输入 http://局域网ip:8001/ 回车访问
});
```

```js
let server = http.createServer((req, res) => {
	// ⭐ req.ul 
	// 当我们访问 127.0.0.1:8001，输出：/
	// 当我们访问 127.0.0.1:8001/test，输出：/test
	console.log(req.url); 
	// ⭐ req.method：获取当前请求的类型
	console.log(req.method); 

	// ⭐ res.setHeader()：设置响应头 
	// 语法：res.setHeader(响应头，响应值)
	// ❗res.setHeader() 在 res.end() 之前才有效，
	// ❗res.setHeader() 可以设置多次
	// 应用举例：解决响应中文字符在浏览器显示成乱码问题
	res.setHeader('Content-Type', 'text/plain;charset=utf-8');

	// ⭐ res.statusCode：设置状态码 
	// 语法：res.statusCode = 状态码，状态码是http协议规定的
	// 500 （服务器异常）    404（资源找不到）
 	// ❗res.statusCode 只有在 res.end() 前执行才有效
	res.statusCode = 404;

	// ⭐ res.end(响应的数据) ：设置响应体并结束本次请求 
	// ❗end() 只能传入 buffer 或者是 String 类型的数据】
	// ❗一次请求只能有一个 res.end() 响应，多个以第一个响应的数据为准，同时服务器报错终止
	// res.end(req.url);
	res.end('ok');
});1
```
{% note success no-icon flat %}
【补充知识】常见的几种文件类型及content-type
```js
- .txt：res.setHeader('content-type', 'text/plain;charset=utf8')
- .html：res.setHeader('content-type', 'text/html;charset=utf8')
- .css：res.setHeader('content-type', 'text/css;charset=utf8')
- .js：res.setHeader('content-type', 'application/javascript')
- .png：res.setHeader('content-type', 'image/png')
- .json：res.setHeader('content-type', 'application/json;charset=utf-8')
```
如果读取.html的文件内容，但是content-type设置为了text/css则浏览器将不会当作是html页面来渲染了
{% endnote %}


## 处理接口响应
### GET接口
```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
	const [url, query] = req.url.split('?');

	// 不带参数的 get 接口（请求得到所有数据）
	if (req.url === '/getHero' && req.method === 'GET') {
		// 读取处理数据库，并转换为数组
		let content = fs.readFileSync('./db/data.json', 'utf-8');
		res.setHeader('Content-Type', 'application/json;charset=utf-8');

		res.end(content);
	}

	// 带参数的 get 接口（请求得到指定的数据）
	else if (url === '/getHero' && req.method === 'GET') {
		// 读取处理数据库，并转换为数组
		let content = fs.readFileSync('./db/data.json', 'utf-8');
		content = JSON.parse(content);
		// 根据参数筛选出对应的数据，假设这里query的值为 name=盖德
		const urlSearch = new URLSearchParams(query);
		const result = content.filter((item) => item.name == decodeURIComponent(urlSearch.get('name')));
		res.setHeader('Content-Type', 'application/json;charset=utf-8');

		res.end(JSON.stringify(result));
	} else {
		res.end('error');
	}
});

server.listen(8001, () => console.log('web服务器准备就绪：127.0.0.1:8001可以访问'));
```

### POST接口
> 场景：在server.js中写代码，提供一个名为add的接口（<http://localhost:8003/add>），它以post的方式请求接口，并传入name和skinname值，把数据保存到db/data.json中去。

前置知识
- post请求参数在请求体中，内容比较大(上传图片，上传文件....)
- 后端是一段一段接收数据的，并不像get在请求行中传递的数据：直接写在url中的查询字符串内，可以立即通过req.url来解析出来

在接收参数的过程中，会涉及req对象的两个事件data,end
- data事件，每次收到一部分参数数据就会触发一次这个事件。
- end事件，全部的参数数据接收完成之后会执行一次。

```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
	let result = '';
	req.on('data', (chunk) => (result += chunk));

	req.on('end', () => {
		// 在 createServer 的回调函数中通过 data 和 end 事件获取 post 请求体中的参数
		let content = fs.readFileSync('./db/data.json', 'utf-8');
		content = JSON.parse(content);
		content.push(JSON.parse(result));

		// 通过fs.writeFileSync将数据写入到db/data.json中
		fs.writeFileSync('./db/data.json', JSON.stringify(content, null, 2), 'utf-8');

		// 使用res.end()将成功或失败的结果响应回浏览器
		res.end('成功');
	});
});

server.listen(8003, () => {
	console.log(8003);
});
```

# 模块化与包管理工具 
## 模块化
{% note success no-icon flat %}
{% label 每个js文件看作是一个模块，每个模块通过固定的方式引入，并且通过固定的方式向外暴露指定的内容 blue %}
- 能够对一类功能做很好的分类管理
- 能够保护成员不被污染
- 不用考虑导入顺序
- {% label 按需导入 pink %}，可以随时更换模块，维护方便
{% endnote %}

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%A8%A1%E5%9D%97%E5%8C%96%E8%8C%83%E4%BE%8B.png)

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%91%BD%E5%90%8D%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA.png)
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E9%BB%98%E8%AE%A4%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA.png)

## NPM包管理工具
node 常用命令（下面举`express`模块为例）
```Bash 
npm init -y #初始化

#切换镜像
#【方法一：直接配置淘宝源】
npm config set registry https://registry.npmmirror.com/ #直接配置淘宝源
npm config get registry #验证命令
#【方法二：通过安装 nrm 切换到淘宝镜像】
npm i nrm -g #安装 nrm
nrm ls #查看有哪些可用镜像
nrm test #测试仓库速度
nrm use taobao #切换到淘宝镜像

#安装模块
npm i '模块名' #等价于npm i '模块名' -S/--save
npm i '模块名' -g #全局安装
npm i '模块名' -S/--save #生产环境
npm i '模块名' -D/--save-dev #开发环境
npm i '模块名'@4.0.0 -g #安装指定版本号的模块

#查看模块
npm root -g #查看全局npm包安装位置
npm ls/list #列出当前目录已安装模块
npm ls -g --depth=0 #列出全局已安装模块

#更新模块
npm update #升级当前目录下的项目的所有模块
npm update '模块名' #升级当前目录下的项目的指定模块
npm update -g '模块名' #升级全局安装的'模块名'模块

#卸载模块
npm uninstall/un/r '模块名' #删除指定的模块

# bootstrap依赖包没有跟着bootstrap一起下载的解决办法
npm install -g install-peerdeps # 1）安装
install-peerdeps bootstrap # 2）运行 
```


# webpack
{% btn 'https://webpack.docschina.org/','webpack中文文档',far fa-hand-point-right,block green center larger %}

> ➤ 定义：一个现代 JavaScript 应用程序的{% label 模块打包器 pink %}，{% label 分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Sass，TypeScript，vue等），并将其转换和打包为合适的格式供浏览器使用 blue %}
> ➤ 好处：让程序员把工作的重心放到具体功能的实现上，{% label 提高了前端开发效率和项目的可维护性 orange %}

```Bash
⭐下载安装 (需要安装 webpack 相关的两个包)
- npm init -y
- npm install webpack webpack-cli --save-dev
```