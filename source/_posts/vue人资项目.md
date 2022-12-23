---
title: vue人资项目
date: 2022-12-18 00:00:00
categories: "vue"
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE%E5%B0%81%E9%9D%A2.png
toc_expand: true
---

# 基础环境搭建

基础模板：vue-admin-template
克隆命令如下：
```bash
git clone https://gitee.com/panjiachen/vue-admin-template.git hr
```

## 安装eslint
- ctrl+shift+P: 打开工作区设置(JSON)
- 在setting.json输入：
```jsx
{ 
    "eslint.run": "onType",
    "eslint.options": {
        "extensions": [
            ".js",
            ".vue",
            ".jsx",
            ".tsx"
        ]
    },
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

## 删除mock代码
- 在main.js删除模拟接口的代码
```jsx
/**
 * If you don't want to use mock-server
 * you want to use MockJs for mock api
 * you can execute: mockXHR()
 *
 * Currently MockJs will be used in the production environment,
 * please remove it before going online ! ! !
 */
if (process.env.NODE_ENV === 'production') {
  const { mockXHR } = require('../mock')
  mockXHR()
}
```
- 在vue.config.js删除这一段
```jsx
  devServer: {
    port: port,
    open: true,
    overlay: {
      warnings: false,
      errors: true
    },
    // 删除下面这一句代码
    before: require('./mock/mock-server.js')
  },
```
- 谨记：重新运行`npm run dev`

## 清理核心功能模块
- 清空permission.js代码
- 清理src/store/modules/user.js
```jsx
export default {
  namespaced: true,
  state: {},
  mutations: {},
  actions: {}
}
```
- 在src/store/getters.js删除user相关的getter
```jsx
const getters = {
  sidebar: state => state.app.sidebar,
  device: state => state.app.device,
  // 删除下面的三句代码
  token: state => state.user.token,
  avatar: state => state.user.avatar,
  name: state => state.user.name
}
export default getters
```

## 导入新公共样式和图片图标
1. 导入新公共样式
  - 把资源包里的“样式”都复制到src/styles
  - 在src/styles/index.scss中导入上面的common.scss
  ```jsx
  @import './common.scss';
  ```

2. 导入图片图标
  - 把资源包里的“图片/common”文件夹复制到src/assets里
  - 把资源包里的“菜单svg”所有文件复制到src/icons/svg里


## 推送项目到自己的仓库
- 删除.git文件夹
- git init
- git add . 
- git commit -m '提交消息'
- git remote add origin 仓库地址
- git push -u origin 本地分支名


## 清理axios和接口封装
- 清理src/utils/request.js
```jsx
// 导出一个axios的实例  而且这个实例要有请求拦截器 响应拦截器
import axios from 'axios'

const service = axios.create({
  baseURL: process.env.VUE_APP_BASE_API, // url = base url + request url
  // withCredentials: true, // send cookies when cross-domain requests
  timeout: 5000 // 超时时间
}) // 创建一个axios的实例

service.interceptors.request.use() // 请求拦截器
service.interceptors.response.use() // 响应拦截器

export default service // 导出axios实例
```
- 清理src/api/user.js
```jsx
// import request from '@/utils/request'

// 登录
export function login(data) {

}

// 获取用户信息
export function getInfo(token) {

}

// 退出登录
export function logout() {

}
```
- 删除src/api/table.js


# 登录模块

![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/vue%E4%BA%BA%E8%B5%84%E9%A1%B9%E7%9B%AE/%E7%99%BB%E5%BD%95%E9%A1%B5%E9%9D%A2.jpg)

## 基础布局

设置头部背景
```jsx
<!-- 放置标题图片 @是设置的别名-->
<div class="title-container">
        <h3 class="title">
          <img src="@/assets/common/login-logo.png" alt="">
        </h3>
</div>
```
设置背景图片
```css
/* reset element-ui css */
.login-container {
  background-image: url('~@/assets/common/login.jpg'); // 设置背景图片
  background-position: center; // 将图片位置设置为充满整个屏幕
}
```
设置手机号和密码的字体颜色
```css
$light_gray: #68b0fe;  // 将输入框颜色改成蓝色
```
设置输入表单整体背景色
```css
  .el-form-item {
    border: 1px solid rgba(255, 255, 255, 0.1);
    background: rgba(255, 255, 255, 0.7); // 输入登录表单的背景色
    border-radius: 5px;
    color: #454545;
  }
```
设置错误信息的颜色
```css
 .el-form-item__error {
    color: #fff
  }
```
设置登录按钮的样式
```css
.loginBtn {
  background: #407ffe;
  height: 64px;
  line-height: 32px;
  font-size: 24px;
}
```
修改显示的提示文本和登录文本
```jsx
   <div class="tips">
        <span style="margin-right:20px;">账号: 13800000002</span>
        <span> 密码: 123456</span>
   </div>
```

## 登录表单的校验

接下来的操作，请参考网址：http://120.24.171.137:9525/#/md/03-%E7%99%BB%E5%BD%95%E6%A8%A1%E5%9D%97