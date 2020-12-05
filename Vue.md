# Vue基础

## 概述

渐进式Javascript框架：声明式渲染 ---- 组件系统 ---- 客户端路由 ---- 集中式状态管理 ---- 项目创建

## Vue模板语法

### 插值表达式

```
<div id="app">{{msg}}</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
	var vm = new Vue({
		// 元素的挂载位置
		el: '#app',
		// 模型数据，值是一个对象
		data: {
			msg: 'helloworld'
		}
	})
</script>
```

### 指令

指令的本质就是自定义属性 以v-开始（比如：v-cloak）

**`v-cloak`指令**

```
v-cloak指令:先隐藏，替换好值后显示最终的结果
// 1.提供样式
[v-cloak] {
	display: none;
}
// 2.在差值表达式所在的标签添加指令
<div id="app" v-cloak>{{msg}}</div>
```

**数据绑定指令**

```
v-text
// 填充纯文本
<span v-text="msg"></span>

v-html
// 填充HTML片段 存在安全问题 本网站内部数据可以使用，来自第三方的数据不可以使用
<span v-html="rawHtml"></span>

v-pre 
// 显示原始信息，跳过编译过程
<span v-pre>{{ this will not be compiled }}</span>
```

**数据响应式**

```
v-once
// 只渲染元素和组件一次，显示内容后不在具有响应式功能 可以用于优化更新性能
<span v-once>This will never change: {{msg}}</span>
```

**双向数据绑定**

```
// 在表单控件或者组件上创建双向绑定
//v-model
<input type="text" v-model="msg">
// v-model的实现原理
<input type="text" :value="msg" @input="msg=$event.target.value">

MVVM设计思想
①M(model)
②V(view)
③VM(View-Model)
```

### 事件绑定

```
// 事件绑定
<div v-on:click="handle">{{num}}</div>
<div @click="num++">{{num}}</div>
<script>
	var vm = new Vue({
		// 元素的挂载位置
		el: '#app',
		// 模型数据，值是一个对象
		data: {
			msg: 'helloworld'
		},
		methods: {
        	handle: function() {
        	// this指的是Vue的实例对象，即vm
        	this.msg = this.msg + 1;
        	}
        }
	})
</script>

普通参数和事件对象
// 默认传递事件对象作为函数第一个参数
<div v-on:click="handle">{{num}}</div>
// 事件对象必须作为最后一个参数显式传递，并且事件对象的名称必须是$event
<div v-on:click="handle("hi",$event)">{{num}}</div> 

// 事件修饰符
// 阻止冒泡
<a v-on:click.stop="handle">跳转</a>
// 阻止默认行为
<a v-on:click.prevent="handle">跳转</a>
// 事件修饰符可以串联，先后顺序有区别
<a v-on:click.stop.prevent="doThat"></a>

按键修饰符
// 回车键
<input v-on:keyup.enter="submit">
// 删除和退格键
<input v-on:keyup.delete="handle">
// 自定义按键修饰符
// 通过全局config.keyCodes对象自定义按键修饰符别名
Vue.config.keyCodes.f1 = 65; // f1是自定义名字，65是按键对应的keyCode值
```

### 属性绑定

```
// 绑定一个 attribute
<img v-bind:src="imageSrc">
// 缩写 
<img :src="imageSrc">
```

### 样式绑定

**`class`样式处理**

```
// 对象语法
// 键值对的键为类名，值为布尔值控制类的存在
<div :class="{active: isActive,selected: isSelected}"></div>
<div :class="classObj"></div>
....
data: {
	classObj: {
		active: true,
		selected: true
	}
}

// 数组语法
<div :class="[classA, classB]"></div>
<div :class="classArr"></div>
....
data: {
	classA: 'A',
	classB: 'B',
	classArr: ['A','B']
}

// 对象绑定和数组绑定可以结合使用
// 默认(原有)的class会保留
```

**`style`样式处理**

```
// // 对象语法
<div :style="{ fontSize: size + 'px' }"></div>

// 数组语法
// 后面的对象样式若出现重复会产生覆盖
<div :style="[styleObjectA, styleObjectB]"></div>
```

### 分支循环结构

**分支结构**

- `v-if`
- `v-else`
- `v-else-if`
- `v-show`

```
// v-else-if和v-else前一兄弟元素必须有v-if或v-else-if
// v-else不需要表达式
<p v-if="score >= 90" v-cloak>优秀</p>
<p v-if="score < 90 && score >= 80" v-cloak>还行</p>
<p v-else v-cloak>垃圾</p>

// v-show
// 控制元素的显示隐藏  display
<h1 v-show="ok">Hello!</h1>

// v-show和v-if的区别
1. v-if控制元素是否渲染到页面
2. v-show控制元素是否显示(已经渲染到了页面)
// 如果需要非常频繁地切换，则使用v-show较好；如果在运行时条件很少改变，则使用v-if较好。
```

**循环结构**

```
// v-for
<ul id="example-1">
  <li v-for="item in items" :key="item.message">
    {{ item.message }}
  </li>
  // 遍历数组时有可选的第二个参数，即当前项的索引
  // <li v-for="(item,index) in items" :key="item.message"></li>
  // 遍历对象时有可选的第二个的参数为属性名称;有可选的第三个参数作为索引
  // <li v-for="(value, name, index) in object"></li>
</ul>

// key的作用：帮助Vue区分不同的元素，从而提高性能
<li v-for="(item,index) in items" :key="index"></li>

// v-if和v-for结合使用
// 当它们处于同一节点，v-for的优先级比v-if更高，这意味着v-if将分别重复运行于每个v-for循环中
<li v-if="item > 10" v-for="item in items" :key="item.message">
```

## Vue常用特性

用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。

`v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 `value` property 和 `input` 事件
- checkbox 和 radio 使用 `checked` property 和 `change` 事件
- select 字段将 `value` 作为 prop 并将 `change` 作为事件

```
<input v-model="message" placeholder="edit me">
<textarea v-model="message" placeholder="add multiple lines"></textarea>

<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">

<input type="radio" id="one" value="One" v-model="picked">
<input type="radio" id="two" value="Two" v-model="picked">

<select v-model="selected" [muitiple]> // 单选或者多选
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
</select>
```

**修饰符**：

- `.lazy` - 将`input` 事件切换为`change` 事件
- `.number` - 输入字符串转为有效的数字
- `.trim` - 输入首尾空格过滤

### 自定义指令

```
// 注册一个全局自定义指令 `v-focus`
Vue.directive('color', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function(el,binding) {
    el.style.backgroundColor = binding.value.color;
  }
})

<input type="text" v-color="{color: 'blue'}">
```

`el`：指令所绑定的元素，可以用来直接操作 DOM

`binding`：一个对象，包含以下 property：

- `name`：指令名，不包括 `v-` 前缀。
- `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
- `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
- `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
- `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
- `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。

**局部指令**

```
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

### 计算属性

```
// 对于任何复杂逻辑，应当使用计算属性
<p>Original message: {{ message }}</p>
<p>Computed message: {{ reversedMessage }}</p>
....
computed: {
    reversedMessage: function () {
    	// 必须要有return
		return this.message.split('').reverse().join('')
    }
}

// 计算属性与方法的区别
计算属性是基于它们的响应式依赖进行缓存的，只在相关响应式依赖发生改变时它们才会重新求值
方法不存在缓存
```

### 侦听器

```
// 当需要在数据变化时执行异步或开销较大的操作时使用侦听器
watch: {
	// 属性名与数据名字一致；val表示数据值
    firstName: function (newV,oldV) {
      this.fullName = newV + ' ' + this.lastName
    },
    lastName: function (newV,oldV) {
      this.fullName = this.firstName + ' ' + newV
    }
}

// 计算属性在大多数情况下更合适
computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
}
```

### 过滤器

```
// 作用：格式化数据
// 全局过滤器 将首字母大写
Vue.filter('capitalize', function (value) {
  return value.charAt(0).toUpperCase() + value.slice(1);
})

// 局部过滤器
filters: {
  capitalize: function (value) {
    return value.charAt(0).toUpperCase() + value.slice(1);
  }
}

// 使用方式
<!-- 在双花括号中 -->
{{ message | capitalize }}
<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
<!-- 过滤器可以串联 -->
{{ message | filterA | filterB }}
<!-- 接收参数 -->
{{ message | filterA('arg1', arg2) }}
```

### 生命周期

- 挂载（初始化相关属性）
  - beforeCreate
  - created
  - beforeMount
  - monuted
- 更新（元素或组件的变更操作）
  - beforeUpdate
  - updated
- 销毁（销毁相关属性）
  - beforeDestroy
  - destroyed

```
// mounted钩子函数
代表模板可以使用，一般用于获取后台数据

// 完全销毁一个实例 触发beforeDestroy和destroyed的钩子
vm.$destroy()
```

### refs

可以获取DOM节点

```
<input type="text" ref="input" />
通过this.$refs.input获取该元素
```

### nextTick

更新DOM是异步进行的  通过nextTick方法在修改数据后执行操作

```
Vue.nextTick(() => {
	// DOM更新了
})
```

### mixin

分发vue组件中的可复用功能

```
const myMixin = {
	data() {
		msg: 'zs'
	},
	methods: {
		show() {
			// 混入方法
		}
	}
}
new Vue({
	el: '#app',
	// 将myMixin混入到实例或组件中，重复以组件为准
	mixins: [myMixin]
})

通过Vue.mixin({})可以创建全局的混入
```



## 数组更新检测

**变更方法**：会更改原来的数组

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

**替换数组**：产生新数组

- `filter()`
- `concat()`
- `slice()`

```
// 修改响应式数据
Vue.set( target, propertyName/index, value )
vm.$set( target, propertyName/index, value )

{Object | Array} target
{string | number} propertyName/index
{any} value
```

## 组件化开发

### 组件化开发思想

- 标准
- 分治
- 重用
- 组合

### 组件注册

```
// 全局注册
Vue.component(组件名称,{
	data: 组件数据,
	template: 组件模板内容
})

// 定义一个名为button-counter的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
<div id="components-demo">
  <button-counter></button-counter>
</div>

// 注意事项
1. data必须是一个函数
2. 组件模板内容必须是单个根元素
3. 组件模板内容可以是模板字符串
4. 组件命名方式
	短横线方式：my-component
	驼峰方式：MyComponent   驼峰式组件只能在字符串模板中使用，在普通标签模板中需要转换为短横线方式
	
// 局部注册
// 局部组件只能在其注册的父组件中使用
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

### 组件间数据交互

**父组件向子组件传递数据**

```
// 子组件内部通过props属性接收数据
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
// 父组件通过属性将数据传递给子组件
<blog-post title="来自父组件的数据"></blog-post>
// 使用v-bind来动态传递 prop
<blog-post :title="msg"></blog-post>

// props属性名规则
1. 在props中使用驼峰方式，在DOM模板中转换为短横线方式
2. 字符串形式的模板没有该限制
```

**子组件向父组件传递数据**

```
// 子组件通过自定义事件向父组件传递信息
// $emit()方法第一个参数传入事件名称，第二个参数传递数据
<button @click="$emit('my-event'[,args])"></button>
// 父组件监听子组件的事件
// 通过$event访问到子组件抛出的值
<my-component v-on:my-event="doSomething += $event"></my-component>
```

**兄弟组件间传值**

```
// 事件中心管理组件间通信
var vm = new Vue();
// 监听事件与销毁事件
vm.$on(event,callback);
vm.$off([event,callback]);
// 触发事件
vm.$emit(eventName[,args]);
```

### 组件插槽

```
// 插槽定义
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <!-- 带有隐含的名字“default” -->
      <slot></slot>
      <!-- 具名插槽 -->
      <slot name="header"></slot>
    </div>
  `;
})

// 插槽内容
// 在一个<template>元素上使用v-slot指令，并以v-slot的参数的形式提供其名称
// v-slot只能添加在<template>上
// v-slot的缩写: #，该缩写只在其有参数的时候才可用   v-slot:header可以被重写为#header
<alert-box>
	<template v-slot:header>
		<h1>Here might be a page title</h1>
	</template>
	<!-- 任何没有被包裹在带有v-slot的<template>中的内容都会被视为默认插槽的内容 -->
	<p>A paragraph for the main content.</p>
  	<p>And another one.</p>
</alert-box>
```

**作用域插槽**

```
// 父组件对子组件的内容进行加工处理
// 插槽定义
<slot>{{ user.lastName }}</slot>

// 插槽内容
// 可以使用带值的v-slot来定义我们提供的插槽prop的名字
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>

// 独占默认插槽写法
// 当被提供的内容只有默认插槽时，组件的标签才可以被当作插槽的模板来使用。这样我们就可以把 v-slot 直接用在组件上
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>

// 作用域插槽的内部工作原理是将你的插槽内容包括在一个传入单个参数的函数里
function (slotProps) {
  // 插槽内容
}
// 这意味着v-slot的值实际上可以是任何能够作为函数定义中的参数的JavaScript表达式
<current-user v-slot="{user}">
  {{ user.firstName }}
</current-user>
```

## 前后端交互

### Promise用法

`Promise` 出现的目的是解决异步编程中回调地狱的问题

```
var promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    if (true) {
      resolve({name: '张三'})
    } else {
      reject('失败了')
    }
  }, 2000);
});

promise
  .then(result => {
  	console.log(result);
  	return newPromise;
  })
  .then(result => {
  	console.log(result);
  	return newPromise;
  })
```

**then参数中的函数返回值**

- `Promise`对象  返回的该实例对象会调用下一个`then`
- 返回普通值   返回的普通值会传递给下一个`then`参数中函数的参数

`Promise`常用API

- 实例方法
  - `then`   获得异步任务的正确结果
  - `catch`   获取异常信息
  - `finally`   成功与否都会执行

- 对象方法
  - `Promise.all()`   并发处理多个异步任务，所有任务都执行完成才能得到结果 
  - `Promise.race()`    并发处理多个异步任务，只要有一个任务完成就能得到结果

```
Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values);
});
// [result1,result2,result3]
```

### 接口调用

#### fetch

```
fetch(url)
	// text()、json()方法的返回值是一个Promise对象
	.then(data => data.text())
	.then(ret => {
		// 获取最终的数据
		console.log(ret);
	})
```

**`fetch`请求参数**

```
fetch(url,{
	method: 'GET',		// GET POST PUT DELETE
	body: JSON.stringify(data), // must match 'Content-Type' header
	headers: {
		'content-type': 'application/json'
	}
})
```

#### axios

`axios`是一个基于Promise用于浏览器和Node.js的HTTP客户端

- 支持浏览器和node.js
- 支持Promise
- 能拦截请求和响应
- 自动转换JSON数据

```
// get,delete
axios.get('/path',{    
		params: {
			id: 123
		}
}).then(response => console.log(response.data))    // data属性用于获取实际数据	
// post,put	
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
```

**`axios`全局配置**

```
// 超时时间
axios.defaults.timeout = 3000;
// 基准地址
axios.defaults.baseURL = "http://localhost:3000"
// 请求头
axios.defaults.headers['mytoken'] = 'hello'
```

**`axios`拦截器**

```
// 请求拦截器
// 发送请求前做一些配置信息
axios.interceptors.request.use(function(config) {
    console.log(config.url);
    config.headers.mytoken = 'nihao';
    return config;
}, function(err) {
    console.log('err');
});

// 响应拦截器
// 获取数据前对数据做一些处理
axios.interceptors.response.use(function(res) {
    return res.data;
}, function(err) {
    console.log('err');
});
```

#### 异步函数

```
async function() { // 函数返回值是一个promise对象
	// await关键字只能用于异步函数
	const data = await new Promise((resolve,reject) => {}) 
}
```

## 前端路由

### 路由

- 后端路由：url地址与服务器资源之间的对应关系
- 前端路由：用户事件与事件处理函数之间的对应关系

**SPA单页面应用程序**：整个网站只有一个页面，内容的变化通过Ajax局部更新实现，同时支持浏览器的前进后退操作

### Vue Router

#### 基本使用

```
// 创建路由链接 <router-link>默认会被渲染成一个<a>标签
<router-link to="/foo">Go to Foo</router-link>
// 路由占位符 将路由匹配到的组件将渲染在这里
<router-view></router-view>
// 定义路由组件
const Foo = { template: '<h1>Foo</h1>'};
// 创建路由实例 传routes配置
const router = new VueRouter({
	// 所有的路由规则
	routes: [{
		path: '/foo',
		component: Foo
	}]
})
// 将路由挂载到vue根实例
new Vue({
	el: '#app',
	data: {},
	router
})
```

#### 路由重定向

```
routes: [{ path: '/', redirect: '/user' }]
```

#### 嵌套路由

```
const Foo = { template: `
	<h1>Foo</h1>
	<router-link to="/foo/child">Go to Foo</router-link>
	<router-view></router-view>
	`
};
const child = {template: '<h1>child</h1>'}
const router = new VueRouter({
	// 所有的路由规则
	routes: [{
		path: '/foo',
		component: Foo,
		children: [{
			path: '/foo/child',  // 或者path: 'child' 都匹配 '/foo/child'
			component: child
		}]
	}]
})
```

#### 动态路由匹配

```
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
// 路由组件通过$route.params访问路由参数
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

#### 路由组件传参

```
// 布尔模式
// route.params将会被设置为组件属性
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true }
  ]
})
```

```
// 对象模式
// 该对象会被按原样设置为组件属性
const User = {
  props: ['name','age'],
  template: '<div>User {{ name }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: {name: 'lisi',age: 20} }
  ]
})
```

```
// 函数模式
const User = {
  props: ['name','age'],
  template: '<div>User {{ name }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: route => ({name: 'lisi',age: 20,id: route.params.id}) }
  ]
})
```

#### 命名路由

```
// 在routes配置中给某个路由设置名称
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
<router-link :to="{name: user,params: {id: 4}}">user3</router-link>
router.push({ name: 'user', params: { userId: 123 }})
```

#### 编程式导航

```
// 通过js代码跳转页面
this.$router.push('hash地址');
this.$router.go(num);

router.push('/user')
router.push({ path: '/user' })
// 命名的路由
// 当path和params同时存在时，会忽略params，只匹配path里面的地址
router.push({ name: 'user', params: { userId: '123' }})
// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

#### 导航守卫

```
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
  // to 将要访问的路径
  // from 代表从哪个路径跳转而来
  // next()代表放行 next('/login')强制跳转
  if(to.path === '/login') return next();
  const token = sessionStorage.getItem('token');
  if(!token) return next('/login')
  next()
})
```

# 前端工程化

## ES6模块化规范

- `AMD`和`CMD`只适用于浏览器端
- `CommonJS`只适用于服务器端
- `ES6`模块化规范是浏览器与服务器通用的模块化规范
  - 导入成员用`import`关键字
  - 暴露成员用`export`关键字

### 基本语法

**默认导出、导入**

- `export default 成员`    如果没有默认导出，导入时会接收到一个空对象 	默认导出只能用一次
- `import 接收名称 from '模块标识符'`

**按需导出、导入**

- `export let s1 = 'aaa'`		可以多次使用按需导出
- `import {s1} from '模块标识符'`

**直接导入并执行**

```
// 不接收成员 单纯的执行某个模块的代码
import '模块标识符'
```

## webpack

提供了模块化支持、代码压缩混淆、处理js兼容问题和性能优化等功能

### 基本使用

- `npm install webpack webpack-cli -D`

- 项目根目录创建`webpack.config.js`配置文件

- 在`webpack.config.js`中初始化配置

  ```
  module.exports = {
      mode: 'development' // development,production
  };
  ```

- 在`package.json`文件的`script`节点下，新增`dev`脚本

  ```
  "scripts": {
      "dev": "webpack"
  }
  ```

- `npm run dev`进行打包

### 打包的入口与出口

- 默认入口：`./src/index.js`
- 默认出口：`./dist/main.js`

```
const path = require('path');
module.exports = {
	// 模式 development/production
    mode: 'development',
    entry: path.join(__dirname, 'src', 'index.js'),
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'bundle.js'
    }
};
```

### 自动打包

```
npm install webpack-dev-server -D

// --open 打包完成后自动打开浏览器页面
// --host 配置IP地址
// --port 配置端口
"dev": "webpack-dev-server"

<script src="/bundle.js"></script>

npm run dev

http://localhost:8080/
```

- `webpack-dev-server`会启动一个实时打包的http服务器
- `webpack-dev-server`打包生成的输出文件默认放到了项目根目录

### 生成预览页面

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
    mode: 'development',
    entry: path.join(__dirname, 'src', 'index.js'),
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    // 存放一些插件列表
    plugins: [
        new HtmlWebpackPlugin({ template: './src/index.html', filename: 'index.html' })
    ]
};
```

### 加载器

webpack只能默认打包js文件，loader加载器可以协助打包处理非js文件

```
module: {
	rules: [
		// test表示某种类型的文件  use表示要调用的loader
		// use数组中指定的loader顺序是固定的
		// 多个loader的调用顺序是从后往前
		{test: /\.css$/, use: ['style-loader', 'css-loader']},
		{test: /\.less$/, use: ['style-loader', 'css-loader','less-loader']},
		{test: /\.scss$/, use: ['style-loader', 'css-loader','sass-loader']},
		// 当图片大小小于limit字节会被转换为base64的图片
		// npm i url-loader file-loader -D
		{test: /\.jpg|png|ttf|eot|svg$/,use: 'url-loader?limit=16940'}
	]
}
```

**配置postCSS自动添加css兼容前缀**

```
npm i postcss-loader autoprefixer -D
// 根目录postcss.config.js
const autoprefixer = require('autoprefixer');
module.exports = {
	plugins: [autoprefixer]
}

// webpack.config.js
module: {
	rules: [
		// test表示某种类型的文件  use表示要调用的loader
		// use数组中指定的loader顺序是固定的
		// 多个loader的调用顺序是从后往前
		{ test: /\.css$/, use: ['style-loader', 'css-loader','postcss-loader'] }
	]
}
```

## vue单文件组件

```
<!-- my-component.vue -->
<template>
    <h1>{{msg}} world</h1>
</template>

<script>
export default {
    data: function() {
        return {
            msg: 'hello'
        }
    }
}
</script>

<style scoped>
    h1 {
        color: red;
    }
</style>
```

### 创建数据格式化规则

```
// 项目根目录下创建 .prettierrc
{
	// 不加分号
	"semi": false,
	// 使用单引号
	"singleQuote": true
}
```

### webpack打包vue单文件组件

```
npm i vue-loader vue-template-compiler -D

const VueLoaderPlugin = require('vue-loader/lib/plugin');
module.exports = {
    plugins: [
        new VueLoaderPlugin()
    ],
    module: {
        rules: [
            { test: /\.vue$/, use: 'vue-loader' }
        ]
    }
};
```

## vue脚手架

```
npm install -g @vue/cli

// 创建项目
vue create my-project

vue ui

npm install -g @vue/cli-init
vue init webpack my-project
```

### vue脚手架自定义配置

```
// 通过package.json配置项目
"vue": {
	"devServer": {
		"open": true,
		"port": 8888
	}
}

// 通过单独的配置文件配置项目
// 根目录下创建vue.config.js
module.exports = {
	devServer: {
		open: true,
		port: 8888
	}
}
```

## ElementUI

```
// main.js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```

# 项目优化

- 生成打包报告

  - 命令行工具	`vue-cli-service build --report`
  - 通过可视化uI面板查看报告

- 第三方库启用CDN

  - 通过externals加载外部CDN资源

    ```
    config.set('externals',{
    	vue: 'Vue'
    	'vue-router': 'VueRouter',
    	axios: 'axios'
    })
    ```

  - 同时在`public/index.html`文件的头部添加CDN资源引用

- Element-UI组件按需加载

- 路由懒加载

  - ```
    // 安装@babel/plugin-syntax-dynamic-import包
    const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
    const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
    const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
    ```

- 首页内容定制

  - ```
    config.plugin('html').tap(args => {
    	args[0].isProd = true
    	return args
    })
    ```

    ```
    <title><%= HtmlWebpackPlugin.options.isProd ? '' : 'dev-' %>电商管理系统</title>
    ```

- 修改webpack的默认配置

  - 通过`vue.config.js`文件修改webpack的默认配置

    ```
    // vue.config.js
    module.exports = {
    	// 新增configureWebpack与chainWebpack节点来定义打包配置
    	chainWebpack: config => {
            config.when(process.env.NODE_ENV === 'production', config => {
                config.entry('app').clear().add('./src/main-prod.js')
            })
            config.when(process.env.NODE_ENV === 'development', config => {
                config.entry('app').clear().add('./src/main-dev.js')
            })
        }
    }
    ```

# 项目上线

- 通过node创建web服务器

  - ```
    const express = require('express');
    const app = express();
    // 托管静态资源
    app.use(express.static('./dist'));
    app.listen(80)
    ```

- 开启gzip配置

  - ```
    const compression = require('compression')
    // 写在静态资源托管前面
    app.use(compression())
    ```

- 配置https服务

  - [申请SSL证书](https://freessl.cn/)

  - ```
    const https = require('https')
    const fs = require('fs')
    const options = {
    	cert: fs.readFileSync('./full_chain.pem'),
    	key: fs.readFileSync('./private.key')
    }
    https.createServer(options,app).listen(443)
    ```

- 使用pm2管理应用

  - `npm i pm2 -g`
  - 启动项目  `pm2 start 脚本 --name 自定义名称`
  - 查看项目  `pm2 ls`
  - 重启项目  `pm2 restart 自定义名称/id`
  - 停止项目  `pm2 stop 自定义名称/id`
  - 删除项目  `pm2 delete 自定义名称/id`

# Vuex

Vuex是实现组件全局状态（数据）管理的一种机制，可以方便的实现组件之间数据的共享

## 基本使用

- 安装导入

  ```
  npm install vuex --save
  
  import Vuex from 'vuex'
  Vue.use(Vuex)
  ```

- 创建store对象

  ```
  const store = new Vuex.Store({
  	// state中存放的是全局共享数据
  	state: { count: 0 }
  })
  ```

- 将store对象挂载到vue实例上

  ```
  new Vue({
  	el: '#app',
  	router,
  	render: h => h(app),
  	store
  })
  ```

## 核心概念

### state

state提供唯一的公共数据源

```
// 访问数据
1. this.$store.state.全局数据名称

2. import { mapState } from 'vuex'
// 将全局数据映射为当前组件的计算属性
computed: {
	...mapState(['count'])
}
```

### mutations

用于变更store的数据

```
const store = new Vuex.Store({
	state: { count: 0 },
	// 不要在mutations函数中执行异步操作
	mutations: {
		// 第一个参数一定是state
		add(state,step) {
			state.count += step
		}
	}
})

// 触发mutation函数
1. 
methods: {
	handle() {
		this.$store.commit('add',3)
	}
}
2.
import { mapMutations } from 'vuex'
// 将全局数据映射为当前组件的方法
methods: {
	...mapMutations(['add']),
	handle() {
		this.add(3)
	}
}
```

### actions

用于处理异步任务

```
const store = new Vuex.Store({
	state: { count: 0 },
	mutations: {
		add(state,step) {
			state.count += step
		}
	},
	actions: {
		// actions不能直接修改数据，必须触发mutations的某个函数才行
		asyncAdd(context,step) {
			setTimeout(() => {
				context.commit('add',step)
			},1000)
		}
	}
})

import { mapActions } from 'vuex'
methods: {
	...mapActions(['asyncAdd']),
	handle() {
		// 触发actions的第一种方式
		this.$store.dispatch('asyncAdd',3)
	},
	handlee() {
		this.asyncAdd(3)
	}
}
```

### getters

用于对store中的数据加工处理返回新数据

```
const store = new Vuex.Store({
	state: { count: 0 },
	getters: {
		showNum: state => {
			return '【'+state.count+'】'
		}
	}
})

1. this.$store.getters.名称
2. import { mapGetters } from 'vuex'
// 将全局数据映射为当前组件的计算属性
computed: {
	...mapGetters(['showNum'])
}
```

### modules

将store分割成模块

```
const moduleA/B = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}
const store = new Vuex.Store({
	modules: {
    	a: moduleA,
    	b: moduleB
  	}
})
```

