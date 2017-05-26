---
title: 统计字符串(面试题)
date: 2017-05-25 17:37:26
tags:
- js
- 面试
---
第一种方法
``` js
var str = "nininihaoa1231231wqasdasdfadsfasdfdsafas";
var result = {};
var arrResult=[]//存放统计结果
str.split('').forEach(function(it){
  var isExist=arrResult.find(function(c){
    return c.char == it;
  })
  if(isExist){
    isExist.count ++
  }else{
    arrResult.push({char:it,count:1})
  }
})
var res=arrResult.reduce(function(i,next){
  if(i.count>next.count){
    return i
  }else{
    return next
  }
})
console.log(res)
```
第二种方法
``` js

var str = "nininihaoa1231231wqasdasdfadsfasdfdsafas";
var result = {};
str.split('').forEach(function(it){
  if(result[it]){
    result[it]++
  }else {
    result[it]=1
  }
})
var max=0
var res={}
for (var key in result){
  if(result[key]>max){
    max=result[key]
    res.char = key;
    res.count = max;
  }
}
console.log(res)
```
第三种方法 递归与正则并用
``` js
var s = "nininihaoa1231231wqasdasdfadsfasdfdsafas";
var max = 0;
var res = {};
function getResult(str){
  if(!str) return
  var data={char:str[0],count:str.match(new RegExp(str[0],'ig')).length}
  if(data.count>max){
    max = data.count;
    res = data;
  }
  getResult(str.replace(new RegExp(str[0],'ig'),''))
}
getResult(s)
console.log(res)
```
