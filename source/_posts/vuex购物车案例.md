---
title: Vuex购物车案例
date: 2022-12-10 18:30:00
categories: "vue"
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%A1%88%E4%BE%8B/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E5%B0%81%E9%9D%A2.png
sticky: 5
toc_expand: true
---

## 案例展示图
{% image https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%A1%88%E4%BE%8B/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%95%88%E6%9E%9C%E5%9B%BE.png, width=300px %}

## 使用Vuex
### 创建并注册商品的vuex模块
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%A1%88%E4%BE%8B/%E5%88%9B%E5%BB%BA%E5%B9%B6%E6%B3%A8%E5%86%8C%E5%95%86%E5%93%81%E7%9A%84vuex%E6%A8%A1%E5%9D%97.png)
		
### 封装商品列表action
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%A1%88%E4%BE%8B/%E5%B0%81%E8%A3%85%E5%95%86%E5%93%81%E5%88%97%E8%A1%A8action.png)
			
### 商品列表组件-读取Vuex中的数据
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%A1%88%E4%BE%8B/%E5%95%86%E5%93%81%E5%88%97%E8%A1%A8%E7%BB%84%E4%BB%B6%20-%20%E8%AF%BB%E5%8F%96%20Vuex%20%E4%B8%AD%E7%9A%84%E6%95%B0%E6%8D%AE.png)

### 处理底栏总数量/总价格
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%A1%88%E4%BE%8B/%E5%A4%84%E7%90%86%E5%BA%95%E6%A0%8F%E6%80%BB%E6%95%B0%E9%87%8F%E5%92%8C%E6%80%BB%E4%BB%B7%E6%A0%BC.png)
			
### 修改商品数量
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Vuex%E8%B4%AD%E7%89%A9%E8%BD%A6%E6%A1%88%E4%BE%8B/%E4%BF%AE%E6%94%B9%E5%95%86%E5%93%81%E6%95%B0%E9%87%8F.png)