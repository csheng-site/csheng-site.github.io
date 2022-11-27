---
title: vue从入门到精通
date: 2022-11-28 00:00:00
updated: 2022-11-28 00:00:00
categories: "vue"
swiper_index: 1
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue.jpeg
---

{% btns rounded center grid5 %}
{% cell vue2文档, https://v2.cn.vuejs.org/, fas fa-book-open %}
{% cell vue3文档, https://cn.vuejs.org/, fas fa-book-open %}
{% endbtns %}


{% note success no-icon %}
- Vue是一套用于构建用户界面的{% span red, 渐进式 %}框架。
- 与其它大型框架不同的是，Vue 被设计为可以{% span red, 自底向上 %}逐层应用。
- Vue的核心库只关注{% span red, 视图层 %}，不仅易于上手，还便于与第三方库或既有项目整合。
- 另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的{% span red, 单页应用 %}提供驱动。
{% endnote %}

# Vue基础
## 起步
{% tabs vue起步 %}
<!-- tab ① 搭建vue开发环境-->
1. 下载开发版本的vue.js
2. 安装开发者调试工具({% span red, 'Vue.js devtools' %}，谷歌应用商店可下载)
3. 关闭浏览器控制台的提示


```js
Vue.config.productionTip = false; // 阻止 vue 在启动时生成生产提示
```
<!-- endtab -->

<!-- tab ② Hello小案例-->
```html
<!-- 准备一个容器 -->
<div id="app">
    <h1>Hello,{{name}}</h1>
</div>

<script src="../js/vue.js"></script>
<script>
   Vue.config.productionTip = false; // 阻止 vue 在启动时生成生产提示

   //创建Vue实例
   new Vue({
   	el: '#app',
   	data: {
   		name: '陈升',
   	},
   });
</script>
```
其中，el与data都有2种写法    
```js
const vm = new Vue({
//    el: '#app', // (1).new Vue时候配置el属性
   data: {
      name: '陈升',
   },
});

vm.$mount('#app'); // (2).先创建Vue实例，随后再通过vm.$mount('#root')指定el的值
```
```js
const vm = new Vue({
    // (1).对象式
	data: {
		name: '陈升',
	},
    // (2).函数式
	data() {
		return {
			name: '陈升',
		};
	},
});
❗如何选择：目前哪种写法都可以，以后学习到组件时，data必须使用函数式，否则会报错。
❗一个重要的原则：由Vue管理的函数，一定不要写箭头函数，一旦写了箭头函数，this就不再是Vue实例了。
```
<!-- endtab -->
{% endtabs %}

## 模板语法
1. <font size = 4 color=green>插值</font>：
	功能：用于解析{% span blue, '标签体内容' %}。
	写法：{% span red, '{{xxx}}' %}，xxx是js表达式，且可以直接读取到data中的所有属性。
    ```html
    <h1>Hello, {{name}}</h1>
    ```
2. <font size = 4 color=green>指令</font>：
	功能：用于解析{% span blue, '标签' %}（包括：标签属性、标签体内容、绑定事件.....）。
	举例：{% span red, 'v-bind:href="xxx"' %} 或  简写为 {% span red, ':href="xxx"' %}，xxx同样要写js表达式，且可以直接读取到data中的所有属性。
	备注：Vue中有很多的指令，且形式都是：v-????，此处我们只是拿v-bind举个例子。
    ```html
    <a v-bind:href="url">点我跳转</a>
    <a :href="url">点我跳转</a>
    ```

## 数据绑定
1. <font size = 4 color=green>单向绑定(v-bind)</font>：数据只能从data流向页面。
   ```js
   <input type="text" :value="name">
   ```
2. <font size = 4 color=green>双向绑定(v-model)</font>：数据不仅能从data流向页面，还可以从页面流向data。
	- 双向绑定一般都应用在{% span blue, 表单 %}类元素上（如：input、select等）
	- v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值。
    
    ```js
    <input type="text" v-model="name">
    ```

## MVVM

{% image https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/MVVM%E5%8E%9F%E7%90%86%E5%9B%BE.jpeg %}

{% note success flat %}
- M：模型（Model）：对应data中的数据
- V：视图（View）：模板
- VM：视图模型（ViewModel）：Vue实例对象
{% endnote %}

## 数据代理

## 事件处理
