# javascript基础语法

**js组成**

- ECMAScript
- DOM
- BOM

**输入输出语句**

- prompt(info)		弹出输入框
- alert(msg)             弹出警示框
- console.log(msg)      控制台打印输出信息

## 变量

```
var 变量名 = 变量值；  				//声明多个变量时以逗号隔开;变量名不能以数字开头，只能包含"_"和"$"两种特殊符号
```

## 数据类型

javascript是一种弱类型或者说动态语言，变量的数据类型在程序运行过程中确定，这就意味着变量的数据类型是可变的。

#### 简单数据类型

- 数字型number：八进制前面加 **0** ，十六进制前面加 **0x**

  ​				最大值：Number.MAX_VALUE			最小值：Number.MIN_VALUE    

  ​				无穷大/小：Infinity / -Infinity			非数字：NaN

  ​				**isNaN()**方法用来判断是否为非数字类型，如果是数字返回false，反之则返回true

- 字符串型string：引号中的任意文本，嵌套是采用外单内双或者外双内单

  ​				常用转义字符：换行 \n 	空格 \b 	tab缩进 \t（转义字符写在引号里面）

  ​				通过字符串的**length**属性获取字符串长度

  ​				字符串拼接：字符串 + 任何类型 = 新字符串

- 布尔型boolean：true / false，当参与运算时当作1 / 0 来看

- 未定义数据类型undefined：变量声明但未赋值或者`var 变量名 = undefined;`

- 空值null：`var 变量名 = null;`返回的变量类型是object

#### 数据类型转换

获取变量的数据类型：`typeof 变量名;`（prompt得到的是字符串型数据）

**转换为字符串**

```
var num = 10;
var str1 = num.toString();		//变量.toString()
var str2 = String(num);			//String(变量)强制转换
var str3 = num + '';			//利用+拼接字符串（也称为隐式转换）		★
```

**转换为数字型**

```
var age = prompt('请输入您的年龄');
var num1 = parseInt(age);		//parseInt(变量)，结果转换为整型，转换类似'10px'时会去掉单位		★
var num2 = parseFloat(age);		//parseFloat(变量)，结果转换为浮点型，转换类似'10px'时会去掉单位		★
var num3 = Number(age);			//Number(变量)强制转换
var num4 = age - 0;				//利用算术运算 - * / % 隐式转换
```

**转换为布尔型**

Boolean()函数：代表空、否定的值如  ' '、0、null、NaN、undefined 会被转换为false，其余值都被转换为true

## 运算符

**前置递增/递减与后置递增/递减**

- 必须和变量配合使用
- 单独使用时，结果一样
- 与其他代码联用时，前置先自加/减，后返回值；后置先返回原值，后自加/减。

#### 比较运算符

程序里面的比较运算符(>  >=  <  <=  ==  !=) 会**默认转换比较值的数据类型**

```
console.log(18 == '18');		//true
console.log(18 > '17');			//true
console.log('18' >= '17');		//true
```

全等号  ===  要求**值和数据类型**都一致

```
console.log(18 === '18');		//false
```

#### 短路运算（逻辑中断）

原理：当有多个表达式（值）时，左边的表达式值可以确定结果时，就不在继续运算右边表达式的值

- 逻辑与：表达式1 && 表达式2

  如果第一个表达式的值为真，则返回表达式2

  如果第一个表达式的值为假，则返回表达式1

```
console.log(123 && 456);	//456
console.log(0 && 456);		//0
```

- 逻辑或：表达式1 || 表达式2

  如果第一个表达式的值为真，则返回表达式1

  如果第一个表达式的值为假，则返回表达式2

```
console.log(123 || 456);	//123
console.log(0 || 456);		//456
```

#### 运算符优先级

| 优先级 | 运算符     | 顺序                    |
| ------ | ---------- | ----------------------- |
| 1      | 小括号     | (   )                   |
| 2      | 一元运算符 | ++   --   !             |
| 3      | 算数运算符 | 先  *  /  %    后  +  - |
| 4      | 关系运算符 | >   >=   <   <=         |
| 5      | 相等运算符 | ==   !=   ===   !==     |
| 6      | 逻辑运算符 | 先&&   后\|\|           |
| 7      | 赋值运算符 | =                       |
| 8      | 逗号运算符 | ,                       |

## 流程控制

#### 分支结构

**三元表达式**

```
条件表达式 ? 表达式1 : 表达式2;		//相当于if else语句
```

**switch语句**

当要针对变量设置一系列的特定值的选项时，就可以使用switch

```
switch (表达式) {
	case value1:
		执行语句1;
		break;
	case value2:
		执行语句2;
		break;
	......
	default:
		执行最后的语句;
}											//利用表达式的值与选项值匹配（全等）
```

#### 循环结构

while和do...while循环适用于判断条件比较复杂的情况，do...while循环至少执行一次，while循环可能一次都不执行

```
var message = prompt('你爱我吗？');
while (message !== '我爱你') {
	message = prompt('你爱我吗？');
}
alert('我也爱你呀！');
……………………………………………………………………………………………………………………………………………………
do {
	message = prompt('你爱我吗？');
} while (message !== '我爱你')
alert('我也爱你呀！');
```

## 数组

#### 创建数组

```
var 数组名 = new Array(n);		// 利用new创建一个长度为n的空数组
var 数组名 = [];				//利用数组字面量创建数组		★
```

**新增数组元素**

- 修改length长度（新增的元素为undefined）
- 修改索引号，追加数组元素（直接给数组名赋值会取代所有数组元素）

#### 冒泡排序

```
var arr = [2, 1, 4, 5, 3];
for (var i = 0; i < arr.length - 1; i++) { 						//趟数
	for (var j = 0; j < arr.length - i - 1; j++) { 				//次数
		if (arr[j] > arr[j + 1]) {
			var temp = arr[j];
			arr[j] = arr[j + 1];
			arr[j + 1] = temp;
		}
	}
}
console.log(arr);
```

## 函数

#### 函数的两种声明方式

利用函数关键字自定义函数（命名函数）

```
function fn() {

}
fn();
```

函数表达式（匿名函数）

```
var fun = function(){

}
fun();						// fun是变量名，不是函数名
```

#### 形参实参匹配

- 如果实参的个数等于形参的个数，正常输出
- 如果实参的个数多于形参的个数，只取到形参的个数
- 如果实参的个数少于形参的个数，多余的形参被定义为undefined

#### return

- return终止函数，return后面的代码不会被执行
- return只能返回一个值，返回的结果是最后一个值
- 返回多个值时采用数组
- 函数是都有返回值的，如果函数没有return，返回的是undefined

#### arguments

arguments是函数特有的的一个内置对象，存储了传递的所有实参，其展示形式是一个伪数组

<!--伪数组：1.具有数组的length属性2.按照索引的方式进行存储3.没有真正数组的一些方法 pop() push()等等-->

## 作用域

- 全局作用域：整个script标签或外部js文件
- 局部作用域：函数内部
- 块级作用域（es6新增）

作用域的目的是提高程序的可靠性，更重要的是减少命名冲突

- 全局变量	注意：如果在函数内部没有声明直接赋值的变量也属于全局变量
- 局部变量

从执行效率来看

①全局变量只有在浏览器关闭的时候才会销毁，比较占内存资源

②局部变量在当程序执行完毕就会销毁，比较节约内存资源

#### 作用域链

内部函数访问外部函数的变量，采取的是链式查找的方式来决定取哪个值（就近原则）

## 预解析	★

js引擎运行分两步：预解析：js引擎把js里面的所有var	function提升到当前作用域的最前面

​								  代码执行：按代码书写的顺序从上往下执行

预解析又分为	变量预解析（变量提升）、函数预解析（函数提升）

- 变量提升：把所有的变量声明提升到当前作用域的最前面		不提升赋值操作

- 函数提升：把所有的函数声明提升到当前作用域的最前面		不调用函数

  ​	**注意：只提升利用function关键字自定义的命名函数，不提升函数表达式，所以函数表达式调用必须写在函数表达式下面**

```
f1();
console.log(c);
console.log(b);
console.log(a);
function f1() {
	var a = b = c = 9;   // 相当于var a = 9;  b = 9; c = 9;所以a是局部变量，b、c是全局变量
	console.log(a);
	console.log(b);
	console.log(c);
}
//相当于执行了以下代码
function f1() {
	var a;
	a = b = c = 9;
	console.log(a);
	console.log(b);
	console.log(c);
}
f1();
console.log(c);
console.log(b);
console.log(a);              // 执行结果为9	9	9	9	9	报错
```

## 对象

对象是一组无序的相关属性和方法的集合。

#### 创建对象

**利用字面量创建对象**

```
var obj = {
	属性1: value1,
	属性2: value2,
	方法1: function() {},
	方法2: function() {}
};

// 调用对象属性
对象名.属性名			obj.属性1
对象名['属性名']		obj['属性2']

// 调用对象方法
对象名.方法名()		obj.方法1()
```

**利用new  Object( )创建对象**

``` 
var obj = new Object();			// 创建了一个空对象
obj.属性1 = value1;
obj.属性2 = value2;
obj.方法1 = function() {}
```

**利用构造函数创建对象**

前两种方法一次只能创建一个对象

构造函数就是把对象里面一些相同的属性和方法抽象出来封装到函数里面去

```
function 构造函数名(形参1,形参2...) {
	this.属性 = 值;
	this.方法 = function() {}
}
// 调用构造函数
new 构造函数名(实参1,实参2...);     // 也称为对象的实例化
```

注意事项：①构造函数名首字母要大写

​				   ②构造函数不需要return，就可以返回结果（一个对象）

​				   ③属性和方法前面必须添加this

#### 遍历对象

for...in  语句遍历对象的属性和方法(方法比较少用)

```
var obj = {
	属性1: value1,
	属性2: value2,
	方法1: function() {}
};
for (var k in obj) {
	console.log(k);				// 得到的是属性名
	console.log(obj[k]);		// 得到的是属性值
}
```

## 内置对象

#### Math

Math不是一个构造器

```
Math.abs();			// 取绝对值，存在隐式转换
// 取整
Math.floor();		// 向下取整
Math.ceil();		// 向上取整
Math.round();		// 四舍五入   .5比较特殊，往大了取整Math.round(-1.5) = -1
// 随机数
Math.random();		//无参数，返回一个浮点型伪随机数字，在[0,1)区间。

function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min; 
}					//得到一个两数之间的随机整数，包括两个数在内含最大值，含最小值 
```

#### Date

```
// Date() 是一个构造函数，需要实例化
var date = new Date();
无参数时date表示当前时间				// Thu Jul 02 2020 15:53:30 GMT+0800 (中国标准时间)
有参数时
常见的参数形式：2020, 10(索引号，实际上是11月), 1         '2020-10-1 6:6:6'		'2020/10/1 6:6:6'

// 日期格式化
var date = new Date();
var year = date.getFullYear();			//返回年份 2020
var month = date.getMonth()+1;			//返回月份0-11，所以通常进行 +1 操作
var dates = date.getDate();				//返回几号
var day = date.getDay();				//返回星期几(0-6)
var arr = ['星期日','星期一','星期二','星期三','星期四','星期五','星期六'];
console.log(year + '年' + month + '月' + dates + '日 ' + arr[day]);   //2020年10月1日 星期四

// 封装时间
function getTimer() {
	var time = new Date();
	var h = time.getHours();       		// 返回 时
	h = h < 10 ? '0' + h : h;			// 补0操作
	var m = time.getMinutes();			// 返回 分
	m = m < 10 ? '0' + m : m;
	var s = time.getSeconds();			// 返回 秒
	s = s < 10 ? '0' + s : s;
	retutn h + ':' + m + ':' + s; 
}

//返回当前时间距离1970.1.1总的毫秒数(时间戳)
var date = new Date();
console.log(date.valueOf());
console.log(date.getTime());
var sum = +new Date();				// +new Date()返回的就是总的毫秒数(可以写参数)
console.log(Date.now());			// H5新增

// 倒计时
// day = parseInt(总的秒数 / 60 / 60 / 24)
// hour = parseInt(总的秒数 / 60 / 60 % 24)
// minute = parseInt(总的秒数 / 60 % 60)
// second = parseInt(总的秒数 % 60)
function countDown(inputTime) {
	var nowTime = +new Date();
	var inputTime = +new Date(inputTime);
	var times = (inputTime - nowTime) / 1000;
	var day = parseInt(times / 60 / 60 / 24);
	day = day < 10 ? '0' + day : day;
	var hour = parseInt(times / 60 / 60 % 24);
	hour = hour < 10 ? '0' + hour : hour;
	var minute = parseInt(times / 60 % 60);
	minute = minute < 10 ? '0' + minute : minute;
	var second = parseInt(times % 60);
	second = second < 10 ? '0' + second : second;
	return day + '天' + hour + '时' + minute + '分' + second + '秒';
}
console.log(countDown('2020-10-1 00:00:00'));   		// 90天06时57分26秒
```

#### Array

Array()  是一个构造函数

```
var arr = new Array(n);					// 表示创建一个长度为n的空数组
var arr = new Array(n1,n2);				// 表示创建的数组为[n1,n2]

// 检测是否为数组
// 1.instanceof 运算符 
var arr = [];
console.log(arr instanceof Array);		// true

// 2.Array.isArray(obj)  ie9以上才支持
var arr = [];
console.log(Array.isArray(arr));		// true

// 添加数组元素
var arr = [1,2,3];
arr.push(n1,n2);				// [1,2,3,n1,n2]，在数组后面添加元素，参数为新增元素，函数返回的是新数组的长度
arr.unshift(n3,n4);				// [n3,n4,1,2,3,n1,n2]，在数组前面添加元素，参数为新增元素，函数返回的是新数组的长度
// 删除数组元素
arr.pop();						// [n3,n4,1,2,3,n1]，删除数组的最后一个元素，无参数，返回的是被删除的元素
arr.shift();					// [n4,1,2,3,n1]，删除数组的第一个元素，无参数，返回的是被删除的元素
arr.splice(begin[,length])		// 从指定索引号删除指定长度的元素

// 数组排序
var arr1 = ['red','green','blue'];
console.log(arr1.reverse());				// 翻转数组，['blue','green','red']

var arr2 = [13,4,77,1,7];
console.log(arr2.sort());    			// 按照转换为的字符串的诸个字符的Unicode位点进行排序排序 [1,13,4,7,77]
arr2.sort(function(a,b) {
	return a - b;
});
console.log(arr2);				// 升序排序 [1,4,7,13,77]
arr2.sort(function(a,b) {
	return b - a;
});
console.log(arr2);				// 降序排序	[77,13,7,4,1]

// 获取数组元素索引
var arr = ['red','green','blue','pink','blue'];
// arr.indexOf(查找的内容,[起始位置])
console.log(arr.indexOf('blue'));		// 2  返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1
console.log(arr.lastIndexOf('blue'));	// 4  返回在数组中可以找到一个给定元素的最后一个索引，如果不存在，则返回-1

// 数组转换为字符串
var arr1 = [1,2,3];
console.log(arr1.toString());			// '1,2,3'
var arr2 = ['red','green','blue'];
console.log(arr2.join('&'));			// 'red&green&blue'，不定义分隔符时默认为逗号
```

#### String

**基本包装类型**

把基本数据类型包装成复杂数据类型，这样基本数据类型就有了属性和方法（String、Number、Boolean）

```
var str = 'andy';
console.log(str.length);  		// 4
// 1.生成临时变量
var temp = new String('andy');
// 2.赋值给我们声明的字符变量
str = temp;
// 3.销毁临时变量
temp = null;
```

**字符串的不可变性**

指的是里面的值不可变，虽然看上去是可以改变内容，但实际上是地址变了，内存中新开辟了一个内存空间。所以应该尽量避免多次为字符串重新赋值或拼接字符串。

```
// 根据字符返回位置(用法和数组的查找索引号基本一致)
var str = '改革春风吹满地';
console.log(str.indexOf('春'));				// 2

// 根据位置返回字符
var str = 'andy';
console.log(str.charAt(3));					// y   charAt(index) 返回对应位置的字符
console.log(str.charCodeAt(0));				// 97  charCodeAt(index) 返回对应位置字符的ASCll码
console.log(str[0]);						// a   str[index]  获取指定位置字符(ie8以上支持)

// 字符串连接 concat(str1,str2...);
var str = 'andy';
console.log(str.concat('red'));				// 'andyred'

// 截取字符串 substring(indexStart[,indexEnd])
var str = '改革春风吹满地';
console.log(str.substring(2,4));				// 春风

// 替换字符 replace('被替换的字符','新字符') 只会替换第一个，被替换的也可以是正则表达式
var str = 'andyandy';
console.log(str.replace('a','b'));			// 'bndyandy'
// 要实现多个替换借助循环
while (str.indexOf('b') !== -1) {
	str = str.replace('a','*');
}
console.log(str);							// '*ndy*ndy'

// 字符串转换为数组 split('分隔符') 分隔符取决于字符串用什么连接
var str = 'red&green&blue';
console.log(str.split('&'));				// ['red','green','blue']
```

## 简单数据类型与复杂数据类型

#### 内存分配

内存分为堆、栈

简单数据类型（值类型）存放在栈里面，存放的是值

复杂数据类型（引用类型）在栈里面存放一个地址(十六进制)，该地址指向堆里面的数据

#### 数据传参

值类型变量传给形参时，把变量在栈空间里面值复制给形参，不影响外部变量

引用类型变量传给形参时，其实是把变量在栈空间里面的堆地址复制给了形参，形参实参操作的是同一个对象，会对外部变量产生影响
