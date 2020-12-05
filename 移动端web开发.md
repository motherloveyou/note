# 移动端web开发

### 视口

布局视口、视觉视口、理想视口(常用)

```
<meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no" >
```


### 二倍图

物理像素比：PC端1px在移动端内包含的物理像素

由于物理像素比的存在，采用倍图来防止在移动端图片放大导致模糊

### 开发选择方案

单独制作移动端页面(主流)

响应式兼容PC移动端

### CSS3盒子模型

padding和border 不会撑开盒子

传统盒子模型默认带有  

```
box-sizing: content-box;
```

在CSS3模型变为  

```
box-sizing: border-box;
```

<u>移动端可完全使用CSS3盒子模型，PC端考虑兼容性问题一般使用传统盒子模型</u>

### 特殊样式

##### 点击高亮清除

```
* {-webkit-tap-highlight-color: transparent;}
```

##### 浏览器默认外观样式清除

```
input {-webkit-appearance: none;}
```

##### 禁用长按页面时弹出的菜单

```
img, a {-webkit-touch-callout: none;}
```

### 移动端常见布局

#### 单独制作移动端页面

加入max-width/height 、min-width/height保护内容

常用初始化样式

```
body {
	margin: 0 auto;
	min-width: 320px;
	max-width: 640px;
	color: #666;
	background: #f2f2f2;
	font-size: 14px;
	font-family: -apple-system, Helvetica, sans-serif;
	line-height: 1.5;
	overflow-x: hidden;
}
```

固定定位的盒子必须设置宽度

```
{
	position: fixed;
	width: 100%;
	min-width: 320px;
	max-width: 640px;
}
```

###### 二倍精灵图做法

1. 把精灵图等比例缩放为原来的一半
2. 根据大小测量坐标
3. background-size:  精灵图原来宽度的一半 auto;

##### 1.流式布局(百分比布局)



##### 2.flex弹性布局

###### flex布局原理

通过给父盒子(容器)添加flex属性，来控制子盒子(项目)的位置和排列方式。为父盒子设为flex布局后，子元素的float、clear、vertical-align属性将会失效

```
父盒子 {display:flex;}
```

###### flex布局父项常见属性

设置主轴的方向(子元素随着主轴排列)

```
flex-direction:row/row-reverse/column/column-reverse;	从左到右/从右到左/从上到下/从下到上
```

设置主轴上子元素的排列方式

```
justify-content: flex-start/flex-end/center/space-around/space-between;
                   从头部开始/从尾部开始/居中/平分剩余空间/先两边贴边再平分剩余空间
```

设置子元素是否换行(列)

```
flex-wrap: nowrap/wrap;不换行/换行
```

设置侧轴上子元素的排列方式

```
单行 align-items: flex-start/flex-end/center/stretch;	  从头部开始/从尾部开始/居中/拉伸(需要去掉子盒子的宽度或高度)
多行 align-content: flex-start/flex-end/center/stretch;
```

主轴和是否换行的复合属性

```
flex-flow: row wrap;
```

###### flex布局子项常见属性

定义子项目分配剩余空间

```
flex: <number>/百分比;
```

控制子项自己在侧轴的排列方式(可以覆盖align-items属性)

```
align-self: auto/flex-start/flex-end/center;    auto表示继承父元素的align-items属性
```

定义项目排列顺序

```
order: <number>;	默认值是0，数值越小越靠前
```

###### 背景线性渐变

```
background: -webkit-linear-gradient(left,red,blue);		必须加浏览器前缀
```

##### 3.grid布局

指定网格的宽高

```css
.wrap {
	display: grid;
	// 可以使用百分比 grid-template-columns: 33.33% 33.33% 33.33%;
	// repeat接受两个参数：第一个数重复的次数，第二个是重复的值 grid-template-columns: repeat(2,100px 20px 80px)
	// 使用auto-fill来自动填充 grid-template-columns: repeat(auto-fill, 100px) 
	// fr表示比例关系 grid-template-columns: 1fr 2fr 1fr; fr可以和绝对长度相结合一起使用
	// grid-template-columns: 1fr 1fr minmax(100px,1fr);  minmax(100px,1fr)表示列宽不小于100px，不大于1fr
	grid-template-columns: 150px 150px 150px;
    grid-template-rows: 150px 150px 150px;
}
```

设置行与行、列与列之间的间隔

```
.wrap {
    grid-row-gap: 10px;
  	grid-column-gap: 10px;
  	// 合并写法 grid-gap: 10px 10px;
} 
```

自定义排列顺序  默认是先行后列

```css
.wrap {
	<!-- row column -->
	grid-auto-flow: colunm;
}
```

设置单元格内容的位置

```
.wrapper {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
place-items: <align-items> <justify-items>; // 如果省略第二个值，则浏览器认为与第一个值相等。
```

设置内容区域在容器里面的位置

```
.wrapper {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
}
place-content: <align-content> <justify-content>
```

指定项目的边框

- `grid-column-start`属性：左边框所在的垂直网格线
- `grid-column-end`属性：右边框所在的垂直网格线
- `grid-row-start`属性：上边框所在的水平网格线
- `grid-row-end`属性：下边框所在的水平网格线

##### 4.less+rem+媒体查询布局

###### rem单位

em: 相对于父元素的字体大小来说的

rem: 相对于html元素字体大小来说的(优点是可以通过修改html里面的文字大小对页面所有元素整体控制)

###### 媒体查询(Media  Query)

可以针对不同的屏幕尺寸设置不同的样式

```
@media mediatype and|not|only (media feature) {
	CSS-Code;
}
媒体类型mediatype: all/print/screen
关键字and|not|only: 且|非|特定
媒体特性media feature(必须有小括号)
```

###### 媒体查询--引入资源

针对不同的屏幕尺寸调用不同的CSS文件

```
<link rel="stylesheet" media="mediatype and|not|only (media feature)" href="mystylesheet.css">
```

###### less基础

一门CSS预处理语言，在CSS语法基础上，引入了变量，Mixin(混入)，运算以及函数等功能

less变量：@变量名：值；（@前缀，不能包含特殊字符，不以数字开头，大小写敏感）

less编译：less文件生成css文件，通过vs code的Easy LESS插件实现

less嵌套：子元素的样式直接写到父元素里面；如果有伪类、伪元素、交集选择器，内层选择器前面需要加&符号

less运算：运算符的左右两侧必须用一个空格隔开；两个数参与运算，若只有一个数有单位，结果以该单位为准，若两个数有不同的单				   位，结果以第一个单位为准；

###### rem适配方案

动态计算并设置html根标签的font-size大小(把屏幕划分为n等份，每一份作为html字体大小)

```
@import "lessname"/"lessname.less";   //导入样式
```

引入flexible.js

github地址：[https://github.com/amfe/lib-flexible](https://link.jianshu.com/?t=https://github.com/amfe/lib-flexible)

css中，元素值同等比例换算为rem为单位的值(cssrem插件)

#### 响应式页面兼容移动端

使用媒体查询针对不同宽度的设备进行布局和样式的设置，从而适配不同的设备

| 设备划分               | 尺寸区间 | 常见响应式布局容器尺寸 |
| :--------------------- | :------- | ---------------------- |
| 超小屏幕(              | <768px   | 100%                   |
| 小屏设备(平板)         | >=768px  | 750px                  |
| 中等屏幕(桌面显示器)   | >=992px  | 970px                  |
| 宽屏设备(大桌面显示器) | >=1200px | 1170px                 |

##### bootstrap初步使用

html骨架结构

```
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- Bootstrap -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@3.3.7/dist/css/bootstrap.min.css" rel="stylesheet">

    <!-- HTML5 shim 和 Respond.js 是为了让 IE8 支持 HTML5 元素和媒体查询（media queries）功能 -->
    <!-- 警告：通过 file:// 协议（就是直接将 html 页面拖拽到浏览器中）访问页面时 Respond.js 不起作用 -->
    <!--[if lt IE 9]>
      <script src="https://cdn.jsdelivr.net/npm/html5shiv@3.7.3/dist/html5shiv.min.js"></script>
      <script src="https://cdn.jsdelivr.net/npm/respond.js@1.4.2/dest/respond.min.js"></script>
    <![endif]-->
</head>
```

##### 栅格系统(grid systems)

将页面布局划分为等宽的列，系统自动把布局容器分为12列

|                           | **超小屏幕** 手机 (<768px) | **小屏幕** 平板 (≥768px) | **中等屏幕** 桌面显示器 (≥992px) | **大屏幕** 大桌面显示器 (≥1200px) |
| ------------------------- | -------------------------- | ------------------------ | -------------------------------- | --------------------------------- |
| `.container` **最大宽度** | 100%                       | 750px                    | 970px                            | 1170px                            |
| **类前缀**                | .col-xs-                   | .col-sm-                 | .col-md-                         | .col-lg-                          |

- “行（row）”必须包含在 `.container` （固定宽度）或 `.container-fluid` （100% 宽度）中，以便为其赋予合适的排列（aligment）和内补（padding）。(row类会取消父元素的padding值，并且高度和父级一样高)
- 通过“行（row）”在水平方向创建一组“列（column）”。
- 你的内容应当放置于“列（column）”内，并且，只有“列（column）”可以作为行（row）”的直接子元素。
- 如果一“行（row）”中包含了的“列（column）”大于 12，多余的“列（column）”所在的元素将被作为一个整体另起一行排列。

###### 列嵌套

通过添加一个新的 `.row` 元素和一系列 `.col-sm-*` 元素到已经存在的 `.col-sm-*` 元素内。被嵌套的行（row）所包含的列（column）的个数不能超过12（其实，没有要求你必须占满12列）。

###### 列偏移

使用 `.col-md-offset-*` 类可以将列向右侧偏移。这些类实际是为当前元素增加了左侧的边距（margin）。例如，`.col-md-offset-4` 类将元素向右侧偏移了4个列（column）的宽度。

###### 列排序

通过使用 `.col-md-push-*` 和 `.col-md-pull-*` 类就可以很容易的改变列（column）的顺序。

###### 响应式工具

| 类名       | 超小屏 | 小屏 | 中屏 | 大屏 |
| ---------- | ------ | ---- | ---- | ---- |
| .hidden-xs | 隐藏   | 可见 | 可见 | 可见 |
| .hidden-sm | 可见   | 隐藏 | 可见 | 可见 |
| .hidden-md | 可见   | 可见 | 隐藏 | 可见 |
| .hidden-lg | 可见   | 可见 | 可见 | 隐藏 |

与之相反的是 `.visible-xs` `.visible-sm` `.visible-md` `.visible-lg`只显示某个页面内容