---
title: 初始JSON
categories:
   - [计算机学科,web,前后端交互]
tags:
   - 计算机学科
   - web
   - 前后端交互
   - 数据格式
---

# 初始JSON

## 	JSON是什么

Ajax发送和接收数据的一种格式

JSON全称是 JavaScript Object Notation

## 为什么需要JSON

JSON有3种形式，每种形式的写法都和JS中的数据类型很像，可以很轻松的和JS中的数据类型互相转换。

## JSON的3中形式

### 简单值形式

JSON的简单值形式就对应着JS中的基础数据类型

数字，字符串，布尔值，null

```json
"username"
```

注意事项：

1.  JSON中没有undefined值
2.  JSON中的字符串必须使用双引号
3.  JSON中是不能注释的

### 对象形式

JSON的对象形式就对应着JS中的对象

```json
{
   "name":"张三",
   "gender":"男",
   "age":18,
   "user":{
   	"name":"李四",
   	"gender":"女",
   	"age":"20"
	},
}
```

注意事项：

JSON中对象的属性名必须用双引号，属性值如果是字符串也必须用双引号。

JSON中只要涉及到字符串，就必须使用双引号。

不支持undefined

### 数组形式

JSON的数组形式就对应着JS中的数组

```json
[
   "张三",
   "李四",
   "刘桑"
]
```

```json
[
   {
      "name":"张三",
      "age":18
   },
   {
      "name":"李四",
      "age":20
   }
]
```

注意事项：

数组中的字符串必须使用双引号

JSON中只要涉及到字符串，就必须使用双引号

不支持undefined

## JSON的常用方法

### JSON.parse

JSON.parse可以将JSON格式的字符串解析成JS中的对应值

一定要是合法的JSON字符串，否则会报错

```js
console.log(JSON.parse(xhr.responseText))
```

### JSON.stringify

JSON.stringify可以将JS的基本数据类型，对象或者数组转换成JSON

```js
xhr.send(JSON.stringify(
   {
      name:"zhangsn",
      age:"18",
      hobby:["zuqi","bingbqiu"],
      userTo:{
         family:"sl",
         mother:"lidama"
      }
   }
))
```

### 使用JSON.parse和JSON.stringify封装localStorage

```js
import {set,get} from './js/storage.js'
set('username','dkx')
set('age',18)
set('zs',{
   name:"张三",
   age:18
})
console.log(get('username'))
console.log(get('age'))
console.log(get('zs'))
```

![image-20230530164849606](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040740061.png)

![image-20230530164902425](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040740654.png)

```js
import {set,get,remove,clear} from './js/storage.js'
set('username','dkx')
set('age',18)
set('zs',{
   name:"张三",
   age:18
})
remove('username')
console.log(get('username'))
console.log(get('age'))
console.log(get('zs'))
```

![image-20230530165013596](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040740615.png)

![image-20230530165025302](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040740239.png)

```js
import {set,get,remove,clear} from './js/storage.js'
set('username','dkx')
set('age',18)
set('zs',{
   name:"张三",
   age:18
})
remove('username')
clear()
console.log(get('username'))
console.log(get('age'))
console.log(get('zs'))
```

![image-20230530165051597](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040740337.png)

![image-20230530165100768](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040740675.png)