---
title: vue面经H5项目
date: 2022-12-05 00:00:00
categories: "vue"
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E9%9D%A2%E7%BB%8FH5.png
sticky: 3
---

## 前期准备
{% tabs 面经H5项目 %}
<!-- tab 效果图 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E9%9D%A2%E7%BB%8FH5%E6%95%88%E6%9E%9C%E5%9B%BE.png)
<!-- endtab -->

<!-- tab 前期准备 -->
技术栈：vue2、Router、axios、vantUI(UI框架)、less

![项目创建选项](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E9%9D%A2%E7%BB%8FH5%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE%E9%80%89%E9%A1%B9.png)

安装vant2：npm i vant@latest-v2 -S
安装axios：npm i axios

导入vant（自动按需导入）：
1.安装babel：npm i babel-plugin-import -D
2.在babel.config.js配置babel：
```js
module.exports = {
	plugins: [
		[
			'import',
			{
				libraryName: 'vant',
				libraryDirectory: 'es',
				style: true,
			},
			'vant',
		],
	],
};
```
3.新建src/utils/vant.js：
```js
// 抽离导入vant的代码
import Vue from 'vue';
import { Button } from 'vant';

// use是Vue提供的注册插件的方法
Vue.use(Button);
```
4.在main.js导入抽离的vant
```js
import './utils/vant';
```
5.重新运行 npm run serve

清理工作
1.清理app.vue
```html
<template>
	<router-view />
</template>
```
2.清理路由
- 删除路由配置
```js
const routes = []; // 上面的import导入也记得删掉
```
- 删除页面组件：views里的所有文件

创建3个页面：home-page、user-login、user-register，随便写点内容
路由配置文件：
```js
const routes = [
	{ path: '/', name: 'HomePage', component: () => import('../views/home-page.vue') },
	{ path: '/register', name: 'UserRegister', component: () => import('../views/user-register.vue') },
	{ path: '/login', name: 'UserLogin', component: () => import('../views/user-login.vue') },
];
```
<!-- endtab -->
{% endtabs %}

## 调用接口
### 封装axios
{% btn 'https://axios-http.com/zh/docs/interceptors','axios拦截器文档',far fa-hand-point-right,block orange center larger %}


### 封装接口

## 登录注册页面
