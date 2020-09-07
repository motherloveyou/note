# Node

**Node**是一个基于Chrome V8引擎的JavaScript**代码运行环境**

Node.js由**ECMAScript**和**Node环境**提供的一些**附加API**组成

### 模块化开发

js的弊端：文件依赖和命名冲突

**规范**

- Node.js规定一个js文件就是一个模块，模块**内部定义的变量和函数**在默认情况下在外部无法得到
- 模块内部使用`exports`或`module.exports`对象进行成员导出，使用`require`方法导入其他模块

- `exports`是`module.exports`的别名（地址引用关系），导出对象最终以`module.exports`为准

### 系统模块

Node运行环境提供的API

**fs  文件操作**

```
// 读取文件内容
fs.readFile('文件路径'[,'文件编码'],callback);	   
(err,doc) => {	// callback错误优先的回调函数
	if (err == null) {
		console.log(doc);	// err为错误对象，doc为文件内容
	}
}

// 写入文件内容
fs.writeFile('文件路径','数据',callback);	// 如果没有该文件，则创建
```

```
// 一般情况下我们都使用绝对路径，相对路径有时候是相对命令行工具的当前目录，如readFile
// 在读取文件或者设置文件路径时都会选择绝对路径，require方法使用相对路径
__dirname		// 获取当前文件所在的绝对路径
```

**path  路径操作**

```
// 路径拼接
// 不同操作系统的路径分隔符不统一 windows /、\    linux /
path.join('路径','路径',...)
```

### 第三方模块

`npm`(node package manager)node的第三方模块管理工具

通常由多个文件组成，又称之为**包**

- 以js文件形式存在，提供实现项目具体功能的API接口
- 以命令行工具形式存在，辅助项目开发

```
npm install 模块名称
npm uninstall 模块名称

全局安装与本地安装
	命令行工具：全局安装 -g
	库文件：本地安装
```

**nodemon**     命令行工具

执行文件时使用`nodemon 文件名`

监控文件的保存操作，重新执行该文件

**nrm**     命令行工具

npm下载地址切换工具

```
nrm ls		// 查询可用下载地址列表
nrm use
```



### Gulp（库文件）

基于node平台开发的前端构建工具

将机械化操作编写成任务，执行一个命令行命令就能自动执行操作

- 项目上线，HTML、CSS、JS文件压缩合并
- 语法转换(es6  less)
- 公共文件抽离
- 修改文件浏览器自动刷新

**使用步骤**

1. 下载gulp库文件
2. 项目根目录下建立gulpfile.js文件
3. 重构项目的文件夹结构    src目录放置源代码文件    dist目录放置构建后文件
4. 在gulpfile.js文件中编写任务
5. 在命令行工具中执行任务

```
gulp.src()		// 获取任务要处理的文件，选中多个时传数组
gulp.dest()		// 输出文件（复制）
gulp.task()		// 建立gulp任务
gulp.watch()	// 监控文件的变化

function 任务名() {};		// 创建任务
exports.任务名 = 任务名;		// 导出任务（公开任务，未被导出则为私有任务）
exports.任务 = series/parallel(任务1,任务2...);	// 使用series和parallel将多个独立的任务组合为一个更大的操作
```

```
npm install gulp-cli -g		// gulp命令行工具
gulp 任务名	// 执行任务
```

**gulp插件**

下载+引用+调用

- gulp-htmlmin        html文件压缩
- gulp-csso        压缩css 
- gulp-babel        javascript语法转化
- gulp-less        less语法转化
- gulp-uglify        压缩混淆javascript
- gulp-file-include        公共文件包含
- browersync        浏览器实时同步

### package.json文件

```
由于node_modules文件夹过于琐碎，项目转发时不传递该文件夹
package.json文件记录了当前项目信息，使用npm init -y 命令生成
再使用npm install安装项目所需的所有模块

package.json文件的script选项存放命令的别名
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "nodemon"
}

使用npm下载开发依赖时加上--save-dev将信息存放在devDependencies
项目依赖（开发和运行）存放在package.json文件里面的dependencies		npm install --production
开发依赖（开发）存放在package.json文件里面的devDependencies
```

**package-lock.json文件**记录模块之间的依赖关系

- 锁定包的版本，确保再次下载不会因为版本不同而产生问题
- 加快下载速度，文件记录了项目依赖第三方包的树状结构和下载地址

### 模块加载机制

**当模块拥有路径但没有后缀**

使用`require`引用模块如果省略了模块后缀，先在当前文件夹下查找同名JS文件，找不到则查找同名文件夹下面的`index.js`，否则查看package.json文件的main选项的入口文件；如果入口文件不存在或不指定入口文件则报错

**当模块没有路径且没有后缀**

首先假设是系统模块，否则去node_modules文件夹下查找（按拥有路径但没有后缀的方式查找）

