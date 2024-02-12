---
title: Cookie
categories:
   - [计算机学科,web,前后端交互]
tags:
   - 计算机学科
   - web
   - 前后端交互
---

# 初始Cookie

## Cookie是什么

Cookie全称HTTP Cookie ，简称 Cookie，是浏览器存储数据的一种方式，因为存储在用户本地，而不是存储在服务器上，是本地存储。一般会自动随着浏览器每次请求发给送到服务器端。

## Cookie有什么用

利用Cookie跟踪统计用户访问该网站的习惯，比如什么时间访问，访问了哪些页面，在每个网页的停留时间等。

## 在浏览器中操作Cookie

先看下页面的请求头信息，里面并没有Cookie

![image-20230529100840334](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040755678.png)

打开控制台找到Cookies在里面设置一个Cookie键值对

![image-20230529100621798](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756713.png)

设置完成后点击NetWork，刷新下页面就会向服务器端发送一次Cookie请求

![image-20230529100930844](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756361.png)

控制台使用js代码来查看Cookie，获取多个Cookie会以分号分隔。

![image-20230529101120541](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756912.png)

Cookie是明文显示的，不要使用Cookie存储敏感信息。

## Cookie的基本用法

### 写入Cookie

```js
//1.写入Cookie
document.cookie = 'username=刘桑'
document.cookie = 'age=18'
//不能一起设置,只能一个一个设置
// document.cookie = 'username=刘桑; age=18'
```

**效果**：

![image-20230529102501653](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756121.png)

### 读取Cookie

```js
//2.读取Cookie
console.log(document.cookie)
// 读取的是一个由键值对构成的字符串,每个键值对之间由 "; " (一个分号和一个空格)隔开
```

**效果**：

![image-20230529102516584](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756735.png)

## Cookie的属性

### 1.Cookie的键(name)和值(value)

最重要的两个属性，创建Cookie时必须填写，其它属性可以使用默认值

>  Cookie的键值如果包含非英文字母，则写入时需要使用encodeURIComponent()编码，读取时使用decodeURIComponent()解码。

```js
document.cookie = `username=${encodeURIComponent('张三')}`
document.cookie = `${encodeURIComponent('用户名')}=${encodeURIComponent('李四')}`
```

<font style="color:red">一般键使用英文字母,不要用中文,值可以用中文,但是要编码</font>。

### 2.失效(到期)时间

对于失效的Cookie，会被浏览器清除

默认会话Cookie

![image-20230529105442704](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756419.png)

>  如果没有设置失效(到期)时间，这样的Cookie称为会话Cookie，它存在内存中，当会话结束，也就是浏览器关闭时(不是关闭当前页面)，Cookie消失

想让Cookie长时间存在，设置Expires或Max-Age

```js
//expires
//值为Date类型
document.cookie = `username1=dkx; expires=${new Date(
   '2100-1-01 00:00:00'
)}`
//max-age
//值 为数字,表示当前时间+多少秒后过期,单位时秒
document.cookie='username=dkx1; max-age=5'
//    设置Cookie存活时间为30天
document.cookie=`username=dkx1; max-age=${24*3600*30}`
```

**效果**：

![image-20230529105522647](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756190.png)

如果max-age的值是0或负数，则Cookie会被删除

```js
// 删除Cookie
document.cookie=`username=dkx1; max-age=0`
document.cookie=`username=dkx1; max-age=-1`
```

**效果**：

![image-20230529105726676](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756761.png)

### 3.Domain域

Domain限定了访问Cookie的范围

使用JS只能读写当前域或父域的Cookie，无法读写其它域的Cookie

```js
document.cookie = 'username=dkx; domain=www.taobao.com'
```

www.taobao.com  main.m.taobao.com 当前域

父域：.taobao.com

也就是需要在它们共同存在的域下设置的Cookie才能访问到

**操作**：

PC端淘宝：www.taobao.com

![image-20230529111416183](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040756721.png)

查看Cookie

![image-20230529111454087](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040757921.png)

查看：手机端的淘宝 main.m.taobao.com

![image-20230529111528150](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040757707.png)

查看Cookie

![image-20230529111537366](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040757623.png)

### 4.Path路径

Path限定了访问Cookie的范围(同一个域名下)

使用JS只能读写当前路径和上级路径的Cookie，无法读写下级路径的Cookie

```js
document.cookie='username=dkx; path=\路径'
```

<font style="color:red">当Name，Domain，Path这3个字段都相同的时候，才是同一个Cookie</font>。

![image-20230529112550487](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040757848.png)

### 5.HttpOnly

设置了HttpOnly属性的Cookie不能通过js去访问

>  前端不能通过js去设置一个HttpOnly类型的Cookie，这种类型的Cookie只能是后端来设置
>
>  只要是HttpOnly类型的，通过document.cookie是获取不到的，也不能进行修改

### 6.Secure安全标志

Secure限定了只有在使用了https而不是http的情况下才可以发送给服务端

<font style="color:red">Domain，Path，Secure 都要满足条件，还不能过期的Cookie才能随着请求发送到服务器端</font>。

## Cookie的封装

### 封装Cookie

创建html页面在script标签中导入封装的js文件和要使用的方法

```html
<!DOCTYPE html>
<html lang="en">
   <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
   </head>
   <body>
      <script type="module">
         //导入js文件和对应要使用的方法
         import {set,get,remove} from './js/cookie.js'
         //设置cookie
         set('username','zhangsan')
         set('user','张三')
         set('age',18)
         set('sex','male',{
            maxAge: 30 * 24 * 3600
         })
         //删除cookie
         remove('age')
         //获取cookie
         console.log(get('age'))
         console.log(get('username'))
      </script>
   </body>
</html>
```

创建js文件

```js
//写入Cookie
//  可选赋值参数           默认值undefined设置为空
const set = (name,value,{maxAge,domain,path,secure}={})=>{
   let cookieText = `${encodeURIComponent(name)}=${encodeURIComponent(value)}`
   if(typeof maxAge === 'number'){
      cookieText += `; max-age=${maxAge}`
   }
   if(domain){
      cookieText += `; domain=${domain}`
   }
   if(path){
      cookieText += `; path=${path}`
   }
   if(secure){
      cookieText += `; secure`
   }
   document.cookie = cookieText
}
// 通过name获取cookie值
const get = name =>{
   name = `${encodeURIComponent(name)}`
   const cookies = document.cookie.split('; ')
   for(let itme of cookies){
      let [cookieName,cookieValue] = itme.split('=')
      if(cookieName == name){
         return cookieValue
      }
   }
   return
}
// 根据name,domain,path删除cookie
const remove = (name,{domain,path}={})=>set(name,'',{domain,path,maxAge:-1})
//导出方法
export {set,get,remove}
```

**效果**：

![image-20230529152224212](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040757775.png)

![image-20230529152234954](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040757698.png)

## Cookie的注意事项

### 前后端都可以写入和获取Cookie

### Cookie有数量 限制

每个域名下的Cookie数量有限

当超过单个域名限制之后，再设置Cookie，浏览器就会清除以前设置的Cookie

### Cookie有大小 限制

每个Cookie的存储容量很小，最多只有4KB左右