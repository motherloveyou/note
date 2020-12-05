# Web  API

Web  API是浏览器提供的一套操作浏览器功能和页面元素的应用程序接口（BOM和DOM）

## DOM

简介：文档对象模型，是W3C组织推荐的处理可扩展标记语言的标准编程接口。通过这些DOM接口可以改变网页的内容、结构和样式。

#### 获取元素

**利用DOM提供的方法(API)获取元素			逻辑性不强，繁琐**

```
// 根据id获取
getElementById('id');						// 返回结果是元素对象

// 根据标签名获取
getElementsByTagName('标签名');			  // 返回结果是指定标签元素对象集合，以伪数组形式存储

// 根据类名获取（H5新增，ie9以上支持）
getElementsByClassName('类名');			  // 返回结果是指定类元素对象集合

// 根据选择器获取（H5新增，ie9以上支持）
document.querySelector('选择器');			// 返回指定选择器的第一个元素对象，选择器需要加符号(.class #id等)	document.querySelectorAll('选择器');		// 返回指定选择器的所有元素对象集合

// 获取特殊元素
document.body;							// 返回body元素对象
document.documentElement;				// 返回html元素对象
```

#### 事件基础

事件：可以被JavaScript侦测到的行为     触发----响应机制

事件三要素：事件源	事件类型	事件处理程序

| 事件         | 类型                           |
| ------------ | :----------------------------- |
| onclick      | 鼠标左键点击                   |
| onmousedown  | 鼠标按下                       |
| onmouseup    | 松开鼠标                       |
| onfocus      | 获得焦点                       |
| onblur       | 失去焦点                       |
| onmouseover  | 鼠标经过，经过子盒子还会触发   |
| onmouseenter | 鼠标经过，只会经过自身盒子触发 |
| onmouseout   | 鼠标离开                       |
| onmouseleave | 鼠标离开，与onmouseenter相对应 |
| onmousemove  | 鼠标移动                       |
| onscroll     | 滚动条发生变化                 |

<!--onmouseenter和onmouseleave只会触发一次的原因是它不会冒泡-->

```
<div> 123 </div>
...
var div = document.querySelector('div');		// 获取事件源
btn.onclick = function() {						// 注册事件，添加事件处理程序
	// 函数体
}
```

#### 操作元素

```
// 改变元素内容（表单使用无效，借助value属性）
element.innerText					// innerText不识别html标签，可读写，去除空格和换行
element.innerHTML					// innerHTML可以识别html标签，可读写，保留空格和换行✔

// 获取元素属性
// H5规定自定义属性以 data- 开头作为属性名
element.属性							// 只能获取元素的内置属性（element.className）
element.getAttribute('属性')			// 主要用于获取程序员自定义的属性（element.getAttribute('class')）
// H5新增获取自定义属性，ie 11以上支持，只能获取data-开头
element.dataset.属性名				   // dataset是所有以data开头的自定义属性的集合，属性名不加data-，并且写成驼峰命名法

// 设置元素属性
element.属性 = 值;						// 只能设置内置属性
element.setAttribute('属性',值);		// 主要用于设置自定义属性

// 移除属性
element.removeAttribute('属性');

// 修改元素的样式属性(宽、高、背景颜色等)
// 1. 样式比较少或功能简单的情况下使用
element.style.样式属性				// 产生的是行内样式，CSS权重较高；样式属性驼峰命名法，如fontSize等
// 2. 样式比较多或功能复杂的情况下使用
element.className				// 在css写一个类，事件触发时更改元素的类名（覆盖原先的类名）
```

**排他思想：所有元素全部清除样式；给当前元素设置样式**

#### 节点操作

**利用节点层级关系获取元素			逻辑性强，兼容性差**

节点：nodeType(节点类型)		nodeName(节点名称)		nodeValue(节点值)

- 元素节点 nodeType 为1
- 属性节点 nodeType 为2
- 文本节点 nodeType 为3（文本节点包含文字、空格、换行等）

**DOM树把节点划分为不同的层级关系，常见的是父子兄层级关系**

```
// 父节点
parentNode					// 得到的是离元素最近的父节点，找不到则返回null

// 子节点
childNodes(标准)			  // 返回所有子节点，包含元素节点、文本节点等等
children(非标准)			 // 返回所有元素子节点	★
firstChild					// 返回第一个子节点
lastChild					// 返回最后一个子节点
firstElementChild			// 返回第一个子元素节点，ie 9以上支持
lastElementChild			// 返回最后一个子元素节点，ie 9以上支持
// 通常情况下，第一个和最后一个子元素节点
children[0]
children[parentNode.children.length - 1]

// 兄弟节点
nextSibling							// 下一个兄弟节点
previousSibling						// 上一个兄弟节点
nextElementSibling					// 下一个兄弟元素节点，ie 9以上支持
previousElementSibling				// 上一个兄弟元素节点，ie 9以上支持
// 要解决兼容性问题，只能自己封装一个兼容性函数
function getNextElementSibling(ele) {
	while (ele = ele.nextSibling) {
		if (ele.nodeType == 1) {
			return ele;
		}
	}
	return null;
}

// 创建和添加元素节点(创建之后需要innerHTML赋值)
var child = document.createElement('');			 	// 创建元素节点
parentNode.appendChild(child);						// 将一个节点添加到指定父节点的子节点列表末尾
parentNode.insertBefore(child,指定子节点);			// 将一个节点添加到指定父节点的指定子节点前面

// 直接添加(创建、赋值、添加三合一)
element.insertAdjacentHTML(position, text);
// position表示插入内容相对于元素的位置，并且必须是以下字符串之一：
'beforebegin'：元素自身的前面。
'afterbegin'：插入元素内部的第一个子节点之前。
'beforeend'：插入元素内部的最后一个子节点之后。
'afterend'：元素自身的后面。

// 删除节点
parentNode.removeChild(child);				// 删除父节点中的指定子节点
node.remove();		// 删除指定节点

// 复制节点(克隆节点)
// 复制后需要进行添加
node.cloneNode();					// 参数为空或false时为浅拷贝，不复制内容；参数为true时为深拷贝，复制内容

// 三种创建元素方式
1. document.write('')				// 如果文档流加载完毕，会导致页面全部重绘
2. innerHTML						// innerHTML创建多个元素时效率更高(不要拼接字符串，采取数组形式拼接)，结构稍微复杂
3. document.createElement('')		// createElement()创建多个元素效率低一些，但是结构更清晰
```

## 事件高级

#### 注册事件（绑定）

传统注册方式：btn.onclick = function() {}		注册事件的唯一性，同一个元素同一个事件只能设置一个处理函数

方法监听注册方式：addEventListener()			同一个元素同一个事件可以注册多个监听器，按注册顺序依次执行

```
// 方法监听注册方式  ie9
element.addEventListener(type,listener[,useCapture])
// 1.事件类型是字符串，必定加引号，而且不带on
// 2.同一个元素同一个事件可以注册多个监听器(事件处理程序)
// 3.第三个参数决定DOM事件流的阶段，默认为false，处于冒泡阶段

// attachEvent ie9以前的版本支持(现在几乎不使用)
element.attachEvent('onclick',回调函数)
```

#### 删除事件（解绑）

```
// 传统注册方式
element.onclick = function() {
	// 处理程序
	element.onclick = null;
}
// 方法监听注册方式
element.addEventListener('click',fn);		// 需要解绑的监听器不能使用匿名函数，里面的fn调用不需要加小括号
function fn() {
	// 执行语句
	element.removeEventListener('click',fn);
}						
```

#### DOM事件流

事件发生时会在元素节点之间按照特定的顺序传播，这个传播过程即DOM事件流

DOM事件流分三个阶段

- 捕获阶段：由DOM最顶层节点逐级向下传播到具体的元素
- 当前目标阶段
- 冒泡阶段：由具体的元素逐级向上传播到DOM最顶层节点

1. js只能执行捕获或冒泡的其中一个阶段
2. 传统注册方式如`onclick`和`attachEvent`只能得到冒泡阶段
3. 有些事件没有冒泡，如`onfocus  onblur  onmouseenter  onmouseleave`
4. 如果`addEventListener`的第三个参数为`true`，则处于捕获阶段   document→html→body→目标元素
5. 如果`addEventListener`的第三个参数为`false`，则处于冒泡阶段    目标元素→body→html→document

#### 事件对象

```
element.onclick = function(event) {}
element.addEventListener('click',function(event) {})
// event对象代表着事件的状态，跟事件相关的一系列信息数据的集合都存放在里面，常写成event或者e的形式
// 注册事件时，event对象会被系统自动创建，是一个不需要传递实参的形参
// 在ie6、7、8中 固定写法window.event


// 常见事件对象属性和方法
e.target					// 返回的是触发事件的元素，而this返回的是绑定事件的元素
e.srcElement				// ie678用来返回触发事件元素
e.type						// 返回事件类型，不带on，如click、mouseover
e.preventDefault()		    // 阻止默认行为（事件），如不让链接跳转、提交按扭提交等
e.returnValue				// 使用传统注册方式，并且在ie678中执行时使用该属性阻止默认行为

// 阻止事件冒泡
e.stopPropagation()			// 标准写法
e.cancleBubble = true;		// ie678
```

#### 事件委托

不是每个子节点单独设置事件监听器，而是给父节点设置事件监听器，然后利用冒泡原理影响设置每个子节点

#### 常用的鼠标事件

```
// 1.禁止鼠标右键菜单
document.addEventListener('contextmenu',function(e) {
	e.preventDefault();
})

// 2.禁止选中文字
document.addEventListener('selectstart',function(e) {
	e.preventDefault();
})
```

**鼠标事件对象属性**

```
// 1.鼠标相对于浏览器窗口可视区域的坐标
e.clientX
e.clientY

// 2.鼠标相对于页面文档的坐标  ie9以上支持
e.pageX
e.pageY

// 3.鼠标相对于电脑屏幕的坐标
e.screenX
e.screenY
```

#### 常用的键盘事件

```
onkeyup				// 键盘按键弹起，不区分大小写
onkeydown			// 键盘按键按下，不区分大小写
onkeypress			// 键盘按键按下，不识别功能键ctrl shift 箭头等，可以区分大小写
// 三个事件的执行顺序 keydown--->keypress--->keyup


// 键盘事件对象
e.key				// 按键名称，兼容性极差
e.keyCode			// 相应键的ASCLL码值，按下字母键时keyup和keydown返回大写字母的ASCLL码值

// 在文本框中，keydown和keypress事件触发时，文字并没有落入到文本框中
```

## BOM

浏览器对象模型，由一系列相关的对象构成，核心对象是window。

BOM是浏览器厂商在各自浏览器上定义的，兼容性较差。

BOM包含DOM，**window**对象里包含**document  location  navigator  screen  history**.

window对象是浏览器的顶级对象，具有双重角色

- 是JS访问浏览器窗口的一个接口
- 是一个全局对象。定义在全局作用域的变量和函数都会变成window对象的属性和方法

#### window对象常见事件

```
// 页面加载事件
window.addEventListener('load',function() {})		// load等页面所有元素加载完毕，包含dom元素、图片、flash、css等
document.addEventListener('DOMContentLoaded',function() {})		// DOM加载完毕，不包含图片、flash、css等

// 页面加载事件，不管存不存在"往返缓存"都会触发
window.addEventListener('pageshow',function() {})

// 调整窗口大小事件
window.addEventListener('resize',function() {})		// 窗口大小变化时触发
window.innerWidth				// 当前屏幕的宽度
window.devicePixelRatio			// 物理像素比
```

#### 定时器

```
// 1.setTimeout 只调用一次
window.setTimeout(调用函数[,延时时间]);  	// window通常省略，延时时间单位为毫秒，不写默认为0，通常为定时器取名便于区分
window.clearTimeout(timeoutID);			// 停止定时器，window可省略，参数是定时器的标识符

// 2.setInterval 重复调用函数
window.setInterval(调用函数[,延时时间]); 	// 用法基本一致，区别就是可重复调用
window.clearInterval(timeoutID);	// 定时器标识符声明需要注意作用域，最好初始化为null，null返回的是一个空对象
```

#### this指向

- 全局作用域或者普通函数**(包括定时器)**中`this`指向全局对象`window`
- 方法调用中`this`指向方法调用者
- 构造函数中`this`指向构造函数的实例

#### JS执行队列

任务分为同步任务和异步任务（回调函数），同步任务放到主线程执行栈，异步任务放到任务队列里

- 执行执行栈中的同步任务
- 把异步任务放入任务队列中
- 一旦执行栈中的同步任务全部执行完，按次序读取任务队列中的异步任务，进入执行栈开始执行

**事件循环：**由于主线程不断重复获取任务队列的的任务、执行任务、再获取、再执行，这种机制称为事件循环**(event loop)**

#### location对象

location属性用于获取或设置窗体的URL，并且可以用于解析URL。因为这个属性返回的是一个对象，也称为location对象

| location对象属性  | 返回值                              |
| ----------------- | ----------------------------------- |
| location.href     | 整个URL                             |
| location.host     | 域名                                |
| location.port     | 端口号，未写则返回空字符串          |
| location.pathname | 路径名                              |
| location.search   | 参数                                |
| location.hash     | 片段    #后面内容  常见于链接  锚点 |

```
// location对象方法
location.assign('url');			// 跳转页面，记录浏览历史，可以实现后退
location.replace('url');		// 跳转页面，不记录浏览历史，不可以实现后退
location.reload();			// 重新加载页面，相当于刷新按钮或者按了F5；参数为true时强制刷新，相当于ctrl+F5
```

#### navigator对象

包含浏览器的相关信息，常用的是`userAgent`属性，该属性可以返回由客户机发送服务器的`user-agent`头部的值

#### history对象

包含用户访问过的URL

```
// history对象方法
history.back();				// 页面后退，相当于浏览器顶部的 ← 按钮
history.forward();			// 页面前进，相当于浏览器顶部的 → 按钮
history.go(n);				// 参数为数字，正数表示前进多少步，负数表示后退多少步
```

## PC端网页特效

#### 元素偏移量offset

使用offset系列相关属性动态得到元素的位置(偏移量)、大小等

| offset系列常用属性   | 作用                                           |
| -------------------- | ---------------------------------------------- |
| element.offsetParent | 返回带有定位的父元素，父级都没有定位则返回body |
| element.offsetTop    | 返回元素相对带有定位父级的上方偏移             |
| element.offsetLeft   | 返回元素相对带有定位父级的左方偏移             |
| element.offsetWidth  | 返回自身宽度，包括padding、内容和边框          |
| element.offsetHeight | 返回自身高度，包括padding、内容和边框          |

<!--element.parentNode返回的是上一级的父元素，不管有没有定位-->

**offset与style的区别**

- offset可以得到任意样式表中的样式值；style只能得到行内样式表中的样式值
- offset系列获得的值是没有单位的；style.width是带有单位的字符串
- offsetWidth包含padding+border+width；style.width不包含padding和border
- offsetWidth等属性是只读属性；style.width是可读写属性
- 获取元素大小位置时，用offset合适；更改元素值时，需要用style去改变

#### 元素可视区client

| client系列属性       | 作用                                        |
| -------------------- | ------------------------------------------- |
| element.clientTop    | 元素上边框的大小                            |
| element.clientLeft   | 元素左边框的大小                            |
| element.clientWidth  | 返回自身包括padding、内容的宽度，不包括边框 |
| element.clientHeight | 返回自身包括padding、内容的高度，不包括边框 |

#### 立即执行函数

```
// 不需要调用，可以立马执行的函数
1.(function() {})();
2.(function() {}());  
// 第二个小括号可以近似看作是调用函数，可以传递参数
// 也可以给函数加命名，如(function fn() {}())
// 立即执行函数的主要作用是创建了一个独立的作用域，不会有命名冲突，里面的所有变量都是局部变量
// 立即执行函数要加分号
```

#### 元素滚动scroll

| scroll系列属性       | 作用                                            |
| -------------------- | ----------------------------------------------- |
| element.scrollTop    | 元素被卷去的上侧距离                            |
| element.scrollLeft   | 元素被卷去的左侧距离                            |
| element.scrollWidth  | 返回自身包括padding、内容的实际宽度，不包括边框 |
| element.scrollHeight | 返回自身包括padding、内容的实际高度，不包括边框 |

- 与`element.clientWidth`的区别在于当内容超出盒子时，`scrollWidth`的值也会超出盒子宽度
- 如果要得到页面被卷去的头部，通过`window.pageYOffset`得到，被卷去的左侧则是`window.pageXOffset`
- `window.scroll(x, y)`可以实现窗口滚动

#### 动画函数封装

```
function animate(obj, target, callback) {
    clearInterval(obj.timer);
    obj.timer = setInterval(function() {
        var step = (target - obj.offsetLeft) / 10;
        step = step > 0 ? Math.ceil(step) : Math.floor(step);
        if (obj.offsetLeft == target) {
            clearInterval(obj.timer);
            callback && callback();
        }
        obj.style.left = obj.offsetLeft + step + 'px';
    }, 15)
}
```

## 移动端网页特效

#### 触屏事件

| 触屏touch事件 | 类型                      |
| ------------- | ------------------------- |
| touchstart    | 手指触摸到一个DOM元素元素 |
| touchmove     | 手指在一个DOM元素上滑动   |
| touchend      | 手指从一个DOM元素离开     |

`touchmove`滑动时，屏幕也会滚动，需要避免可以用`e.preventDefault()`阻止屏幕一起滚动

#### 触屏事件对象

| 触摸事件对象属性 | 说明                              |
| ---------------- | --------------------------------- |
| touches          | 正在触摸屏幕的所有手指列表        |
| targetTouches    | 正在触摸当前DOM元素的所有手指列表 |
| changedTouches   | 手指状态发生了改变的列表          |

`transitionend`事件在过渡完成后出发

```
element.classList属性返回元素的类名列表，以伪数组形式存储，ie 10以上支持
element.classList.add('类名');			// 添加类名（追加）
element.classList.remove('类名');			// 移除类名
element.classList.toggle('类名');			// 切换类名，有则移除，无则添加
```

#### click延迟

移动端`click`事件会有300ms的延迟，原因是移动端屏幕双击会缩放页面

**click延迟解决方案**

**1.禁用缩放功能**

```
<meta name="viewport" content="user-scalable=no">
```

**2.利用touch事件封装一个函数**

```
//封装tap，解决click 300ms 延时
function tap (obj, callback) {
    var isMove = false;
    var startTime = 0; // 记录触摸时候的时间变量
    obj.addEventListener('touchstart', function (e) {
        startTime = Date.now(); // 记录触摸时间
    });
    obj.addEventListener('touchmove', function (e) {
        isMove = true;  // 看看是否有滑动，有滑动算拖拽，不算点击
    });
    obj.addEventListener('touchend', function (e) {
        if (!isMove && (Date.now() - startTime) < 150) {  // 如果手指触摸和离开时间小于150ms 算点击
            callback && callback(); // 执行回调函数
        }
        isMove = false;  //  取反 重置
        startTime = 0;
    });
}
//调用  
tap(div, function(){   // 执行代码  });
```

**3.fastclick插件**

GitHub官网地址： [https://github.com/ftlabs/fastclick](https://github.com/ftlabs/fastclick)

引入js文件后，把下面的代码写在最前面

```
if ('addEventListener' in document) {
	document.addEventListener('DOMContentLoaded', function() {
		FastClick.attach(document.body);
	}, false);
}
```

#### 常用插件

swiper中文网：https://www.swiper.com.cn/

superslide：http://superslide2.com/

视频插件：https://github.com/ireaderlab/zyMedia

## 本地存储

### webStorage

- 数据存储在用户浏览器中，不参与服务器通信
- 设置、读取方便，甚至页面刷新也不会丢失数据
- 容量较大，约5M
- 只能存储字符串，可以将对象`JSON.stringify()`编码后存储

#### sessionStorage

**特性**

1. 生命周期为关闭页面或浏览器窗口
2. 相同浏览器的不同页面无法共享sessionStorage的信息
3. 以键值对的形式存储使用

```
// 存储数据
sessionStorage.setItem(key,value);

// 获取数据
sessionStorage.getItem(key);

// 删除数据
sessionStorage.removeItem(key);

// 删除所有数据
sessionStorage.clear();
```

#### localStorage

**特性**

1. 生命周期永久生效，除非手动删除否则关闭页面也会存在
2. 相同浏览器的不同页面可以共享相同的localStorage（前提是页面属于相同域名和端口）；
3. 以键值对的形式存储使用

```
// 存储数据
localStorage.setItem(key,value);

// 获取数据
localStorage.getItem(key);

// 删除数据
localStorage.removeItem(key);

// 删除所有数据
localStorage.clear();
```

### cookie

**特性**

1. 生命周期为在cookie设置的过期时间之前一直有效，即使窗口或者浏览器关闭；
2. 存放数据大小为4K；
3. 有存储个数限制（各浏览器不同），一般不超过20个；

**优点：极高的扩展性和可用性**
1、数据持久性
2、不需要任何服务器资源，因为cookie是存储在客户端并发送给服务器读取
3、可配置到期规则，控制cookie的生命周期，使之不会永远有效，偷盗者可能拿到的是一个过期的cookie
4、简单性，基于文件的轻量结构
5、通过良好的编程，控制保存在cookie中的Session对象的大小
6、通过加密和安全传输技术（ssl），减少cookie被破解的可能性
7、只要cookie中不存放敏感的数据，即使被盗也不会有重大损失

**缺点：**
1、 cookie的数量和长度都有限制
数量：cookie的数量有限

- IE6及以下的版本最多20个cookie
- IE7以后的可以有50个cookie
- Firefox可以有50个cookie
- chrome和safri没有限制

长度：每个cookie的长度不超过4k，否则会被截掉

2、潜在的安全风险：cookie可能被截取篡改，如果cookie被拦截，就可能会获取到所有的Session信息
3、用户配置为禁用，有的用户禁用了浏览器或者客户端设备接受cookie的能力，因此限制了这一功能
4、有些状态不可能保存在客户端，例如，为了防止重复提交表达，需要在服务器端保存一个计时器，如果把这个计时器保存在客户端，它将不起作用。