---
title: vue从入门到精通
date: 2022-11-28 00:00:00
categories: vue
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

# Vue基础
## 模板语法 (插值/指令)
{% tabs 模板语法 %}
<!-- tab 插值 -->
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
<!-- endtab -->

<!-- tab 指令 -->
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
<!-- endtab -->
{% endtabs %}

## 计算属性/侦听器
{% tabs 计算属性/侦听器 %}
<!-- tab 计算属性computed -->
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
<!-- endtab -->

<!-- tab 侦听器watch -->
用于{% span red, '监听data里面的数据是否被修改，一旦修改就可以执行一些其他的操作' %}【注：watch监听器只能监听第一次】
```js
	watch: {
        // num 的变量名必须跟 data 里面的变量一致
        // 作用 : 只要 num 的值发生变化,这个方法就会被调用
		num: function (newVal, oldVal) {
			console.log(newVal, oldVal);
		},
	},
```

⭐ 对象和数组都是引用类型，引用类型变量存的是地址，地址没有变，所以不会触发watch。这时我们需要进行深度监听，就需要加上一个属性 deep，值为 true
⭐ 当希望进入页面就触发，必须设置 `immediate: true`
```js
	methods: {
		setArray() {
			// ⭐ 当我们想要更改数组的值的时候
			// this.array[0] = '我是新的'; ❌
			// 因为在vue的数据双向绑定中，调用的是set和get函数，但是在数组里面没有这两个函数
			// 所以我们只能通过vue封装的$set和变异方法(也就是push)
			this.$set(this.array, 0, '我是新的');
		},
	},
	watch: {
		obj: {
			handler(newVal, oldVal) {
				console.log(newVal, oldVal);
			},
			deep: true, // 开启深度监听
            immediate: true, // 立即处理，进入页面就触发
		},
        // 当然直接监听对象的某个属性可以触发，不一定要用 deep: true
        'obj.name'(newVal, oldVal) {
            console.log(newVal, oldVal);
        }
	},
```
<!-- endtab -->

<!-- tab computed和methods区别 -->
{% span red, '计算属性是基于它们的响应式依赖进行缓存的，只在相关响应式依赖发生改变时它们才会重新求值' %}。
这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。
这也同样意味着下面的计算属性将不再更新，因为 Date.now() 不是响应式依赖：
```js
computed: {
    now: function () {
        return Date.now() // 此处不会更新
    }
}
```
相比之下，{% span red, '每当触发重新渲染时，调用方法将总会再次执行函数' %}。
<!-- endtab -->	

<!-- tab watch和computed的区别 -->
1. computed支持{% span red, '缓存' %}，只有依赖数据发生改变,才会重新进行计算;而watch不支持缓存，数据变,直接会触发相应的操作
2. computed不支持{% span red, '异步' %}，当computed内有异步操作时无效，无法监听数据的变化，而watch支持异步
3. computed属性值会默认走缓存，计算属性是基于它们的响应式依赖进行缓存的，也就是基于data中声明过或者父组件传递的props中的数据通过计算得到的值;而watch监听的函数接收两个参数，第一个参数是最新的值，第二个参数是输入之前的值
4. 如果一个属性是由其它属性计算而来的，这个属性依赖其它属性，多对一或者一对一，一般用computed；而当一个属性发生变化时，需要执行对应的操作，一对多，一般用watch
<!-- endtab -->    
{% endtabs %}

## Class 与 Style 绑定
{% tabs Class 与 Style 绑定 %}
<!-- tab 绑定 HTML Class -->
```html
<!-- 对象语法 -->
<div :class="{ active: isActive }"></div>
<!-- 也可以与普通的 class 属性 共存 -->
<!-- 这里假设 isActive: true, hasError: false 等同于 class= "static active text-danger" -->
<div class="static" :class="{ active: isActive, 'text-danger': hasError }"></div>

<!-- 数组语法 -->
<!-- 这里假设 activeClass: 'active', errorClass: 'text-danger' 等同于 class="active text-danger" -->
<div v-bind:class="[activeClass, errorClass]"></div>
<!-- 三元表达式 -->
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
<!-- 在数组语法中也可以使用对象语法 -->
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```
<!-- endtab -->

<!-- tab 绑定内联样式-->
```html
<!-- 对象语法 -->
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
<!-- 直接绑定到一个样式对象通常更好，这会让模板更清晰 -->
<!-- 这里 styleObject: {color: 'red',fontSize: '13px'} -->
<div :style="styleObject"></div>

<!-- 数组语法 -->
<div :style="[baseStyles, overridingStyles]"></div>

<!-- 多重值 -->
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
<!-- endtab -->
{% endtabs %}

## 条件渲染（v-if / v-show）
{% tabs 条件渲染 %}
<!-- tab v-if-->
```html
<div v-if="type === 'A'">A</div>
<div v-else-if="type === 'B'">B</div>
<div v-else-if="type === 'C'">C</div>
<div v-else>Not A/B/C</div>
```
**用 key 管理可复用的元素**
Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染
登陆注册的应用中，切换 登陆/注册 不会清除用户已经输入的内容。因为两个模板使用了相同的元素，`<input>` 不会被替换掉——仅仅是替换了它的 placeholder。
我们可以添加一个具有唯一值的 {% span red, key %} 属性来表达“这两个元素是完全独立的，不要复用它们”
```html
		<div v-if="loginType === 'username'">
			<label>用户名</label>
			<input placeholder="请输入用户名" key="username" />
		</div>
		<div v-else>
			<label>邮箱</label>
			<input placeholder="请输入邮箱" key="email" />
		</div>
		<button @click="loginType = loginType === 'username' ? 'email' : 'username'">登陆/注册</button>
```
注意，`<label>` 元素仍然会被高效地复用，因为它们没有添加 key 属性。
<!-- endtab -->

<!-- tab v-show-->
```html
<h1 v-show="ok">Hello!</h1>
```
{% span red, '带有 v-show 的元素始终会被渲染并保留在 DOM 中' %}。
v-show 只是简单地切换元素的 CSS property display。
注意，v-show 不支持 `<template>` 元素，也不支持 v-else。
<!-- endtab -->

<!-- tab v-if与v-show的区别-->
v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。
因此，{% span red, '如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。' %}
<!-- endtab -->

<!-- tab v-if与v-for不能同时使用-->
当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的{% span red, 优先级 %}。
<!-- endtab -->
{% endtabs %}

## 列表渲染 (v-for)
{% tabs 列表渲染 %}
<!-- tab v-for -->
```html
<ul>
   <li v-for="(item, index) in array" :key="index">{{ item.name }}</li>
   <li v-for="(value, key) in obj" :key="value"><p>{{ key }}-{{ value }}</p></li>
   <li v-for="item in 6" :key="item">{{ item }}</li>
   <li v-for="item in 6" :key="item">{{ item + 4 }}</li>
</ul>
```
<!-- endtab -->

<!-- tab key的作用 -->
为了高效的更新虚拟DOM
<!-- endtab -->

<!-- tab for in和for of的区别 -->
1. 推荐{% span red, '在循环对象属性的时候使用 for...in，在遍历数组的时候的时候使用 for...of' %} 
2. for...in 循环出的是 key，for...of 循环出的是 value 
3. 注意，for...of 是 ES6 新引入的特性。修复了 ES5 引入的 for...in 的不足 
4. for...of 不能循环普通的对象（如通过构造函数创造的），需要通过和 {% span red, 'Object.keys()' %}搭配使用

{% span blue, 'for in遍历数组的毛病：' %}
1. index索引为字符串型数字，不能直接进行几何运算
2. 遍历顺序有可能不是按照实际数组的内部顺序
3. 使用for in会遍历数组所有的可枚举属性，包括原型。例如上栗的原型方法method和name属性
所以for in更适合遍历对象，不要使用for in遍历数组。
那么除了使用for循环，如何更简单的正确的遍历数组达到我们的期望呢（即不遍历method和name），ES6中的for of更胜一筹.
<!-- endtab -->

<!-- tab v-for与v-if不建议一起使用 -->
> 注意我们不推荐在同一元素上使用 v-if 和 v-for

当它们处于同一节点，{% span red, 'v-for 的优先级比 v-if 更高' %}，这意味着 v-if 将分别重复运行于每个 v-for 循环中。
当你只想为部分项渲染节点时，这种优先级的机制会十分有用。
```html
<li v-for="todo in todos" v-if="!todo.isComplete">{{ todo }}</li>
```
如果你的目的是有条件地跳过循环的执行，那么可以将 v-if 置于外层元素 (或 `<template>`) 上
```js
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```
<!-- endtab -->

<!-- tab diff算法 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E7%9A%84diff%E7%AE%97%E6%B3%95.png)
<!-- endtab -->
{% endtabs %}

## 事件处理 (修饰符/修饰键)
{% tabs 事件处理 %}
<!-- tab 事件修饰符 -->
```html
<!-- 阻止事件冒泡 -->
<a @click.stop="doThis"></a>
<!-- 阻止默认行为-->
<form @submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联 -->
<a @click.stop.prevent="doThat"></a>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div @click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div @click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a @click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<!-- .passive 修饰符尤其能够提升移动端的性能 -->
<div @scroll.passive="onScroll">...</div>
```
<!-- endtab -->

<!-- tab 按键修饰符 -->
```html
<input v-on:keyup.enter="submit">
<input v-on:keyup.tab="submit">
<input v-on:keyup.delete="submit">
<input v-on:keyup.esc="submit">
<input v-on:keyup.space="submit">
<input v-on:keyup.up="submit">
<input v-on:keyup.down="submit">
<input v-on:keyup.left="submit">
<input v-on:keyup.right="submit">
```
<!-- endtab -->

<!-- tab 系统修饰键 -->
```html
<!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>

【.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件】
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>
<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>
<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```
<!-- endtab -->
{% endtabs %}

## 表单输入绑定 (v-model)
{% tabs 表单输入绑定 %}
<!-- tab 基础用法 -->
```js
		<p>Message is: {{ message }}</p>

		<!-- 文本 -->
		<input type="text" v-model="message" placeholder="请输入文本" /> <br />
		<textarea v-model="message" placeholder="请输入文本"></textarea> <br />

		<!-- 复选框 -->
		<input type="checkbox" id="checkbox" v-model="checked" /><label for="checkbox">{{ checked }}</label> <br />

		<!-- 单选按钮 -->
		<input type="radio" id="one" value="One" v-model="picked" /><label for="one">One</label><br />
		<input type="radio" id="two" value="Two" v-model="picked" /><label for="two">Two</label><br />

		<!-- 选择框 -->
		<select v-model="selected1">
			<option disabled value="">请选择</option>
			<option>A</option>
			<option>B</option>
			<option>C</option>
		</select>
		<!-- 多选时 (绑定到一个数组)： -->
		<select v-model="selected2" multiple style="width: 50px">
			<option>A</option>
			<option>B</option>
			<option>C</option>
		</select>
		<!-- 用 v-for 渲染的动态选项： -->
		<select v-model="selected3">
			<option v-for="option in options" :value="option.value" :key="option.value">
				{{ option.text }}
			</option>
		</select>
```
```js
			message: '',
			checked: '',
			picked: '',
			selected1: '',
			selected2: [],

			selected3: 'A',
			options: [
				{ text: 'One', value: 'A' },
				{ text: 'Two', value: 'B' },
				{ text: 'Three', value: 'C' },
			],
```
<!-- endtab -->

<!-- tab 值绑定-->
对于单选按钮，复选框及选择框的选项，v-model 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：
```html
		<!-- 当选中时，`picked` 为字符串 "a" -->
		<input type="radio" v-model="picked" value="a" />
		<!-- `toggle` 为 true 或 false -->
		<input type="checkbox" v-model="toggle" />
		<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
		<select v-model="selected">
			<option value="abc">ABC</option>
		</select>
```

但是有时我们可能想把值绑定到 Vue 实例的一个动态 属性 上，这时可以用 v-bind 实现，并且这个 属性 的值可以不是字符串。
```html
		<!-- 复选框 -->
		<input type="checkbox" v-model="toggle" true-value="yes" false-value="no" />
		<!-- 当选中时 vm.toggle === 'yes'  -->
		<!-- 当没有选中时 vm.toggle === 'no' -->

		<!-- 单选按钮 -->
		<input type="radio" v-model="pick" v-bind:value="a" />
		<!-- 当选中时 vm.pick === vm.a -->

		<!-- 选择框的选项 -->
		<select v-model="selected">
			<!-- 内联对象字面量 -->
			<option v-bind:value="{ number: 123 }">123</option>
		</select>
		<!-- 当选中时  -->
		<!-- typeof vm.selected // => 'object'  -->
		<!-- vm.selected.number // => 123 -->
```
<!-- endtab -->

<!-- tab 修饰符(lazy/number/trim) -->
<font size=5 color=green>.lazy</font>
在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步。
你可以添加 lazy 修饰符，从而转为在 change 事件_之后_进行同步：
```js
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```

<font size=5 color=green>.number</font>
作用：自动将用户的输入值转为数值类型
这通常很有用，因为即使在 type="number" 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 parseFloat() 解析，则会返回原始的值。
```js
<input v-model.number="age" type="number">
```

<font size=5 color=green>.trim</font>
作用：自动过滤用户输入的首尾空白字符
```js
<input v-model.trim="msg">
```
<!-- endtab -->
{% endtabs %}

## 学生成绩管理📝
{% tabs 学生成绩管理 %}
<!-- tab 前期准备 -->
```bash
vue create vue_study(此为项目名) 选择vue2版本
npm run serve (启动项目，并删除多余的结构)
```
通过修改 vue.config.js 文件来覆盖脚手架下的 webpack 配置
```diff
const { defineConfig } = require('@vue/cli-service');
module.exports = defineConfig({
	transpileDependencies: true,
+       devServer: { port: 4080 }, // 配置本地开发服务器的端口号
+       lintOnSave: false, // 这里先暂时关闭代码检查
});
```
如果编辑器右下角出现 “1 known issue”，则需要在 jsconfig.json 添加 以下代码
```js
"vueCompilerOptions": {
	"target": 2.7
}
```
<!-- endtab -->

<!-- tab 渲染结构 -->
{% span red, 'css样式引入在main.js，而js引入在需要用到的组件通过es6模块化导入' %}

思路：
```txt
1. 数据结构的编写用的是 bootstrap ，用到 bootstrap
2. 不及格写的样式用到 less, 安装：npm i less less.loader -D
3. 处理数据中的时间，用到 moment.js
4. 要求成绩低于60分的，字体显示为红色
```

- bootstrap的使用
```js
1. 复制老师给的 bootstrap 的 HTML 代码到 App.vue
2. 安装： npm i bootstrap
3. 在 vue 项目的 main.js 导入 bootstrap 样式：
   import 'bootstrap/dist/css/bootstrap.css';
```

- moment.js的使用
```js
1. 安装：npm i moment
2. App.vue 导入 moment：
   import moment from 'moment'
3. 在 methods 声明格式化函数 format
4. 在 html 结构中，把时间数据 item.time 传给 format 函数
```

实现代码：
```html
							<tr v-for="(item, index) in list" :key="index">
								<th scope="row">{{ item.id }}</th>
								<td>{{ item.subject }}</td>
								<td :class="{ 'not-passed': item.score < 60 }">{{ item.score }}</td>
								<td>{{ format(item.time) }}</td>
								<td>
									<button type="button" class="btn btn-link">删除</button>
								</td>
							</tr>
```
```js
	data() {
		return {
			list: [
				{ id: 100, subject: '语文', score: 99, time: new Date('2010-08-12') },
				{ id: 101, subject: '数学', score: 34, time: new Date('2020-09-01') },
				{ id: 102, subject: '英语', score: 25, time: new Date('2018-11-22') },
				{ id: 103, subject: '体育', score: 100, time: new Date('2020-12-12') },
			],
		};
	},
	methods: {
		format(time) {
			return moment(time).format('YYYY-MM-DD HH:mm:ss');
		},
	},
```
<!-- endtab -->

<!-- tab 单个删除 -->
思路：
1. 删除功能：	
方法1：可以通过filter删除指定id的项
方法2：直接通过遍历接收的索引，通过splice(指定索引,1)【建议方法，简洁代码少】
```html
// 方法1：<button @click="del(index)">删除</button>
// 方法2：<button @click="del(item.id)">删除</button>
```
```js
	methods: {
		// 方法1：
		del(index) {
			this.list.splice(index, 1);
		},
		// 方法2：
		del(id) {
			this.list = this.list.filter((item) => item.id !== id);
		},
	},
```
1. 当数据全部清空，显示暂无数据，通过v-if来判断，(可选：逆向思维的非运算）
谨记：先把tfoot里面的style="display: none"去掉
```html
<!-- 方法1： -->
<tfoot v-if="!list.length">
<!-- 方法2： -->
<tfoot v-if="list.length === 0">
```
<!-- endtab -->

<!-- tab 添加成绩 -->
```html
<form @submit.prevent="add">
		<input type="text" placeholder="请输入科目" v-model.trim="subject" />
		<input type="text" placeholder="请输入分数" v-model.number="score" />
		<button type="submit">添加</button>
</form>
```
```js
	methods: {
		add() {
			this.list.push({
				// 数组有数据，取最后一条id + 1；没数据给一个默认id
				id: this.list.length > 0 ? this.list[this.list.length - 1].id + 1 : 100,
				subject: this.subject,
				score: this.score,
				time: new Date(),
			});

      		// 记得清空输入框
			this.subject = '';
			this.score = '';
		},
	},
```
<!-- endtab -->

<!-- tab 总分和平均分 -->
{% span red, '对于任何复杂逻辑，都应当使用计算属性。' %}
谨记：计算属性不用再在data里面声明变量
分数值的合法性，通过watch监听进行处理
```js
	computed: {
		total() {
			return this.list.reduce((total, value) => total + value.score, 0).toFixed(0);
		},
		average() {
			return this.list.length === 0 ? 0 : this.total / this.list.length.toFix(2);
		},
	},
	watch: {
		score(newVal, oldVal) {
			if (newVal > 100) {
				alert('最大100分');
				this.score = null;
			} else if (newVal < 0) {
				alert('不允许负分');
				this.score = null;
			}
		},
	},
```
<!-- endtab -->

<!-- tab 本地存储-->
- 本地存储：localStorage.setItem / getItem
- 监听list数据的改变123

```js
	created() {
		const result = localStorage.getItem('list');
		if (result) this.list = JSON.parse(result);
	},
	watch: {
		list: {
			deep: true,
			handler(newVal) {
				localStorage.setItem('list', JSON.stringify(newVal));
			},
		},
	},
```
<!-- endtab -->
{% endtabs %}

## 全选反选案例📝
{% tabs 全选反选案例 %}
<!-- tab 效果 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%85%A8%E9%80%89%E5%8F%8D%E9%80%89%E5%B0%8F%E6%A1%88%E4%BE%8B.png)
<!-- endtab -->

<!-- tab 实现代码 -->
```html
	<div id="app">
		<p>全选：<input type="checkbox" v-model="isAll" /> <button @click="reverseClick">反选</button></p>
		<ul>
			<li v-for="item in list" :key="item.name"><input type="checkbox" v-model="item.checked" /> {{ item.name }}</li>
		</ul>
	</div>
```
```js
	data() {
		return {
			list: [
				{ name: '唐增', checked: false },
				{ name: '孙悟空', checked: false },
				{ name: '猪八戒', checked: true },
				{ name: '沙师弟', checked: false },
			],
		};
	},
	methods: {
		reverseClick() {
			this.list.forEach((item) => (item.checked = !item.checked));
		},
	},
	computed: {
		isAll: {
			get() {
				return this.list.every((item) => item.checked);
			},
			set(value) {
				this.list.map((item) => (item.checked = value));
			},
		},
	},
```
<!-- endtab -->
{% endtabs %}

# 深入了解组件
## 组件注册
{% tabs 组件注册 %}
<!-- tab 组件注册 -->
举例：生成 tasty-food 的组件
```html
<template>
	<div id="tasty-food">
		<h1>爱的魔力转圈圈</h1>
	</div>
</template>

<script>
export default {
	name: 'TastyFood',
};
</script>
```
在App.vue中使用上面的插件
```html
<template>
	<div id="app">
		<tasty-food></tasty-food>
	</div>
</template>

<script>
import TastyFood from './components/tasty-food';

export default {
	name: 'App',
	components: { TastyFood },
};
</script>
```
<!-- endtab -->

<!-- tab scoped原理 -->
1. 组件标签增加data-v-hash属性
2. 样式会增加data-v-hash属性选择器
<!-- endtab -->
{% endtabs %}

## 组件间通讯
{% tabs 组件注册 %}
<!-- tab 父传子 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E7%88%B6%E4%BC%A0%E5%AD%90.png)
<!-- endtab -->

<!-- tab 子传父 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%AD%90%E4%BC%A0%E7%88%B6.png)
<!-- endtab -->
{% endtabs %}

## 自定义事件
### 自定义组件的v-model
一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件，但是像单选框、复选框等类型的输入控件可能会将 value 属性 用于不同的目的。model 选项可以用来避免这样的冲突：
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6%E7%9A%84v-model%E4%BD%BF%E7%94%A8.png)

### .sync修饰符
可以处理多个数据
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/snyc%E4%BF%AE%E9%A5%B0%E7%AC%A6%E5%9C%A8%E7%BB%84%E4%BB%B6%E7%9A%84%E4%BD%BF%E7%94%A8.png)

## $ref / $refs
举例：回复自动获取输入框焦点
{% tabs '$ref / $refs'  %}
<!-- tab 获取dom元素 -->
```html
<template>
	<div id="app">
		<input ref="comment" type="text" /> <button @click="reply">回复</button>
	</div>
</template>

<script>
export default {
	name: 'App',
	data() {
		return {};
	},
	methods: {
		reply() {
			this.$refs.comment.focus();
		},
	},
};
</script>
```
<!-- endtab -->

<!-- tab 获取组件对象 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/$refs%E8%8E%B7%E5%8F%96%E7%BB%84%E4%BB%B6%E5%AF%B9%E8%B1%A1.png)
<!-- endtab -->
{% endtabs %}

## $nextTick
{% tabs '$nextTick'  %}
<!-- tab 具体使用 -->
Vue更新DOM是异步的
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/$nextTick%E7%9A%84%E7%94%A8%E6%B3%95.png)
<!-- endtab -->

<!-- tab 搜索小案例 -->
```html
<template>
	<div id="app">
		<input type="text" placeholder="输入要搜索的关键字" ref="inp" v-if="isShown" />
		<button @click="search" v-else>点击搜索</button>
	</div>
</template>

<script>
export default {
	name: 'App',
	data() {
		return {
			isShown: false,
		};
	},
	methods: {
		search() {
			this.isShown = true;
			this.$nextTick(() => {
				this.$refs.inp.focus();
			});
		},
	},
};
</script>
```
<!-- endtab -->
{% endtabs %}

## 动态组件
```html
<template>
	<div id="app">
		<button @click="current = 'little-cat'">看猫咪</button>
		<button @click="current = 'little-dog'">看小狗</button>
		<button @click="current = 'little-monkey'">看猴子</button>

		<component :is="current"></component>
	</div>
</template>

<script>
import LittleCat from './components/little-cat.vue';
import LittleDog from './components/little-dog.vue';
import LittleMonkey from './components/little-monkey.vue';

export default {
	name: 'App',
	components: { LittleCat, LittleDog, LittleMonkey },
	data() {
		return {
			current: 'little-cat',
		};
	},
};
</script>
```

## 自定义指令
> 自定义指令：自己定义指令, 封装dom操作，扩展额外功能

{% tabs '自定义指令' %}
<!-- tab 局部注册 -->
```html
<template>
	<div id="app">
		<h1 v-color="'red'">hello world</h1>

		<p v-color="'blue'">你好，世界</p>

		<p v-color="myColor">变色内容</p>
		<button @click="myColor = myColor === 'red' ? 'pink' : 'red'">点击变色</button>
	</div>
</template>

<script>
export default {
	name: 'App',
	data() {
		return {
			myColor: 'pink',
		};
	},
	// 自定义一个局部指令
	directives: {
		//【⭐写法1】
		// 这里的color为指令名字
		color: {
			// inserted是指令所在的标签DOM创建完成并插入到父节后触发
			// binding: { value } , value 就是指令的值
			inserted(el, { value }) {
				// el就是指令所在的标签的DOM
				el.style.color = value;
			},
			// update：指令的值改变时触发，binding.value 指令的值修改触发
			update(el, { value }) {
				el.style.color = value;
			},
		},

		//【⭐写法2】
		color(el, { value }) {
			el.style.color = value;
		},
	},
};
</script>
```
<!-- endtab -->

<!-- tab 全局注册 -->
在`main.js`添加以下代码
```js
Vue.directive('upper', {
	inserted(el) {
		el.textContent = el.textContent.toUpperCase();
	},
});
```

在对应的html结构增加指令：`v-upper` 即可
```html
<h1 v-upper>hello world</h1>
```
<!-- endtab -->
{% endtabs %}

## 插槽
{% tabs 插槽 %}
<!-- tab 基本使用 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%8F%92%E6%A7%BD%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.png)
<!-- endtab -->

<!-- tab 插槽默认值 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%8F%92%E6%A7%BD%E7%9A%84%E9%BB%98%E8%AE%A4%E5%80%BC.png)
<!-- endtab -->

<!-- tab 具名插槽 -->
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E5%85%B7%E5%90%8D%E6%8F%92%E6%A7%BD%E7%9A%84%E4%BD%BF%E7%94%A8.png)
<!-- endtab -->

<!-- tab 作用域插槽 -->
> 作用域插槽: 定义 slot 插槽的同时, 是可以传值的。给插槽上可以绑定数据，将来使用组件时可以用。

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E4%BD%9C%E7%94%A8%E5%9F%9F%E6%8F%92%E6%A7%BD%E7%9A%84%E4%BD%BF%E7%94%A8.png)
<!-- endtab -->

<!-- tab 书本列表案例 -->
渲染的数据：
```js
bookList: [
	{ id: 0, name: '红楼梦', author: '曹雪芹/高鹗' },
	{ id: 1, name: '水浒传', author: '施耐庵' },
	{ id: 2, name: '三国演义', author: '罗贯中' },
	{ id: 3, name: '西游记', author: '吴承恩' },
],
heroes: [
	{ id: 0, title: '齐天大圣', category: '猴子', weapon: '定海神针' },
	{ id: 1, title: '汉寿亭侯', category: '统帅', weapon: '青龙偃月刀' },
	{ id: 2, title: '黑猫警长', category: '猫', weapon: '沙漠之鹰' },
],
```
App.vue
```html
		<hm-table :data="bookList">
			<template #head>
				<tr>
					<th scope="col">id</th>
					<th scope="col">书名</th>
					<th scope="col">作者</th>
				</tr>
			</template>

			<template #content="obj">
				<td>{{ obj.row.id }}</td>
				<td>{{ obj.row.name }}</td>
				<td>{{ obj.row.author }}</td>
			</template>
		</hm-table>
```
子组件 hm-table.vue
```html
<template>
	<table class="table table-bordered w-75 mx-auto mt-3">
		<thead>
			<slot name="head"></slot>
		</thead>
		<tbody>
			<tr v-for="item in data" :key="item.id">
				<slot name="content" :row="item"></slot>
			</tr>
		</tbody>
	</table>
</template>

<script>
export default {
	name: 'HmTable',
	props: {
		data: Array,
	},
};
</script>
```
<!-- endtab -->
{% endtabs %}

# 生命周期
{% hideBlock 生命周期图示, green %}
这里有张图片：
<img src="https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.jpg" alt="生命周期图示" style="zoom:30%;" />
{% endhideBlock %}

{% note info no-icon flat %}
Vue的生命周期：Vue组件实例从创建到销毁的全过程

生命周期函数：是由 vue 框架提供的内置函数，会伴随着组件的生命周期，自动按次序执行。
{% endnote %}

{% note success no-icon flat %}
八大生命周期钩子函数：
1. beforeCreate：data数据初始化之前，组件还没有数据
2. ⭐**created**：组件创建完毕后，发起Ajax 请求，从而初始化 data 数据
3. beforeMount：DOM渲染之前，DOM还没渲染
4. ⭐**mounted**：DOM渲染之后，可以操作DOM了
5. beforeUpdate: 数据更新，DOM更新前
6. updated: 数据更新，DOM更新后
7. beforeDestroy: 组件销毁前
8. destroyed: 组件销毁后
{% endnote %}

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E5%B8%B8%E7%94%A8%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%98%B6%E6%AE%B5.png)

# 路由基础
{% tabs 路由基础 %}
<!-- tab 单页面应用 -->
{% note info no-icon flat %}
单页应用程序: SPA - Single Page Application
单页面应用(SPA): 所有功能在{% span red, '一个html页面' %}上实现     (多页面应用程序MPA)
**优点：**
- {% span red, '不整个刷新页面' %}，每次请求仅获取需要的部分，{% span red, '用户体验更好' %}
- 数据传递容易, {% span red, '开发效率高' %}

**缺点：**
- 开发成本高(需要学习专门知识 - {% span red, '路由' %})
- 首次加载会比较慢一点，不利于seo
{% endnote %}
<!-- endtab -->

<!-- tab 路由使用 -->
路由安装命令：npm i vue-router@3

src/router/index.js
```js
// 导入路由
import Vue from 'vue';
import VueRouter from 'vue-router';

import HmHeader from '../views/hm-header.vue';
import HmMain from '../views/hm-main.vue';
import HmFooter from '../views/hm-footer.vue';

// 注册路由
Vue.use(VueRouter);

// 路由配置页面
const routes = [
	{ path: '/header', component: HmHeader,  },
	{ path: '/main', component: HmMain },
	{ path: '/footer', component: HmFooter },
];

// 实例化路由
const router = new VueRouter({
	routes,
});

// 导出路由
export default router;
```

src/main.js
```js
// 导入抽离的路由
import router from './router';

new Vue({
	router,
	render: (h) => h(App),
}).$mount('#app');
```

App.vue
```html
<template>
	<div id="App">
		<router-link to="/header">header页面</router-link> |
		<router-link to="/main">main页面</router-link> |
		<router-link to="/footer">footer页面</router-link>
		<router-view></router-view>
	</div>
</template>
```

<!-- endtab -->

<!-- tab 两个类名 -->
`router-link-active`: 激活的导航链接，`模糊匹配`（to="/my"：可以匹配 /my、/my/a、/my/b）
`router-link-exact-active`: 激活的导航链接，`精确匹配`（to="/my"：仅可以匹配 /my）

```css
.router-link-active {
	color: red;
}
```
<!-- endtab -->

<!-- tab 跳转传参 -->
/path?参数名=值
```html
<router-link :to="{ path: '/shop', query: { sort: 1 } }">商城</router-link>

<!-- 在跳转到的对应组件可以获取地址参数 -->
<script>
	created() {
		console.log(this.$route.query.sort);
	},
</script>
```
/path/值 – 需要路由对象提前配置 path: “/path/:参数名”
```html
<!-- src/router/index.js给页面路径增加占位 -->
const routes = [
	{ path: '/cart/:abc', component: HmCart },
];

<!-- app.vue 跳转页面，拼接嵌套的参数 -->
<router-link :to="'/cart/' + 666">购物车</router-link>

<!-- 跳转到的路由接收参数 -->
<script>
	created() {
		console.log(this.$route.params.abc);
	},
</script>
```
<!-- endtab -->

<!-- tab 重定向 -->
> 重定向：匹配path后, 强制跳转path路径

- 网页打开url默认hash值是/路径
- redirect是设置要重定向到哪个路由路径

```js
// 路由配置页面
const routes = [
	{ path: '/', redirect: '/index' },
	{ path: '/index', component: HmIndex },
];
```
<!-- endtab -->

<!-- tab 404 -->
404：当找不到路径匹配时，给个提示页面
路由最后, path匹配*(任意路径) – 前面不匹配就命中最后这个
```js
// 在src/router/index.js添加代码
import NotFound from '../views/not-found.vue';

const routes = [
	// 谨记：必须放在最后一个位置，*匹配任何路径
	{ path: '*', component: NotFound },
];
```
<!-- endtab -->


<!-- tab 模式设置  -->
问题：路由的路径看起来不自然, 有#，能否切成真正路径形式?
答：vue路由-模式设置，修改路由在地址栏的模式
- hash路由（例如: http://localhost:8080/#/home）
- history路由（例如: http://localhost:8080/home，以后上线需要服务器端支持）

```js
// src/router/index.js
const router = new VueRouter({
	routes,
	mode: 'history',
});
```
<!-- endtab -->

<!-- tab 编程式跳转  -->
```html
// ❗不能在目标页面进行点击跳转，不然会无效报错
<button @click="jump">跳转到个人中心</button>

<script>
  jump() {
  	// this.$router.push('/center');
  	this.$router.push({ path: '/center', query: { sort: 1, all: 2 } });
	this.$router.push({ name: 'HmCenter' })
  },
</script>
```
<!-- endtab -->
{% endtabs %}

# 路由守卫

# vuex

{% btns rounded center grid5 %}
{% cell vuex官方文档, https://v3.vuex.vuejs.org/zh/, fas fa-book-open %}
{% endbtns %}
