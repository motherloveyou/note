# Express框架

基于`Node`平台的web应用开发框架			`npm install express`

```
const express = require('express');
const app = express();
app.get('/',(req,res) => {
	// send()方法响应
	// 自动检测响应内容的类型;自动设置http状态码;自动设置响应的内容类型及编码
	res.send('ok');
});
app.listen(3000);
```

### 中间件

中间件就是一堆方法，可以接收请求、对请求做出回应，也可以将请求交给下一个中间件处理

中间件主要由两部分组成，中间件方法以及请求处理函数

```
app.get('/',(req,res,next) => {
	req.name = 'aa';
	next(); // 将请求的控制权交给下一个中间件
});
app.get('/',(req,res) => {
	res.send(req.name);
});

// use()接收所有请求
app.use(['/',](req,res,next) => { // 以/开头而不是精确匹配
	
})

// 错误处理中间件 比普通的中间件多了err参数
app.get('/',(req,res,next) => {
	throw new Error('程序发生未知错误');
});
// 只能自动捕获同步api错误，异步api需要手动调用next()方法
app.get('/',(req,res,next) => {
	fs.readFile('/no-exist.txt',(err,result) => {
		if (err != null) {
			next(err);	// 加参数代表触发错误处理中间件
		} else {
			res.send(result);		
		}
	})
});
// 对于异步函数的错误，使用try catch语句(也可以捕获同步错误，但不能捕获回调函数及promise对象错误)
const promisify = require('util').promisify;
const readFile = promisify(fs.readFile);
app.get('/',async (req,res,next) => {
	try {
		// 可能发生错误的异步函数语句
		await readFile('./no-exist.txt');
	} catch(error) {
		next(error);	// 手动触发错误处理中间件
	}
});
// err是错误对象
app.use((err,req,res,next) => {  
	res.status(500).send(err.message); // 设置Http状态码
})
```

### 模块化路由

```
// 创建路由对象
const home = express.Router();
// 将路由和请求路径进行匹配
app.use('/home',home);
// 创建二级路由
home.get('/index',(req,res) => {
	res.send();
})
```

### 请求参数获取

```
// 获取GET请求参数(对象形式)
req.query 

// 获取POST请求参数
// 引入body-parser第三方模块
const bodyParser = require('body-parser');
// 配置body-parser模块
// extended: false表示使用querystring模块处理请求参数格式
// extended: true表示使用第三方模块qs处理请求参数格式
app.use(bodyParser.urlencoded({ extended: false }));
// 拿到数据对象
req.body
```

### 路由参数

```
app.get('/index/:id/:age',(req,res) => {
	res.send(req.params);
});

localhost:3000/index/123/20  // {"id":"123","age":"20"}
```

### 静态资源处理

```
app.use(express.static(path.join(__dirname,'public')));

// 访问资源
http://localhost:3000/css/base.css
http://localhost:3000/img/logo.png
```

### 模板引擎

```
// 引入模块
npm install art-template express-art-template

// 当渲染后缀为art的模板时，使用express-art-template
app.engine('art',require('express-art-template'));
// 设置模板存放目录
app.set('views',path.join(__dirname,'views'));
// 设置默认后缀
app.set('view engine','art');
app.get('/index',(req,res) => {
	// 渲染模板
	res.render('index',{});
})

// app.locals对象
app.locals.user = ...	// 在所有模板都可以获取user数据
```

### 密码加密

```
// 依赖的环境
python 2.x
node-gyp   					npm install -g node-gyp
windows-build-tools    		npm install --global --production windows-build-tools

npm install bcrypt

const bcrypt = require('bcrypt');
// 生成随机字符串
let salt =await bcrypt.genSalt(10);		// 数字参数越大，生成的随机字符串越复杂
// 使用随机字符串对明文加密
let pass = await bcrypt.hash('明文',salt);

// 比对密码
// 1 获取随机字符串
// 2 加密，并添加随机字符串到密文
// 3 对比
let isEqual = await bcrypt.compare('明文','密文');	// true or false
```

### cookie与session

**cookie**: 浏览器在电脑硬盘中开辟的一块空间，主要用于服务端存储数据

- cookie中的数据是以域名的形式进行区分的
- cookie中的数据是有过期时间的
- cookie中的数据会随着请求自动发送到服务器端

**session**: 实际上就是一个对象，存储在服务器端的内存中，在session的数据都有一个sessionid作为唯一标识

```
借助第三方模块npm install express-session
```

### formidable

```
表单默认情况下enctype属性的值为application/x-www-form-urlencoded
上传文件时需要设置enctype属性为multipart/form-data
```

formidable模块作用：解析表单，支持get请求参数、post请求参数、文件上传

```
const formidable = require('formidable');
// 创建表单解析对象
const form = new formidable.IncomingForm();
// 设置文件上传路径
form.uploadDir = '';
// 是否保留文件扩展名
form.keepExtensions = false;
// 解析表单
form.parse(req,(err,fields,files) => { // 参数都是对象类型
	// fields 存储普通请求参数
	// files 存储上传的文件信息
})
```

### 文件读取操作

```
file.onchange= function() {
	var reader = new FileReader();
	reader.readAsDataURL(this.file[0]);
	reader.onload = function() {
		console.log(reader.result);
	}
}
```

