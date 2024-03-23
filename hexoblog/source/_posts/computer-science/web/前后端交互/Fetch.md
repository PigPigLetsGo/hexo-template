---
title: Fetch
categories:
   - [计算机学科,web,前后端交互]
tags:
   - 计算机学科
   - web
   - 前后端交互
---

# Fetch

## Fetch是什么

Fetch也是前后端通信的一种方式。

Fetch是Ajax(XMLHttpRequest)的一种替代方案，它是基于Promise的。

Ajax的兼容性比Fetch好。

Fetch中目前还没有abort，timeout，这些想要使用还要我们去实现。

## Fetch的基本用法

```js
console.log(fetch)
```

效果：在js中是有fetch这个东西的

![image-20230531201344579](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040734678.png)

1. ```
   Response {type: 'cors', url: 'http://127.0.0.1:81/data', redirected: false, status: 200, ok: true, …}
   body: ReadableStream
   bodyUsed: false
   headers: Headers {}
   ok: true
   redirected: false
   status: 200
   statusText: ""
   type: "cors"
   url: "http://127.0.0.1:81/data"
   [[Prototype]]: Response
   ```

2. body/bodyUsed：只能读一次，读过之后就不让再读了。

3. OK：如果OK为true，表示可以读取数据，不用再去判断HTTP状态码了。

```js
let url = 'http://127.0.0.1:81/data'
fetch(url).then(response=>{
   console.log(response)
   if(response.ok){
      console.log(response.json())
   }
}).catch(err=>{
   console.log(err)
})
```

效果：返回了一个Promise对象，我们需要将他返回出去才能看到数据

![image-20230531202444465](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040735239.png)

```js
let url = 'http://127.0.0.1:81/data'
fetch(url).then(response=>{
   console.log(response)
   if(response.ok){
      console.log(response.json())
      return response.json()
   }
}).then(data=>{
   console.log(data)
}).catch(err=>{
   console.log(err)
})
```

报错了，翻译一下：

![image-20230531202627455](./images/image-20230531202627455.png)

```
类型错误：无法在“响应”上执行“json”：正文流已读取
取.html：16：29
```

原因是读取了两次，上面body明确不可以读取两次的只能读取一次

将返回值上面的console.log读取数据的给注释掉

```js
let url = 'http://127.0.0.1:81/data'
fetch(url).then(response=>{
   console.log(response)
   if(response.ok){
      // console.log(response.json())
      return response.json()
   }
}).then(data=>{
   console.log(data)
}).catch(err=>{
   console.log(err)
})
```

数据就读取出来了。

![image-20230531202830539](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040735394.png)

如果数据不是JSON格式的则使用response.text()来返回文本格式的数据：

```js
let url = 'http://127.0.0.1:81/data'
fetch(url).then(response=>{
   console.log(response)
   if(response.ok){
      // console.log(response.json())
      // return response.json()
      return response.text()
   }
}).then(data=>{
   console.log(data)
}).catch(err=>{
   console.log(err)
})
```

效果：

![image-20230531203055670](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040735213.png)

## Fetch第二个参数是对象，用来配置Fetch。

```js
let url = 'http://127.0.0.1:81/json'
fetch(url,{
   method:'post',
   headers:{
      'Content-Type':'application/json'
   },
   // boey发送请求体数据，发送json使用body直接传会报错
   // 直接传递对应的类型为x-www-form-urlencoded
   // body:null,
   // body:'name=张三&age=18',
   // 不能直接传对象需要自己转
   body:JSON.stringify({name:'张三',age:18}),
   // 跨域资源共享,默认就是cors
   mode:'cors',
   // 跨域请求是否携带Cooki,传入的不再是布尔值
   credentials:'include'
}).then(response=>{
   if(response.ok){
      return response.json()
   }
}).then(data=>{
   console.log(data)
}).catch(err=>{
   console.log(err)
})
```

效果：

![image-20230531205447013](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040735416.png)