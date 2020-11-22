# ECMAScript新特性

## 概述

### 1. 解决原有语法上的一些问题或者不足

### 2. 对原有语法进行增强

### 3. 全新的对象、全新的方法、全新的功能

### 4. 全新的数据类型和数据结构

## ES6新关键词

### let与块级作用域

### const

- 不用var，主用const，配合let

### 数组的解构

- const [foo, bar, baz] = [1, 2, 3]
- ...的方式只能在最后一个位置出现

### 对象的解构

- const { name: objName } = { name: 'zce', age: 18 }
- 重新命名：const { name: objName = 'jack' } = obj

### 模版字符串

- ``
- 直接换行
- ${}

### 标签函数的模板字符串

### 字符串的扩展方法

- startsWith
- endWith
- includes

### 参数默认值

- 带有默认值的参数在最后

### 剩余参数/展开数组

- ...args

	- 取代arguments

- 放在最后，而且只能出现一次
- ...将数组展开传参

### 箭头函数

- 简化函数的定义
- 箭头函数不会改变this的指向

### 对象字面量的增强

- 如果变量名和属性值一致的话，那就可以省略冒号
- 属性值如果是function的话，可以省略冒号和function
- 计算属性名

### 对象扩展方法

- Object.assgin

	- 将多个源对象中的属性复制到一个目标对象中
	- 用后面对象的属性去覆盖第一个对象
	- 复制一个新的对象

- Object.is

	- Object.is(NaN, NaN) --> true
	- Object.is(+0, -0) --> false

### Proxy

- Object.defineProperty

	- 监视对象属性的读写

-  new Proxy , get/set

	- 不只是监视对象属性
	- 支持数组对象的属性
	- 是以非侵入的方式监视对象的读写

### Reflect

- 内部封装了一系列对对象的底层操作
- 统一提供一套用于操作对象的API

### Promise

- 一种更优的异步编程解决方案
- 解决了传统异步编程中回调函数嵌套过深的问题

### class类

- 代替
  
```javascript
  Person.prototype.say = function () {
    console.log(`hi, my name is ${this.name}`)
  }
  ```
  
- ```javascript
  class Person {
    constructor (name) {
      this.name = name
  }
  
    say () {
      console.log(`hi, my name is ${this.name}`)
  }
  }
  ```

### static

- ```javascript
    static create (name) {
    return new Person(name)
  }
  ```

### extends

### Set数据结构

- new Set()
- has() 是否包含
- delete() 删除集合中某个值
- clear() 清除集合中所有元素
- 成员是独一无二的

	- 作用：去重

- 得到数组Array.from(new Set(arr))//[...new Set(arr)]

### Map数据结构

- 严格意义上的键值对集合
- 用任意对象的数据作为键
- new Map()
- set/get

###  Symbol

- 一种全新的原始数据类型
- 表示一个独一无二的值

	- 最主要的作用就是为对象添加独一无二的属性名
	- 每次都是全新的，如果想要相同的Symbol，使用Symol.for('string')

- Symbol() === Symbol() --> false
- 7种数据类型，未来可能会有一个 BigInt的数据类型
