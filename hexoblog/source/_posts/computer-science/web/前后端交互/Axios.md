---
title: Axios
categories:
   - [计算机学科,web,前后端交互]
tags:
   - 计算机学科
   - web
   - 前后端交互
---

# Axios

## Axios是什么

Axios是一个给予Peromise的HTTP库，可以用在浏览器和node.js中

第三方Ajax库

Axios中文官方文档：http://www.axios-js.com/

或者电脑安装了node.js则可以使用node.js来进行下载axios.js到本地，这样可以提升访问的速度了。

**步骤**：

1 执行命令：下载axios到本地

```sh
npm install axios
```

执行后下载到的地方一般在用户文件夹中的node_modules中

![image-20230813171704981](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131717320.png)

2 也可以直接在项目中打开终端执行命令下载可以直接使用。然后将下载的axios文件夹 剪切到项目中

>  注意：不要直接剪切走 node_modules文件夹，这是在项目中执行命令生成的。

![image-20230813171832066](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131719218.png)

3 引入axios到html页面中

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
   //引入axios
   <script type="text/javaScript" src="./node_modules/axios/dist/axios.min.js"></script> 
</head>
<body>
   <script>
    //打印一下axios函数 看看是否引入成功了。
    console.log(axios)
   </script> 
</body>
</html>
```

**结果**： OK ! 可以使用了

![image-20230813172101445](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131721605.png)

## Axios的基本用法

引入Axios第三方库

```js
<script type="text/javascript" src="https://unpkg.com/axios@1.4.0/dist/axios.min.js"></script>
```

查看是否引入成功

```js
<script>
   console.log(axios)
</script> 
```

![image-20230531191826664](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040732260.png)

使用Axios发送请求：

使用结构：

url：请求地址

method：直接使用axios需要使用method指定请求方式如：post，get

headers：指定请求头信息

params：请求头携带的数据

data：请求体携带的数据

params和data可以同时设置参数不会报错

timeout：设置请求超时时间

withCredentials：跨域请求是否携带Cookie

Axios的超时报错效果：

![image-20230531195700272](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040732887.png)

```js
const url = 'http://127.0.0.1:81/json'
axios(url,{
   method:'post',
   // 请求时的头信息
   heaers:{
      'Content-Type':'application/json'
      // 'Content-Type':'application/x-www-form-urlencoded'
   },
   // 通过请求头携带的数据
   params:{
      name:'张三'
   },
   // 通过请求体携带的数据,传递JSON数据
   data:{
      name:'李四',
      age:18
   },
   //传递application/x-www-form-urlencoded数据
   // data:'name=张三李四&age=18'
   // 设置超时时间，超出时间后报错
   timeout:10,
   // 跨域是否携带Cookie
   withCredentials:true
}).then(response=>{
   console.log(response)
   console.log(response.data)
   console.log(response.data.data)
}).catch(err=>{
   console.log(err)
})
```

效果：

![image-20230531194731633](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040732256.png)

使用Axios调用get，post请求方式：

可以不用写headers

==GET==

```js
const url1 = 'http://127.0.0.1:81/json-get'
axios.get(url1,{
   params:{
      name:'张三',
      age:18
   },
   timeout:10,
   withCredentials:true
}).then(response=>{
   console.log(response)
}).catch(err=>{
   console.log(err)
})
```

效果：

![image-20230531195732003](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040732190.png)

==POST==

还是同样的：'name=张三&age=18' 对应数据格式为：application/x-www-form-urlencoded

而 {name:'张三',age:18} 对应数据格式为：appliction/json，如果后端需要json而传递了x-www-form-urlencoded则会报错415

```js
const url = 'http://127.0.0.1:81/json'
axios.post(url,'name=张三&age=18'/*{ name:'张三',age:18 }*/)
   .then(response=>{
   console.log(response)
}).catch(err=>{
   console.log(err)
})
```

## Content-Type与data-Type的区别

content-Type：告诉服务器，我要发什么类型的数据。

data-Type：告诉服务器，我想要什么类型的数据，如果没有指定，那么会自动推断是返回XML，还是JSON，还是script，还是String。

## axios请求拦截器

axios 请求拦截器：发起请求之前，触发的配置函数，对<font title=red>请求参数</font>进行额外配置

```js
//做统一设置
axios.interceptors.request.use(function(config) {
   //在发送请求之前做些什么
   //统一携带token 令牌字符串在请求头上 
   const token = localStorage.getItem('token')
   //逻辑中断判断，如果有token则不进行设置，如果没有则进行设置token
   token && config.haeads.Authorization = `Bearer ${toke}`
   //在发送请求之前做些什么
   return config
},function(error) {
   //对请求错误做些什么
   return Promise.reject(error)
})
```

当我们在请求个人信息，所有频道，文章列表时都会先经过请求拦截器，然后访问服务器查看token是否合法，合法则返回数据到 axios 请求中 得到个人信息，所有频道数据，文章列表数据等。

![image-20230813212511388](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151310161.png)

>  总结：
>
>  1.  什么是axios请求拦截器？
>      -  发起请求之前，调用一个<font title=red>函数</font>，对<font title=red>请求参数</font>进行<font title=red>设置</font>.
>  2.  axios请求拦截器，什么时候使用？
>      -  有<font title=red>公共配置</font>和设置时，==统一==设置在请求拦截器中

![image-20230813215516420](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308132155054.png)

## axios()与axios.get()的区别

<span alt='solid'>拿一个接口来做对比演示</span>：

后端接口

```java
@ApiOperation("添加角色接口")
@PostMapping("save")
//  @RequestBody：不能使用get方式提交,传递JSON格式数据,把json格式数据封装到对象里面
public Result saveRole(@RequestBody SysRole sys) {
   return sysRoleService.save(sys) ? Result.ok() : Result.fail();
}
```

##### 一，使用axios()进行发送请求

```js
// 添加
async saveRole(sys) {
   // roleName: sys.roleName, roleCode: sys.roleCode
   return await request({
      // 接口路径
      url: `${url}/save`,
      // 请求方式
      method: 'post',
      // 请求参数，传递JSON格式
      data: sys
   })
}
```

结果：

![image-20230906142119290](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309061421525.png)

##### 二，使用axios.post()进行发送请求

```js
// 添加
async saveRole(sys) {
   // roleName: sys.roleName, roleCode: sys.roleCode
   // 正确写法可以传递到后端接口值
   return await request.post(`${url}/save`, { roleName: sys.roleName, roleCode: sys.roleCode })
   // 错误写法不能传递到后端接口值
   // request.post(`${url}/save`, { sys: sys })
}
```

结果：

![image-20230906142507359](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309061425789.png)

如果按照错误写法呢？ 结果如下：

```js
// 添加
async saveRole(sys) {
   // roleName: sys.roleName, roleCode: sys.roleCode
   // 正确写法可以传递到后端接口值
   return await /* request.post(`${url}/save`, { roleName: sys.roleName, roleCode: sys.roleCode }) */
   // 错误写法不能传递到后端接口值
   request.post(`${url}/save`, { sys: sys })
}
```

结果：结果就是接收到了两个没有传递过来的值null了。

![image-20230906142656845](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309061426213.png)