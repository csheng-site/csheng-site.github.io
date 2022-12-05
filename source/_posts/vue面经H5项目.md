---
title: vue面经H5项目
date: 2022-12-05 00:00:00
categories: "vue"
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E9%9D%A2%E7%BB%8FH5.png
sticky: 3
toc_expand: true
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

新建src/utils/request.js
```js
import axios from 'axios';

axios.defaults.baseURL = 'http://interview-api-t.itheima.net/h5/';

// 添加请求拦截器
axios.interceptors.request.use(
	function (config) {
		// 在发送请求之前做些什么
		return config;
	},
	function (error) {
		// 对请求错误做些什么
		return Promise.reject(error);
	}
);
// 添加响应拦截器
axios.interceptors.response.use(
	function (response) {
		// 2xx 范围内的状态码都会触发该函数。
		// 对响应数据做点什么
		return response;
	},
	function (error) {
		// 超出 2xx 范围的状态码都会触发该函数。
		// 对响应错误做点什么
		return Promise.reject(error);
	}
);

export default axios;
```

### 封装接口
新建src/api/use.js
```js
import request from '../utils/request';

export function register(userInfo) {
	return request.post('/user/register', userInfo);
}
```

### 封装token
新建src/utils/token.js
```js
const storageKey = 'h5-token';

export function saveToken(token) {
	localStorage.setItem(storageKey, token);
}

export function getToken() {
	return localStorage.getItem(storageKey);
}

export function deleteToken() {
	localStorage.removeItem(storageKey);
}
```


## 登录注册页面
### 配置响应拦截器
在src/utils/request.js拿到接口报错
```diff
# vant的Notify插件有点特殊，只需导入，无需use注册
+ import { Notify } from 'vant';

axios.interceptors.response.use(
	function (response) {
		// 2xx 范围内的状态码都会触发该函数。
		// 对响应数据做点什么
		return response;
	},
	function (error) {
		// 超出 2xx 范围的状态码都会触发该函数。
		// 对响应错误做点什么
+               const msg = error.response.data.message;
+               if (msg) {
+       	        Notify(msg);
+               }

		return Promise.reject(error);
	}
);
```

### 身份验证token
成功登录之后，获取到的token存储到本地，并设置请求头，供访问的其他页面的请求用

### 登录注册实现
{% tabs 登录注册实现 %}
<!-- tab 注册功能 -->
user-register.vue
```html
<template>
	<div id="user-register">
		<van-nav-bar title="用户注册" />

		<van-form @submit="onSubmit">
			<van-field v-model="username" name="username" label="用户名" placeholder="用户名" :rules="[{ required: true, message: '请填写用户名' }]" />
			<van-field v-model="password" type="password" name="password" label="密码" placeholder="密码" :rules="[{ required: true, message: '请填写密码' }]" />
			<div style="margin: 16px">
				<van-button block type="primary" native-type="submit">注册</van-button>
			</div>
		</van-form>

		<div class="go-to-login">
			<router-link to="/login">有账号，去登录</router-link>
		</div>
	</div>
</template>

<script>
import { register } from '../api/user';

import { Notify } from 'vant';

export default {
	name: 'UserRegister',
	data() {
		return { username: '', password: '' };
	},
	methods: {
		async onSubmit() {
			await register({ username: this.username, password: this.password });
			Notify({ type: 'success', message: '恭喜您，注册成功' });
			this.$router.push('/login');
		},
	},
};
</script>

<style lang="less" scoped>
.go-to-login {
	text-align: right;
	padding-right: 16px;
}
</style>
```
<!-- endtab -->

<!-- tab 登录功能 -->
user-login.vue
```html
<template>
	<div id="user-register">
		<van-nav-bar title="面经登录" />

		<van-form @submit="onSubmit">
			<van-field v-model="username" name="username" label="用户名" placeholder="用户名" :rules="[{ required: true, message: '请填写用户名' }]" />
			<van-field v-model="password" type="password" name="password" label="密码" placeholder="密码" :rules="[{ required: true, message: '请填写密码' }]" />
			<div style="margin: 16px">
				<van-button block color="#fa6d1d" native-type="submit">登录</van-button>
			</div>
		</van-form>

		<div class="go-to-register">
			<router-link to="/register">注册账号</router-link>
		</div>
	</div>
</template>

<script>
import { login } from '../api/user';

import { Notify } from 'vant';

import { saveToken } from '../utils/token';

export default {
	name: 'UserRegister',
	data() {
		return { username: '', password: '' };
	},
	methods: {
		async onSubmit() {
			const result = await login({ username: this.username, password: this.password });
			saveToken(result.data.data.token);
			Notify({ type: 'success', message: '登录成功' });
			this.$router.push('/');
		},
	},
};
</script>

<style lang="less" scoped>
.go-to-register {
	text-align: right;
	padding-right: 16px;
}
</style>
```
<!-- endtab -->
{% endtabs %}

## 首页
{% tabs 首页 %}
<!-- tab 代码实现过程 -->
思路：涉及到 嵌套路由 的知识
1. 新建4个页面，(article-best / my-collection / my-favorite / my-center)
2. 在home-page.vue配置子页面的挂载点
```html
<router-view></router-view>
<!-- 底部导航栏 -->
<van-tabbar active-color="#fa6d1d">
   ......
</van-tabbar>
```
1. 在src/router/index.js导入子路由，并给首页配置重定向以及嵌套路由（children）
```js
const routes = [
	{
		path: '/',
		name: 'HomePage',
		component: () => import('../views/home-page.vue'),
		redirect: '/article',
		children: [
			{ path: '/article', component: () => import('../views/best-article.vue') },
			{ path: '/collection', component: () => import('../views/my-collection.vue') },
			{ path: '/favorite', component: () => import('../views/my-favorite.vue') },
			{ path: '/my', component: () => import('../views/my-center.vue') },
		],
	},
    ......
];
```
<!-- endtab -->

<!-- tab 首页导航完整代码 -->
```html
<template>
	<div id="home-page">
		<router-view></router-view>
		<!-- 底部导航栏 -->
		<van-tabbar route active-color="#fa6d1d">
			<van-tabbar-item to="/article" icon="notes-o">面经</van-tabbar-item>
			<van-tabbar-item to="/collection" icon="star-o">收藏</van-tabbar-item>
			<van-tabbar-item to="/favorite" icon="like-o">喜欢</van-tabbar-item>
			<van-tabbar-item to="/my" icon="manager-o">我的</van-tabbar-item>
		</van-tabbar>
	</div>
</template>

<script>
export default {
	name: 'HomePage',
};	
</script>

<style lang="less" scoped></style>
```
<!-- endtab -->
{% endtabs %}



