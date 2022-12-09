---
title: yarn，一个比npm更好用的命令行程序
date: 2022-12-10 00:00:00
categories: 工具
description: yarn，一个比npm更好用的命令行程序
cover: "https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/yarn%E5%B0%81%E9%9D%A2.png"
toc_expand: true
---
## 简介和安装
{% btn 'https://www.yarnpkg.cn/cli/install','yarn官方文档',far fa-hand-point-right,block blue center larger %}

{% note info no-icon flat %}
Yarn是facebook发布的一款取代npm的包管理工具。
{% endnote %}

{% note success no-icon flat %}
特点：
➤ 速度超快：Yarn 缓存了每个下载过的包，所以再次使用时无需重复下载，同时利用并行下载以最大化资源利用率，安装速度更快。
➤ 超级安全：在执行代码之前，Yarn 会通过算法校验每个安装包的完整性。
➤ 超级可靠：使用详细、简洁的锁文件格式和明确的安装算法，Yarn 能够保证在不同系统上无差异的工作。
{% endnote %}

{% note primary no-icon flat %}
yarn安装:
```bash
npm install -g yarn
yarn --version (安装成功后，查看版本号)
```
{% endnote %}

---

## npm 与 yarn的比较
### 命令比较
| npm | yarn     | 说明 |
| :-------- | :------- | :--- |
| npm init  | yarn init | 初始化某个项目   |
| npm install | yarn(或yarn install) | 默认的安装依赖操作   |
| npm install <依赖包名> [-D]    | yarn add <依赖包名> [-D]  | 安装某个依赖 |
| npm install <依赖包名> -g  | yarn global add <依赖包名>   | 全局安装某个依赖 |
| npm uninstall <依赖包名>    | yarn remove <依赖包名>  | 移除某个依赖 |
| npm update <依赖包名>    | yarn upgrade <依赖包名>  | 更新某个依赖 |
| npm publish/login/logout    | yarn publish/login/logout  | 发布/登录/登出，一些列NPM Registry 操作 |
| npm run serve/start    | yarn serve/start  | 运行某个命令 |

### 相关问题比较
npm存在一些历史遗留问题，请看下图：
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/npm%E5%AD%98%E5%9C%A8%E4%B8%80%E4%BA%9B%E5%8E%86%E5%8F%B2%E9%81%97%E7%95%99%E9%97%AE%E9%A2%98.png)
比如说你的项目模块依赖是图中描述的，@1.2.1代表这个模块的版本。在你安装A的时候需要安装依赖C和D，很多依赖不会指定版本号，默认会安装最新的版本，这样就会出现问题：比如今天安装模块的时候C和D是某一个版本，而当以后C、D更新的时候，再次安装模块就会安装C和D的最新版本，如果新的版本无法兼容你的项目，你的程序可能就会出BUG，甚至无法运行。这就是npm的弊端，而yarn为了解决这个问题推出了yarn.lock的机制，这是作者项目中的yarn.lock文件。

yarn.lock文件格式:
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/yarn.lock%E6%96%87%E4%BB%B6%E6%A0%BC%E5%BC%8F.png)

大家会看到，这个文件已经把依赖模块的版本号全部锁定，当你执行yarn install的时候，yarn会读取这个文件获得依赖的版本号，然后依照这个版本号去安装对应的依赖模块，这样依赖就会被锁定，以后再也不用担心版本号的问题了。其他人或者其他环境下使用的时候，把这个yarn.lock拷贝到相应的环境项目下再安装即可。
注意：这个文件不要手动修改它，当你使用一些操作如yarn add时，yarn会自动更新yarn.lock。

---

## 常用命令
{% note danger no-icon flat %}
yarn安装:
```js
npm install -g yarn
yarn --version // 安装成功后，查看版本号
```
初始化项目：
```js
yarn init // 同 npm init，生成package.json文件
```
yarn的配置项：
```js
yarn config list // 显示所有配置项
yarn config get <key> // 显示某配置项
yarn config delete <key> // 删除某配置项
yarn config set <key> <value> [-g|--global] // 设置配置项
```
安装包：
```js
yarn // 等同 yarn install，安装package.json里所有包，并将包及它的所有依赖项保存进yarn.lock
yarn install --flat // 安装一个包的单一版本
yarn install --force // 强制重新下载所有包
yarn install --production // 只安装dependencies里的包
yarn install --no-lockfile // 不读取或生成yarn.lock
yarn install --pure-lockfile // 不生成yarn.lock
```
添加包（会更新package.json和yarn.lock）：
```js
yarn add [package] // 在当前的项目中添加一个依赖包，会自动更新到package.json和yarn.lock文件中
yarn add [package]@[version] // 安装指定版本，这里指的是主要版本，如果需要精确到小版本，使用-E参数
yarn add [package]@[tag] // 安装某个tag（比如beta,next或者latest）
// 不指定依赖类型默认安装到dependencies里，你也可以指定依赖类型：

yarn add --dev/-D // 加到 devDependencies
yarn add --peer/-P // 加到 peerDependencies
yarn add --optional/-O // 加到 optionalDependencies
// 默认安装包的主要版本里的最新版本，下面两个命令可以指定版本：

yarn add --exact/-E // 安装包的精确版本。例如yarn add foo@1.2.3会接受1.9.1版，但是yarn add foo@1.2.3 --exact只会接受1.2.3版
yarn add --tilde/-T // 安装包的次要版本里的最新版。例如yarn add foo@1.2.3 --tilde会接受1.2.9，但不接受1.3.0
```
发布包
```js
yarn publish
```
移除一个包
```js
yarn remove <packageName> // 移除一个包，会自动更新package.json和yarn.lock
```
更新一个依赖
```js
yarn upgrade // 用于更新包到基于规范范围的最新版本
```
运行脚本
```js
yarn serve/start... // 用来执行在 package.json 中 scripts 属性下定义的脚本
```
显示某个包的信息
```js
yarn info <packageName> // 可以用来查看某个模块的最新版本信息
```
查看 yarn 全局安装过的包
```js
yarn global list --depth=0
```
缓存
```js
yarn cache
yarn cache list // 列出已缓存的每个包 
yarn cache dir // 返回 全局缓存位置 
yarn cache clean // 清除缓存
```
{% endnote %}

--- 

## 使用yrm工具管理npm源
{% note danger no-icon flat %}
```bash
yarn global add yrm # 安装
yrm ls # 查看可用源
yrm test # 测试所有源的响应时间
yrm use taobao # 选择源（建议淘宝）

# rimraf是node的一个包，可快速删除node_modules
yarn global add rimraf # 安装，可 yarn global list --depth=0 检测是否安装成功
rimraf node_modules # 使用
```
{% endnote %}