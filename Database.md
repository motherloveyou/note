# 数据库

| 术语       | 解释说明                                    |
| :--------- | :------------------------------------------ |
| database   | 数据库，mongoDB数据可软件可以创建多个数据库 |
| collection | 集合，一组数据的集合                        |
| document   | 文档，一条具体的数据                        |
| field      | 字段，文档中的属性名称                      |

### 连接数据库

```
// node.js连接mongodb数据库
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/demo', { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log('数据库连接成功'))
    .catch(err => console.log(err, '数据库连接失败'))
```

### 创建集合

- 对集合设定规则
- 创建集合

```
// 设定集合规则
const courseSchema = new mongoose.Schema({
	name: String,
	author: String,
	isPublished: Boolean
});
// 创建集合并应用规则
// mongoose.model返回一个构造函数
// 第1个参数是集合名称，需要大写，数据库集合名称实际为courses
const Course = mongoose.model('Course',courseSchema); 
```

### 创建文档

- 创建集合实例
- 调用实例的save方法保存数据

```
// 方法1
let course = new Course({
    name: 'node.js',
    author: 'zcy',
    isPublished: true
});
course.save();
// 方法2
Course.create({},(err,result) => {
	console.log(err);
	console.log(result);
});          // 返回的是promise对象
Course.create({})
	.then(result => console.log(result))
	.catch(err => console.log(err));
```

### 导入数据

```
mongoimport -d 数据库名称 -c 集合名称 --file 导入的文件
```

### 查询文档

```
// find()方法查询所有文档，返回一个数组
User.find({// 条件对象}).then(result => console.log(result));
// findOne()方法返回一条文档，一个对象
User.findOne({// 条件对象}).then(result => console.log(result));

// 匹配大于小于
User.find({age: {$gt: 20,$lt: 50}}).then(result => console.log(result));

// 匹配包含
User.find({hobbies: {$in: ['打豆豆']}}).then(result => console.log(result));

// 选择查询字段
// 查询name age，不查询 _id
User.find().select('name age -_id').then(result => console.log(result));

// 根据字段升序排序
User.find().sort('age').then(result => console.log(result));
// 根据字段降序排序
User.find().sort('-age').then(result => console.log(result));

// skip跳过多少条数据，limit限制查询数量		
// 常用来做数据分页
User.find().skip(2).limit(2).then(result => console.log(result));

// 查询数据总量
User.conutDocuments({}).then(result => console.log(result));
```

### 删除文档

```
// 删除单个
// 返回删除的文档
User.findOneAndDelete({查询条件}).then(result => console.log(result));

// 删除多个
// 返回一个对象 { n = 4, ok = 1 }表示删除了四个文档
User.deleteMany({查询条件}).then(result => console.log(result));
```

### 更新文档

```
// 更新单个
// 返回一个对象 { n: 1, nModified: 1, ok: 1 }
User.updateOne({ name: '李四' }, { name: '李狗蛋' }).then(result => console.log(result));

// 更新多个
// 返回一个对象 { n: 5, nModified: 4, ok: 1 }
User.updateMany({}, { name: '李狗蛋' }).then(result => console.log(result));
```

### 验证

```
// 数据类型
type: String
// 必选字段
required: true/[true,'自定义错误'],
// 字符串最小长度
minlength: n
// 字符串最大长度
maxlength: n
// 去除字符串两边空格
trim: true
// 数值最小值
min: n
// 数值最大值
max: n
// 默认值
default: value
// 枚举 列举出当前字段可以拥有的值
enum: {
	values: [],		// 枚举值
	message: ''    // 错误信息
}

// 自定义规则
validate: {
	validator: v => {}, 	// 返回一个布尔值；v是要验证的值
	message: ''		// 自定义错误信息
}

// 获取错误信息
text.create({ title: '哒', author: 'zcy' })
    .then(result => console.log(result))
    .catch(error => {
    	// 获取错误信息对象
        const err = error.errors;
        for (let k in err) {
            console.log(err[k]['message']);
        }
    });
```

### 集合关联

- 使用`id`对集合进行关联
- 使用`populate()`方法进行关联集合查询

```
const textSchema = new mongoose.Schema({
	author: {
    	type: mongoose.Schema.Types.ObjectId,  // id固定类型
    	ref: 'User'		// 关联的集合
	}
});
const User = mongoose.model('User', userSchema);
const Text = mongoose.model('Text', textSchema);

text.find().populate('author')
    .then(result => console.log(result))
```

### joi

javascript对象的规则描述语言和验证器

```
const Joi = require('joi');
const schema = {
	name: Joi.string().min(2).max(10).required().error(new Error('自定义错误')),
	password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/),
	role: Joi.string().valid('normal','admin')
}
Joi.validate({name: '张三'},schema);	// 返回一个promise对象
```

### 数据加密

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

### 数据分页模块

```
const pagination = require('mongoose-sex-page');
// page指定当前页;	size指定每页显示的数据条数;	display指定客户端要显示的页码数量
pagination(集合构造函数).page(1).size(20).diplay(8).exec();
```

### mongodb数据库账号

```
mongo
show dbs
use admin
db.createUser({user: 'root',pwd: 'root',roles: ['root']})  // 超级用户
use blog
db.createUser({user: 'blog',pwd: 'blog',roles: ['readWrite']})	// 普通用户
exit

net stop mongodb
mongod --remove
mongod --logpath='D:\mongodb\Server\4.1\log\mongod.log' --dbpath='D:\mongodb\Server\4.1\data' --install --auth
net start mongodb

// 连接数据库时
mongoose.connect('mongodb://blog:blog@localhost/blog')
```

# 模板引擎

多用于字符串拼接

```
const template = require('art-template'); // 返回一个方法
// template(模板路径,数据对象) 返回拼接好的字符串
const view = path.join(__dirname, 'views', 'add.art');
const html = template(view, {
    name: 'zhangsan',
    age: 20
});
```

### 输出

- 标准语法：`{{数据}}`
- 原始语法： `<%= 数据 %>`

### 原文输出

默认情况下，模板引擎不解析数据里面的html标签，会将其转义后输出

- 标准语法：`{{@ 数据}}`
- 原始语法： `<%- 数据 %>`

### 条件判断

```
// 标准语法
{{if 条件}} ... {{else if 条件}} ... {{else}} ... {{/if}}

// 原始语法
<% 
if (条件) {%> 
	...	
<%} else if (条件) {%> 
	...
<%}
%>
```

### 循环

```
// 标准语法
{{each 数据}} 
	{{$index}}	// 数据索引
	{{$value}}	// 数据的值
{{/each}}

// 原始语法
<% 
for (var i = 0; i < 数据.length; i++) {%> 
	<%= i%>			 // 数据索引
    <%= 数据[i] %>	// 数据的值
<%} 
%>
```

### 子模板

使用子模板可以将公共区域抽离到单独的文件中

```
// 将抽离部分引回文件
{{include '模板路径'}}
<% include('模板路径') %>
```

### 模板继承

将HTML骨架抽离到单独文件中，其他页面模板可以继承骨架文件

```
{{extend '模板路径'}}
{{block 'name'}} ... {{/block}}	// 填充自定义内容
```

### 模板配置

```
// 向模板中导入变量
template.defaults.imports.变量名 = 变量值;	// 变量值可以是一些方法或第三方模块
// 设置模板根目录
template.defaults.root = 路径
// 设置模板默认后缀
template.defaults.extname = '.art'
```

# 环境配置

```
设置NODE_ENV环境变量，值为development或者production
process.env.NODE_ENV获取环境变量的值
```

### config模块

- 根目录下新建`config`文件夹
- `config`文件夹下面新建`default.json development.json production.json`文件
- 导入`config`模块
- 通过模块内部提供的`get`方法获取配置信息
- `config`文件夹新建`custom-environment-variables.json`文件，将敏感信息写在系统环境变量中，json文件中对应的属性值为环境变量名

