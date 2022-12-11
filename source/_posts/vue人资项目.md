---
title: vue人资项目
date: 2022-12-10 00:00:00
categories: "vue"
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE%E5%B0%81%E9%9D%A2.png
sticky: 5
---

# 项目准备
## 接口&展示图
{% btn 'https://www.apifox.cn/apidoc/project-934563/api-19465917','接口文档',far fa-hand-point-right,block center green larger %}

{% note success no-icon flat %}
接口根路径: &nbsp; `http://interview-api-t.itheima.net/`
账号：admin，密码：admin
技术栈: vue2、vue-router、vuex、sass、其他(Element-UI、axios、Echarts、富文本编辑-vue-quill-editor))
{% endnote %}

{% tabs 页面展示 %}
<!-- tab 登录页 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E7%99%BB%E5%BD%95%E9%A1%B5.png)
<!-- endtab -->
<!-- tab 首页 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E9%A6%96%E9%A1%B5.png)
<!-- endtab -->
<!-- tab 面经管理（列表） -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E9%9D%A2%E7%BB%8F%E7%AE%A1%E7%90%86%20-%20%E5%88%97%E8%A1%A8%E5%B1%95%E7%A4%BA.png)
<!-- endtab -->
<!-- tab 面经管理（预览） -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E9%9D%A2%E7%BB%8F%E7%AE%A1%E7%90%86%20-%20%E9%A2%84%E8%A7%88%E6%95%88%E6%9E%9C.png)
<!-- endtab -->
<!-- tab 面经管理（删除） -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E9%9D%A2%E7%BB%8F%E7%AE%A1%E7%90%86%20-%20%E5%88%A0%E9%99%A4%E5%8A%9F%E8%83%BD.png)
<!-- endtab -->
<!-- tab 面经管理（添加） -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E9%9D%A2%E7%BB%8F%E7%AE%A1%E7%90%86%20-%20%E6%B7%BB%E5%8A%A0%E5%8A%9F%E8%83%BD.png)
<!-- endtab -->
<!-- tab 面经管理（修改） -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E9%9D%A2%E7%BB%8F%E7%AE%A1%E7%90%86%20-%20%E4%BF%AE%E6%94%B9%E5%8A%9F%E8%83%BD.png)
<!-- endtab -->
{% endtabs %}

## 创建项目

```bash
vue create cms # 新建vue项目
```
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/cms%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE.png)

```bash
yarn add element-ui axios echarts vue-quill-editor # 安装相关依赖
yarn serve # 运行vue项目
```


## 按需导入element-ui

{% btn 'https://element.eleme.cn/#/zh-CN/component/quickstart','element-ui官方文档',far fa-hand-point-right,block orange center larger %}

减轻将来打包后的包的体积

- 安装`babel-plugin-component`

```jsx
yarn add babel-plugin-component -D
```

- 在` babel.config.js `中配置

```jsx
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset'
  ],
  // 新增plugins插件节点,修改完配置文件一定重启项目
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
```

- 新建`src/utils/element-ui.js`，完整按需导入如下：

```jsx
import Vue from 'vue';
import { Button } from 'element-ui';

Vue.use(Button);
```

- 直接导入main.js中

```jsx
import '@/utils/element-ui.js'
```

## 清理脚手架
- 清理`App.vue`

```jsx
<template>
	<router-view />
</template>
```

- 删除`views`下的所有文件，并新建`index.vue`和`login.vue`

- 在`src/router/index.js`注册路由

```jsx
import Vue from 'vue';
import VueRouter from 'vue-router';

Vue.use(VueRouter);

const routes = [
	{ path: '/', component: () => import('../views/index.vue') },
	{ path: '/login', component: () => import('../views/login.vue') },
];

const router = new VueRouter({
	routes,
});

export default router;
```

## 导入公共样式
- 新建`src/styles/index.scss`

```scss
// 修改主题色
$--color-primary: rgba(114, 124, 245, 1);
$--font-path: '~element-ui/lib/theme-chalk/fonts';
@import '~element-ui/packages/theme-chalk/src/index';

body {
	margin: 0;
	padding: 0;
	background: #fafbfe;
}
```

- 在`main.js`导入

```scss
import './styles/index.scss';
```

- 把用到的图片，全部存放在 `assets`


# 模块封装

## request模块 - axios封装

接口文档地址：https://www.apifox.cn/apidoc/project-934563/api-19465917

我们会使用 axios 来请求后端接口, 一般都会对 axios 进行一些配置 (比如: 配置基础地址等)

一般项目开发中, 都会对 axios 进行基本的二次封装, 单独封装到一个模块中, 便于使用

- 新建 `utils/request.js` 封装 axios 模块

   利用 axios.create 创建一个自定义的 axios 来使用

   http://www.axios-js.com/zh-cn/docs/#axios-create-config

```js
/* 封装axios用于发送请求 */
import axios from 'axios'

// 创建一个新的axios实例
const request = axios.create({
  baseURL: 'http://interview-api-t.itheima.net/',
  timeout: 5000
})

// 添加请求拦截器
request.interceptors.request.use(function (config) {
  // 在发送请求之前做些什么
  return config
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error)
})

// 添加响应拦截器
request.interceptors.response.use(function (response) {
  // 对响应数据做点什么
  return response.data
}, function (error) {
  // 对响应错误做点什么
  return Promise.reject(error)
})

export default request
```



## storage模块 - 本地存储

新建 utils/storage.js

```jsx
// 以前 token 令牌，如果存到了本地，每一次都写这么长，太麻烦
// localStorage.setItem(键， 值)
// localStorage.getItem(键)
// localStorage.removeItem(键)

const KEY = 'my-token-element-pc'

// 直接用按需导出，可以导出多个
// 但是按需导出，导入时必须 import { getToken } from '模块名导入'

// 获取
export const getToken = () => {
  return localStorage.getItem(KEY)
}

// 设置
export const setToken = (newToken) => {
  localStorage.setItem(KEY, newToken)
}

// 删除
export const delToken = () => {
  localStorage.removeItem(KEY)
}
```



## 路由设计配置

但凡是单个页面，独立展示的，都是一级路由

路由设计：

- 登录页 （一级） Login
- 首页架子（一级） Layout
  - 数据看板（二级）Dashboard
  - 文章管理（二级）Article

### 新建目录

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E8%B7%AF%E7%94%B1%E8%AE%BE%E8%AE%A1%E9%85%8D%E7%BD%AE-%E6%96%B0%E5%BB%BA%E7%9B%AE%E5%BD%95.png)

`login/index.vue`

```jsx
<template>
  <div>我是一级登录</div>
</template>

<script>
export default {
  name: 'LoginIndex'
}
</script>

<style>

</style>
```

`layout/index.vue`

```jsx
<template>
  <div>我是一级首页架子</div>
</template>

<script>
export default {
  name: 'LayoutIndex'
}
</script>

<style>

</style>
```

`dashboard/index.vue`

```jsx
<template>
  <div>我是二级数据看板页面</div>
</template>

<script>
export default {
  name: 'DashBoard'
}
</script>

<style>

</style>
```

`article/index.vue`

```jsx
<template>
  <div>我是二级文章管理页</div>
</template>

<script>
export default {
  name: 'ArticleIndex'
}
</script>

<style>

</style>
```



### 配置路由

`router/index.js`

```jsx
import VueRouter from 'vue-router'
import Vue from 'vue'

import Layout from '@/views/layout'
import Login from '@/views/login'
import Dashboard from '@/views/dashboard'
import Article from '@/views/article'

Vue.use(VueRouter)

const router = new VueRouter({
  routes: [
    { path: '/login', component: Login },
    {
      path: '/',
      component: Layout,
      redirect: '/dashboard',
      children: [
        { path: 'dashboard', component: Dashboard },
        { path: 'article', component: Article }
      ]
    }
  ]
})

export default router
```

`layout/index` 配置二级路由出口

```jsx
<template>
  <div>
    <div>头部</div>
    <div>侧边</div>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'LayoutIndex'
}
</script>

<style>

</style>
```

测试路径1： http://localhost:8080/#/login

测试路径2： http://localhost:8080/#/dashboard

测试路径3： http://localhost:8080/#/article





# 登录模块

## 基本表单

```jsx
<template>
	<div id="login">
		<el-card class="box-card">
			<div slot="header" class="clearfix">
				<span>黑马面经运营后台</span>
			</div>

			<!-- 登录表单 -->
			<el-form label-position="top" label-width="80px" :model="form">
				<el-form-item label="用户名">
					<el-input v-model="form.username"></el-input>
				</el-form-item>
				<el-form-item label="密码">
					<el-input type="password" v-model="form.password"></el-input>
				</el-form-item>
				<el-form-item class="login-operation">
					<el-button type="primary" @click="onSubmit">登录</el-button>
					<el-button>重置</el-button>
				</el-form-item>
			</el-form>
		</el-card>
	</div>
</template>

<script>
export default {
	name: 'Login',
	data() {
		return {
			form: { username: '', password: '' },
		};
	},
	methods: {
		onSubmit() {
			console.log('提交');
		},
	},
};
</script>

<style lang="scss" scoped>
#login {
	display: flex;
	justify-content: center;
	align-items: center;
	height: 100vh;
	background: url(../assets/login-bg.svg) no-repeat center / 150%;
	.box-card {
		width: 30%;
		::v-deep .el-card__header {
			text-align: center;
			color: #fff;
			background-color: #727cf5;
		}
		.login-operation {
			text-align: center;
		}
	}
	.el-form {
		padding: 0 20px;
	}
}
</style>
```

## 表单校验
el-form: model 属性，rules 规则
el-form-item: 绑定 prop 属性
el-input: 绑定 v-model

### element-ui 基本校验

说明：在向后端发请求调用接口之前，我们需要对所要传递的参数进行验证，把用户的错误扼杀在摇篮之中。

讲解内容:

- element-ui的校验

  - el-form:  `model`属性, `rules`规则

  - el-form-item:  绑定 `prop` 属性

  - el-input:   绑定` v-model`

Form 组件提供了表单验证的功能

1.  form组件需要 `:model`绑定form对象（必须）， 需要通过 `rules` 属性传入约定的验证规则 

```jsx
<el-form :model="form" :rules="rules">
    
export default {
  data() {
    return {
      form: {
        username: '',
        password: ''
      }
    }
  }
}
```

2. 在 data 中准备 rules 规则

```js
rules: {
  username: [
    { required: true, message: '请输入用户名', trigger: ['blur', 'change'] },
    { min: 5, max: 11, message: '长度在 5 到 11 个字符', trigger: ['blur', 'change'] }
  ]
}
```

3.  将 Form-Item 的 `prop` 属性设置为需校验的字段名 

```html
<el-form-item label="用户名：" prop="username">
  <el-input v-model="form.username" placeholder="请输入手机号" />
</el-form-item>
```



### element-ui 正则校验

下面是常用内置的基本验证规则：其余校验规则参见 [async-validator](https://github.com/yiminghe/async-validator)

| 规则     | 说明                                           |
| -------- | ---------------------------------------------- |
| required | 必须的，例如校验内容是否非空                   |
| pattern  | 正则表达式，例如校验手机号码格式、校验邮箱格式 |

```jsx
rules: {
  username: [
    { required: true, message: '请输入用户名', trigger: ['blur', 'change'] },
    { min: 5, max: 11, message: '长度在 5 到 11 个字符', trigger: ['blur', 'change'] }
  ],
  password: [
    { required: true, message: '请输入密码', trigger: ['blur', 'change'] },
    { pattern: /^\w{5,11}$/, message: '请输入 5 到 10 位的密码', trigger: ['blur', 'change'] }
  ]
}

// \d 数字 0-9
// \w 字母数字下划线
// {m,n} 前面的字符，可以出现 m次 ~ n次
```

不要忘了配置prop

```html
<el-form-item prop="password">
```

上述已经可以完成大部分需求，如果需要更复杂业务校验需求，可以自定义校验~ （项目课程：人力资源系统会进一步讲解）



### 提交表单校验 和 重置

每次点击按钮, 进行ajax登录前, 应该先对整个表单内容校验, 不然还是会发送很多无效的请求!!!

要通过校验了, 才发送请求!!!

**作用: `ref` 属性配合 `$refs` 可以获取 dom 元素 (或者 vue组件实例)**

1. 给组件或者元素, 添加  ref 属性

```html
<hello ref="bb"></hello>
```

2. 通过 this.$refs 可以获取对应的引用, 并且调用方法

```jsx
this.$refs.bb.sayHi()
```

**添加登录提交的校验**

```js
<el-form ref="form" :model="form" :rules="rules" autocomplete="off">
...
<el-button @click="login" type="primary">登 录</el-button>

methods: {
  async login () {
    try {
      const valid = await this.$refs.form.validate()
      console.log(valid)
    } catch (e) {
      console.log(e)
    }
  }
}
```

**添加重置功能**

```jsx
<el-button @click="reset">重 置</el-button>

methods: {
  reset () {
    this.$refs.form.resetFields()
  }
}
```



### 封装登录api登录请求

新建 `api/user.js` 提供api接口函数

```jsx
import request from '@/utils/request'

export const login = ({ username, password }) => {
  return request.post('/auth/login', {
    username,
    password
  })
}
```

发送请求获取token

```jsx
methods: {
  async login () {
    try {
      const valid = await this.$refs.form.validate()
      if (valid) {
        const res = await login(this.form)
        console.log(res)
      }
    } catch (e) {
      console.log(e)
    }
  }
}
```

### vuex user 模块 - 存token

新建 `store/modules/user.js`

```jsx
import { getToken, setToken } from '@/utils/storage'

export default {
  namespaced: true,
  state () {
    return {
      token: getToken()
    }
  },
  mutations: {
    setToken (state, user) {
      state.token = user.token
      setToken(user.token)
    }
  }
}
```

挂载模块

```jsx
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user'

Vue.use(Vuex)

export default new Vuex.Store({
  modules: {
    user
  }
})
```

登录时调用

```jsx
async login () {
  try {
    const valid = await this.$refs.form.validate()
    if (valid) {
      const res = await login(this.form)
      this.$store.commit('user/setToken', res.data)
      this.$router.push('/')
    }
  } catch (e) {
    console.log(e)
  }
},
```



### 登录访问拦截

`router/index.js`

没有token 且 访问的不是 登录页，就直接拦截到登录

```jsx
router.beforeEach((to, from, next)=>{
  const { token } = store.state.user;
  if (to.path !== '/login' && !token ) return next('/login')
  next()
})
```

# 首页
## 登录检查

在`router/index.js`配置导航守卫

```jsx
// 导入 Vuex 仓库
import store from '../store';

const router = new VueRouter({
	routes,
});

// 配置导航守卫
router.beforeEach((to, from, next) => {
	if (to.path === '/login') {
		// 登录不需要检查
		next();
	} else if (store.state.user.token) {
		// 有 token，已经登陆，允许访问
		next();
	} else {
		// 没有 token，也不是登录页，阻止访问，跳转登录
		next('/login');
	}
});
```

## 请求头设置和首页接口封装

- 复制自带的`login.vue`代码和`element-ui.js`代码

- 在`src/utils/request.js`设置请求头

```jsx
// 添加请求拦截器
instance.interceptors.request.use(
	function (config) {
		// 在发送请求之前做些什么
		const { token } = store.state.user;
		if (token) config.headers.Authorization = `Bearer ${token}`;

		return config;
	},
	function (error) {
		// 对请求错误做些什么
		return Promise.reject(error);
	}
);
```

- 在`src/api/user.js`封装获取首页数据的接口

```jsx
export function getUser() {
	return request.get('/auth/currentUser');
}
```



