---
title: 'Web APIs 项目'
date: 2022-11-02 14:32:44
categories: JavaScript
description: 《捐赠管理》《今日待办》《打地鼠》《点名系统》等案例详解
cover: https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/Web%20APIs%20%E9%A1%B9%E7%9B%AE.png
---
# 捐赠管理
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%8D%90%E8%B5%A0%E7%AE%A1%E7%90%86.png)

```javascript
let tbody = document.querySelector('tbody');
let add = document.querySelector('.add');
let getDonor = document.querySelector('.getDonor');
let getUnit = document.querySelector('.getUnit');
let getMoney = document.querySelector('.getMoney');
let getDate = document.querySelector('.getDate');
let editusername = document.querySelector('#editusername');
let editdepart = document.querySelector('#editdepart');
let editmoney = document.querySelector('#editmoney');
let editdate = document.querySelector('#editdate');
let btnedit = document.querySelector('.btnedit');
let search = document.querySelector('.search');
let searchValue = document.querySelector('.searchValue');

let donorlist = JSON.parse(localStorage.getItem('donorData')) || [
	{ id: 1, person: 'sun', unit: '壹基金', money: 1000, date: '2021-11-23 ' },
	{ id: 2, person: 'li', unit: '自然之友', money: 1000, date: '2021-01-15 ' },
	{ id: 3, person: 'wang', unit: '嫣然基金', money: 1000, date: '2021-06-7 ' },
];

// 渲染数据
function init(data = donorlist) {
	let htmlStr = '';
	data.forEach((value, index) => {
		let { id, person, unit, money, date } = value;
		htmlStr += `<tr class="table-content">
                            <td>${index + 1}</td>
                            <td>${person}</td>
                            <td>${unit}</td>
                            <td>${money}</td>
                            <td>${date}</td>
                            <td>
                                <a href="#" class="del" data-id="${id}">删</a>
                                <a href="#" class="update" data-id="${id}">改</a>
                            </td>
                        </tr>`;
	});
	tbody.innerHTML = htmlStr;
}
init();

// 新增
add.addEventListener('click', function () {
	let obj = {
		id: donorlist.length > 0 ? donorlist[donorlist.length - 1].id + 1 : 1,
		person: getDonor.value,
		unit: getUnit.value,
		money: getMoney.value,
		date: dateFormat(getDate.value),
	};
	donorlist.push(obj);
	init();
	save();
});

let temp; // 全局，方便后面确认更改操作
tbody.addEventListener('click', function (e) {
	// 删除
	if (e.target.className === 'del') {
		donorlist = donorlist.filter((value) => value.id != e.target.dataset.id);
		save();
		// 删除之后，再查询渲染
		search.click();
	}

	// 更改
	if (e.target.className === 'update') {
		$('#editModal').modal('show');
		temp = donorlist.filter((value) => value.id == e.target.dataset.id)[0];
		let { id, person, unit, money, date } = temp;
		editusername.value = person;
		editdepart.value = unit;
		editmoney.value = money;
		editdate.value = dateFormat(date);
	}
});

// 确认更改
btnedit.addEventListener('click', function () {
	$('#editModal').modal('hide');
	donorlist.forEach((value) => {
		if (value.id == temp.id) {
			value.person = editusername.value;
			value.unit = editdepart.value;
			value.money = editmoney.value;
			value.date = dateFormat(editdate.value);
		}
	});
	init();
	save();
});

// 查询
search.addEventListener('click', function () {
	let unit = searchValue.value;
	if (unit == '请选择') {
		init();
	} else {
		temp = donorlist.filter((value) => value.unit == unit);
		init(temp);
	}
});

// 日期格式化
function dateFormat(str) {
	let date = new Date(str);
	let year = date.getFullYear();
	year = year < 10 ? '0' + year : year;
	let month = date.getMonth() + 1;
	month = month < 10 ? '0' + month : month;
	let day = date.getDate();
	day = day < 10 ? '0' + day : day;
	let hour = date.getHours();
	hour = hour < 10 ? '0' + hour : hour;
	let minute = date.getMinutes();
	minute = minute < 10 ? '0' + minute : minute;

	return `${year}-${month}-${day} ${hour}:${minute}`;
}

// 本地存储
function save() {
	localStorage.setItem('donorData', JSON.stringify(donorlist));
}
```

# 今日待办
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E4%BB%8A%E6%97%A5%E5%BE%85%E5%8A%9E.gif)

```html
		<div id="app">
			<div class="box">
				<h1>待办列表</h1>
				<div class="tool">
					<input autofocus="" class="userTask" type="text" placeholder="ctrl+Enter,输入代办事项" />
				</div>
				<ul>
					<!-- <li class="finished">
						<div><input class="ck" type="checkbox" /><span class="false">任务名称-已完成状态</span></div>
						<button class="del">X</button>
					</li>
					<li class="">
						<div><input class="ck" type="checkbox" /><span class="false">任务名称-未完成状态</span></div>
						<button class="del">X</button>
					</li> -->
				</ul>
				<section>
					<span>4 未完成</span><a href="#">清理 <b>0</b> 已完成</a>
				</section>
			</div>
		</div>
```
```css
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	background-color: #ccc;
}
ul {
	list-style: none;
}
li {
	padding: 20px;
	text-align: left;
	font-size: 30px;
	border-bottom: 1px dashed #ccc;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
li input {
	margin-right: 10px;
}
li button {
	display: none;
	padding: 5px;
}
li:hover button {
	display: inline-block;
	cursor: pointer;
}
.finished .false{
	text-decoration: line-through;
}
h1 {
	margin-bottom: 10px;
}
.box {
	background-color: #fff;
	width: 60vw;
	padding: 20px 20px 0;
	margin: 50px auto;
}
.box .tool input {
	width: 100%;
	height: 50px;
	text-indent: 20px;
	font-size: 20px;
	font-style: italic;
	color: #666;
	font-weight: 700;
}
section {
	height: 50px;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
a {
	text-decoration-color: #666;
	color: inherit;
}
```
```javascript
let userTask = document.querySelector('.userTask');
let ul = document.querySelector('ul');
let numEle = document.querySelector('section span');
let clear = document.querySelector('section a');

let tasks = JSON.parse(localStorage.getItem('tasksData')) || [
	{ id: 1, name: '写100行代码', state: true },
	{ id: 2, name: '帮同桌解决两个bug', state: false },
	{ id: 3, name: '吃晚餐', state: false },
];

// 渲染数据
function init() {
	let num = 0;
	let htmlStr = '';
	tasks.forEach((value) => {
		let { id, name, state } = value;
		htmlStr += `<li class="${state ? 'finished' : ''}">
						<div>
							<input class="ck" type="checkbox" ${state ? 'checked' : ''} data-id="${id}"/><span class="false">${name}</span>
						</div>
						<button class="del" data-id="${id}">X</button>
					</li>`;

		if (!value.state) {
			num++;
		}
	});
	ul.innerHTML = htmlStr;
	numEle.innerHTML = `${num} 未完成`;
	numEle.nextElementSibling.querySelector('b').innerHTML = tasks.length - num;
}
init();

// 点击 ctrl+Enter 新增信息
userTask.addEventListener('keyup', function (e) {
	if (e.ctrlKey & (e.keyCode == 13)) {
		let obj = {
			id: tasks.length > 0 ? tasks[tasks.length - 1].id + 1 : 1,
			name: this.value,
			state: false,
		};
		tasks.push(obj);
		init();
		save();
		this.value = '';
	}
});

ul.addEventListener('click', function (e) {
	// 点击单选框
	if (e.target.className == 'ck') {
		tasks.forEach((value) => {
			if (value.id == e.target.dataset.id) {
				value.state = !value.state;
			}
		});
		init();
		save();
	}

	// 单项删除
	if (e.target.className == 'del') {
		tasks = tasks.filter((value) => value.id != e.target.dataset.id);
		init();
		save();
	}
});

// 清空已完成事项
clear.addEventListener('click', function () {
	tasks = tasks.filter((value) => !value.state);
	init();
	save();
});

function save() {
	localStorage.setItem('tasksData', JSON.stringify(tasks));
}
```

# 打地鼠
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E6%89%93%E5%9C%B0%E9%BC%A0.gif)
```html
		<h1>Whack-a-mole! <span class="score">0</span></h1>
		<p>
			<button>Start</button><span class="totalTime" style="font-size: 50px"></span>
		</p>

		<div class="game">
			<div class="hole hole1">
				<div class="mole"></div>
			</div>
			<div class="hole hole2">
				<div class="mole"></div>
			</div>
			<div class="hole hole3">
				<div class="mole"></div>
			</div>
			<div class="hole hole4">
				<div class="mole"></div>
			</div>
			<div class="hole hole5">
				<div class="mole"></div>
			</div>
			<div class="hole hole6">
				<div class="mole"></div>
			</div>
		</div>

		<h2 class="game-status"></h2>
```
```css
html {
	box-sizing: border-box;
	font-size: 10px;
	background: #ffc600;
}
*,
*:before,
*:after {
	box-sizing: inherit;
}
body {
	padding: 0;
	margin: 0;
	font-family: 'Amatic SC', cursive;
}
h1,
h2 {
	text-align: center;
	font-size: 10rem;
	line-height: 1;
	margin-bottom: 0;
}
h2 {
	font-size: 8rem;
}
.score {
	background: rgba(255, 255, 255, 0.2);
	padding: 0 3rem;
	line-height: 1;
	border-radius: 1rem;
}
p {
	height: 5rem;
	text-align: center;
}
button {
	margin: 0 auto;
	width: 10rem;
	font-size: 3rem;
	font-family: 'Amatic SC', cursive;
	-webkit-appearance: none;
	appearance: none;
	font-weight: bold;
	background: rgba(255, 255, 255, 0.2);
	border: 1px solid black;
	border-radius: 1rem;
}
.game {
	width: 600px;
	height: 400px;
	display: flex;
	flex-wrap: wrap;
	margin: 0 auto;
}
.hole {
	flex: 1 0 33.33%;
	overflow: hidden;
	position: relative;
}
.hole:after {
	display: block;
	background: url(dirt.svg) bottom center no-repeat;
	background-size: contain;
	content: '';
	width: 100%;
	height: 70px;
	position: absolute;
	z-index: 2;
	bottom: -30px;
}
.mole {
	background: url('mole.svg') bottom center no-repeat;
	background-size: 60%;
	position: absolute;
	top: 70%;
	width: 100%;
	height: 100%;
	transition: all 0.4s;
}
.hole.up .mole {
	top: 0;
}
```
```javascript
const start = document.querySelector('button');
const moles = document.querySelectorAll('.mole');
const totalTime = document.querySelector('.totalTime');
const scoreEle = document.querySelector('.score');

start.addEventListener('click', function () {
	this.style.display = 'none';
	let total = 5;
	let score = 0;
	let index = 0;
	let flag = true;
	scoreEle.innerHTML = score;
	let timerId = setInterval(() => {
		flag = true;

		total--;
		totalTime.innerHTML = total;

		index = parseInt(Math.random() * moles.length);
		moles[index].style.top = '0%';
		
		setTimeout(() => {
			moles[index].style.top = '70%';
		}, 800);
	}, 1000);

	moles.forEach((ele) => {
		ele.addEventListener('click', function () {
			if (flag) {
				flag = false;
				moles[index].style.top = '70%';
				score++;
				scoreEle.innerHTML = score;
			}
		});
	});

	// 游戏结束
	setTimeout(() => {
		clearInterval(timerId);
		this.style.display = 'block';
		totalTime.innerHTML = '';
	}, 5000);
});
```

# 点名系统
![](https://csheng-fly.oss-cn-guangzhou.aliyuncs.com/%E7%82%B9%E5%90%8D%E7%B3%BB%E7%BB%9F.gif)

```html
		<h1>点名系统</h1>
		<h2 id="selectTitle">被选中的小伙伴：<span></span></h2>
		<button>开始</button>
		<select>
			<option value="1000">1秒</option>
			<option value="2000">2秒</option>
			<option value="3000">3秒</option>
		</select>
		<div id="content">
			<!-- <div class="cell">陈升</div> -->
		</div>
```
```css
* {
	font-family: '微软雅黑';
	/*transition-duration: ;*/
}
h1,
h2 {
	animation: changeColor 2s linear infinite;
	animation-direction: alternate;
}
h1 {
	background-color: yellowgreen;
	color: white;
	text-align: center;
}
#content > div {
	float: left;
	width: 100px;
	height: 50px;
	margin: 5px;
	font-size: 20px;
	line-height: 50px;
	text-align: center;
}
.cell {
	background-color: black;
	color: white;
}
.cell.active {
	background-color: skyblue;
	color: crimson;
}
.selected {
	background-color: skyblue;
	color: crimson;
}
.current,
.a {
	background-color: greenyellow;
	color: blueviolet;
}
button {
	display: inline-block;
	height: 50px;
	/*width: 50px;*/
	background-color: yellowgreen;
	color: white;
	font-size: 16px;
	margin: 10px;
}
select {
	display: inline-block;
	height: 30px;
	width: 100px;
	border: 1px solid yellowgreen;
	background-color: blanchedalmond;
	color: black;
	font-size: 14px;
	margin: 10px;
}
@keyframes changeColor {
	from {
		color: pink;
	}
	to {
		color: blueviolet;
	}
}
```
```javascript
const content = document.querySelector('#content');
const start = document.querySelector('button');
const select = document.querySelector('select');
const selectTitle = document.querySelector('#selectTitle>span');

let indexArr = []; // 索引值集合
let num = 0; // 随机索引
let timerId, setTime;

// 渲染数据
let students = ['陈升', '曾小梅', '周国栋', '高剑锋', '冯浩南', '黄浦荣', '李奇', '文清波', '范君鹏', '谭晓燕', '熊春萌', '刘剑锋', '黎明', '郭富城'];
let htmlStr = '';
students.forEach((item, index) => {
	htmlStr += `<div class="cell">${item}</div>`;
	indexArr.push(index);
});
content.innerHTML = htmlStr;

let cells = document.querySelectorAll('.cell');
let flag = true;
start.addEventListener('click', function () {
	if (flag) {
		flag = false;
		timerId = setInterval(() => {
			num = parseInt(Math.random() * indexArr.length);
			// 闪烁效果
			cells[indexArr[num]].classList.add('current');
			setTime = setTimeout(() => {
				cells[indexArr[num]].classList.remove('current');
			}, 100);
		}, 200);

		// 停止点名
		setTimeout(() => {
			cells[indexArr[num]].classList.add('current');
			selectTitle.innerHTML += cells[indexArr[num]].innerHTML + ' ';
			indexArr.splice(num, 1);

			clearInterval(timerId);
			clearTimeout(setTime);

			flag = true;
		}, select.value);
	}
});
```