---
title: vue从入门到精通
date: 2022-11-28 00:00:00
updated: 2022-11-28 00:00:00
categories: "vue"
swiper_index: 1
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue.jpeg
toc_expand: true
---

# 介绍
{% btns rounded center grid5 %}
{% cell vue2文档, https://v2.cn.vuejs.org/, fas fa-book-open %}
{% cell vue3文档, https://cn.vuejs.org/, fas fa-book-open %}
{% endbtns %}

{% note success no-icon %}
- Vue是一套用于构建用户界面的{% span red, 渐进式 %}框架。
- 与其它大型框架不同的是，Vue 被设计为可以{% span red, 自底向上逐层 %}应用。
- Vue的核心库只关注{% span red, 视图层 %}，不仅易于上手，还便于与第三方库或既有项目整合。
- 另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的{% span red, 单页应用 %}提供驱动。
{% endnote %}

# 新手入门（直接用 `<script>` 引入）
声明式渲染
Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：
数据和 DOM 被建立了关联，所有东西都是响应式的。
Vue 应用会将其挂载到一个 DOM 元素上 (对于这个例子是 #app) 然后对其进行完全控制。那个 HTML 是我们的入口，但其余都会发生在新创建的 Vue 实例内部。
```html
<div id="app">{{ message }}</div>

<!-- 2选1：开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<!-- 2选1：生产环境版本，优化了尺寸和速度 -->
<!-- <script src="https://cdn.jsdelivr.net/npm/vue@2"></script> -->
<script>
	var app = new Vue({
		el: '#app',
		data: {
			message: 'Hello Vue!',
		},
	});
</script>
```


# vue-cli脚手架（正式入门）
## 创建项目
@vue/cli  也叫 vue脚手架, vue官方提供的一个全局命令工具，可以帮助我们快速的创建一个vue项目的基础架子。
1. 安装脚手架 @vue/cli
```bash
npm i  @vue/cli  -g    
```
2. 创建vue项目
```bash
vue create vue-test(此为项目名)
# ❗项目名不能不含中文特殊符号
# ❗这里暂时手动选择 Vue2 安装
```

3. 运行项目
```bash
cd vue-test 
npm run serve
```

4. 通过修改`vue.config.js`文件来覆盖脚手架下的webpack配置
```diff
const { defineConfig } = require('@vue/cli-service');
module.exports = defineConfig({
	transpileDependencies: true,
+       devServer: { port: 4080 }, // 配置本地开发服务器的端口号
+       lintOnSave: false, // 这里先暂时关闭代码检查
});
```

目录文件简述
```txt
|- node_modules：项目依赖的第三方包
|- pubilc：浏览器运行的页面
  |- favicon.ico：浏览器小图标
  |- index.html：单页面的html文件(网页浏览的就是它)
|- src：业务文件夹
  |- assets：静态资源
    |- logo.png：vue的logo图片
  |- components：组件目录
    |- HelloWorld.vue：欢迎页面的vue代码文件
  |- App.vue：整个应用的根组件
  |- main.js：入口js文件
|- .gitignore：git提交忽略配置
|- babel.config.js：babel配置
|- package.json：依赖包列表
|- vue.config.js：脚手架项目配置文件
|- README.md：项目说明
```

如果编辑器右下角出现 "1 known issue"，则需要在 jsconfig.json 添加 以下代码
```js
	"vueCompilerOptions": {
		"target": 2.7
	}
```

## 组件详解
```html
<!-- template只能有一个根标签 -->
<template>
	<div id="hello">你好，{{ name }}</div>
</template>

<script>
export default {
	name: 'Hello',
	data() {
		return {
			name: '陈升',
		};
	},
};
</script>

<!-- 使用less语法必须安装相关依赖：举例：npm i less less-loader -D -->
<style lang="less" scoped></style>
```

# Vue语法
## 模板语法
### 插值
- 文本
 ```HTML
 <!-- 数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值 -->
 <span>Message: {{ msg }}</span>
 <!-- 通过使用 v-once 指令，也能执行一次性地插值，当数据改变时，插值处的内容不会更新 -->
 <span v-once>这个将不会改变: {{ msg }}</span>
 ```
- 原始HTML
 ```html
 <!-- 双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，需要使用 v-html 指令： -->
 <!-- 此例子相当于在span标签添加了子元素 -->
 <span v-html="strHTML"></span>
 ```
- 属性
 ```html
 <div :id="dynamicId"></div>
 <button :disabled="isButtonDisabled">Button</button>
 ```
- JavaScript表达式
 ```html
 {{ number + 1 }}
 {{ ok ? 'YES' : 'NO' }}
 {{ message.split('').reverse().join('') }}

 <div :id="'list-' + id"></div>

 <!-- ❌这是语句，不是表达式 -->
 {{ var a = 1 }}

 <!-- ❌流控制也不会生效，请使用三元表达式 -->
 {{ if (ok) { return message } }}
 ```

### 指令
- 参数
```js
<a :href="url">...</a>
<a @click="doSomething">...</a>
```
- 动态参数
```html
<a :[attributeName]="url"> ... </a>
<a @[eventName]="doSomething"> ... </a>
```
- 修饰符
```html
<form @submit.prevent="onSubmit">...</form>
```

## 计算属性和侦听器
### 计算属性
> 对于任何复杂逻辑，都应当使用计算属性。

```html
<p>处理之后的信息: "{{ reversedMessage }}"</p>
<script>
	var app = new Vue({
		el: '#app',
		data: {
			message: 'Hello',
		},
		computed: {
			// 计算属性的 getter
			reversedMessage: function () {
				// `this` 指向 app 实例
				return this.message.split('').reverse().join('');
			},
		},
	});
</script>
```
❓但是methods方法也可以实现同样的效果，有啥区别呢
计算属性是基于它们的响应式依赖进行{% span red, 缓存 %}的，只在相关响应式依赖发生改变时它们才会重新求值。
这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。
这也同样意味着下面的计算属性将不再更新，因为 Date.now() 不是响应式依赖：
```js
computed: {
    now: function () {
        return Date.now() // 此处不会更新
    }
}
```


相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。
### 侦听器

## Class 与 Style 绑定
## 条件渲染
## 列表渲染
## 事件处理
## 表单输入绑定
## 组件基础















