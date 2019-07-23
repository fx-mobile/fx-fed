# fx-fed
## web 前端 - 开发规范

为提高团队协作效率， 便于后台人员添加功能及前端后期优化维护，输出高质量的文档，特制订此文档。 本规范文档一经确认， 前端开发人员须按本文档规范进行开发。 本文档如有不对或者不合适的地方请及时提出，经讨论决定后可以更改此文档。

---

#### 1、基本原则
符合 web 标准 ( UTF-8  HTML5 ) ，语义化 html ( HTML5 新增要求，减少 div 和 span 等无特定语义的标签使用 ) ，结构表现行为分离 ( HTML-CSS-JS代码分离，不同行为代码高内聚低耦合 ) ，兼容性优良 ( 早期版本浏览器兼容，移动端和PC端设备兼容 ) 。页面性能方面 ( **减少请求次数**，例如使用精灵图和sass语法 ) ，代码要求简洁明了有序，**尽可能的减小服务器负载，保证最快的解析速度 ( 减小 repaint 和 reflow )** 。**禁止直接操作DOM**。
#### 2、文件命名、结构规范
- 文件名不能含有空格
- 文件名建议只使用小写字母，不使用大写字母。( 但比如说像 github 的说明类文件，README，则应该全部使用大写 )
- 文件名包含多个单词时，单词之间建议使用半角的连词线 ( - ) 分隔。
#### 3、iconfont--使用字体图标减少图片资源消耗
#### 4、JS 书写规范 - 遵照ES6标准
- **命名规范**
let 定义的变量不会被变量提升，const 定义的常量不能被修改，let 和 const 都是块级作用域
尽量不实用 var
const 定义常量全部大写并单词间用下划线分隔
小驼峰式定义普通变量比如对象的属性或方法名、变量名、函数名
小驼峰式但首字母大写定义类名 ( 构造器 )
小驼峰式但需要用_开头定义私有变量
```
// const 定义常量
export const VOUCHER_STATUS_NORMAL = 1 // 正常
export const VOUCHER_STATUS_ADD = 2 // 新增
export const VOUCHER_STATUS_EDIT = 3 // 编辑 (查看状态下进行了修改)
// 对象的属性或方法名、变量名、函数名
pageChanged = (pageIndex, pageSize) => {
    let page = this.metaAction.gf('data.pagination').toJS(),
        filter = this.metaAction.gf('data.filter').toJS()

    if(pageIndex){
        page.pageIndex = pageIndex
    }
    if(pageSize){
        page.pageSize = pageSize
    }
    this.load( page, filter)
}
// 定义类名 ( 构造器 )
class DragTitle extends Component {
	updateTransform = transformStr => {
		this.modalDom.style.transform = transformStr;
	};
	componentDidMount() { ...
...
// 定义私有变量
let _current, _defaultConfig
```
- **import导入模块、export导出模块**
```
// 全部导入
import people from './example'
 
// 将整个模块当作单一对象进行导入，该模块的所有导出都会作为对象的属性存在
import * as example from "./example.js"
console.log(example.name)
console.log(example.getName())
 
// 导入部分，引入非 default 时，使用花括号
import {name, age} from './example'
 
// 导出默认, 有且只有一个默认
export default App
 
// 部分导出
export class App extend Component {};
```
- **class、extends、super**
ES5中最令人头疼的的几个部分：原型、构造函数，继承，ES6 引入了 Class ( 类 ) 这个概念。
ES6 允许我们用 class 定义一个“类”。
Class 之间可以通过 extends 关键字实现继承。
super 关键字，它指代父类的实例 ( 即父类的  this 对象 )。子类必须在 constructor 方法中调用 super 方法，否则就得不到 this 对象。
更多 ES6 类声明继承机制详见 [ES6 类](https://www.jianshu.com/p/76df2750e348) 共同学习共同进步
```
class Animal {
	constructor() {
		this.type = 'animal';
	}
	says(say) {
		console.log(this.type + ' says ' + say);
	}
}

let animal = new Animal();
animal.says('hello'); //animal says hello

class Cat extends Animal {
	constructor() {
		super();
		this.type = 'cat';
	}
}

let cat = new Cat();
cat.says('hello'); //cat says hello
```
- **arrow functions ( 箭头函数 )**
函数的快捷写法。不需要 function 关键字来创建函数，省略 return 关键字，继承当前上下文的 this 关键字
箭头函数小细节：当你的函数有且仅有一个参数的时候，是可以省略掉括号的；当你函数中有且仅有一个表达式的时候可以省略{}
JavaScript 语言的 this 对象一直是一个令人头痛的问题，运行上面的代码会报错，这是因为 setTimeout 中的 this 指向的是全局对象。
当我们使用箭头函数时，函数体内的 this 对象，就是定义时所在的对象
更多 ES6 箭头函数 this 巧用详见 [ES6写法优化 箭头函数](https://www.jianshu.com/p/65e3cbde736c) 共同学习共同进步
```
// ES5
var arr1 = [1, 2, 3];
var newArr1 = arr1.map(function(x) {
	return x + 1;
});

// ES6
let arr2 = [1, 2, 3];
let newArr2 = arr2.map((x) => {
	x + 1
});

let arr2 = [1, 2, 3];
let newArr2 = arr2.map(x => x + 1);
```
- **template string ( 模板字符串 )**
字符串拼接。将表达式嵌入字符串中进行拼接，用 ` 和${}`来界定。
在ES5时我们通过反斜杠来做多行字符串拼接。ES6反引号 `` 直接搞定。
更多 ES6 标签模版字符串用法详见 [ES6写法优化 字符串模板](https://www.jianshu.com/p/b3143e7ee73c) 共同学习共同进步
```
// es5
var name1 = "bai";
console.log('hello' + name1);
 
// es6
const name2 = "ming";
console.log(`hello${name2}`)

// es5
var msg = "Hi \
man!";
 
// es6
const template = `<div>
<span>hello world</span>
</div>`;

// includes：判断是否包含然后直接返回布尔值
let str = 'hahah';
console.log(str.includes('y')); // false
 
// repeat: 获取字符串重复n次
let s = 'he';
console.log(s.repeat(3)); // 'hehehe'
```
- **destructuring ( 解构 )**
解构能让我们从对象或者数组里取出数据存为变量以数组为例
更多 ES6 解构用法详见 [ES6 对象、数组解构](https://www.jianshu.com/p/76a87b97cc32) 共同学习共同进步
```
const numbers = [ 'one', 'two', 'three', 'four' ];

const [one, , three, , five = 'have no five'] = numbers; // 相对应位置的值 默认值写法同对象一样用=
console.log(one, three, five); // one three have no five
```
- **Promise 用同步的方式去写异步代码**
更多 ES6 Promise 用法详见 [ES6 Promise](https://www.jianshu.com/p/630869019e80) 共同学习共同进步
```
// 发起异步请求
fetch('/api/todos')
	.then(res => res.json())
	.then(data => ({
		data
	}))
	.catch(err => ({
		err
	}));
```
- **ES6 Generator 生成器控制 ajax 工作流**
用 Generator 生成器来达到 Promise 的效果，控制 ajax 工作流，代码有更好的可读性，从上到下，一气呵成。
```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>

    function ajax(url) { // 创建 ajax 请求流程
        axios.get(url).then(res => userGen.next(res.data));
    }
    
    function* steps() { // 创建生成器函数
        console.log('ajax start users')
        const users = yield ajax('https://api.github.com/users');
        console.log(users, 'ajax start firstUser')
        const firstUser = yield ajax(`https://api.github.com/users/${users[0].login}`);
        console.log(firstUser, 'ajax start followers')
        const followers = yield ajax(firstUser.followers_url);
        console.log(followers, 'ajax end')
    }

    const userGen = steps(); 

    userGen.next(); // 流程开启

</script>
```
- **使用严格的条件判断符**
用 === 代替 == ，用 !== 代替 != ，避免在条件判断时掉入 == 造成的陷阱
```
// 这样的一些值表示 false 。
null
undefined 与 null 相等
字符串 ''
数字 0
NaN

(function () {
    var undefined;
    undefined == null; // true
    1 == true; //true
    2 == true; // false
    0 == false; // true
    0 == ''; // true
    NaN == NaN;// false
    [] == false; // true
    [] == ![]; // true
})();
```
#### 5、关于VUE的开发规范
- **数据管理优先使用 Vuex ，通用组件选择性使用 props 方式**
- **store 的定义可以以组件的维度做好分解，在入口处做好聚合**
- **api 接口模块管理--统一管理 api 请求**
- **vue 生命钩子**

![vue 生命钩子](https://upload-images.jianshu.io/upload_images/17067702-1e70917025a325d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **vue文件基本结构**
```
  <template>
          <div>
            <!--必须在div中编写页面-->
          </div>
        </template>
        <script>
          export default {
            components : {},//5.注册需要用到的组件
            props:{},//7.传递到本组件的props
            data () {//8.组件的 data 必须是一个函数
              return {
              }
            },
            computed:{},//8
            watch:{},//9
            //所有的钩子函数：如created
            created(){},//9
            methods: {//10
            },
         }
        </script>
        <!--声明语言，并且添加scoped-->
        <style lang="scss" scoped>
        </style>...
...
```
- **vue文件方法声明顺序**
```
1.副作用 (触发组件外的影响)
--el
2.全局感知 (要求组件以外的知识)
--name
--parent
3.组件类型 (更改组件的类型)
--functional
4.模板修改器 (改变模板的编译方式)
--delimiters
--comments
5.模板依赖 (模板内使用的资源)
--components
--directives
--filters
6.组合 (向选项里合并属性)
--extends
--mixins
7.接口 (组件的接口)
--inheritAttrs
--model
--props/propsData
8.本地状态 (本地的响应式属性)
--data
--computed
9.事件 (通过响应式事件触发的回调)
--watch
生命钩子函数
---beforeCreate
---created
---beforeMount
---mounted
---beforeUpdate
---updated
---activated
---deactivated
---beforeDestroy
---destroyed
10.非响应式的属性 (不依赖响应系统的实例属性)
--methods
11.渲染 (组件输出的声明式描述)
--template/render
--renderError
```
- **元素特性的顺序**
```
- v-for
- v-if / v-show
- id
- ref / key / slot
- v-model
- v-on//强烈建议用简写 @，如@change
```
- **单文件组件的顶级元素的顺序**
单文件组件应该总是让<script>、<template> 和 <style> 标签的顺序保持一致。且 <style> 要放在最后，因为另外两个标签至少要有一个。
```
<!-- ComponentA.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```
- **多个特性的元素**
多个特性的元素应该分多行撰写，每个特性一行。
```
<img
	src="https://vuejs.org/images/logo.png"
	alt="Vue Logo"
>
<MyComponent
	foo="a"
	bar="b"
	baz="c"
/>
```
- **模板中简单的表达式**
组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。
复杂表达式会让你的模板变得不那么声明式。我们应该尽量描述应该出现的是什么，而非如何计算那个值。而且计算属性和方法使得代码可以重用。
```
<!-- 在模板中 -->
{{ normalizedFullName }}
// 复杂表达式已经移入一个计算属性
computed: {
  normalizedFullName: function () {
    return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
  }
}
```
- **简单的计算属性**
```
computed: {
	basePrice: function() {
		return this.manufactureCost / (1 - this.profitMargin)
	},
	discount: function() {
		return this.basePrice * (this.discountPercent || 0)
	},
	finalPrice: function() {
		return this.basePrice - this.discount
	}
}
```
- **谨慎使用 ( 有潜在危险的模式 )**
```
// 如果一组 v-if + v-else 的元素类型相同，最好使用 key (比如两个 <div> 元素)。
<div
  v-if="error"
  key="search-status"
>
  错误：{{ error }}
</div>
<div
  v-else
  key="search-results"
>
  {{ results }}
</div>

// 元素选择器应该避免在 scoped 中出现。在 scoped 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。
<template>
	<button class="btn btn-close">X</button>
</template>

<style scoped>
	.btn-close {
		background-color: red;
	}
</style>

// 隐性的父子组件通信 应该优先通过 prop 和事件进行父子组件之间的通信，而不是 this.$parent 或改变 prop。
Vue.component('TodoItem', {
	props: {
		todo: {
			type: Object,
			required: true
		}
	},
	template: `
    <input
      :value="todo.text"
      @input="$emit('input', $event.target.value)"
    >
  `
})

// 非 Flux 的全局状态管理 应该优先通过 Vuex 管理全局状态，而不是通过 this.$root 或一个全局事件总线。
// store/modules/todos.js
export default {
	state: {
		list: []
	},
	mutations: {
		REMOVE_TODO (state, todoId) {
		    state.list = state.list.filter(todo => todo.id !== todoId)
		}
	},
	actions: {
		removeTodo ({ commit, state }, todo) {
			commit('REMOVE_TODO', todo.id)
		}
	}
}
<!-- TodoItem.vue -->
<template>
	<span>
		{{ todo.text }}
		<button @click="removeTodo(todo)">
			X
		</button>
	</span>
</template>

<script>
import { mapActions } from 'vuex'

export default {
	props: {
		todo: {
			type: Object,
			required: true
		}
	},
	methods: mapActions(['removeTodo'])
}
</script>
```
#### 6、CSS 书写规范
- webApp 自适应布局 -- 使用 rem 。
使用 rem 单位可以让我们的设计更加灵活，能够控制元素整体放大缩小，而不是固定大小。 我们可以使用这种灵活性，使我们在开发期间，能更加快速灵活的调整，允许浏览器用户调整浏览器大小来达到最佳体验。
- class 与 id 的使用：id 是唯一的并是父级的，class 是可以重复的并是子级的，所以 id 仅使用在大的模块上，class 可用在重复使用率高及子级中。
- class 命名，体现嵌套统一使用 - 来连接，参考 [TTK 框架](https://github.com/thethreekingdoms) 企业开发平台，每一个app的最外层与文件夹保持一致，header、content、footer 依次排列层级分明
避免使用通配规则和相邻兄弟选择符、子选择符,、后代选择符、属性选择符等选择器
不要限定 id 选择符，如 div#header（提权的除外）
不要限定类选择器，如 ul.recommend（提权的除外）
不要使用 ul li a 这样长的选择符
避免使用标签子选择符，如 #header > li > a 
```
// dom 结构对应 css className
name: 'root',
component: 'Layout',
className: 'open-portal-config-project-list', // app 名称
children: [{
	name: 'header',
	component: 'Layout',
	className: 'open-portal-config-project-list-header',
...

name: 'content',
component: 'Layout',
className: 'open-portal-config-project-list-content',
children: [{
...

name: 'footer',
component: 'Layout',
_visible: false,
className: 'open-portal-config-project-list-footer',
...
// 对应 less 文件结构
.open-portal-config-project-list{
    align-items: center;
    margin: 0 auto;
    width: 100%;
    height: 100%;
    background-color: #fff;
	&-header{ // 尽量减少样式 class 嵌套 方便解析查找以及日后修改
        width: 100%;		
        flex: initial;
		flex-direction: row;
		justify-content: space-between;
		line-height: 40px;
		padding: 10px;

		.mk-input {
			width: 200px;
			margin-right: 10px;
		}
    }

    &-content {
    	flex: 1;
    	width: 100%;
    }

    &-footer {
		flex: initial;
		width: 100%;
		padding: 10px;

		.mk-pagination {
			justify-content: flex-end;
		}
    }
}
```
- 书写代码前，提高样式重复使用率。充分利用 html 自身属性及样式继承原理减少代码量。
- 样式表中中文字体名，请务必转码成 unicode 码，以避免编码错误时乱码。
- 减少使用影响性能的属性，比如 position：absolute || float。
- 兼容多个浏览器时，将标准属性写在底部。
```
-moz-border-radius: 15px; /* Firefox */
-webkit-border-radius: 15px; /* Safari和Chrome */
border-radius: 15px; /* Opera 10.5+, 以及使用了IE-CSS3的IE浏览器 */ //标准属性
```
- 尽量避免使用CSS Hack。
```
property:value; /* 所有浏览器 */
+property:value; /* IE7 */
_property:value; /* IE6 */
*property:value; /* IE6/7 */
property:value\9; /* IE6/7/8/9，即所有IE浏览器 */
```
#### 7、注释规范
- 单行注释
单独一行：// ( 双斜线 ) 与注释文字之间保留一个空格
在代码后面添加注释：// ( 双斜线 ) 与代码之间保留一个空格，并且 // ( 双斜线 ) 与注释文字之间保留一个空格。
注释代码：// ( 双斜线 ) 与代码之间保留一个空格。
```
// 调用了一个函数；1)单独在一行
setTitle();
var maxCount = 10; // 设置最大量；2)在代码后面注释
// setName(); // 3)注释代码
```
- 多行注释
若开始和结束都在一行，推荐采用单行注释
若至少三行注释时，第一行为 / ，最后行为 / ，其他行以开始，并且注释文字与 * 保留一个空格。
```
/*
 * 代码执行到这里后会调用setTitle()函数
 * setTitle()：设置title的值
 */
setTitle();
```
- 函数 ( 方法 ) 注释
```
/** 
 * 函数说明 
 * @关键字 
 */
```
#### 8、HTML 书写规范
- 文档类型声明及编码：统一为 html5 声明类型。书写时利用 IDE 实现层次分明的缩进 ( 默认缩进4空格 ) 。
- 非特殊情况下 CSS 文件放在 body 部分 <meta> 标签后。非特殊情况下大部分 JS 文件放在 <body> 标签尾部 ( 如果需要界面未加载前执行的代码可以放在 head 标签后 ) 避免行内 JS 和 CSS 代码。
```
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="css/main.css">
</head>
<body>
    <!-- 逻辑代码 -->
    <!-- 逻辑代码底部 -->
    <script src="lib/jquery/jquery-2.1.1.min.js"></script>
</body>
</html>
```
- 所有编码需要遵循 html ( XML ) 标准，标签 & 属性 & 属性命名必须由小写字母及下划线数字组成，且所有标签必须闭合，包括 br()，hr() 等。属性值用双引号。应当按照以下给出的顺序依次排列，来确保代码的易读性。
```
class
id 、 name
data-*
src、for、 type、 href
title、alt
aria-*、 role
```
- 引入 JS 库文件，文件名须包含库名称及版本号及是否为压缩版，比如 jquery-1.4.1.min.js。引入插件，文件名格式为库名称 + 插件名称，比如 jQuery.bootstrap.js。
```
jQuery-1.8.3.min.js
```
- 书写页面过程中，请考虑向后扩展性。class & id 参见 css 书写规范。
- 语义化 html，如标题根据重要性用 h* ( 同一页面只能有一个 h1 ) ，段落标记用 p ，列表用 ul ，内联元素中不可嵌套块级元素。
- 尽可能减少 div 多层级嵌套。
- 书写链接地址时，必须避免重定向，例如：href="[https://github.com/thethreekingdoms/](https://github.com/thethreekingdoms)
"，即须在 URL 地址后面加上 “ / ” 。
- 在页面中尽量避免使用 style 属性，即 style = " … " 。
- 能以背景形式呈现的图片，尽量写入 css 样式中。
- 重要图片必须加上 alt 属性。给重要的元素和截断的元素加上 title 。
- 给区块代码及重要功能 ( 比如循环 ) 加上注释，方便后台添加功能。
- 特殊符号使用：尽可能使用代码替代：比如 <(<)&>(>)& 空格 ()&»(») 等等。
- 语义化 html ( HTML5 新增要求，减少 div 和 span 等无特定语义的标签使用 ) 
