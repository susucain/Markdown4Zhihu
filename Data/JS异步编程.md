# JS异步编程

## 同步模式与异步模式

### 同步模式

- 代码中的任务依次执行，排队执行

### 异步模式

- 不会等待这个任务的结束才开始下一个任务
- 开启之后就立即往后执行下一个任务，后续逻辑一般会通过回调函数的方式定义
- 代码的执行顺序混乱

### 运行环境提供的API是以同步或异步模式的方式工作

## 事件循环与消息队列

## 异步编程的几种方式

### 回调函数是所有异步编程方案的根基

## Promise异步方案、宏任务/微任务队列

### Promise

- pending

	- Fulfilled

		- onFulfilled

	- Rejected

		- onRejected

### 宏任务/微任务

- 宏任务：回调队列中的任务
- setTimeOut:宏任务
- 宏任务执行过程中可以临时加上一些额外的需求，可以选择作为一个新的宏任务进到队列中排队，也可以作为当前任务的微任务
- 微任务：直接在当前任务结束过后立即执行
- Promise：微任务

## Generator 异步方案、Async/Await语法糖

