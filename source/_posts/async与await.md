---
title: async与await
date: 2017-09-29 13:55:03
tags:
- js
- es7
---
#### async await简单记录
- 1.`async`和`await`分别是generator`*`与`yield`的替换，但是`async`函数执行的时候和普通函数执行一样，而generator需要一个函数执行器
- 2.`async`返回的是一个promise
- 3.`await` 异步的情况下，`await`返回的结果是promise 返回的值举个例子
```js
async function foo(){
    let bar = await new Promise(resolve=>{
        setTimeout(()=>resolve('666'),1000)
    })
    console.log(bar)
}
```
- 4.`async`函数内部return语句返回的值，会成为then方法回调函数的参数。
```js
async function func() {
  return '666';
}

func().then(v => console.log(v))
```
- 5.`await`命令后面的Promise对象，运行结果可能是rejected，最好把`await`命令放在try...catch代码块中。
- 6.`await`命令只能用在`async`函数之中，如果用在普通函数，就会报错。

- [详细信息](http://es6.ruanyifeng.com/#docs/async)