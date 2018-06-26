---
title: JavaScript技巧
date: 2018-1-16 17:16:47
tags:
- js
---
### new 操作符
```javascript
    function A(){}
    newFunc(A){
        var instance = {}
        instance.__proto__=A.prototype
        instance.__proto__.constructor=A
        return instance
    }
```
### instanceof
其实现类似于，instanceof运算符是根据实例的指向的其构造函数的原型对象层层往上找。
```javascript
function instance_of(L, R) {//L 表示左表达式，R 表示右表达式
 var O = R.prototype;// 取 R 的显示原型
 L = L.__proto__;// 取 L 的隐式原型
 while (true) { 
   if (L === null) 
     return false; 
   if (O === L)// 这里重点：当 O 严格等于 L 时，返回 true 
     return true; 
   L = L.__proto__; 
 } 
}
```
### Object.create(proto, [propertiesObject])
方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 
其内部实现类似于
```javascript
    function create(o){
        let f = function(){}
        f.prototype = o
        return new f();
    }
```
### 原型链继承
```javascript
    function A(name){
        this.name = name || 'A'
        console.log(this.name+':I am a constructor')
    }
    A.prototype.echoName = function(){
        console.log('My name is '+this.name)
    }
    // AA 原型链继承A
    function AA(name){
        this.name = name
        console.log(this.name+':I am a constructor')
    }
    AA.prototype = new A()
    AA.prototype.echoChildName = function(){
        console.log('My name is '+this.name)
    }

    var aa  = new AA('原型链继承');
    aa.echoChildName()
    console.log(aa instanceof AA) //true
    console.log(aa instanceof A) //false
```
### 组合继承
```javascript
    function A(name){
        this.name = name || 'A'
        console.log(this.name+':I am a constructor')
    }
    A.prototype.echoName = function(){
        console.log('My name is '+this.name)
    }
    // AA 原型链继承A
    function AA(name){
        A.apply(this,arguments)
        console.log(this.name+':I am a constructor')
    }
    AA.prototype = new A();
    AA.prototype.echoChildName = function(){
        console.log('My name is '+this.name)
    }
    AA.prototype.constructor = AA  //把构造函数替换回来
    var aa  = new AA('fffff');
    aa.echoChildName()
    console.log(aa instanceof AA)
    console.log(aa instanceof A)
```
### 寄生组合继承
```javascript
    function A(name){
        this.name = name || 'A'
        console.log(this.name+':I am a constructor')
    }
    A.prototype.echoName = function(){
        console.log('My name is '+this.name)
    }
    // AA 原型链继承A
    function AA(name){
        A.apply(this,arguments)
        console.log(this.name+':I am a constructor')
    }
    var f = function(){}
    f.prototype = A.prototype
    AA.prototype = new f()
    AA.prototype.constructor = AA  //把构造函数替换回来
    AA.prototype.echoChildName = function(){
        console.log('My name is '+this.name)
    }

    var aa  = new AA('fffff');
    aa.echoChildName()
    console.log(aa instanceof AA)
    console.log(aa instanceof A)
```
### 基于Object.create()方法继承
```javascript
    function A(name){
        this.name = name || 'A'
        console.log(this.name+':I am a constructor')
    }
    A.prototype.echoName = function(){
        console.log('My name is '+this.name)
    }
    function AA(name){
        A.apply(this,arguments)
        console.log(this.name+':I am a constructor')
    }
    // Object.create start
    var f = function(){} 
    f.prototype = A.prototype
    AA.prototype = new f() 
    // Object.create end
    // AA.prototype = Object.create(A.prototype) 在es5可以使用Object.create代替
    AA.prototype.constructor = AA  //把构造函数替换回来
    AA.prototype.echoChildName = function(){
        console.log('My name is '+this.name)
    }

```
### 原型链图解
https://www.cnblogs.com/libin-1/p/5820550.html
```javascript
    Object.__proto__ == Object.prototype //(原型对象)
    Object.constructor == Function  //原因是，Object本身就是一个函数，函数的构造函数都是Function
    Object.prototype.__proto__ == null //Object构造函数的原型对象，它的构造函数函数的原型对象为null，因此，Object.prototype.__proto__为原型链的顶端
    Object.prototype.constructor == Object //正常情况下,函数的prototype(原型对象)的构造函数为其本身
    Function.prototype.constructor == Function //正常情况下,函数的prototype(原型对象)的构造函数为其本身
    Number.prototype.constructor == Number //正常情况下,函数的prototype(原型对象)的构造函数为其本身

```

