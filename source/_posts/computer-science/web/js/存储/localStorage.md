---
title: 初始localStorage
categories:
   - [计算机学科,web,js,存储]
tags:
   - 计算机学科
   - web
   - js
   - 存储
---

# 初始localStorage

## localStorage是什么

localStorage也是一种浏览器存储数据的方式(本地存储)，它只是存储在本地，不会发送到服务器端。

单个域名下的localStorage总大小有限制。

## 在浏览器中操作localStorage

### localStorage的基本用法

```js
console.log(localStorage)
```

![image-20230529160720332](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040740380.png)

### 向localStorage中添加数据

```js
localStorage.setItem('username','zhangsan')
localStorage.setItem('username','lisk')
localStorage.setItem('age',18)
localStorage.setItem('sex','male')
```

![image-20230529160746545](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040741101.png)

### 获取localStorage中的数据

```js
console.log(localStorage.getItem('username'))
console.log(localStorage.getItem('age'))
// 获取不存在的返回null
console.log(localStorage.getItem('name'))
```

![image-20230529161039653](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040741897.png)

删除localStorage中的数据

```js
localStorage.removeItem('username')
localStorage.removeItem('age')
console.log(localStorage)
```

![image-20230529161250485](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040741739.png)

清空localStorage中所有数据

```js
localStorage.clear()
```

![image-20230529161408488](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040741469.png)



## 使用localStorage实现自动填充

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
      <form id="login" action="#" method="post">
         <input type="text" name="username"/><br>
         <input type="password" name="password"/><br>
         <button id="nth" type="submit">提交</button>
      </form> 
      <script>
         // 通过id获取form标签
         let loginForm = document.getElementById('login')
         // 通过id获取按钮标签
         let nth = document.getElementById('nth')
         // 从localStorage中获取username的值
         const username = localStorage.getItem('username')
         // 判断是否为null
         if(username){
            // 不为null则赋值给username里
            loginForm.username.value = username
         }
         // 绑定点击事件
         nth.addEventListener('click',e => {
            // 阻止默认的点击事件执行
            e.preventDefault()
            // 将获取到username中的value添加到localStorage中
            localStorage.setItem('username',loginForm.username.value)
            // 执行提交事件
            loginForm.submit()
         })
      </script>
   </body>
</html>
```

![image-20230529164617853](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040753445.png)

![image-20230529164606522](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040753926.png)

## localStorage的注意事项

### localStorage的存储期限

localStorage是持久化的==本地存储==，除非==手动清除==(比如==通过js删除==，或者==清除浏览器缓存==)，否则数据是==永远不会过期的==。

>  sessionStorage
>
>  当会话结束(比如关闭浏览器)的时候，sessionStorage中的数据会被清空

```js
sessionStorage.setItem('username','zhangsan')
sessionStorage.getItem('username')
sessionStorage.removeItem('username')
sessionStorage.clear()
```

### localStorage键和值的类型

localStorage存储的键和值<font color=red>**只能是字符串类型**</font>.

不是字符串类型，也会先转化为字符串类型再存进去

### 不同域名下能否共用localStorage

不同的域名是不能共用localStorage的

### localStorage的兼容性

查询网站：https://caniuse.com/

![image-20230529170617884](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040744887.png)