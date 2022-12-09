---
title: vue面试题
date: 2022-12-10 00:00:00
categories: "面试"
description: 摘录一些面试中遇到的vue相关高频面试题（持续更新中）
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E9%9D%A2%E8%AF%95%E9%A2%98.avif
toc_number: false
---

# vue2基础⚙️
## 对MVVM的理解
{% note success no-icon flat %}
Model-View-ViewModel 的简写，即模型-视图-视图模型
- 模型（Model） 指的是后端传递的数据，
- 视图(View)指的是所看到的页面，
- 视图模型(ViewModel)是 mvvm 模式的核心，它是连接 view 和 model 的桥梁

有两个方向： 
- 将模型（Model）转化成视图(View)，即将后端传递的数据转化成所看到 的页面，实现的方式是：数据绑定
- 是将视图(View)转化成模型(Model)，即将所看到的页面转化成后端的数据

实现的方式是：DOM 事件监听，这两个方向都实现的，我们称之为数据的双向绑定
{% endnote %}

## 插槽
{% note success no-icon flat %}
> 定义：预留内容的占位符

插槽的使用
  - 默认(或匿名)插槽 : 在子组件中写入slot，slot所在的位置就是父组件要显示的内容
  - 具名插槽：
    - 在子组件中定义了三个slot标签，其中有两个分别添加了name属性header和footer
    - 在父组件中使用template并写入对应的slot名字来指定该内容在子组件中现实的位置
  - 作用域插槽：
    - 在子组件的slot标签上绑定需要的值`<slot name="content" :data="obj"></slot>`
    - 在父组件上使用template并写入#content="data"来接收子组件传过来的值
{% endnote %}

## vue组件中data为什么必须是一个函数
{% note success no-icon flat %}
- 每复用一次组件就会返回一份新的data，类似创建一个私有的数据空间，每个组件实例维护自己的数据
- 如果是写一个对象，那就是所有组件实例共用一份数据，造成数据污染
{% endnote %}

## Vue数据双向绑定的原理
> vue.js是采用<font color=red>数据劫持</font>结合<font color=red>发布者-订阅者模式</font>的方式，通过<font color=red>Object.defineProperty()</font>来劫持各个属性的<font color=red>setter，getter</font>，在数据变动时发布消息给订阅者，触发相应的监听回调来渲染视图。

{% note info no-icon flat %}
具体步骤：

① 需要<font color=red>observer(观察者)</font>对数据对象进行<font color=red>递归遍历</font>，包括子属性对象的属性，都<font color=red>加上 setter和getter</font>
这样的话，给这个对象的某个值<font color=red>赋值</font>，就会触发<font color=red>setter</font>，那么就能监听到了数据变化

② <font color=red>compile(模板解析器)</font>解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

③ <font color=red>Watcher(订阅者)</font>是<font color=red>Observer</font>和<font color=red>Compile</font>之间通信的桥梁，主要做的事情是:
1、在自身实例化时往属性订阅器(<font color=red>dep</font>)里面添加自己
2、自身必须有一个<font color=red>update()</font>方法
3、待属性变动<font color=red>dep.notice()</font>通知时，能调用自身的<font color=red>update()</font>方法，并触发<font color=red>Compile中绑定的回调</font>，则功成身退。

④ <font color=red>MVVM</font>作为数据绑定的入口，<font color=red>整合Observer、Compile和Watcher三者</font>，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。
{% endnote %}

<div class="btn-center">
{% btn 'http://t.csdn.cn/SPiNB','文档详解',far fa-hand-point-right,green larger %}
{% btn 'https://www.bilibili.com/video/BV1934y1a7MN/?spm_id_from=333.337.search-card.all.click&vd_source=51dc0d76d16e5d6eb2c55984ed42ac56','视频详解',far fa-hand-point-right,green larger %}
</div>

## v-model 的实现原理
{% note success no-icon flat %}
我们在 vue 项目中主要使用 v-model 指令在表单 input、textarea、select 等元素上创建双向数据绑定，我们知道 v-model 本质上不过是语法糖，v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：
● text 和 textarea 元素使用 value 属性和 input 事件；
● checkbox 和 radio 使用 checked 属性和 change 事件；
● select 字段将 value 作为 prop 并将 change 作为事件。

以 input 表单元素为例：
```js
<input v-model='something'> 相当于     
<input v-bind:value="something" v-on:input="something = $event.target.value">
```

如果在自定义组件中，v-model 默认会利用名为 value 的 prop 和名为 input 的事件，如下所示：
```js
<ModelChild v-model="message"></ModelChild>

子组件：
<div>{{value}}</div>

props:{
    value: String
},
methods: {
  test1(){
     this.$emit('input', '小红')
  },
},
```
{% endnote %}

## SPA单页面应用
{% note success no-icon flat %}
> 单页面应用 (SPA，Single Page Application): 所有功能在一个 html 页面上实现

优点：
- 不整个刷新页面，每次请求仅获取需要的部分，用户体验更好
- 数据传递容易，开发效率高

缺点：
- 开发成本高 (需要学习专门知识 - 路由)
- 首次加载会比较慢一点，不利于 seo
{% endnote %}

## vuex📁
### Vuex核心属性
{% note success no-icon flat %}
➤ <font color=red>state</font>：定义需要管理的数据
➤ <font color=red>getters</font>：state派生出来的数据，相当于state的计算属性
➤ <font color=red>mutation</font>：里面定义的是同步的更新数据方法，每个方法里都有两个参数（state、payload），通过store.commit调用
➤ <font color=red>action</font>：里面定义的是异步的方法，每个方法里面有两个参数（store、payload），通过store.dispatch调用，在actions里也可以提交mutation，通过store.commit
➤ <font color=red>module</font>：将vuex模块化，可以让每一个模块拥有自己的state、mutation、action、getters，结构清晰，方便管理
{% endnote %}

### 举一个使用vuex的完整过程
{% note success no-icon flat %}
我就描述一下登录token存放在vuex中的一个完整过程吧
① 先创建一个vuex中的user模块，里面定义好对应的 state,actions,mutations
② 将登录请求封装在actions中，一旦获取到了服务器响应回来的token我们就通过commit触发mutations中的方法，将其存储到state中
③ 为了解决刷新整个浏览器导致token丢失的问题，我们采取的是本地存储解决
④ 如果是一次集成vuex，还会先安装vuex，然后进行use绑定到vue框架，最后new 一个vuex的实例，挂到new Vue配置中
{% endnote %}

## 路由📁
### 前置路由守卫
{% note success no-icon flat %}
> 定义：路由跳转之前, 会触发的一个函数

语法：router.beforeEach((to, from, next) => {这里可以写路径的跳转判断/有无token值的情况分析})
作用 : 防止别人猜到网址的hash值后直接跳过登录就可以查看数据
里面的3个参数：
- to : 到哪里去
- from : 从哪里来
- next : 放行函数，next():放行 , next(false):不放行
{% endnote %}

### vue-router传参方式
{% note success no-icon flat %}
动态路由-**路径拼接传参**
- 传：this.$router.push({path: \`/xxx/${id}\`)
- 接：this.$route.params.id

动态路由-**params属性传参**
- 传：this.$router.push({name: 'xxx', params: {id: 值})
- 接：this.$route.params.id

**query属性传参**
- 传：this.$router.push({ name: 'xxx', query: { id: 值 } })
- 接：this.$route.query.id
{% endnote %}

## webpack的了解
{% note success no-icon flat %}
webpack主要是我们做工程化开发的打包工具，它会让你指定一个入口，然后通过这个入口分析出此项目的所有依赖，最终将它们打包合并成一个或者多个js文件。
同时他还有很多插件和loader能帮我们提升开发效率。
{% endnote %}

{% note warning no-icon flat %}
> 一个打包模块化的工具，在webpack中一切文件皆为模块，通过loader转换文件，通过plugin注入钩子，最后输出由多个模块组合成的文件

作用：**由于浏览器对于js中的很多代码不可以直接进行解析读取，这个时候需要先通过 wabpack 把资源进行打包，解析成浏览器可以识别的代码**
配置： 
- 入口：指示webpack使用哪个模块作为构建其内部依赖图的开始，默认值是：‘./src/index.js’
- 出口：告诉webpack在哪里输出它所创建的bundle，以及如何命名这些文件，主要输出文件的默认值是‘./dist/main.js’
- mode：配置模式，development(开发环境)、production(生产环境)、none(不使用任何默认优化选项)
- loader：自带能力，用于转换某些类型的模块
- plugin：打包优化，资源管理，注入环境变量等

流程： 
- 初始化参数
- 开始编译
- 确定入口
- 编译
- 完成模块编译
- 输出资源
- 输出完成
{% endnote %}


## v-for中key的作用
{% note success no-icon flat %}

对于用<font color=red>v-for渲染的列表数据</font>来说，数据量一般很庞大，而且我们经常还要对这个数据进行一些增删改操作。那么整个列表都要重新进行渲染一遍，那样就会<font color=red>很费性能</font>。而<font color=red>key的出现</font>就尽可能的回避这个问题，<font color=red>提高效率</font>。<br/>
v-for默认使用<font color=red>就地复用的策略</font>，列表数据修改的时候，它会<font color=red>根据key值去判断某一个值是否修改，如果修改则重新渲染该项，否则复用之前的元素</font>。在v-for中我们的key一般为id，也就是唯一的值，但是一般不要使用index作为key，因为index的值会重复。
{% endnote %}

## 说5个vue的指令
{% note success no-icon flat %}
<font size = 5>
1. v-bind：绑定属性 <br/>
2. v-if 、v-show：条件渲染 <br/>
3. v-for： 列表渲染 <br/>
4. v-model：双向绑定 <br/>
5. v-html：解析html字符串 <br/>
6. v-on：绑定事件
</font>

{% endnote %}

## v-show和v-if的区别
{% note success no-icon flat %}
作用：添加渲染，切换显示与隐藏
不同：v-if会移除dom或组件树，v-show则只是通过样式隐藏
场景：
1. v-if：切换不频繁、敏感数据的隐藏（如权限按钮）
2. v-show：切换频繁的场景
{% endnote %}

## v-for和v-if为什么不能一起使用
{% note success no-icon flat %}
在vue2中v-for<font color=red>优先级</font>高于v-if，如果二者放在同一级标签里面，每次都要先循环，再判断，消耗很多性能。
对于同一组数据来说，如果我们要先判断再渲染，可以在外层包装一个div，使用v-if做一次判断即可。Vue3解决了这个问题，将v-if的优先级调整为高于v-for了。
{% endnote %}

## diff算法
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E7%9A%84diff%E7%AE%97%E6%B3%95.png)

## watch深度侦听和立即侦听
{% note success no-icon flat %}
1. deep（深度侦听）：默认情况下，侦听器无法侦听对象的属性值的变化，如果想实现这个效果，则需要添加deep配置为true
2. <u>handler（固定方法触发）：因为你要添加deep的配置，所以，侦听器的形式要变更为对象形式，只有对象才能添加其它的配置, 同时侦听函数必须为handler</u>
3. immediate（立即侦听）：如果需要默认一进页面就触发一次，添加immediate配置选项为true
{% endnote %}

## webpack
{% note success no-icon flat %}
{% span red, 定义 %}：是一个打包模块化的工具，在webpack中一切文件皆为模块，通过loader转换文件，通过plugin注入钩子，最后输出由多个模块组合成的文件
{% span red, 作用 %}：由于浏览器对于js中的很多代码不可以直接进行解析读取，这个时候需要先通过 wabpack 把资源进行打包，解析成浏览器可以识别的代码
{% span red, 配置 %}： 
  - 入口：指示webpack使用哪个模块作为构建其内部依赖图的开始，默认值是：‘./src/index.js’
  - 出口：告诉webpack在哪里输出它所创建的bundle，以及如何命名这些文件，主要输出文件的默认值是‘./dist/main.js’
  - mode：配置模式，development(开发环境)、production(生产环境)、none(不使用任何默认优化选项)
  - loader：自带能力，用于转换某些类型的模块
  - plugin：打包优化，资源管理，注入环境变量等

{% span red, 流程 %}： 
  - 初始化参数
  - 开始编译
  - 确定入口
  - 编译
  - 完成模块编译
  - 输出资源
  - 输出完成
{% endnote %}

## 一个页面从输入 URL 到页面加载显示完成的过程
{% note success no-icon flat %}
- 浏览器查找域名对应的IP地址，向服务器发送一个请求
- 服务器重定向
- 浏览器追踪重定向地址，请求另一个带www的网址
- 服务器处理请求读取资源，返回一个响应
- 浏览器构建dom树，发请求获取页面资源完成页面
{% endnote %}

## 纯函数
纯函数就是一个函数，只不过具有一些特点，你可能平时开发中都有用到，只是没有意识到这是一个纯函数。
纯函数（Prue function）具有以下特点：
1. 纯函数每一次调用时传入同样的参数，返回的都是同样的结果；它不会改变参数的值，也不会改变外部变量的值；它不会依赖于外部的变量，仅依赖于你传入的参数；
2. 纯函数没有其他副作用（side effect）
3. 如果你每次传入的参数一样，但是返回的结果不一样，则不是一个纯函数

这是一个纯函数:
```js
/*
    它没有改变外部变量的值
    每次调用时如果传递的值相同，那每次返回的结果也相同
*/
let a = 1;
let b = 2;
function add(x, y) {
    return x + y;
}
add(a, b);
```
这不是一个纯函数:

## H5事件
{% note success no-icon flat %}
- onblur失焦事件
- onfocus聚焦事件
- onchange改变事件
- onclick点击事件
- onerror错误事件
- oninput输入事件
- onkeydown键盘按下事件
- onkeyup键盘抬起事件
- onmousemove鼠标移动事件
- onmouseover鼠标进入事件
- onmouseout鼠标移出事件等
{% endnote %}

## Vue组件通讯
{% note success no-icon flat %}
- 父传子：子组件定义props属性接收
- 子传父：子组件中使用this.$emit方法
- 兄弟传值：事件总线，$on方法
- 父传孙：provide和inject方式
{% endnote %}

## 如何封装组件
{% note success no-icon flat %}
- 原因：封装组件可以提升项目开发效率，把页面抽象成多个相对独立的模块，复用性高
- 步骤： 
  - 创建一个组件
  - Vue.component注册组件
  - 子组件需要数据，可以在props中接受定义
  - 而子组件修改好数据后，想把数据传递给父组件，可以采用emit方法
{% endnote %}

## nextTick的理解
{% note success no-icon flat %}
使用nextTick的原因：Vue是异步修改DOM的，并且不鼓励开发者直接接触DOM，但是有时候必须对数据更改后的DOM元素做相应的处理（例如：下面代码），但是获取到的DOM数据并不是更改后的数据，这时候就需要nextTick()来帮我们实现了
```html
<button @click="change()">按钮</button><h1 ref="gss">{{msg}}</h1>

<script>
export default{
  name:"app",
    data(){
    return {
      msg:"123"
    }
  },
  methods:{
    change(){
      this.msg = "456";
      console.log(this.refs["gss"].innerHTML)//123
      this.$nextTick(function(){
        console.log(this.refs["gss"].innerHTML)//456
      })
    }
  }    
}
</script>
```
{% endnote %}

## 路由模式
{% note success no-icon flat %}
hash模式： 
  - 浏览器中符号是“#”，#以及#后面的字符称之为 hash，又叫前端路由
  - 用 window.location.hash 读取
  - hash 虽然在 URL 中，但不被包括在 HTTP 请求中
  - hash 改变会触发 hashchange 事件
  - hash发生变化的url都会被浏览器记录下来，从而浏览器的前进后退都可以用

history模式： 
  - history 采用 HTML5 的新特性
  - history 模式不仅可以在url里放参数，还可以将数据存放在一个特定的对象中
  - 它也有个问题：不怕前进，不怕后退，就怕{% span red, '刷新' %}（如果后端没有准备的话，会分分钟刷出一个404来），因为刷新是实实在在地去请求服务器的
{% endnote %}

## SPA
{% note success no-icon flat %}
- 单页面应用，将所有的活动局限于一个web页面中，仅在初始化的时候加载HTML、JS、CSS，一旦页面加载完成，就不会因为用户的操作而进行页面的重新加载或跳转
- 利用JS的动态变化HTML内容，实现交互，避免页面的重新加载
{% endnote %}

## 路由之间跳转方式
{% note success no-icon flat %}
四种方式： 
- router-link：搭配to属性，在模板中使用
- push()：跳转到指定页面
- replace()：跳转到指定页面，但是没有历史记录跳不回去
- go(N)N：可以为正数也可以为负数，正数是向前跳转，负数是向后跳转
{% endnote %}

## 组件创建和挂载相关的钩子函数 
{% note success no-icon flat %}
- beforeCreate
- created
- beforeMount
- mounted
{% endnote %}

## 组件更新相关的钩子函数
{% note success no-icon flat %}
- beforeUpdate
- updated
{% endnote %}

## Vue组件之间的传递方式
{% note success no-icon flat %}
`[回答第1,2条-合格，因为是常用的]`
1. 父组件向子组件传递数据，使用props属性；子组件向父组件中传递数据，在子组件中使用$emit派发事件，父组件中使用v-on监听事件。缺点:组件嵌套层次多的话，传递数据比较麻烦。
2. 通过Vuex，实现多个组件进行数据共享，推荐使用这种方式进行项目中各组件间的数据传递。

`[如果还能将第3条回答出-良好]`
1. 通过事件总线(eventbus)的方式，可以实现任意两个组件间进行数据传递;缺点:不支持响应式，这个概念是vue1.0版本中的，现在已经废弃

`[如果还能将第4,5条回答出-优秀]`
1. 祖先组件通过依赖注入(inject/provide)的方式，向其所有子孙后代传递数据;缺点:无法监听数据修改的来源，不支持响应式。
2. 通过属性parent/$children/ref，访问根组件、父级组件、子组件中的数据;缺点:要求组件之间要有传递性。
{% endnote %}

## 三次握手和四次挥手：
{% note success no-icon flat %}
三次握手： 
- 发送端首先发送一个带有SYN标志的数据包给接收方
- 接收方接收后，回传一个带有SYN/ACK标志的数据包传递确认信息，表示我收到了
- 发送方再回传一个带有ACK标志的数据包，代表我知道了，表示‘握手’结束

四次挥手： 
- Client发送一个FIN，用来关闭Client到Server的数据传送，Client进入FIN_WAIT_1状态
- Server收到FIN后，发送一个ACK给Client，确认序号为收到序号+1（与SYN相同，一个FIN占用一个序号），Server进入CLOSE_WAIT状态
- Server发送一个FIN，用来关闭Server到Client的数据传送，Server进入LAST_ACK状态
- Client收到FIN后，Client进入TIME_WAIT状态，接着发送一个ACK给Server，确认序号为收到序号+1，Server进入CLOSED状态，完成四次挥手
{% endnote %}

## 组件销毁相关的钩子函数
{% note success no-icon flat %}
- beforeDestroy
- destroyed
{% endnote %}

## 路由懒加载 
{% note success no-icon flat %}
路由懒加载也叫{% span red, '延迟加载' %}，即在需要的时候进行加载，随用随载
出现的原因： 
  - 单页面应用如果没有用懒加载则webpack打包后文件很大
  - 进入首页的时候需要加载的内容过多，出现白屏现象，不利于用户体验

实现方法： 
  - 配置异步组件：{% span red, '()=> import(“vue页面路径”)' %}
{% endnote %}

## vue-router动态路由
{% note success no-icon flat %}
场景：把某种模式匹配到的所有路由，全都映射到同个组件
解决办法：以在 vue-router 的路由路径中使用动态路径参数
- 使用 ：开头
- 在路径中最后使用 ？传参
{% endnote %}

## webpack的了解
{% note success no-icon flat %}
{% endnote %}

## 对拦截器进行说明
{% note success no-icon flat %}
作用：
- Axios 是一个基于 promise 的 HTTP 库，支持promise所有的API
- 可以拦截请求和响应
- 可以转换请求数据和响应数据，并对响应回来的内容自动转换成 JSON类型的数据
- 安全性更高

相关配置：
- url：请求的服务器地址
- method：请求方法
- baseURL：基准路径
- headers：请求头
- parmas：路径参数

做了什么：
- 请求拦截器：在请求发送前进行的操作，如：每个请求体里加上token
- 响应拦截器：接收到响应后进行的操作，如：服务器返回的登录状态失效，就跳转到登录页
{% endnote %}

## vue给对象添加新属性界面不刷新原因和解决办法
{% note success no-icon flat %}
原因：Vue 不允许在已经创建的实例上，动态添加新的响应式属性；

三种解决办法：
1. 使用Vue.set( target , key , value)
2. 使用$fourceUpdate强制刷新
3. 克隆新对象，如this.persons ={...this.persons}， this.persons = Object.assign({}, this.persons)
{% endnote %}

# vue高阶
## vuex面试高频
