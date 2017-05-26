---
title: lazy man 面试题
date: 2017-05-25 10:21:44
tags:
- js
- 面试题
---
这是一套比较常见的js面试题，题目如下：
>实现一个LazyMan，可以按照以下方式调用:
LazyMan(“Hank”)输出:
Hi! This is Hank!

>LazyMan(“Hank”).sleep(10).eat(“dinner”)输出
Hi! This is Hank!
//等待10秒..
Wake up after 10
Eat dinner~

>LazyMan(“Hank”).eat(“dinner”).eat(“supper”)输出
Hi This is Hank!
Eat dinner~
Eat supper~

>LazyMan(“Hank”).sleepFirst(5).eat(“supper”)输出
//等待5秒
Wake up after 5
Hi This is Hank!
Eat supper

>以此类推。  

### 题目分析
- 根据函数调用行为可以看出，要实现一个能链式调用的函数
- LazyMan(“Hank”).sleep(10).eat(“dinner”)  eat方法需要放在sleep的回调里
- LazyMan(“Hank”).eat(“dinner”).eat(“supper”) 正常调用
- LazyMan(“Hank”).sleepFirst(5).eat(“supper”) 所有方法得放在sleepFirst 的回调里  

第一次看到这个题目，按照普通的思维去看，得到的信息如上，毫无规律可循。
事实上，他们更像一种异步任务的行为，每调用一个方法可以先用一个队列存起来，然后依次把队列里面的任务执行了，
但是同时需要考虑异步的情况
### 相关知识点
1. 链式调用
2. 异步任务(主要)
3. setTimeout
4. 代码封装  

### 链式调用实现
在每个方法返回this就好了
### 代码封装
使用原型封装，这个没啥好说的
### 异步任务(主要)
当遇到异步的情况，我们直接把队列的函数拿出来调用显然是不可行的。
怎么办呢？此时，我们在当先的任务显示的调用下一个任务，形成一个链。
当然，我们使用promise也是可以的。
### 具体代码如下
``` js
function _LazyMan(name){
  this.name = name;
  this.tasks=[];
  var _this = this;
  //每一次调用，都会调用addTask，放进this.tasks
  this.addTask(function(){
    console.log('Hi i am '+ _this.name);
    _this.next()
  })
  // 这里非常巧妙，利用了setTimeout这个函数来做任务的第一个调用。
  // 具体原理，JavaScript在浏览器运行时是单线程的，在执行setTimeout方法的时候，其实是把setTimeout放在事件队列里面。并没有开线程。
  // setTimeout(function(){}, 0);就是说当前代码会按顺序执行，执行完了会立即调用tasks的第一个任务
  setTimeout(function(){
        _this.next();
    }, 0);
}
//添加到队列的第一个位置去
_LazyMan.prototype.unshiftTask=function(task){
  this.tasks.unshift(task)
}
  //添加任务，其实就是往tasks里面添加一个回调
_LazyMan.prototype.addTask=function(task){
  this.tasks.push(task)
}
//eat方法，链式调用需要返回this
_LazyMan.prototype.eat=function(){
  var _this = this;
  this.addTask(function(){
    console.log('eat'+this.name)
    _this.next();
  })
  return this;
}
//sleep方法，链式调用需要返回this
_LazyMan.prototype.sleep=function(time){
  var _this =this;
  this.addTask(function(){
    setTimeout(function(){
      console.log('sleep '+_this.name)
      _this.next();
    },time)
  })
  return this;
}
//next方法，取出队列中第一个任务并且执行，涉及到异步的情况，我们需要在队列里面的每个callback里面都主动调用this.next()
//确保调用完所有的任务
_LazyMan.prototype.next = function(){
  var task=this.tasks.shift();
  task && task();
}
//其实这个任务就是需要放在队列的第一位
_LazyMan.prototype.sleepFirst=function(time){
  var _this =this;
  this.unshiftTask(function(){
    setTimeout(function(){
      console.log('sleepFirst'+_this.name)
      _this.next()
    },time)
  })
  return this;
}
function LazyMan(name){
  var man = new _LazyMan(name)
  return man
}
LazyMan('ing60').sleepFirst(1000).eat().sleep(2000).eat();
```
### 总结
虽然，我们在大多数实际开发中没有这样的需求。但是还是挺有意思的，可以让我们学到很多，我们平时忽略的东西
