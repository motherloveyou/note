# jQuery

一个JavaScript库，把js中的DOM操作做了封装，可以快速的查询使用里面的功能

## 基本使用

jQuery的入口函数

```
// 等着DOM结构渲染完毕即可执行内部代码，相当于原生js中的DOMContentLoaded
$(document).ready(function() {
	... // 此处是页面DOM加载完成的入口
});

$(function() {
	... // 此处是页面DOM加载完成的入口
});
```

`$`是jQuery的别称，也是jQuery的顶级对象

- jQuery对象的本质是用`$`对DOM对象包装后产生的对象，以伪数组形式存储
- jQuery对象只能使用jQuery方法，不能使用原生js的属性和方法；同样的DOM对象也不能使用jQuery的方法

**jQuery对象和DOM对象对换**

```
// 1.DOM对象转换为jQUery对象
$("div")

// 2.jQUery对象转换为DOM对象
$("div")[index]
$("div").get(index)		// index是索引号
```

## 选择器

**基础选择器**

`$("选择器");`		写法与css一致

**隐式迭代**

遍历内部DOM元素（以伪数组形式存储）的过程

**筛选选择器，:前面可以加选择器限定**

- `:first`获取第一个元素
- `:last`获取最后一个元素
- `:eq(index)`获取索引号为index的元素
- `:odd`获取索引号为奇数的元素
- `:even`获取索引号为偶数的元素
- `:checked`返回被选中的复选框元素

**筛选方法**

- `parent();`返回亲爸爸
- `parents(selector);`返回指定祖先元素
- `children(selector);`返回亲儿子，类似于子代选择器
- `find(selector);`返回所有后代，类似于后代选择器
- `siblings(selector);`返回所有兄弟节点，不包含自身
- `nextAll([expr]);`返回当前元素之后的同辈元素
- `prevAll([expr]);`返回当前元素之前的同辈元素
- `eq(index);`返回索引号为index的元素
- `hasClass(class);`判断是否有某个类名，有则返回true

## 操作样式

```
// 获取属性值
$("选择器").css("属性");		

// 修改一组属性值，属性必须加引号，值如果是数字可以不加
$("选择器").css("属性","值");	

// 以对象形式修改多组属性值，属性(驼峰命名法)可以不加引号，值如果是数字可以不加引号
$("选择器").css({
	属性1: "值1",
	属性2: "值2"
});	

// addClass  removeClass  toggleClass，返回的是当前jQuery对象
$("选择器").addClass("类名");		// 添加类
$("选择器").removeClass("类名");		// 移除类
$("选择器").toggleClass("类名");		// 切换类
```

## 动画效果

**显示与隐藏**

```
show([speed,[easing],[fn]]);		// 显示
hide([speed,[easing],[fn]]);		// 隐藏
toggle([speed,[easing],[fn]]);		// 显示隐藏切换
// speed:三种预定速度之一的字符串("slow","normal", or "fast")或表示动画时长的毫秒数值(如：1000)
// easing:(Optional) 用来指定切换效果，默认是"swing"，可用参数"linear"
// fn:在动画完成时执行的函数，每个元素执行一次。
```

**滑动效果**

```
slideDown([speed],[easing],[fn]);		// 下拉
slideUp([speed],[easing],[fn]);			// 上拉
slideToggle([speed],[easing],[fn]);		// 上拉下拉切换
```

**事件切换 hover**

鼠标经过和离开复合写法

```
hover(function() {},function() {});
// 只写一个函数时，鼠标经过和离开都会触发
// 写两个函数时，第一个是鼠标经过函数，第二个是鼠标离开函数
```

**动画队列及其停止排队方法**

动画或者效果一旦触发就会执行，如果多次触发，就会造成多个动画或者效果排队执行

```
stop()		// stop()方法用于停止动画或者效果，写到动画或者效果的前面，相当于停止结束上一次的动画
```

**淡入淡出效果**

```
fadeIn([[speed],[easing],[fn]]);			// 淡入
fadeOut([[speed],[easing],[fn]]);			// 淡出
fadeToggle([[speed],[easing],[fn]]);		// 淡入淡出切换
fadeTo(speed,opacity,[easing],[fn]);	    // 修改透明度，速度和透明度一定要写
// opacity:一个0至1之间表示透明度的数字。
```

**自定义动画 animate**

```
animate(params,[speed],[easing],[fn]);
// params:想要更改的样式属性，以对象形式传递；如果是复合属性则需采用驼峰命名法
```

## 属性操作

**固有属性**

```
// 获取固有属性
prop("属性");

// 设置固有属性
prop("属性","属性值")
```

**自定义属性**

```
// 获取自定义属性
attr("属性");

// 设置自定义属性
attr("属性","属性值")
```

**数据缓存**

```
data("属性");					// 获取以data-开头的H5自定义属性时，不要加data-
data("属性","属性值");		// 把元素当变量看，里面的数据存放在元素的内存里面
```

## 内容文本值

**普通元素内容**

```
// 相当于原生的innerHTML
html()					// 获取
html("内容")			  // 设置
```

**普通元素文本内容**

```
// 相当于原生的innerText
text()						// 获取
text("文本内容")			// 设置
```

**表单的值**

```
val()				// 获取
val("值")			// 设置
```

`index()`返回索引号

`toFixed(n)`保留n位小数

## 元素操作

**遍历元素**

```
// 遍历DOM元素
$("选择器").each(function(index,domEle) {
	// 第一个参数是索引号
	// 第二个参数是DOM对象，使用jQuery方法之前需要转换
});

// 主要用于数据处理，遍历数组、对象等
$.each(obj,function(index,domEle) {
	// 第一个参数是索引号或者属性名
	// 第二个参数是对应的值
});
```

**创建元素**

```
$("<li>..</li>")		// 创建了一个li
element.html("<li>..</li>")
```

**添加元素**

```
// 内部添加(父子关系)
element.append()		// 放到匹配元素内部的最后面
element.prepend()		// 放到匹配元素内部的最前面

// 外部添加(兄弟关系)
element.after()			// 把内容放入目标元素后面
element.before()		// 把内容放入目标元素前面
```

**删除元素**

```
element.remove()			// 删除匹配元素
element.empty()				// 删除匹配元素的子节点
element.html("")			// 2 3等价
```

## 位置和尺寸

```
// 尺寸
width()					     // 设置或返回元素的宽度
height()   				     // 设置或返回元素的高度
innerWidth()			     // 返回元素的宽度（包含 padding）
innerHeight()                // 返回元素的高度（包含 padding）
outerWidth([options])        // 返回元素的宽度（包含 padding 和 border），参数为true时还包括margin值
outerHeight([options])       // 返回元素的高度（包含 padding 和 border），参数为true时还包括margin值

// 位置
offset()							// 获取匹配元素在当前视口的相对偏移
offset({top:value,left:value})		// 设置匹配元素在当前视口的相对偏移
position()							// 获取匹配元素相对父元素的偏移，只读属性
scrollLeft()						// 返回第一个匹配元素的滚动条的水平位置
scrollLeft(position)				// 设置所有匹配元素的滚动条的水平位置
scrollTop()							// 返回第一个匹配元素的滚动条的垂直位置
scrollTop(position)					// 设置所有匹配元素的滚动条的垂直位置
```

# 事件

## 事件处理

```html
// 注册事件
element.click(function() {})		// 只能绑定一个事件
element.on("types",[sonSelector],fn);	// 可以绑定多个事件，多个事件处理程序
$("div").on({
	click: function() {},
	mouseenter: function() {}
});
$("div").on("click mouseenter",function() {});
// 可以实现事件委托。事件委托就是把原来加在子元素身上的事件绑定到父元素身上，就是把事件委托给父元素
// on绑定事件可以给未来动态创建的元素绑定事件          注意：动态绑定时需要使用事件委托
$("ul").on("click","li",function() {});// click是绑定在ul身上的，触发对象是里面的li

element.one("types",[selector],fn); // 注册一次性事件


// 解绑事件
element.off();			//参数为空解除所有事件
element.off("types");	// 解除某个事件
element.off("types",selector);		// 解除事件委托


// 自动触发事件
element.click();					// 会触发元素的默认行为
element.trigger("types");			// 会触发元素的默认行为
element.triggerHandler("types");		// 不会触发元素的默认行为
```

## jQuery拷贝对象

```
$.extend([deep,]target,obj1,obj2);		// 会覆盖target里面原来的数据
// deep默认为false，浅拷贝把被拷贝对象的复杂数据类型的地址拷贝给目标对象，修改目标对象会影响被拷贝对象
// deep为true时，深拷贝把里面的数据完全复制给目标对象，如果里面有不冲突的属性会合并到一起
```

## 多库共存

- 如果是`$`符号冲突，可以把`$`符号改为`jQuery`
- 让`jQuery`释放对`$`的控制权，让用户自己决定     `var zidingyi = $.noConflict();`

## jQuery插件

[jQuery之家](http://www.htmleaf.com/)

瀑布流

图片懒加载

全屏滚动

## JSON

```
JSON.stringify(data);		// 把对象转换为字符串
JSON.parse(data);			// 把字符串转换为对象格式
```

