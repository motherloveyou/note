

# JavaScript面向对象

**面向对象特性**

- 封装性
- 继承性
- 多态性

### 类与对象

```
class name {	// 类名首字母大写
	// 类的共有属性放到构造constructor里面
	constructor(形参) {
		// 构造函数
	}
	方法名() {
		// 函数体
	}			// 类里面的函数不需要function关键字，函数方法之间不能加逗号分隔
}
var xx = new name();	// 创建实例
```

- ES6 中类没有变量提升，必须先定义类再实例化对象
- 类里面共有的属性和方法一定要加`this`使用
- `constructor`里面的`this`指向实例对象，方法里面的`this`指向方法的调用者

### 类的继承

```
class Father{
	constructor() {}
	父方法() {}
}
class Son extends Father{
	constructor() {
		super();	// super关键字，用于调用父类的构造函数，必须写在子类this之前
	}
	方法() {
		super.父方法();	// 调用父类的普通方法
	}
}
```

# 构造函数和原型

### 构造函数

- 构造函数里面的属性和方法称为成员
- 实例成员是构造函数内部通过`this`添加的成员，只能通过实例化的对象来访问
- 静态成员是在构造函数本身上添加的成员，只能通过构造函数来访问`eg: Star.sex = '男';`

### 构造函数原型

- 构造函数通过原型分配的函数是所有对象共享的

- 每个构造函数都有一个`prototype`对象,里面的属性和方法都会被构造函数所拥有

- 把那些不变的方法，直接定义到`prototype`对象上，所有对象的实例就可以共享这些方法，不需要开辟新的内存空间

- 一般情况下，公共属性定义构造函数里面，公共的方法放到原型对象身上

- 每个对象都有`__proto__`属性，指向构造函数的原型对象`prototype`

- `__proto__`对象原型和`prototype`原型对象是等价的

- 原型对象`prototype`里面的`__proto__`指向`Object.prototype`

- `Object.prototype`里面的`__proto__`指向为`null`

- `__proto__`对象原型和`prototype`原型对象都有一个`constructor`属性，指回构造函数本身

- 如果以对象的形式给`prototype`原型对象赋值，需要手动利用`constructor`这个属性指回原来的构造函数

  **按原型链方向的成员查找机制**

![2020-07-23_104340](E:\笔记\img\2020-07-23_104340.png)

**原型对象的`this`指向的是实例对象**

**通过`prototype`原型对象可以拓展内置对象方法，如`Array`**

### 继承

ES6之前没有`extends`继承，通过构造函数+原型对象模拟实现继承，被称为组合继承

**`call()`方法**

```
fn.call(thisArg,arg1,arg2...)
// 可以调用函数
// 可以改变函数的this指向
```

```
function Father(uname,age) {
	this.uname = uname;
	this.age = age;
};
Son.prototype = new Father();		// 实现继承方法
Son.prototype.constructor = Son;	// 手动指回Son构造函数
function Son(uname,age) {
	Father.call(this,uname,age);	// 实现继承属性
}
```

### ES5新增方法

**数组**

```
// forEach	迭代（遍历）数组
// 在forEach里面写return不会终止迭代
arr.forEach(function(value,index,array) {});	// 数组元素 元素索引号 数组本身

// map	处理数组元素
arr.map(function(value,index,array) {
	return 元素处理;			// 返回一个经过处理后的元素组成的新数组
});

// filter	筛选数组
// 在filter里面写return不会终止迭代
arr.filter(function(value,index,array) {
	return 条件;			// 返回一个满足条件元素组成的新数组
});

// some   查找数组中是否有满足条件的元素
// 直接return true直接终止迭代
arr.some(function(value,index,array) {
	return 条件;			// 返回一个布尔值，查找到第一个满足条件元素时终止循环返回true
});

// every   检测数组所有元素是否都符合指定条件
arr.every(function(value,index,array) {
	return 条件;			// 返回一个布尔值，查找到第一个不满足条件元素时终止循环返回false
});
```

**字符串**

```
// trim 方法去除字符串左右两边的空格
str.trim();		// 返回一个新字符串
```

**对象**

```
// 获取对象的所有属性
Object.keys(obj);		// 返回一个由属性名组成的数组

// 定义新属性或修改原有的属性
Object.defineProperty(obj,prop,descriptor);
// 第三个参数descriptor以对象形式{}书写
// value:设置属性值，默认为undefined
// writable:属性值是否可以被重写，默认为false
// enumerable:目标属性是否可以被遍历，默认为false
// configurable:目标属性是否可以被删除和是否可以再次修改特性，默认为false

// 删除属性
delete obj.属性;
```

# 函数进阶

```
// 函数的三种定义方式
function fn() {};
var fn = function() {};
new Function('参数','函数体');	// 所有的函数都是Function的实例对象
```

### this指向

```
// 改变函数内部的this指向  call()  apply()  bind()
fn.call(thisArg,arg1,arg2...)	// 可以调用函数；会改变函数的this指向；主要作用是可以实现继承
fn.apply(thisArg,[argsArray])	// 可以调用函数；会改变函数的this指向；参数必须写成数组形式
fn.bind(thisArg,arg1,arg2...)	// 不会调用函数；会改变函数的this指向；返回的是原函数改变this之后产生的新函数
```

### 严格模式

**1.变量规定**

- 严禁不声明变量就赋值
- 严禁删除已经声明的变量

**2.this指向**

- 全局作用域的函数的`this`不在指向`window`，而是`undefined`
- 定时器、事件、对象里面的函数`this`不变
- 构造函数必须加`new`调用

**3.函数变化**

- 函数参数不允许重名

不允许在非函数的代码块内声明函数

```
// 为整个脚本开启严格模式
'use strict';	// 写在脚本文件最前面

// 为某个函数开启严格模式
'use strict';	// 写在函数体最前面
```

### 高阶函数

接收函数作为参数或将函数作为返回值输出的函数

### 闭包

闭包指有权访问另一个函数作用域中变量的函数。

闭包的主要作用是延伸了变量的作用范围

### 递归

在函数内部调用自身的函数

由于递归容易发生“栈溢出”错误，所以必须加退出条件

```
// n!
function fn(n) {
	if (n == 1) {
		return 1;
	}
	return n * fn(n - 1);
}
```

**浅拷贝和深拷贝**

```
// 浅拷贝
Object.assign(target, ...sources)	

// 深拷贝
function deepCopy(target,obj) {
	for (var k in obj) {
		var item = obj[k];
		if (item instanceof Arrar) {
			target[k] = [];
			deepCopy(target[k],item);
		} else if (item instanceof Object) {
			target[k] = {};
			deepCopy(target[k],item);
		} else {
			target[k] = item;
		}
	}
}
```

# 正则表达式

正则表达式是用于匹配字符串中字符组合的模式。在js中，正则表达式也是一个对象

- 匹配符合某种模式规则的字符串或文本，如表单验证
- 过滤(替换)页面内容的指定单词
- 提取字符串特定部分

### 匹配

**创建正则表达式**

```
// 利用RegExp对象来创建
var 变量 = new RegExp(/表达式/);

// 通过字面量创建
var 变量 = /表达式/;		// 正则表达式不需要加引号
```

**测试正则表达式**

```
// test()方法
regexObj.test(str);		// 返回true和false
```

**正则表达式的特殊字符**

[MDN链接](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)

`var rg = /abc/;`表示只要包含abc就可以

边界符：`^`  限定开头；`$`限定结尾；如果两者写在一起，则表示必须是精确匹配

字符类：`[]`表示有一系列字符可供选择，匹配其中一个就可以；如果`[]`里面有`^`表示取反

量词符：`*`  >=0     `+`  >=1     `?`  0或1     `{n}`  n      `{n,}`  >=n      `{n,m}` [n,m]		花括号里面不能有空格

预定义类：`\d`[0-9]	`\w`[a-zA-Z0-9_]	`\s`[\t\r\n\v\f]		大写则取反

### 替换

```
str.replace(regexp,newStr);		// 返回一个新字符串
```

正则表达式后面可以接参数

- `/表达式/g`		 全局匹配
- `/表达式/i`         忽略大小写
- `/表达式/gi`       全局匹配 + 忽略大小写

### 提取

```
str.match(regexp);		// 返回的是一个数组
```

# ES6

### 变量

**`let`关键字声明的变量**

- 具有块级作用域，防止循环变量变成全局变量
- 没有变量提升，变量必须先声明再使用
- 具有暂时性死区，变量会与块级作用域绑定，不受外部代码影响

### 常量

**`const`关键字声明的常量**

- 具有块级作用域
- 声明常量时必须赋值
- 常量赋值后，值不能修改（值对应的内存地址不可更改）

### 解构赋值

**数组解构**

```
// 按照一一对应的关系从数组中提取值，赋值给变量
let [a,b,c] = [1,2,3];		// 当变量个数超过数组长度时，超出的值为undefined
```

**对象解构**

```
// 使用变量的名字匹配对象的属性
let person = {name: 'zhangsan', age: 20};
let {name, age} = person;	// name,age是变量
let {name: myName, age: myAge} = person;	// myName,myAge是变量
```

### 箭头函数

```
() => {}	// 通常赋值给一个变量用于调用
// 若函数体中只有一句代码，且执行结果就是返回值，可以省略return和大括号
function sum(a,b) {
	return a + b;
}
const sum = (a,b) => a + b;
// 若形参只有一个，外侧的小括号可以省略
```

箭头函数不绑定`this`，没有自己的`this`关键字；箭头函数中的`this`将指向箭头函数定义位置中的`this`

```
 var obj = {
	say: () => {
		console.log(this);
	}
}
obj.say();			// 打印结果是window而不是obj对象
```

### 剩余参数

箭头函数里面不能使用`arguments`内置对象

```
function fn(...args) {
	// args是一个数组
}
```

剩余参数和解构配合使用

```
let arr = [1,2,3];
let [s1,...s2] = arr;		// s1: 1   s2: [2,3]
```

### 内置对象扩展

**扩展运算符`...`**

`...`扩展运算符可以将数组拆分成以逗号分隔的参数序列

```
let arr = [1,2,3];
...arr;  	// 1,2,3
```

扩展运算符可以用于合并数组

```
// 方法1
let arr1 = [1,2,3];
let arr2 = [4,5,6];
let arr3 = [...arr1,...arr2];	// [1,2,3,4,5,6]
// 方法2
arr1.push(...arr2);
```

扩展运算符可以将伪数组转换为数组

```
var divs = document.querySelector('div');
var arr = [...divs]; 
```

**构造函数方法`Array.from()`**

- 伪数组对象（拥有一个 `length`属性和若干索引属性的任意对象）

```
Array.from(arrayLike[,(item,index) => {}]);	// 回调函数是对数组元素进行操控，类似于map方法
```

**`Array`的实例方法**

```
arr.find((item,index) => {});	// 返回数组中第一个符合条件的数组成员，找不到则返回undefined
```

```
arr.findIndex((item,index) => {});	// 返回数组中第一个符合条件的数组成员索引号，找不到则返回-1
```

```
arr.includes(valueToFind);	// 表示某个数组是否包含指定的值，返回true或false
```

**模板字符串**

模板字符串使用反引号定义  ` `` `

- 可以解析变量`${变量}`
- 可以实现换行
- 可以调用函数`${fn()}`

```
let obj = {
	name: 'zhangsan',
	age: 20
}
const str = `
	<span>${obj.name}</span>
	<span>${obj.age}</span>
`;			// <span>zhangsan</span>
			   <span>20</span>
```

**字符串方法**

```
str.startsWith(searchString);	// 判断str字符串是否以参数字符串开头，返回true或false
str.endsWith(searchString);	    // 判断str字符串是否以参数字符串结尾，返回true或false
```

```
str.repeat(n);		// 将字符串重复n次，返回新字符串
```

### Set数据结构

类似于数组，区别在于值是唯一的，无重复；存在`size`属性而不是`length`

```
var s = new Set();		// 创建
var s = new Set(arr);	// 传递数组参数初始化，会自动去重
```

```
var arr = [1,2,3,1,2];
var s = new Set(arr);
var newArr = [...s];	// [1,2,3]
```

**`Set`提供的实例方法**

```
add(value)			// 添加值，返回Set结构本身
delete(value)		// 删除值，返回true或false
has(value)			// 判断是否为Set成员，返回true或false
clear()				// 清除所有成员，无返回值
```

```
var s = new Set(arr);
s.forEach((item,index) => {});		// 遍历Set数据结构
```

