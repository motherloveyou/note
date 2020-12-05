# TypeScript

`TypeScript`是`JavaScript`的超集(js有的ts都有)

`npm i -g typescript`：typescript提供了tsc命令实现ts ---> js的转化

`npm i -g ts-node`：内部执行ts ---> js ，提供`ts-node`命令

## 类型注解

```
// 数值
let age: number = 18; 
// 字符串
let name: string = "bob";
// 布尔值
let isDone: boolean = false;
// 数组
let arr: string[] = ['a','b','c']
let arr: Array(string) = ['a','b','c']
// 元组Tuple
let x: [string,number]
// 枚举
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
// Any
let list: any[] = [1, true, "free"];
// void
function warnUser(): void {
    console.log("This is my warning message");
}
```

- ts里面的运算符没有默认类型转换
- 内容为数字的字符串前面加上`+`可以转换为`number`类型的数据

## 类型断言

“尖括号”语法

```
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

`as`语法

```
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

## 接口

```typescript
interface Person {
	name: string,
	age: number
}
let info: Person = { name: 'zs',age: 18}
```

## 类

```
class Greeter {
	// ts要求先定义成员，不能动态添加
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}
```

### 继承和修饰符

```
// public成员可以类外访问
// private私有成员不能在类外访问，且不能继承
// protected成员不能在类外访问，但可以被继承
// readonly关键字将属性设置为只读的，不允许修改 只读属性必须在声明时或构造函数里被初始化
class son extends Greeter {
	// 修饰符默认为public
	public age: number;
	constructor(mes: string,age: number) {
		// 在构造函数里访问this的属性之前，我们 一定要调用super()
		super(mes)
		this.age = age;
	}
}
```

### 参数属性

```
class Person {
	// 把声明和赋值合并至一处
	constructor(public name: string) {}
}
```

### 存取器

```
class Person {
	private _age: number;
	get age() {
		return this._age
	}
	set age(val) {
		if(val < 0) {
			throw new Error('年龄不能为负数')
		}
		this._age = val
	}
}
let p1 = new Person()
p1.age = -10
```

### 静态成员

```
// 静态成员只能通过类本身访问 Person.age
class Person {
	static age: number
}
```

## 函数

### 可选参数

```
// 可选参数必须跟在必须参数后面
function fn(x: number,y?: string) {}
```

## 默认参数

```
function fn(x: number,y: string = 'aa') {}
```

