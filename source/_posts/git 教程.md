---
title: git 教程
date: 2022-11-14 00:00:10
categories: 工具
description: 一个免费的开源分布式版本控制系统，旨在快速高效地处理从小型项目到大型项目的所有内容。
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/git.jpeg
--- 
<div class="btn-center">
</div>

<div class="btn-center">
{% btn 'https://git-scm.com/',git官网,far fa-hand-point-right,orange larger %}
{% btn 'https://git-scm.com/book/zh/v2','git命令文档',far fa-hand-point-right,green larger %}
</div>


## 前置知识
git仓库会分成三个区
{% note info no-icon flat %}
`工作区`：我们**书写代码**的地方，工作的目录就叫工作区。
`暂存区`：暂时存储的区域，在git中，代码无法直接从工作区提交到仓库区，而是需要先从工作区添加到暂存区，然后才能从暂存区提交到仓库区。暂存区的目的是**避免误操作，记录Git操作**。
`本地仓库区`：将保存在暂存区域的内容**永久转储到 Git 仓库中**，**生成版本号**。生成版本号之后，就可以任何的回退到某一个具体的版本。
{% endnote %}

## 配置git邮箱与账号
```Bash
# 使用--global参数，配置全局的用户名和邮箱，只需要配置一次即可。推荐配置gitee的用户名和密码
git config --global user.name '自己的用户名'
git config  --global user.email '自己的正确的邮箱'

# 查看配置信息
git config --list

# 如果配置错了，可以覆盖(重新配置)，也可以删除配置后再配置
git config --global --unset user.name "自己的用户名"
git config --global --unset user.email "自己的邮箱"

# 生成 ssh 公钥
ssh-keygen -t rsa -C "自己的邮箱"
```


## git常用命令

```Bash
# 创建仓库
git clone # 克隆仓库（文件夹名称和被克隆项目一致）
git clone [文件夹名字] # 克隆仓库（自定义文件夹名称）

git init # 初始化

# 处理修改
git add # 把修改添加到暂存区
git add . # .是当前路径
git mv 文件路径1 文件路径2 # 移动文件

git status 【查看文件状态】
# 红色：工作区中的文件需要提交add；绿色：暂存区中的文件需要提交
# 编辑器中的标识
# - U：未被追踪的文件，说明当前文件还没有add
# - A：已经执行了git add,但是还没有commit到仓库
# - M：文件被修改，但是还没有add
# - C：conflict:文件冲突

git commit 【添加文件到仓库】
# git commit -m "提交说明"：将文件从【暂存区】提交到【仓库】
# git commit：如果不写提交说明，会进入vi编辑器，没有写提交说明，是提交不成功的。需要使用vi输入内容  
# git commit -a -m '提交说明'：如果是一个已经暂存过的文件，可以快速提交，如果是未追踪的文件，那么命令将不生效。
# git commit --amend -m "提交说明"：修改最近的一次提交说明， 如果提交说明不小心输错了，可以使用这个命令

git log 【查看提交的日志】
# git log：查看提交的日志
# git log --oneline：将每次提交的日志通过一行显示
# git reflog：查看所有日志，包括回退操作的日志

git diff 【查看每次提交的内容差异】
# git diff：查看工作区与暂存区的不同,先不去做add
# git diff --cached：查看暂存区与仓库区的不同，先add
# git diff HEAD：查看工作区与仓库区的不同，HEAD表示最新的那次提交
# git diff c265262 de4845b：查看两个版本之间的不同

git restore 文件路径 # 还原代码

git mv 文件路径1 文件路径2 # 移动文件
git rm 文件路径 # 删除文件

git reset 【版本回退】
# git reset --hard 版本号：将代码回退到某个指定的版本(版本号只要有前7位即可)
# git reset --hard head~1：将版本回退到上一次提交
# - ~1：上一次提交
# - ~2：上上次提交
# - ~0：当前提交
# 当使用了git reset命令后，版本会回退，
# 使用git log只能看到当前版本之前的信息。使用git reflog可以查看所有的版本信息

.gitignore 【忽略文件】
# 在仓库的根目录创建 .gitignore，并在里面添加忽略的文件
# 比如：idea.txt：忽视idea.txt文件
#      css/index.js：忽视css下的index.js文件
#      css/*.js：忽视css下的所有的js文件
#      css/*.*：忽视css下的所有文件
#      css：忽视css文件夹

# 团队合作
git pull # 拉取并更新代码
git push # 推送代码到远程仓库
git fetch # 更新远程代码的信息

# git历史
git diff # 对比代码
git log # 查看提交日志
git show # 查看不同类型的内容，比如 git show 98f3530:查看某一条commit提交了什么
git status # 查看当前工作区状态
git grep # 搜索代码

```

## git分支操作
```bash
git branch：查看分支
git branch "分支名称"：创建分支
git checkout "分支名称"：切换分支
git checkout -b "分支名称"：创建并切换分支
git merge "分支名称"：合并分支
git branch -d "分支名称"：删除分支
```

## git远程仓库
<div class="btn-center">
{% btn 'https://github.com/',Github,far fa-hand-point-right,green larger %}
{% btn 'https://gitee.com/',Gitee码云,far fa-hand-point-right,green larger %}
</div>

`git`是一个{% label 版本控制工具 pink %};
`github(或gitee)`是一个代码托管平台，开源社区，是git的一个{% label 远程代码仓库 green %}。

```bash
git push 【将本地仓库中代码提交到远程仓库】
# git push "仓库地址" master

git clone 【完整克隆远程仓库的代码到本地】
# git clone [远程仓库地址]
# 比如：git clone git://github.com/autumnFish/test.git
# 会在本地新建一个`test`文件夹，在test中包含了一个`.git`目录，用于保存所有的版本记录，
# 同时test文件中还有最新的代码，你可以直接进行后续的开发和使用。
# git克隆默认会使用远程仓库的项目名字，也可以自己指定。需要是使用以下命令：git clone [远程仓库地址] [本地项目名]

git pull 【将远程的代码下载到本地】
# git pull "仓库地址" master
# 通常在push前，需要先pull一次
# git pull：获取远程仓库的更新，并且与本地的分支进行合并

```

```bash
# 给远程仓库设置一个别名
git remote add 仓库别名 仓库地址
git remote add autumnFish git@github.com:autumnFish/test.git

# autumnFish
git remote remove autumnFish

# 检查是否关联成功
git remote -v

# 一般情况需要先pull一下：git pull origin master

# push到远程库：
git push -u autumnFish master
git push
git pull

# git clone的仓库默认有一个origin的别名
```

## 实战操作
```txt
一：
1.配置邮箱和账号
2.创建一个项目目录，进行相应的文件的添加
3.初始化：git init
4.git add .
5.git commit -m '说明'

二：
1.修改或者添加文件：add + commit
2.重复上面的操作至少三次
3.通过 git log 确认有多次记录

三：
1.基于之前的多次提交
2.使用 git reset --hard 版本号 实现版本回退
3.通过 git log | git log --oneline | git reflog 查看版本号
4.回退之后在已提交版本间穿梭

四：使用分支解决登陆 bug
1.查看你想创建的分支是否会重名 -- git branch
2.创建一个分支，如名称为 login -- git branch login
3.切换到 login 分支，进行相关操作，且 add + commit  -- git checkout login
4.切换到主分支 master -- git checkout master
5.将 login 分支合并到主分支 -- git merge login
6.删除 login 分支： -- git branch -d login
7-到此：bug解决

五：演示合并冲突的产生
准备模拟两个小伙伴，分别开两个不同的分支解决不同的问题，但是两个小伙伴都会修改同一个文件common.js
产生冲突的前提：都修改了同一个文件同一行代码，且修改的结果不一样

A:解决登陆 bug
1.创建一个分支： git branch login
2.切换到 login 分支：git checkout login
3.在 login 分支上进行操作，且 add + commit

---同时还有一个业务线在开展
B:他要解决商品页面的 bug -- 我们要先切换到 master 分支
1.创建一个分支： git branch goods
2.切换到 goods 分支：git checkout goods
3.在 goods 分支上进行操作，且 add + commit

---经理来了，要将你们两个人的分支代码合并到主分支
1.切换到主分支：git checkout master
2.先合并 login : git merge login
3.再合并 goods : git merge goods --- 冲突来了
4.手动解决冲突
5.再次进行 add + commit

六：使用 https 的方式将代码推送到远程
1.注册账号
2.创建一个仓库，找到它的 https 的地址
3.在本地代码 add + commit 之后，输入命令：git push 仓库地址 master
4.输入 gitee 的账号和密码(如果输入错误了，则删除凭据管理器的相关记录，再次输入) 
5.进入到仓库页面查看是否有内容

七：实现远程代码共享
1.在你的本地修改代码，add + commit
2.推送到远程仓库
3.在同事目录下拉取代码到她的本地
```