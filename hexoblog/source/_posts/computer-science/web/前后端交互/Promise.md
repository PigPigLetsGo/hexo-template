---
title: Promise
categories:
   - [计算机学科,web,前后端交互]
tags:
   - 计算机学科
   - web
   - 前后端交互
---

## Promise

### 基本使用与概述

**定义**：

Promise对象用于表示一个==异步操作==的==最终完成== (或==失败==) 及其==结果值==。

>  <span alt=solid>好处</span>：
>
>  1.  逻辑更清晰
>  2.  了解axios 函数内部运作机制
>  3.  能解决回调函数地狱问题
>
>  ![image-20230813094654122](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308130946670.png)

```js
//1创建Promise对象
const p = new Promise((resolve,reject) => {
   //2.执行异步任务—并传递结果
   //成功调用：resolve (值) 触发 then()执行
   //失败调用：reject (值) 触发 catch()执行
})
//3.接收结果
p.then(result => {
   //成功
}).catch(error => {
   //失败
})
```

>  <span alt=solid>总结</span>：
>
>  1.  什么是Promise?
>      -  表示(管理)一个<font title=red>异步</font>操作<font title=red>最终状态</font>和<font title=red>结果值</font>的对象
>  2.  为什么学习Promise?
>      -  成功和失败状态，可以关联对应处理程序
>      -  了解 axios 内部原理
>  3.  Promise使用步骤？
>      1.  创建Promise对象 `new Promise((成功回调函数，失败回调函数)=> {})` 
>      2.  执行异步任务—并传递结果
>          1.  成功调用：成功回调函数(值) 触发then()执行
>          2.  失败调用：失败回调函数(值) 触发catch()执行
>      3.  接收结果
>          1.  成功：then(回调函数 => {})
>          2.  失败：catch(回调函数 => {})

### Promise—三种状态

**作用**：理解Promise对象如何 <font title=red>关联</font> 的<font title=red>处理函数</font>，以及代码执行顺序



![image-20230813100906484](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131009808.png)

待定(pending)：初始状态，既没有被兑现，也没有拒绝

已兑现(fulfilled)：意味着，操作成功完成

已拒绝(rejected)：意味着，操作失败

<blockquote alt=danger>
	<div>
      <p>
         <span><font title=red>注意</font>：</span>
      </p>
      <p>
         <span>Promise对象一旦被<font title=red>兑现/拒绝</font>就是<font title=red>已敲定了，状态无法再被改变</font></span>
      </p>
   </div>
</blockquote>

```js
//目标：使用Promise管理异步任务
//1.创建Promise对象
const p = new Promise((resolve, reject) => {
   // 在下面的定时器任务还没执行 成功或失败的回调函数之前展开console
   // Promise处于pending状态 - 待定状态
   //2.执行异步代码
   setTimeout(() => {
      // resolve() => fulfilled状态 - 已兑现状态 执行 = then()
      resolve('模拟AJAX请求—成功结果')
      // reject() => rejected状态 - 已拒绝状态 执行 = catch()
      reject(new Error('模拟AJAX请求—失败结果'))
   }, 2000)
})
// 程序执行到这里Promise的状态就已经敲定了不可改变了
// 上面同时执行成功和失败最终是？ fulfilled 因为不可变
console.log(p)
// 3.获取结果
p.then(result => {
   console.log(result)
}).catch(error => {
   console.log(error)
})
```

**效果**：

![image-20230813102333221](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131023902.png)

>  <span alt=solid>总结</span>：
>
>  1.  Promise 对象有哪3中<font title=red>状态</font>？
>      -  待定：pending
>      -  已兑现：fulfilled
>      -  已拒绝：rejected
>  2.  Promise 状态有什么用？
>      -  状态改变后，调用关联的处理函数

### Promise+XHR获取省份列表

```js
<body>
    <p class="my-p"></p>
    <script>
        /*
        目标：使用Promise管理XHR请求省份列表
        1.创建Promise对象
        2.执行XHR异步代码，获取省份列表
        3.关联成功或失败函数，做后续处理
        */
        // 1.创建Promise对象
        const p = new Promise((resolve, reject) => {
            // 2.执行XHR代码，获取省份列表
            const xhr = new XMLHttpRequest()
            xhr.open('GET', 'http://hmajax.itheima.net/api/province')
           //发送请求
            xhr.send()
            xhr.addEventListener('loadend', function () {
                // 3.xhr如何判断响应成功还是失败的?
                // 2xx开头都是成功响应状态码
                if (xhr.status >= 200 && xhr.status < 300) {
                    resolve(JSON.parse(xhr.response))
                } else {
                    reject(new Error(xhr.response))
                }
                // console.log(xhr.response)
            })
        })
        // 3.关联成功或失败函数，做后续处理
        p.then(result => {
            console.log(result)
            // 将成功获取的数据添加到p标签中展示
            document.querySelector('.my-p').innerHTML = result.list.join('</br>')
        })
        p.catch(error => {
            // 错误对象要用dir详细打印
            console.dir(error)
            // 服务器返回错误提示信息，插入到p标签展示
            document.querySelector('.my-p').innerHTML = error.message
        })
    </script>
</body>
```

**效果**：

![image-20230813110046300](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131100796.png)

如果要发送一个JSON数据而且后端接收数据警告了：text/plain;charset=UTF-8此时需要设置下请求头的数据格式了：

写在open与send之间

```js
xhl.setRequestHeader('Content-Type', 'application/json')
```

### 封装_简易axios _ 获取省份列表

**需求**：基于==Promise== + ==XHR== 封装 myAxios 函数，获取省份列表展示

**步骤**：

1.  定义myAxios 函数，接收 <font title=red>配置对象</font>，返回<font title=red>Promise对象</font>.

    -  基本结构

    ```js
    function myAxios(config) {
       return new Promise((resolve,reject) => {
          //XHR请求
          //调用 成功/失败 的处理程序
       })
    }
    myAxios({
       url: '目标资源地址'
    }).then(result => {
       
    }).catch(error => {
       
    })
    ```

2.  发起<font title=red>XHR请求，默认请求方法</font> 为 GET

3.  调用 成功 / 失败 的处理程序

4.  使用 myAxios 函数，获取省份列表展示

```js
<body>
    <p class="my-p"></p>
    <script>
        /*
        目标：封装_简易axios函数_获取省份列表
        1.定义myAxios函数，接收配置对象，返回Promise对象
        2.发起XHR请求，默认请求方法为GET
        3.调用 成功/失败 的处理程序
        4.使用 myAxios 函数，获取省份列表展示
        */
        //    1.定义myAxios函数，接收配置对象，返回Promise对象
        function myAxios(config) {
            return new Promise((resolve, reject) => {
                // 2.发起XHR请求，默认请求方式为GET
                const xhr = new XMLHttpRequest()
                // config.method调用请求方法 使用逻辑中断将默认值赋值为GET请求
                xhr.open(config.method || 'GET', config.url)
                xhr.addEventListener('loadend', () => {
                    // 3.调用 成功/失败 的处理程序
                    if (xhr.status >= 200 && xhr.status < 300)
                        resolve(JSON.parse(xhr.response))
                    else
                        reject(new Error(xhr.response))
                })
                // 发送请求
                xhr.send()
            })
        }
        // 4.使用myAxios函数，获取省份列表展示
        myAxios({
            url: 'http://hmajax.itheima.net/api/province'
        }).then(result => {
            // 使用dir打印对象 详细查看
            console.dir(result)
            document.querySelector('.my-p').innerHTML = result.list.join('</br>')
        }).catch(error => {
            // 使用dir打印对象 详细查看
            console.dir(error)
            document.querySelector('.my-p').innerHTML = error.message
        })
    </script>
</body>
```

**效果**：

![image-20230813113448862](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131134277.png)

### 封装_简易axios _ 获取地区列表 (查询参数)

**需求**：修改 myAxios 函数支持传递 <font title=red>查询参数</font>，获取 ‘辽宁省’ ，‘大连市’ 对应地区列表展示

**要求**：

1.  myAxios 函数调用后，传入 <font title=red>params</font> 选项

    -  基本结构

    ```js
    function myAxios(config) {
       return new Promise((resolve,reject) => {
          //XHR请求 - 判断params选项，携带查询参数
          //调用 成功/失败 的处理程序
       })
    }
    myAxios({
       url: '目标资源地址',
       params: {
          参数名1: 值1,
          参数名2: 值2
       }
    })
    ```

2.  基于 `URLSearchParams` 转换<font title=red>查询参数字符串</font>.

3.  使用自己封装的 myAxios 函数展示地区列表

```js
<body>
    <p class="my-p"></p>
    <script>
        /*
        目标：封装_简易axios函数_获取地区列表
        1.判断params选项，携带查询参数
        2.使用URLSearchParams转换，并携带到url上
        3.使用myAxios函数，获取地区列表
        */
        //    1.定义myAxios函数，接收配置对象，返回Promise对象
        function myAxios(config) {
            return new Promise((resolve, reject) => {
                // 1.判断params选项，携带查询参数
                if (config.params) {
                    // 2.使用URLSearchParams转换，并携带到url上
                    const paramsObj = new URLSearchParams(config.params)
                    console.dir(paramsObj)
                    // 将获取到的查询参数转换为字符串类型
                    const queryString = paramsObj.toString()
                    // 把查询参数字符串，拼接在url的 ? 后面
                    config.url += `?${queryString}`
                }
                // 2.发起XHR请求，默认请求方式为GET
                const xhr = new XMLHttpRequest()
                // config.method调用请求方法 使用逻辑中断将默认值赋值为GET请求
                xhr.open(config.method || 'GET', config.url)
                xhr.addEventListener('loadend', () => {
                    // 3.调用 成功/失败 的处理程序
                    if (xhr.status >= 200 && xhr.status < 300)
                        resolve(JSON.parse(xhr.response))
                    else
                        reject(new Error(xhr.response))
                })
                // 发送请求
                xhr.send()
            })
        }
        // 4.使用myAxios函数，获取省份列表展示
        myAxios({
            url: 'http://hmajax.itheima.net/api/area',
            params: {
                pname: '辽宁省',
                cname: '大连市'
            }
        }).then(result => {
            // 使用dir打印对象 详细查看
            console.dir(result)
            document.querySelector('.my-p').innerHTML = result.list.join('</br>')
        }).catch(error => {
            // 使用dir打印对象 详细查看
            console.dir(error)
            document.querySelector('.my-p').innerHTML = error.message
        })
    </script>
</body>
```

**效果**：

![image-20230813132951994](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131329652.png)

### 封装_简易 _axios _注册用户 (请求体数据)

**需求**：修改 myAxios 函数支持传递<font title=red>请求体</font>数据，完成注册用户功能

步骤：

1.  myAxios 函数调用后，判断<font title=red>data</font>选项

    -  基本结构

    ```js
    function myAxios(config) {
       return new Promise((resolve,reject) => {
          //XHR请求 - 判断params选项，携带查询参数
          //调用 成功/失败 的处理程序
       })
    }
    myAxios({
       url: '目标资源地址',
       data: {
          参数名1: 值1,
          参数名2: 值2
       }
    })
    ```

2.  转换数据类型，在`send()`方法中发送

3.  使用自己封装的 myAxios 函数完成注册用户功能

```js
<body>
    <button class="reg-btn">注册用户</button>
    <script>
        /*
        目标：封装_简易axios函数_注册账号
        1.判断data选项，携带请求体
        2.转换数据类型，在send中发送
        3.使用myAxios函数，完成注册用户
        */
        //    1.定义myAxios函数，接收配置对象，返回Promise对象
        function myAxios(config) {
            return new Promise((resolve, reject) => {
                // 1.判断params选项，携带查询参数
                if (config.params) {
                    // 2.使用URLSearchParams转换，并携带到url上
                    const paramsObj = new URLSearchParams(config.params)
                    console.dir(paramsObj)
                    // 将获取到的查询参数转换为字符串类型
                    const queryString = paramsObj.toString()
                    // 把查询参数字符串，拼接在url的 ? 后面
                    config.url += `?${queryString}`

                }
                // 2.发起XHR请求，默认请求方式为GET
                const xhr = new XMLHttpRequest()
                // config.method调用请求方法 使用逻辑中断将默认值赋值为GET请求
                xhr.open(config.method || 'GET', config.url)
                xhr.addEventListener('loadend', () => {
                    // 3.调用 成功/失败 的处理程序
                    if(xhr.status >= 200 && xhr.status < 300)
                    resolve(JSON.parse(xhr.response))
                    else
                    reject(new Error(xhr.response))
                })
                // 1.判断data选项，携带请求体
                if (config.data) {
                    // 2.转换数据类型，在send中发送
                    const jsonStr = JSON.stringify(config.data)
                    // 设置请求头数据类型为application/json 是JSON数据格式
                    xhr.setRequestHeader('Content-Type', 'application/json')
                    // 发送请求,携带请求体数据
                    xhr.send(jsonStr)
                } else {
                    //发送请求
                    xhr.send()
                }
            })
        }
        document.querySelector('.reg-btn').addEventListener('click', () => {
            // 3.使用myAxios函数，完成注册用户
            myAxios({
                url: 'http://hmajax.itheima.net/api/register',
                method: 'POST',
                data: {
                    username: 'qweqjisd1123',
                    password: '123123'
                }
            }).then(result => {
                console.dir(result)
            }).catch(error => {
                console.dir(error)
            })
        })
    </script>
</body>
```

**效果**：

![image-20230813140045017](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131400425.png)

### 同步代码&异步代码

#### 同步代码：

>  我们应该注意的是，实际上浏览器是按照我们<span alt=solid>书写代码的顺序一行一行执行程序的</span>。浏览器会等待代码的解析和工作，在<span alt=solid>上一行完成后才会执行下一行</span>。这样做是很有必要的，因为每一行新的代码都是建立在前面代码的基础之上的。
>
>  这也使得它成为一个<span alt=solid>同步程序</span>.

#### 异步代码：

>  异步编程技术使得你的程序可以在执行一个<span alt=solid>可能长期运行的任务</span>的同时继续对其它事件做出反应而<span alt=solid>不必等待任务完成</span>。与此同时，你的程序也将在任务<span alt=solid>完成后显示结果</span>。

**同步代码**：逐行执行，<font title=red>需要原地等待结果</font>后，才继续向下执行

**异步代码**：调用后<font title=red>耗时</font>，不阻塞代码继续执行 (不必原地等待)，在将来完成后触发一个 <font title=red>回调函数</font>.

>  JS 线程 详细查看 [点击查看](../Web/JavaScript/WebAPIs.md) 打开后ctrl + F 搜索：JS执行机制

---

### 回调函数地狱

**需求**：展示默认第一个省，第一个城市，第一个地区在下拉菜单中

**概念**：在回调函数中<font title=red>嵌套回调函数</font>，一直嵌套下去就形成了回调函数地狱

**缺点**：可读性差，异常无法捕获，耦合性严重，牵一发动全身

![image-20230813142433861](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131424346.png)

```js
axios({url: 'http://hmajax.itheima.net/api/province'}).then(result => {
   const pname = result.data.list[0]
   document.querySelector('.province').innerHTML = pname
   //获取第一个省份默认下属的第一个城市名称
   axios({url: 'http://hmajax.itheima.net/api/city',params: {pname}}).then(result => {
      const cname = result.data.list[0]
      document.querySelector('.city').innerHTML = cname
      //获取第一个城市默认下属第一个地区的名字
      axios({url: 'http://hmajax.itheima.net/api/area',params: {pname , cname}}).then(result = > {
         document.querySelector('.area').innerHTML = result.data.list[0]
      })
   })
}).catch(error => {
      console.dir(error)
   })
```

可读性 太差了，不好维护，耦合性严重

**演示**：

###### html代码：

```html
<form action="#" method="get">
    <span>省份:&nbsp;</span><select>
        <option class="province"></option>
    </select>
    <span>城市:&nbsp;</span><select>
        <option class="city"></option>
    </select>
    <span>地区:&nbsp;</span><select>
        <option class="aname"></option>
    </select>
</form>
```

**js代码**：

在第三个最里面的axios的url中 故意多加一个1然它报错看看最外层的是否能捕获到它的错误信息呢

```js
/*
 目标：演示回调函数地狱
 需求：获取默认第一个省，第一个市，第一个区并展示在下拉菜单中
 概念：在回调函数中嵌套回调函数，一直嵌套下去就形成了回调函数地狱
 缺点：可读性差，异常无法捕获，耦合性严重，牵一发动全身
 */
//1.获取默认第一个省份的名称
axios({ url: 'http://hmajax.itheima.net/api/province' }).then(result => {
   const pname = result.data.list[0]
   document.querySelector('.province').innerHTML = pname
   // 2.获取默认第一个城市的名称
   axios({ url: 'http://hmajax.itheima.net/api/city', params: { pname: pname } }).then(result => {
      const city = result.data.list[0]
      document.querySelector('.city').innerHTML = city
      // 3.获取默认第一个地区的名称
      // 我们故意将接口的地址写错看下异常的处理是什么样子的 多加一个1
      axios({ url: 'http://hmajax.itheima.net/api/area1', params: { pname: pname, cname: city } }).then(result => {
         const aname = result.data.list[0]
         document.querySelector('.aname').innerHTML = aname
      })
   })
}).catch(error => {
   console.dir(error)
})
```

**结果**： <font title=red>不行</font>！！

![image-20230813153339502](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131533753.png)

>  <span alt=solid>总结</span>：
>
>  1.  什么是回调函数地狱？
>      -  在回调函数一直向下<font title=red>嵌套回调函数</font>，形成回调函数地狱
>  2.  回调函数地狱问题？
>      -  可读性差
>      -  异常捕获困难
>      -  耦合性严重

#### 下面就是解决回调函数地狱问题的方式：

### Promise-链式调用

使用Promise的特性来解决 回调函数地狱问题！

![image-20230813153913043](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131543365.png)

**上图解释**：

在创建一个Promise对象的时候，里面就会管理一个异步任务。而拿到异步任务成功的结果就可以调用对象内置的.then( ) 方法中传入回调函数接收成功的结果。而这个.then( ) 本身也是方法的调用本身也有返回值，而它的返回值又是一个新的Promise对象。这又是一个新的Promise对象，那里面还能再管理一个异步任务。

**解决方式**：

获取省份的Promise中调用.then在它的.then中获取城市，而获取城市的Promise中调用.then在它的.then中获取地区。

这样回调函数嵌套的问题就变成了线性的结构！

>  概念：
>
>  -  依靠then()方法返回一个<font title=red>新生成的Promise对象</font>特性，继续串联下一环任务，直到结束
>
>  细节：
>
>  -  then()回调函数中的<font title=red>返回值</font>，会影响新生成的Promise对象<font title=red>最终状态和结果</font>.

![image-20230813154716607](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131547443.png)

```js
/*
 目标：掌握Promise的链式调用
 需求：把省市的嵌套结构，改成链式调用的结构
 */
//1.创建Promise对象-模拟请求省份名称
const p = new Promise((resolve, reject) => {
   setTimeout(() => {
      resolve('北京市')
   }, 2000)
})
// 2.获取省份名称
const p1 = p.then(result => {
   console.log(result)
   // 3.创建Promise对象-模拟请求城市名称
   // return Promise 对象最终状态和结果，影响到新的Promise对象
   return new Promise((resolve, reject) => {
      setTimeout(() => {
         resolve(result + '---北京')
      }, 2000)
   })
})
p1.then(result => {
   console.log(result)
})
//then() 返回的Promise是一个新对象 两个对象 的地址不同 返回 为 false
console.log(p1 === p)
```

**结果**：

![image-20230813160612061](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131606112.png)

>  <span alt=solid>总结</span>：
>
>  1.  什么是Promise的链式调用 ？
>      -  使用then函数返回新Promise对象特性，一直串联下去
>  2.  then回调函数中，return的值会传给哪里？
>      -  传给then函数生成的新Promise对象
>  3.  Promise链式调用有什么用？
>      -  解决回调函数嵌套(地狱)问题

### Promise-链式应用

**目标**：使用Promise链式调用，解决回调函数地狱问题。

**做法**：每个Promise对象中管理一个异步任务，用then返回Promise对象，串联起来。

![image-20230813161938585](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131619195.png)

每一个axios就相当于Promise 可以看下上面的<a href="#Promise+XHR获取省份列表">Promise+XHR</a>.

```js
/*
 目标：把回调函数嵌套代码，改成Promise链式调用结构
 需求：获取默认第一个省，第一个市，第一个地区并展示在下拉菜单中
 */
//定义全局变量
let pname = ''
//1.获取第一个省Promise对象
axios({ url: 'http://hmajax.itheima.net/api/province' }).then(result => {
   pname = result.data.list[0]
   document.querySelector('.province').innerHTML = pname
   // 2.获取第一个城市Promise对象 并返回 多加一个1让它报错查看捕获的结果
   return axios({ url: 'http://hmajax.itheima.net/api/city1', params: { pname: pname } })
}).then(result => {
   cname = result.data.list[0]
   document.querySelector('.city').innerHTML = cname
   // 3.获取第一个地区Promise对象 并返回
   return axios({ url: 'http://hmajax.itheima.net/api/area', params: { pname: pname, cname: cname } })
}).then(result => {
   document.querySelector('.aname').innerHTML = result.data.list[0]
}).catch(error => {
   console.dir(error)
})
```

**结果**：

![image-20230813164606240](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131646743.png)

## async函数和await

>  <span alt=solid>定义</span>：
>
>  async函数是使用`async`关键字声明的函数。async函数是 `AsyncFunction`构造函数的实例，并且其中允许使用`await`关键字。<span alt=solid>`async`和`await`关键字让我们可以用一种更简洁的方式写出基于`Promise`的异步行为，而无需刻意地链式调用`Promise`</span>。
>
>  <span alt=solid>概念</span>：在async函数内，使用await关键字取代then函数，<font title=red>等待</font>获取Promise对象成<font title=red>功状态的结果值</font>.

**示例**：

###### html代码：

```html
<form action="#" method="get">
   <span>省份:&nbsp;</span><select>
   <option class="province"></option>
   </select>
   <span>城市:&nbsp;</span><select>
   <option class="city"></option>
   </select>
   <span>地区:&nbsp;</span><select>
   <option class="aname"></option>
   </select>
</form>
```



```js
//获取默认省市区
async function getDefaultArea() {
   const pObj = await axios({url: 'http://hmajax.itheima.net/api/province'})
   const pname = pObj.data.list[0]
   const cObj = await axios({url: 'http://hmajax.itheima.net/api/city' , params: {pname: pname}})
   const cname = cObj.data.list[0]
   const aObj = await axios({url: 'http://hmajax.itheima.net/api/area' , params: {pname: pname , cname: cname}})
   const aname = aObj.data.list[0]
   //赋予到页面上
   document.querySelector('.province').innerHTML = pname
   document.querySelector('.city').innerHTML = cname
   document.querySelector('.area').innerHTML = aname
}
getDefaultArea()
```



---

### async函数和await_解决回调函数地狱问题

```js
/*
 目标：掌握async和await语法，解决回调函数地狱问题
 概念：在async函数内，使用await关键字，获取Promise对象，"成功状态"   结果值
 注意：await必须用在async修饰的函数内 (await会阻止 "异步函数" 代码继续执行，原地等待结果)
 */
//1.定义async修饰函数
async function getData() {
   // axios原地返回一个Promise对象而await等待一个Promise对象成功状态的结果值 所以在前面使用await接收将结果取在原地使用 一个变量接收
   // await必须用在async修饰内
   const pObj = await axios({ url: 'http://hmajax.itheima.net/api/province' })
   const pname = pObj.data.list[0]
   document.querySelector('.province').innerHTML = pname
   //故意在地址尾部多加一个1 当这里发生错误后 后面不会再执行，你可能会想到下面不是依赖了这里一个变量吗应该是报错了吧不是没有执行。这个结论是错误的下面的代码并不会再去执行了
   const cObj = await axios({ url: 'http://hmajax.itheima.net/api/city1', params: { pname: pname } })
   const cname = cObj.data.list[0]
   document.querySelector('.city').innerHTML = cname
   const aObj = await axios({ url: 'http://hmajax.itheima.net/api/area', params: { pname: pname, cname: cname } })
   const aname = aObj.data.list[0]
   document.querySelector('.aname').innerHTML = aname
}
getData()
// 只是终止了aysnc块中的代码执行并不是终止程序 哦~
console.log('123')
```

**结果**：

![image-20230813174800891](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131748288.png)

没有对异常做处理，对异常做处理请看下章

---

### async函数和await_捕获错误

**使用**：try-catch

`try-catch`语句标记要尝试的语句块，并指定一个出现异常时抛出的响应。

**语法**：

```js
try{
   //要执行的代码
} catch(error) {
   //error接收的是，错误信息
   //try里面代码，如果有错误，直接进入这里执行
}
```



```js
/*
 目标：掌握async和await语法，解决回调函数地狱问题
 概念：在async函数内，使用await关键字，获取Promise对象，"成功状态"   结果值
 注意：await必须用在async修饰的函数内 (await会阻止 "异步函数" 代码继续执行，原地等待结果)
 */
//1.定义async修饰函数
async function getData() {
   // 1.使用try包裹可能发生错误的代码
   try {
      // axios原地返回一个Promise对象而await等待一个Promise对象成功状态的结果值 所以在前面使用await接收将结果取在原地使用 一个变量接收
      // await必须用在async修饰内
      const pObj = await axios({ url: 'http://hmajax.itheima.net/api/province' })
      const pname = pObj.data.list[0]
      document.querySelector('.province').innerHTML = pname
      //故意在地址尾部多加一个1 当这里发生错误后 后面不会再执行
      const cObj = await axios({ url: 'http://hmajax.itheima.net/api/city1', params: { pname: pname } })
      const cname = cObj.data.list[0]
      document.querySelector('.city').innerHTML = cname
      const aObj = await axios({ url: 'http://hmajax.itheima.net/api/province' })
      const aname = aObj.data.list[0]
      document.querySelector('.aname').innerHTML = aname
   } catch (error) {
      // 2.使用catch捕获发生错误的信息
      // 打印捕获到的错误信息 是一个对象是axios返回的错误使用dir查看详细
      console.dir(error)
   }
}
getData()
// 只是终止了aysnc块中的代码执行并不是终止程序 哦~
console.log('123')
```

**结果**：

![image-20230813175557560](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308131755747.png)

## 宏任务与微任务

ES6之后引入了Promise对象，让JS引擎也可以发起异步任务

**异步任务分为**：

<span alt=solid>宏任务</span>：由<font title=red>浏览器</font>环境执行的异步代码

<span alt=solid>微任务</span>：由<font title=red>JS引擎</font>环境执行的异步代码

宏任务

| 任务(代码)             | 执行所在环境 |
| ---------------------- | ------------ |
| JS脚本执行事件(Script) | 浏览器       |
| setTimeout/setInterval | 浏览器       |
| Ajax请求完成事件       | 浏览器       |
| 用户交互事件等         | 浏览器       |

微任务

| 任务(代码)         | 执行所在环境 |
| ------------------ | ------------ |
| Promise对象.then() | JS引擎       |

Promise本身是同步的，而then和catch<font title=red>回调函数</font>是异步的

![image-20230813195951215](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308132000229.png)

执行步骤：

```
 1.script标签js脚本执行的事件就会交给浏览器环境

 2.然后把整个js脚本代码推入到宏任务队列中

 3.调用栈是空闲的它会立刻执行第一个宏任务就是script脚本(标签)然后执行里面代码

 4.读到console.log(1)放入到调用栈进行执行
  
 5.读取setTimeout是异步任务，是个宏任务，宏任务交给浏览器来执行

 6.读取new Promise放入到调用栈中，Promise对象本身是同步的,立刻打印里面的值

 6.1 Promise是同步的立刻调用了里面的成功回调函数，它是微任务的

 7.将console.log(5)推入调用栈执行

 8.调用栈空闲了就反复去查看任务队列中是否有可执行的异步回调函数如果宏任务，微任务两个任务队列都有可执行的异步代码会优先执行微任务中的异步回调函数，因为微任务更接近与JS引擎

 9.将微任务队列的任务推入到调用栈执行

 10.此时微任务队列中没有任务了去找宏任务并将其推入到调用栈执行

 11.程序运行完毕!!
```

>  总结：
>
>  1.  什么是宏任务?
>      -  <font title=red>浏览器执行</font>的异步代码
>      -  例如：JS执行脚本事件，setTimeout / setInterval，Ajax请求完成事件，用户交互事件等
>  2.  什么是微服务？
>      -  <font title=red>JS 引擎</font> 执行的异步代码
>      -  例如：Promise对象.then()的回调
>  3.  JavaScript 内部代码如何执行？
>      -  执行第一个script脚本事件宏任务，里面<font title=red>同步</font>代码
>      -  遇到<font title=red>宏任务 / 微任务</font>交给宿主环境，有结果回调函数进入对应队列
>      -  当执行栈空闲时，<font title=red>清空微任务</font>队列，再执行<font title=red>下一个宏任务</font>，从 1 再来
>
>  ![image-20230813200500564](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308132005359.png)

## Promise.all静态方法

**概念**：合并多个Promise对象，等待所有<font title=red>同时成功</font>完成(或某一个失败)，做后续逻辑

![image-20230813202342010](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308132023381.png)

**语法**：

```js
//all的构造函数传入一个数组，数组里面就是要合并的一个一个小的Promise对象然后返回一个新的大Promise对象
const p = Promsie.all([Promise对象，Promise对象，...])
p.then(result => {
   //result结果：[Promise对象成功结果，Promise对象成功结果，...]
}).catch(error => {
   //第一个失败的Promise对象，抛出的异常
})
```

