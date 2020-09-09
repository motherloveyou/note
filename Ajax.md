# Ajax

浏览器提供的一套方法，可以实现页面无刷新更新数据

**应用场景**

- 页面上拉加载更多数据
- 列表数据无刷新分页
- 表单项离开焦点数据验证
- 搜索框提示下拉文字列表......

**Ajax需要运行在网站环境才能生效**

# 实现步骤

```
// 创建Ajax对象
var xhr = new XMLHttpRequest();
// 指定请求方式和请求地址
xhr.open('get', 'http://www.example.com');
// 发送请求
xhr.send();
// 获取服务端给出的响应数据
xhr.addEventListener('load', function() {
    console.log(xhr.responseText);
})
```

# 响应内容格式

服务端大多数情况下是以JSON对象作为响应数据的格式

在http请求与响应的过程中，无论是请求参数还是响应内容，如果是对象类型，最终都会转换为对象字符串输出

```
JSON.parse();	// 将JSON字符串转换为JSON对象
```

# 请求参数

在Ajax中，请求参数需要自己拼接

**GET请求方式**

```
xhr.open('get', 'http://localhost:3000/get?name=zhangsan&age=20');
xhr.send();
```

**POST请求方式**

```
// 请求参数放在send方法中
// 必须设置请求参数的格式类型
xhr.open('post', 'http://localhost:3000/post');
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send('name=zhangsan&age=20');
```

**参数格式**

1. application/x-www-form-urlencoded

```
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.send('name=zhangsan&age=20');
```

2. application/json

```
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.send(JSON.stringify({name: 'zhangsan', age: 20}));
```

注意：get请求方式和传统表单提交不能提交json对象数据格式

# 服务端响应

`xhr.readyState`获取Ajax状态码，表示Ajax请求的过程状态

- 0 	创建了Ajax对象
- 1     配置了Ajax对象
- 2     发送了请求
- 3     接收到部分数据
- 4     数据全部接收完成

```
// 相比load事件的优点是兼容IE低版本浏览器；缺点是需要被调用多次，而load只需要调用一次
xhr.onreadystatechange = function() {
	console.log(xhr.readyState);
	if (xhr.readyState == 4 && xhr.status == 200)
		console.log(xhr.responseText);
};
```

# 错误处理

网络畅通，服务器端能接收请求，但没有返回预期的结果

```
// 通过xhr.status获取http状态码，表示请求的处理结果
// 判断服务器返回的状态码，分别进行处理
```

网络畅通，服务器端没有接收到请求，返回404状态码

```
// 检查请求地址是否错误
```

网络畅通，服务器端能接收到请求，返回500状态码

```
// 服务器端错误，甩给后端人员
```

网络中断，请求无法发送到服务器

```
// 会触发xhr对象下面的onerror事件，在onerror事件处理函数中对错误进行处理
```

# Ajax缓存

问题：在低版本IE浏览器中，Ajax请求有严重的缓存问题，即在请求地址不变的情况下，只有第一次请求会发送到服务端，后续的请求会从缓存中获取结果

解决方案：在请求地址后面加上请求参数，保证每次的请求参数不相同

# Ajax封装

```
// 获取响应头中的数据
xhr.getResponseHeader('属性值');
// 对象覆盖
Object.assign(obj1,obj2);	// 使用obj2对象中的属性覆盖obj1对象中的属性；会更改obj1的属性值
```

```
function Ajax(obj) {
	var defaults = {
		type: 'get',
		url: '',
		params: {},
		contentType: 'application/x-www-form-urlencoded',
		success: function() {}
	}
	Object.assign(defaults, obj);
	var xhr = new XMLHttpRequest();
	var params = [];
	for (var attr in defaults.params) {
		params.push(attr + '=' + defaults.params[attr]);
	}
	params = params.join('&');
	if (defaults.type == 'get') {
		defaults.url += '?' + params;
	}
	xhr.open(defaults.type, defaults.url);
	if (defaults.type == 'post') {
		var contentType = defaults.contentType;
		xhr.setRequestHeader('Content-Type', contentType);
		if (contentType == 'application/json') {
			xhr.send(JSON.stringify(defaults.params));
		} else {
			xhr.send(params);
		}
	} else {
		xhr.send();
	}
	xhr.addEventListener('load', function() {
		var responseText = xhr.responseText;
		if (xhr.getResponseHeader('Content-Type').includes('application/json')) {
			responseText = JSON.parse(responseText);
		}
		if (xhr.status == 200)
			defaults.success(responseText);
	})
}
Ajax({
	url: 'http://localhost:3000/first',
	params: {
		name: 'zhangsan',
		age: 20
	},
	success: function(data) {
		console.log(data);
	}
});
```

# 模板引擎

```
// 引入art-template库文件
<script src="/js/template-web.js"></script>
1.模板写在script标签中
// 准备模板
<script id="tpl" type="text/html">

</script>
// 哪一个数据和哪一个模板拼接
var html = template('tpl',{ name: 'zhangsan', age: 20 })  // 模板id，拼接对象
// 将拼接好的html字符串添加到页面
element.innerHTML = html;

2.模板写在js文件中
var tpl = `模板字符串`;
var html = template.render(tpl, { name: 'zhangsan', age: 20 });
element.innerHTML = html;
```

# FormData对象

- 模拟html表单，将html表单映射成表单对象，自动将表单对象的数据拼接成请求参数的格式
- 异步上传二进制文件

```
// 不能用于get请求
var formData = new FormData(form);
xhr.send(formData)
```

**FormData对象的实例方法**

```
// 获取表单对象中属性的值
formData.get('key');

// 设置表单对象中属性的值
formData.set('key','value'); // 若属性不存在将会创建该属性

// 删除表单对象中属性的值
formData.delete('key');

// 向表单对象中追加属性值
formData.append('key','value'); // 与set方法的区别是在属性名称已存在的情况下，append方法会保留两个值
```

**FormData二进制文件上传**

```
file.onchange = function() {
	var formData = new FormData();
	formData.append('attrName',this.files[0]);
	var xhr = new XMLHttpRequest();
	xhr.open('post','http://localhost:3000');
    // 文件上传进度
    xhr.upload.onprogress = function(e) {
    	// e.loaded  e.total
    	var result = (e.loaded / e.total) * 100 + '%';
    	// 再将结果赋给进度条
    }
	xhr.send(formData);
}
```

# 同源政策

协议相同、域名相同、端口相同就属于同源

**Ajax请求限制**

Ajax只能向自己的服务器发送请求，无法向非同源地址发送Ajax请求

## JSONP

```
// 将不同源的服务器端请求地址写在script标签的src属性
<script src="http://eg.com"></script>

// 服务器端响应数据必须是一个函数的调用，要发送给客户端的数据作为函数参数
const data = 'fn({name: 'zhangsan', age: 20})';
res.send(data);

// 在客户端全局作用域下定义函数fn
function fn(data) {
	// 对数据操作
	console.log(data);
}
```

JSONP代码优化

```
// 客户端需要将函数名称传递到服务端
<script src="http://eg.com/better?callback=fn"></script>
app.get('/better',(req,res) => {
	let fnName = req.query.callback;
	const data = fnName + '({name: 'zhangsan', age: 20})';
	res.send(data);
})

// 将script请求的发送变成动态请求
var script = document.createElement('script');
script.src = 'http://eg.com/better?callback=fn';
document.body.appendChild(script);
script.onload = function() {
	document.body.removeChild(script);
}

// 封装jsonp函数，方便发送
jsonp({
	url: 'http://eg.com/better',
	data: {
		name: '张三',
		age: 20
	},
	success: function(data) {
		console.log(data);
	}
})
function jsonp(obj) {
	var script = document.createElement('script');
    var params = '';
    for (var attr in obj.data) {
    	params += ('&' + attr + '=' + obj.data[attr]);
    }
	// 随机生成函数名
	var fnName = 'jsonp' + Math.random().toString().replace('.','');
	// 将参数函数变成全局函数
	window[fnName] = obj.success;
	script.src = obj.url+'?callback=' + fnName + params;
	document.body.appendChild(script);
	script.onload = function() {
    	document.body.removeChild(script);
    }
}

// 服务端代码优化
app.get('/better',(req,res) => {
	// let fnName = req.query.callback;
	// let data = JSON.stringify({name: 'zhangsan', age: 20});
	// const data = fnName + '( + data + )';
	// res.send(data);
	res.jsonp({name: 'zhangsan', age: 20});
})
```

## CORS跨域资源共享

```
app.use((req,res,next) => {
    // 允许哪些客户端访问
    res.header('Access-Control-Allow-Origin', '*');
    // 允许客户端使用哪种请求方式访问
    res.header('Access-Control-Allow-Methods', 'get,post,put,delete,options');
    res.header("Access-Control-Allow-Headers", "X-Requested-With,Content-Type,mytoken");
    next();
})
```

跨域请求发送默认不携带`cookie`信息

```
// 当发送跨域请求时携带cookie信息
xhr.withCredentials = true;
// 允许客户端发送跨域请求时携带cookie信息
res.header('Access-Control-Allow-Credentials', true);
```

## 服务端访问

```
// 客户端向同源服务端发送请求，同源服务端请求非同源服务端的数据
const request = require('request');
app.get('/server',(req,res) => {
	request('http:localhost:3001',(err,response,body) => {
		res.send(body);
	})
})
```

# jQuery的ajax方法

### $.ajax()方法

```
$.ajax({
	type: 'get',
	url: 'http://www.example.com',
	data: { name: '张三', age: 20 } / 'name=张三&age=20',
	// 不要解析data对应的请求参数
	processData: false,
	// 如果为contentType的值为application/json格式，需要使用JSON.stringify将对象转换为字符串
	contentType: 'application/x-www-form-urlencoded',
	// 请求发送之前调用该函数
	beforeSend: function() {
		return false;
	},
	// 发送jsonp请求
	dataType: 'jsonp',
	// 修改callback参数名称
	jsonp: 'cb',
	// 指定函数名称
	jsonCallback: 'fnName',
	success: function(response) {},
	error: function(xhr) {}
})
```

### serialize()方法

作用：将表单中的数据拼接成字符串类型的参数

```
var params = $("#form").serialize(); 	// name=zhangsan&age=20

// 将表单对象数据转换为对象形式
function serializeObject(obj) { // obj是一个jQuery对象
	var result = {};
	var params = obj.serializeArray();
	$.each(obj,function(index,item) {
		result[item.name] = item.value;
	})
	return result;
}											// { name: 'zhangsan', age: 20 }
```

### $.get()、$.post()方法

作用：用于发送get、post请求

```
// 请求参数可以是对象也可以是字符串
$.get/post('http://www.eg.com', {name: 'zhangsan',age: 20}, function(response) {}) 
```

### 全局事件

```
// ajaxStart	ajax请求发送时触发
// ajaxComplete	ajax请求完成时触发
// 事件绑定在document身上
$(document).on('ajaxStart/ajaxComplete', function() {})

// NProgress进度条插件引入
NProgress.start()	// 进度条开始运动
NProgress.done()	// 进度条结束运动
```

# RESTful风格的API

**请求方式规范**

传统表单不支持put和delete方式，而ajax方式支持

- GET	获取数据
- POST    添加数据
- PUT    更新数据
- DELETE    删除数据

```
app.get('/users/:id', (req,res) => {	// /:id主要是用来占位的
	const id = req.params.id;
})
```

# XML基础

可扩展标记语言，作用是传输和存储数据

```
// 设置返回的数据类型
res.header('content-type', 'text/xml');

// 当服务端返回xml数据时，需要用xhr.responseXML拿到返回数据
var xmlDocument = xhr.responseXML;
var title = xmlDocument.getElementsByTagName('title')[0].innerHTML;
```

