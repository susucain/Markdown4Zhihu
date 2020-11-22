# TypeScript语言

## 类型安全

- 强类型与弱类型

- 强类型：语言层面限制函数的实参类型必须与形参类型相同

- 弱类型语言层面不会限制实参的类型

- 强类型有更强的类型约束，而弱类型中几乎没有什么约束，强类型中不允许任意的隐式类型转换，弱类型就允许

## 类型系统

### 静态类型与动态类型

### 静态类型：一个变量声明时，它的类型就是明确的，声明过后，它的类型就不允许再修改

### 动态类型：运行阶段才能明确变量类型，变量的类型随时可以改变

### 动态类型语言中的变量没有类型，变量中存放的值是有类型的

## JavaScript 自有类型系统特征

### 弱类型 且 动态类型

### 缺失了类型系统的可靠性

### 早前的JavaScript应用简单

## Flow 静态类型检查方案

### 使用方式

- 文件头部加上// @flow
- 在参数后面使用 : number
- 编辑器关闭javascript的语法校验：javascript validate
- 安装：yarn add flow-bin
- 初始化：yarn flow init
- 使用：yarn flow

### 移除方式

- 1. flow-remove-types

	- 安装: yarn add flow-remove-types
	- 使用：yarn flow-remove-types src -d dist

- 2. babel

	- 安装：@babel/cli  @babel/core  @babel/preset-flow
	- 配置：.babelrc

		- {
  "presets": ["@babel/preset-flow"]
}

	- 使用：yarn babel src -d dist

### flow功能

- 类型推断

	- 根据运算符来推断

- 类型注解

	- 参数类型
	- 变量类型
	- 函数返回类型，没有返回值就是void

### flow原始类型

- const a: string = 'foobar'
const b: number = Infinity // NaN // 100
const c: boolean = false // true
const d: null = null
const e: void = undefined
const f: symbol = Symbol()

### flow数组类型

- 全是数字组成的数组

	- const arr1: Array<number> = [1, 2, 3]
const arr2: number[] = [1, 2, 3]

- 元组 - 固定长度的数组

	-  const foo: [string, number] = ['foo', 100]

###  flow对象类型

- const obj1: { foo: string, bar: number } = { foo: 'string', bar: 100 }
const obj2: { foo?: string, bar: number } = { bar: 100 }
const obj3: { [string]: string } = {}
obj3.key1 = 'value1'
obj3.key2 = 'value2'

### flow函数类型

- function foo (callback: (string, number) => void) {
  callback('string', 100)
}
foo(function (str, n) {
  // str => string
  // n => number
})

### 特殊类型

- 字面量类型

	- const a: 'foo' = 'foo'

const type: 'success' | 'warning' | 'danger' = 'success'

- 声明类型

	- type StringOrNumber = string | number

const b: StringOrNumber = 'string' // 100

- Maybe 类型 

	- const gender: ?number = undefined
// 相当于
// const gender: number | null | void = undefined

- 所有类型的联合类型

	- Mixed类型

		- 强类型
		- function passMixed (value: mixed) {
  if (typeof value === 'string') {
    value.substr(1)
  }

  if (typeof value === 'number') {
    value * value
  }
}

passMixed('string')

passMixed(100)

	- Any 类型
	
		- 弱类型
		- function passAny (value: any) {
  value.substr(1)

  value * value
}

passAny('string')

passAny(100)

### flow运行环境API

- https://github.com/facebook/flow/blob/master/lib/core.js
- https://github.com/facebook/flow/blob/master/lib/dom.js
- https://github.com/facebook/flow/blob/master/lib/bom.js
- https://github.com/facebook/flow/blob/master/lib/cssom.js
- https://github.com/facebook/flow/blob/master/lib/node.js

## TypeScript 语言规范与基本应用

### 基本介绍

- Javascript的超集，Javascript 和 es6+ 和 类型系统
-  任何一个JavaScript运行环境都支持
- 功能更为强大，生态也更健全、更完善
- Angular / Vue3.0
- 前端领域中的第二语言
- 缺点

	- 语言本身需要了解更多内容

		- 渐进式

	- 项目初期，TypeScript会增加一些成本

### 用法

- 自动生成js文件
- 安装：yarn add typescript --dev
- 编译ts文件：yarn tsc src/01-getting-started.ts 
- 初始化: yarn tsc --init
- 使用：yarn tsc
- 使用中文的报错信息

	- 控制台 yarn tsc --locale zh-CN
	- 编辑器 typescript locale

### 类型

- 原始类型

	- string/number/boolean/void/null/undefined

- object类型

	- 除了原始类型以外的其它类型
	- const foo: object = function () {} // [] // {}
	- const obj: { foo: number, bar: string } = { foo: 123, bar: 'string' }
	- 如果需要明确限制对象类型，则应该使用这种类型对象字面量的语法，或者是「接口」

- Array类型

	- const arr1: Array<number> = [1, 2, 3]
	- const arr2: number[] = [1, 2, 3]

- 元组（Tuple）类型

	- 字面量的类型
	- const tuple: [number, string] = [18, 'zce']
	- const [age, name] = tuple

- 枚举（Enum）类型

	- // 标准的数字枚举
enum PostStatus {
  Draft = 0,
  Unpublished = 1,
  Published = 2
}
	- // 数字枚举，枚举值自动基于前一个值自增
enum PostStatus {
  Draft = 6,   
  Unpublished, // => 7
  Published // => 8
}
	- 枚举会侵入编译结果，常量枚举不会侵入编译结果

- 函数类型
- 任意类型

	- :any
	- 不会有任何的类型检查，所以轻易不要尝试

### 标准库

- 标准库就是内置文件的标准库

	- 修改tsconfig.json中的lib
	- "lib": ["ES2015", "DOM", "ES2017"]

### 作用域问题

- 默认文件中的成员会作为全局成员，多个文件中有相同成员就会出现冲突
- 解决办法1：IIFE 提供独立作用域
- 解决办法2：在当前文件使用 export，也就是把当前文件变成一个模块

### 隐式类型推断

- 如果无法推断的话，就是任意类型

### 类型断言

- res as number
- <number>res // JSX 下不能使用

### 接口

- interface Post {
  title: string
  content: string
}

function printPost (post: Post) {
  console.log(post.title)
  console.log(post.content)
}
- 约束对象的结构
- 可选成员

	- subtitle?: string// 相当于string和undefined

- 只读成员

	- readonly

- 动态成员

	- [key: string]: string

### 类

- 描述一类具体事物的抽象特征
- ES6 以前，函数+原型 模拟实现类
-  ES6 开始 JavaScript 中有了专门的class
-  TypeScript 增强了 class 的相关语法

	- 类的属性在使用之前必须要在类型中去声明，为了给属性加标注
	- 私有属性 private 表示只能在方法内部去使用
	- 共有属性 public 默认
	- 受保护的 protected 只允许子类访问成员

- 只读属性

	- readonly

- 类与接口

	- 一个接口只有一个能力，用类去组合
	- interface /  implements

- 抽象类

	- abstract
	- 只能被继承，不能被实例化new
	- 抽象方法

		- 不需要方法体
		- 但当父类中有抽象方法时，子类需要去实现它。
		- 可以使用代码修正的功能

### 泛型

- 在声明时不指定类型，在使用过程中使用类型
- <T>

### 类型声明

- 引入的模块没有类型声明

	- 安装类型声明模块
	- declare

*XMind - Trial Version*