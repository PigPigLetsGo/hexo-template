---
title: NodeJs与WebPack
categories:
    - [计算机学科,web]
tags:
    - 计算机学科
    - web
---

#### 什么是前端工程化？

**前端工程化**：开发项目直到上线，过程中集成的所有<font title=red>工具和技术</font>.

Node.js 是前端工程化的基础 (因为Nde.js可以主动读取前端代码内容)

![image-20230814084343685](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141436216.png)

1.  **压缩工具**：如果可以把代码中的回车，注释，换行，一些长的变量名 变成短的这些都是属于对前端代码进行压缩处理
    -  **压缩目的**：是为了让前端的项目体积更小，让项目更快的在用户电脑上被加载出来。
2.  **格式化工具**：多人协同同一个项目代码的时候代码风格要统一，那么可以集成格式化工具
3.  **转换工具**：比如编写的是：less，sass这种CSS预处理的语言那最终运行到浏览器上，中间需要一个转换过程将less，sase语法转成原生的CSS，那么就可以集成相应的转换工具。而且也可以对高级的JS进行降级处理。
4.  **打包工具**：比如WebPack就是一个静态模块的打包工具，它可以对前端代码进行转换，压缩以及整合等处理。
5.  **脚手架工具**：vue，react会用到脚手架工具，把开发项目时用到的所有的准备过程全都在这个工具里为我们准备好了。
6.  。。。。还有其它更多工具

# Node.js

## fs模块-读写文件

**模块**：类似插件，封装了<font title=red>方法 / 属性</font>.

**fs模块**：封装了本机文件系统进行交互的，方法 / 属性

**语法**：

1.  <font title=red>加载</font> fs 模块对象

    ```js
    const fs = require('fs') // fs 是模块标识符：模块的名字
    ```

2.  <font title=red>写入</font>文件内容

    ```js
    fs.writeFile('文件路径'，'写入内容'，err => {
                 //写入后的回调函数
    })
    ```

3.  <font title=red>读取</font>文件内容

    ```js
    fs.readFile('文件路径'，(err，data) => {
       //读取后的回调函数
       //data 是文件内容的Buffer数据流
    })
    ```


**代码**：

```js
/**
 * 目标：基于fs模块读写文件内容
 * 1.加载fs模块对象
 * 2.写入文件内容
 * 3.读取文件内容
 */
// 1.加载fs模块对象,要引入的是fs模块所以直接写fs就行了
const fs = require('fs')
// 2.写入文件内容
// 参数1：要写入到哪一个文件
// 参数2：要写入到文件的内容
// 参数3：回调函数 ->参数：错误信息
fs.writeFile('../text.txt', 'hello,node.js', err => {
    if (err)
        console.log(err)
    else
        console.log('写入成功')
})
// 3.读取文件内容
// 参数1：指定要读取文件的地址
// 参数2：回调函数 ->参数1：错误信息，参数2：Buffer数据流对象
fs.readFile('../text.txt', (err, data) => {
    if (err)
        console.log(err)
    else
        // console.log(data)//<Buffer 68 65 6c 6c 6f 2c 6e 6f 64 65 2e 6a 73>
        console.log(data.toString())//hello,node.js 读取到的是十六进制 需要转换为十进制
})
```

## path模块-路径处理

**问题**：Node.js代码中，相对路径是根据<font title=red>终端所在路径</font>来查找的，可能无法找到你想要的文件

![image-20230814092605129](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141436824.png)

终端起点路径是 ==03-Code== 执行 ==03/index.js== 文件里面读取文件形式为：==../test.txt== 上一级目录查找，那么就会从终端的起点目录 ==03-Code== 的 上一级 ==03_Node.js与Webpack== 这个目录中去找，那么就找不到了就会报错了。

虽然执行的是 03目录下的index.js 但是终端的起始目录位置是 03-Code 目录 执行代码../以终端所在路径为主。

**建议**：在Node.js 代码中，使用<font title=red>绝对路径</font>.

**补充**：<font title=red>_ _dirname</font>^下划线之间没有空格，为了分辨才加的空格^内置变量 (获取当前模块目录 - 绝对路径)

-  windows：`D:\备课代码\3-B站课程\03_Node.js与Webpack\03-Code\03` 
-  mac：`/Users/xxx/Descktop/备课代码/3-B站课程/03Node.js与Webpack/03-Code/03` 
-  它们之间的路径分隔符不同

**注意**：`path.join()`会使用特定于平台的分隔符，作为定界符，将所有给定的路径片段链接在一起。

```js
path.join('03','dist/js','index.js')
//windows: '03\dist\js\index.js'
//mac: '03/dist/js/index.js'
```

**语法**：

1.  加载path模块

    ```js
    const path = require('path')
    ```

2.  使用`path.join`方法，拼接路径

    ```js
    path.join('路径1','路径2',...)
    ```

```js
/**
 * 目标：在Node.js环境的代码中，应使用绝对路径
 * 原因：代码的相对路径是以终端所在文件夹为起点，而不是VsCode资源管理器
 * 容易造成目标文件找不到错误
 */
const fs = require('fs')
// 1.引入path模块对象
const path = require('path')
// 2.调用path.join()配合__driname组成目标文件的绝对路径
// 使用__dirname无论你在终端的哪个目录下都不会影响我找我想要的文件去读取否则就报错
console.log(__dirname)// E:\前置课\NodeJs与Webpack\js
fs.readFile(path.join(__dirname, '../text.txt'), (err, data) => {
    if (err)
        console.log(err)
    else
        console.log(data.toString())// hello,node.js
})
```

## 案例—压缩前端 html 文件

**需求**：把 回车符 (\r) 和换行符 (\n) 去掉后，写入到新 html 文件中

**步骤**：

1.  <font title=red>读取</font> 源 html 文件内容
2.  正则 <font title=red>替换</font> 字符串
3.  <font title=red>写入</font>到新 的 html 文件中

```js
/**
 * 目标：压缩html代码
 * 需求：把回车符 \r , 换行符 \n 去掉，写入到新html文件中
 * 1.1 读取源 html 文件内容
 * 1.2 正则替换字符串
 * 1.3 写入到新的html文件中
 */
//  1.1 读取源 html 文件内容
// 导入两个模块 文件，路径
const fs = require('fs')
const path = require('path')
// 读取文件 path.join(__dirname绝对路径下的html文件)
fs.readFile(path.join(__dirname, 'tudolist.html'), (err, data) => {
    if (err) {
        console.log(err)
    } else {
        // 1.2 正则替换字符串
        const htmlStr = data.toString().replace(/[\r\n]/g, '')
        // 1.3 写入到新的html文件中
        fs.writeFile(path.join(__dirname, 'tudolist.html'), htmlStr, (err) => {
            if (err)
                console.log(err)
            else
                console.log('html代码 压缩完毕 !!')
        })
    }
})
```

**效果**：

![image-20230814102152964](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141021437.png)

测试压缩后的代码能否正常使用。

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141024425.gif)

## URL中的端口号

URL：==统一资源定位符==，简称 ==网址==，用于访问服务器里的资源

![image-20230814102459698](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141025188.png)

## http模块-创建Web服务

**需求**：创建Web服务并响应内容给浏览器

**步骤**：

1.  加载<font title=red>http模块</font>，创建Web服务对象
2.  监听<font title=red>request</font>请求事件，设置响应头和响应体
3.  配置<font title=red>端口号</font>并<font title=red>启动</font>Web服务
4.  浏览器请求 http://localhost:3000 测试

(localhost:固定代表本机的域名)

```js
/**
 * 目标：基于http模块创建Web服务程序
 * 1.加载http模块，创建Web服务器对象
 * 2.监听request请求事件，设置响应头和响应体
 * 3.配置端口号并启动web服务
 * 4.浏览器请求http://localhost:3000测试
 */
// 1.加载http模块，创建Web服务器对象
const http = require('http')
const server = http.createServer()
// 2.监听request请求事件，设置响应头和响应体
server.on('request', (req, res) => {
    // 设置响应头内容类型为 普通文本
    res.setHeader('Content-Type', 'text/plain;charset=utf-8')
    // 设置响应体内容，结束本次请求与响应
    res.end('欢迎使用Node.js和http模块创建的Web服务程序')
})
// 3.配置端口号并启动web服务
server.listen(3000, () => {
    console.log('Web服务启动成功了!')
})
```

**效果**：

![image-20230814104801413](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141048766.png)

## Node.js模块化

什么是模块化？

>  <span alt=solid>定义</span>：
>
>  CommonJS 模块是为 Node.js打包JavaScript 代码的原始方式。Node.js还支持浏览器和其他 JavaScript 运行时使用的ECMAScript 模块标签。
>
>  <span alt=solid>在Node.js中，每个文件都被视为一个单独的模块</span>。

![image-20230814105115230](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141051486.png)

在前面编写web服务的时候index.js就是一个模块

1.  有读取文件需求时借助了：fs 模块
2.  拼接路径借助了：path 模块
3.  创建Web服务借助了：http 模块
4.  将查询字符串转换为查询对象借助了：querystring 模块
5.  不光可以引入Node.js自己内置的模块，我们自己也可以定义一些js的模块文件，然后引入到自己的代码中进行使用。

**概念**：<span alt=wavy>项目是由很多个模块文件组成的</span>。

**好处**：提高代码复用性，按需加载，<font title=red>独立作用域</font>.

**使用**：需要标准语法<font title=red>导出</font>和<font title=red>导入</font>进行使用

### CommonJS标准

**需求**：定义 utils.js 模块，封装基地址 和 求数组总和的函数

**使用**：

1.  **导出**：`module.exports = {}` 

    -  基本结构

    ```js
    const baseURL = 'http://hmajax.itheima.net'
    const  getArraySum = arr => arr.reduce((sum , val) => sum + val , 0)
    
    module.exports = {
       对外属性名1: baseURL,
       对外属性名2: getArraySum
    }
    ```

2.  **导入**：`require('模块名或路径')` 

    ```js
    const obj = require('模块名或路径')
    //obj 就等于 module.exports 导出的对象
    ```

##### 模块名或路径

-  **内置模块**：直接写名字  (例如：`fs`,`path`,`http`)
-  **自定义模块**：需要写模块文件路径 (例如：`./utils.js`)

**目录结构**：

![image-20230814111920506](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141119859.png)

utils.js

```js
/**
 * 目标：基于CommonJS标准语法，封装属性和方法并导出
 */
const baseURL = 'http://hmajax.itheima.net'
const getArraySum = arr => arr.reduce((pre, current) => pre + current, 0)
// 导出
module.exports = {
    url: baseURL,
    arraySum: getArraySum
}
```

01.js

```js
/**
 * 目标：基于CommonJS 标准语法，导入工具属性和方法使用
 */
// 导入
// 导入要么写模块名要么写路径
// 模块名 是内置的
// 路径 是自定义的
const obj = require('./utils.js')
console.log(obj)
const arrSum = obj.arraySum([5,1,2,3,4])
console.log(arrSum)
```

**效果**：

![image-20230814111845035](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141118433.png)

>  <span alt=solid>总结</span>：
>
>  1.  Node.js中什么是模块化？
>      -  每个<font title=red>文件</font>都是独立的模块
>  2.  模块之间如何联系呢？
>      -  使用特定语法，<font title=red>导出</font>和<font title=red>导入</font>使用
>  3.  CommonJS 标准规定如何导出和导入模块呢？
>      -  导出：`module.exports = {}` 
>      -  导入：`require('模块名或路径')` 
>  4.  模块名 / 路径如何选择？
>      -  内置模块，直接写<font title=red>名字</font>。例如：fs，path，http等
>      -  自定义模块，写模块文件<font title=red>路径</font>。例如：`./utils.js` 

---

#### ECMAScript 标签-默认导出和导入

**需求**：封装并导出基地址和求数组元素和的函数

##### 默认标准使用：

1.  导出：`export default {}` 

    -  基本结构

    ```js
    const baseURL = 'http://hmajax.itheima.net'
    const  getArraySum = arr => arr.reduce((sum , val) => sum + val , 0)
    
    export default {
       对外属性名1: baseURL,
       对外属性名2: getArraySum
    }
    ```

2.  导入：`import 变量名 from '模块名或路径'` 

    ```js
    import obj from '模块名或路径'
    // obj 就等于 export default 导出的对象
    ```

<blockquote alt=danger>
	<div>
      <p>
         <span><font title=red>注意</font>：Node.js默认支持 CommonJS 标准语法</span>
      </p>
      <p>
         <span>如需使用ECMAScript标准语法，在运行模块所在文件夹新建 package.json 文件 (项目的清单文件)，并设置{"type": "module"}</span>
      </p>
      <p>
         <span>这样当我们在运行模块所在文件夹下用Node命令执行js文件时它就能够正确的识别ECMAScript标准的导出/导入语法了</span>
      </p>
   </div>
</blockquote>

![image-20230814113034848](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141130482.png)

**目录结构**：

![image-20230814115320121](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141153427.png)

utls1.js

```js
/**
 * 目标：基于ECMAScript标准语法，封装属性和方法并 "默认" 导出
 */
const baseURL = 'http://hmajax.itheima.net'
const getArraySum = function (arr = []) {
    return arr.reduce(function (pre, cur) {
        return pre + cur
    }, 0)
}
// 默认导出
export default {
    url: baseURL,
    arrSum: getArraySum
}
```

02.js

```js
/**
 * 目标：基于ECMAScript标准语法，"默认" 导入，工具属性和方法使用
 */
//默认导入
import obj from './utils1.js'
console.log(obj)// { url: 'http://hmajax.itheima.net', arrSum: [Function: getArraySum] }
const arrSum = obj.arrSum([4, 1, 2, 3])
console.log(arrSum)// 10
```

package.json

```json
{
    "type": "module"
}
```

>  <span alt=solid>总结</span>：
>
>  1.  ECMASCript 标准规定如何<font title=red>默认</font>导出和导入模块呢？
>      -  导出：<span alt=solid>export default{ }</span> 
>      -  导入：<span alt=solid>import 变量名 from ‘模块名或路径’</span>
>  2.  如何让Node.js 切换模块标准为 ECMAScript?
>      -  运行模块所在文件夹，新建 package.json 并设置
>      -  `{"type": "module"}`

---

#### ECMAScript标准—命名导出和导入

**命名标准使用**：

1.  导出：`export修饰定义语句` 

    -  基本结构

    ```js
    export const baseURL = 'http://hmajax.itheima.net'
    export const getArraySum = arr => arr.reduce((pre,cur) => pre + cur , 0)
    ```

2.  导入：`import {同名变量} from '模块名或路径'` 

    ```js
    import {baseURL,getArraySum} from '模块名或路径'
    //baseURL和getArraySum 是变量，值为模块内命名导出的同名变量的值
    ```

<blockquote alt=danger>
	<div>
      <p>
         <span><font title=red>注意</font>：Node.js默认支持 CommonJS 标准语法</span>
      </p>
      <p>
         <span>如需使用ECMAScript标准语法，在运行模块所在文件夹新建 package.json 文件 (项目的清单文件)，并设置{"type": "module"}</span>
      </p>
      <p>
         <span>这样当我们在运行模块所在文件夹下用Node命令执行js文件时它就能够正确的识别ECMAScript标准的导出/导入语法了</span>
      </p>
   </div>
</blockquote>
**目录结构**：

![image-20230814133015038](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141330497.png)

utils.js

```js
/**
 * 目标：基于ECMAScript标准语法，封装属性和方法并 "命名" 导出
 */
export const baseURL = 'http://hmajax.itheima.net'
export const getArraySum = function (arr) {
    return arr.reduce(function (pre, cur) {
        return pre + cur
    }, 0)
}
```

1.js

```js
/**
 * 目标：基于ECMAScript标准语法 "命名" 导入，工具属性和方法使用
 */
import {baseURL,getArraySum} from './utils.js'
console.log(baseURL)// http://hmajax.itheima.net
console.log(getArraySum([1,2,3,5,6,3]))// 20
```

package.json

```json
{
    "type": "module"
}
```

<span alt=solid>注意</span>：:warning: 

导出和导入的变量命名要相同否则报错未知变量：

```js
export const baseURL = 'http://hmajax.itheima.net'
export const getArraySum = function (arr) {
    return arr.reduce(function (pre, cur) {
        return pre + cur
    }, 0)
}
```

```js
import {obj,getArraySum} from './utils.js'
console.log(obj)
console.log(getArraySum([1,2,3,5,6,3]))
```

结果：

![image-20230814133742075](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141337607.png)

#### 默认导出和导入 & 标准命名导出和导入 如何选择

<font title=red>按需</font>加载，使用<font title=red>命名</font>导出和导入

<font title=red>全部</font>加载，使用<font title=red>默认</font>导出和导入

>  <span alt=solid>总结</span>：
>
>  1.  Node.js 支持哪2中模块化标准？
>      -  <span alt=solid>CommonJS 标准语法 (默认)</span>.
>      -  ECMAScript 标准语法
>  2.  ECMAScript标准，<font title=red>命名</font>导出和导入的语法？
>      -  导出：<span alt=solid>export修饰定义的语句</span>.
>      -  导入：<span alt=solid>`import {同名变量} from '模块名或路径'`</span>.
>  3.  ECMAScript 标准，<font title=red>默认</font>导出和导入的语法？
>      -  导出：<span alt=solid>export default { }</span>
>      -  导入：<span alt=solid>`import 变量名 from '模块名或路径'`</span>.

## 包的概念

**包**：将<font title=red>模块，代码，其它资料</font>聚合成一个文件夹

**包分类**：

-  项目包：主要用于编写项目和业务逻辑
-  软件包：<font title=red>封装工具和方法</font>进行使用

**要求**：跟目录中，必须有 package.json文件 (记录包的清单信息)

![image-20230814144121175](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141442151.png)

package.json对当前所在utils的一些描述信息

![image-20230814144111345](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141442509.png)

<font title='red'>注意</font>：导入软件包时，引入的默认是 `index.js` 模块文件 / main 属性指定的模块文件

**需求**：封装数组求和函数的模块，判断用户名和密码长度函数的模块，形成一个<font title='red'>软件包</font>.

>  <span alt=solid>总结</span>：
>
>  1.  什么是包？
>      -  将模块，代码其它资料聚合成的<font title='red'>文件夹</font>.
>  2.  包分为哪2类呢？
>      -  项目包：编写项目代码的文件夹
>      -  <font title='red'>软件包</font>：封装工具和方法供开发者使用
>  3.  package.json文件的作用？
>      -  记录<font title='red'>软件包的名字</font>，作者，<font title='red'>入口</font>文件等信息
>  4.  导入一个包文件夹的时候，导入的是哪个文件？
>      -  <font title='red'>默认 index.js 文件</font>，或者 main 属性指定的文件

**目录结构**：

![image-20230814151449238](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141514842.png)

arr.js

```js
/**
 * 目标：封装数组常用的方法
 */
// 数组求和函数
getArraySum = arr => arr.reduce((pre, cur) => pre + cur, 0)

module.exports = {
    arraySum: getArraySum
}
```

str.js

```js
/**
 * 目标：封装校验用户名和密码长度的函数
 * 要求：用户名最少 8 位，密码最少为 6 位
 */
const checkUserName = username => username.length >= 8

const checkPassword = password => password.length >= 6

module.exports = {
    uname: checkUserName,
    pwd: checkPassword
}
```

index.js

```js
/**
 * 本文件是，utils 工具包的唯一出口
 * 作用：把所有工具模块方法集中起来，统一向外暴漏
 */
const { arraySum } = require('./lib/arr.js')
// 多个对象使用对象解构 比较方便
const { uname, pwd } = require('./lib/str.js')

// 统一导出所有函数
module.exports = {
    arraySum,
    uname,
    pwd
}
```

package.json

```json
{
    "name": "cz_utils",
    "version": "1.0.0",
    "description": "数组求和与验证用户表单函数",
    "main": "index.js",
    "author": "Dkx",
    "license": "MIT"
}
```

server.js

```js
/**
 * 目标：导入utils软件包，使用里面封装的工具函数
 */
const obj = require('./utils')
const sum = obj.arraySum([1,2,3,4,5])
console.log(sum)
const name = obj.uname('shmishadmin')
console.log(name)
const pwd = obj.pwd('12387976')
console.log(pwd)
```

**结果**：

![image-20230814154949235](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141552111.png)

## npm—软件包管理器

>  定义：
>
>  <span alt='solid'>npm简介</span>.
>
>  `npm`是Node.js标准的<span alt='solid'>软件包管理器</span>。
>
>  在2017年1月时，npm仓库中就已有超过350000个软件包，这使其成为世界上最大的单一语言代码仓库，并且可以确定几乎有可用于一切的软件包。
>
>  它起初是作为<span alt='solid'>下载和管理Node.js</span>包依赖的方式，但其现在也成为前端JavaScript中使用的工具

使用：

1.  初始化清单文件：`npm init -y` (得到 package.json 文件，有则略过此命令)
2.  下载软件包：`npm i 软件包名称` 其中的<span alt='solid'> `i` 是 install 的简写</span>。
3.  使用软件包

##### 用下面案例体验npm软件包管理器的使用

**需求**：使用 dayjs 软件包，来格式化日期时间

**图解**：

![image-20230814161933879](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141619386.png)

1 创建一个目录来存放编写项目

![image-20230814163335174](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141633693.png)

2 右击这个目录打开集成终端因为执行命令生成的文件或软件是需要到这个目录中的。

2.1 执行命令：`npm init -y`初始化清单，会创建一个package.json文件如果自己创建过就不需要这一步了。npm创建的package.json里面自己初始化好了的。

![image-20230814163557137](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141635471.png)

![image-20230814163649661](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141636375.png)

2.2 使用命令下载dayjs软件包：`npm i dayjs` 

![image-20230814163836786](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141638611.png)

下载完成后就会出现一个 package-local.json 文件是固定当前软件版本的清单文件，并且会多个一个node_modules里面就是我们下载的 dayjs 软件了

![image-20230814164125589](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141641972.png)

3 使用下载好的软件包

在server.js中导入软件包的名称然后使用

```js
/**
 * 目标：使用npm下载 dayjs 软件包来格式化日期时间
 * 1.(可选) 初始化项目清单文件,命令：npm init -y 得到 package.json文件(自己创建了则不用执行这个命令了)
 * 2.下载软件包到当前项目，命令: npm i 软件包名称
 * 3.使用软件包
 */
const dayjs = require('dayjs')
const nowDateStr = dayjs().format('YYYY-MM-DD')
console.log(nowDateStr)
```

**结果**：

![image-20230814164033005](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141640582.png)

>  总结：
>
>  1.  npm软件包管理器作用？
>      -  下载软件包以及管理版本
>  2.  初始化项目清单文件 package.json 命令？
>      -  <font title='red'>npm init -y</font>
>  3.  下载软件包的命令？
>      -  <font title='red'>npm i 软件包名称</font> ，其中i是install的简写
>  4.  下载的包存放在哪里？
>      -  当前项目下的 <font title='red'>node_modules</font>中，并记录在 package.json 中

### npm-安装所有依赖

[^1]:命令

**举个栗子**：比如我从一个小伙伴哪里拿来了一个它的项目，这时会发现这个项目里有package.json还有他写完的业务逻辑代码server.js但是缺少了一个node_modules文件夹。而这个node_modules里面放的都是这个项目需要依赖的软件包，这个项目中package.json文件中记录了这个项目都下载过哪些软件包还有对应的版本号，还有package-local.jsno用来固定每一个软件包对应的版本的。

![image-20230814164818948](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141648908.png)

在它写的业务逻辑代码中导入了 dayjs，lodash两个软件包 那么有个问题当我拿到这样的项目能正常的运行吗？

答案肯定是不行的。

为什么不能正常运行呢？

它需要导入 dayjs ，lodash 但是我们项目中并没有这两个软件包对应的源码。所以他现在缺少的就是这两个需要导入的软件包

![image-20230814165240736](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141652858.png)

你肯定就会有一个小问题，那为什么他给我这个项目的时候不也直接把node_modules也给我呢？

**原因**：因为，自己用npm下载依赖比磁盘传递拷贝要快得多

**解决**[^1]：项目终端输入命令：`npm i` 注意这里不能代码软件包的名称 ，因为里面的package.jsno记录了

下载 package.json 中记录的所有的软件包

![image-20230814170030860](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141700492.png)

>  <span alt='solid'>总结</span>：
>
>  1.  当项目中只有 package.json 没有 node_modules怎么办？
>      -  当前项目下，执行 `npm i`安装package.json中记录的所有依赖软件包
>  2.  为什么node_modules 不进行传递？
>      -  因为用 npm 下载比磁盘传递快

### npm-全局软件包 nodemon

**软件包区别**：

-  本地软件包：<font title='red'>当前项目</font>内使用，封装<font title='red'>属性和方法</font>，存在于 node_modules
-  全局软件包：<font title='red'>本机</font>所有项目使用，封装<font title='red'>命令和工具</font>，存在于系统设置的位置

<span alt='solid'>nodemon作用</span>：替代 node 命令，检测代码更改，自动重启程序

**使用**：

1.  **安装**：`npm i nodemon -g` (-g代表安装到全局环境中)
2.  **运行**：ndemon 待执行的目标js文件

**需求**：启动准备好的项目，修改代码保存后，观察自动重启应用程序

下载的是全局位置所以就不用考虑终端所在的文件夹位置了在哪里都可以。

![image-20230814180544049](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141805466.png)

执行命令后下载到的位置

![image-20230814190347549](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141903082.png)

下载完成后我们就可以使用 nodemon 命令了，执行后启动了一个服务

![image-20230814190843481](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141908423.png)

当我们修改某处代码的时候服务就会自动重启

![image-20230814191051342](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141910300.png)

>  总结：
>
>  1.  本地软件包和全局软件包区别？
>      -  本地软件包，作用在<font title='red'>当前项目，封装属性和方法</font>.
>      -  全局软件包，<font title='red'>本机</font>所有项目使用，<font title='red'>封装命令和工具</font>.
>  2.  nodemon作用？
>      -  替代node命令，检测代码更改，<font title='red'>自动重启</font>程序
>  3.  nodemon怎么用？
>      -  先确保安装`npm i nodemon -g` 
>      -  使用<font title='red'>nodemon执行目标js文件</font>.

## Node.js总结：

### Node.js模块化：

概念：每个文件当做一个模块，独立作用域，按需加载

使用：采用特定的标准语法导出和导入进行使用

<center>CommonJS 标准</center>

|      | 导出                | 导入                    |
| ---- | ------------------- | ----------------------- |
| 语法 | module.exports = {} | require(‘模块名或路径’) |

<center>ECMAScript标准</center>

| 1    | 导出                | 导入                                  |
| ---- | ------------------- | ------------------------------------- |
| 默认 | export default{}    | import 变量名 from ‘模块名或路径’     |
| 命名 | export 修饰定义语句 | import {同名变量} from ‘模块名或路径’ |

CommonJS 标准：一般应用在Node.js项目环境中

ECMAScript标准：一般应用在前端工程化项目中

### Node.js包

概念：把模块文件，代码文件，其它资料聚合成一个文件夹

项目包：编写项目需求和<font title='red'>业务逻辑</font>的文件夹

软件包：<font title='red'>封装工具和方法</font>进行使用的文件夹 (一般使用npm管理)

-  本地软件包：作用在<font title='red'>当前</font>项目，一般封装的<font title='red'>属性/方法</font>，供项目调用编写业务需求
-  全局软件包：作用在<font title='red'>所有</font>项目，一般封装的<font title='red'>命令/工具</font>，支撑项目运行

![image-20230814193605361](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141936232.png)

### Node.js常用命令

| 功能               | 命令                 |
| ------------------ | -------------------- |
| 执行JS文件         | node xxx.js          |
| 初始化package.json | npm init -y          |
| 下载本地软件包     | npm i 软件包名称     |
| 下载全局软件包     | npm i 软件包名称 -g  |
| 删除软件包         | npm uni 软件包名称   |
| 删除全局软件包     | npm uni 软件包名称-g |



---

# Webpack

<span alt='solid'>定义</span>：

本质上，webpack是一个用于现代JavaScript应用程序的 静态模块打包工具。当webpack处理应用程序时，它会在内部从一个或多个入口点构建一个 依赖图 (dependency graph)，然后将你项目中所需的每一个模块组合成一个或多个 bundles ，它们均为静态资源，用于展示你的内容。

<span alt='solid'>静态模块</span>：指的是编写代码过程中的html，css，js，img等固定内容的文件

<font title='red'>打包</font>：把静态模块内容，压缩，整合，转译等 (前端工程化)

-  把less / sass 转成css代码
-  把ES6+ 降级成 ES5
-  支持多种模块标准语法

**问题**：为何不学 vite?

**因为**：很多项目还是基于webpack构建，并为vue ，react 脚手架使用做铺垫！

![image-20230814195038448](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308141950849.png)

## 使用Webpack

**需求**：封装util包，校验手机号长度和验证码长度，在 <font title='red'>src / index.js</font> 中使用并打包观察

**步骤**：

1.  新建并初始化项目，编写业务<font title='red'>源代码</font>.

    ```js
    npm init -y
    ```

    1.  创建文件夹src / utils里面创建check.js

2.  下载webpack webpack-cli 到当前项目中 (版本独立) ，并<font title='red'>配置</font>局部自定义命令

    ```js
    npm i webpack webpack-cli --save-dev
    ```

    

    ```js
    "scripts": {
       //key为自定义命令，value为运行自定义命令后真正要触发的命令，后面是固定的叫webpack
       "build": "webpack"
    },
    ```

3.  运行<font title='red'>打包</font>命令，自动产生dist分发文件夹 (压缩和优化后，用于最终运行的代码)

    ```js
    npm run build
    ```

    ![image-20230814203041052](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142030731.png)

    执行完 npm run build 命令后 在src同级目录中会出现一个dist目录里面一个main.js最终运行的代码

    ![image-20230814203137756](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142031380.png)

    

    <blockquote alt='danger'>
    	<div>
          <p>
             <span><span alt='solid'>注意</span>：src目录中有index.js默认就会打包 ，如果没有执行 npm run build就会报错，其它位置的js文件需要修改入口文件位置下一章有讲解</span>
          </p>
       </div>
    </blockquote>
    
    
    
    

![Snipaste_2023-08-14_20-01-08](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142001753.png)

check.js

```js
//封装校验手机号长度和校验验证码长度的函数
export const chekPhone = phone => phone.length === 11
export const checkCode = code => code.length === 6
```



```js
/**
 * 目标1：体验webpack 打包过程
 */
// 1.1 准备项目和源码
import { checkPhone, checkCode } from './utils/check.js'
console.log(checkPhone('1230918203'))
console.log(checkCode('123098103'))
// 1.2准备webpack打包环境
// 执行命令：npm i webpack webpack-cli --save-dev
// webpack下载完毕后去package.json中的scripts中配置局部自定义命令
//1.3 运行配置的局部自定义命令打包观察效果 执行命令: npm run 自定义命令(build)
```

打包后main.js

```js
(()=>{"use strict";var e={d:(o,t)=>{for(var r in t)e.o(t,r)&&!e.o(o,r)&&Object.defineProperty(o,r,{enumerable:!0,get:t[r]})},o:(e,o)=>Object.prototype.hasOwnProperty.call(e,o),r:e=>{"undefined"!=typeof Symbol&&Symbol.toStringTag&&Object.defineProperty(e,Symbol.toStringTag,{value:"Module"}),Object.defineProperty(e,"__esModule",{value:!0})}},o={};e.r(o),e.d(o,{P:()=>t});const t=e=>6===e.length;console.log((0,o.checkPhone)("1230918203")),console.log(t("123098103"))})();
```



>  webpack默认打包入口是index.js，默认出口是 main.js

## 修改webpack打包入口和出口

[webpack配置](https://webpack.docschina.org/concepts/#entry)：影响webpack打包过程和结果

**步骤**：

1.  项目根目录，新建<font title='red'>webpack.config.js</font>配置文件

    ![image-20230814210705080](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142107448.png)

2.  导出<font title='red'>配置对象</font>，配置入口，出口文件的路径

    ```js
    const path = require('path')
    //导出配置对象
    module.exports = {
        // 设置入口
        entry: path.join(__dirname, 'src/login/index.js'),
        // 设置出口
        output: {
            path: path.resolve(__dirname, 'dist'),
            filename: './login/index.js'
        }
    }
    ```

3.  重新打包观察

    ```js
    npm run build
    ```

    ![image-20230814210854669](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142108510.png)

    执行完成后查看目录结构。dist目录中就生成了login文件夹里面有压缩打包后的最终执行index.js文件

    ![image-20230814211006996](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142110210.png)

###### webpack配置clean

而上次执行webpack打包的出口还存在下面加上一个配置就可以再打包时删除上一次打包的出口

```js
const path = require('path')

module.exports = {
    // 设置入口
    entry: path.join(__dirname, 'src/login/index.js'),
    // 设置出口
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: './login/index.js',
        clean: true //生成打包后内容之前，清空输出目录，此命令只在5.以上版本中适用
    }
}
```

再次打包观察效果

![image-20230814211349425](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142113995.png)

查看目录的变化 之前的出口被删除掉了

![image-20230814211415554](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142114088.png)

<span alt='solid'>注意</span>：只有和入口产生直接 / 间接的引入关系，才会被打包

![image-20230814205700730](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308142057201.png)

## 案例—用户登录—长度判读

**需求**：点击登录按钮，判断手机号和验证码长度

**步骤**：

1.  准备用户登录页面
2.  编写核心JS逻辑代码
3.  打包并手动复制网页到dist下，引入打包后的js，运行

![image-20230815150927533](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151509187.png)

<font title='red'>核心</font>：webpack打包后的代码，作用在前端网页中使用

![image-20230815150905346](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151509692.png)

创建骨架

![image-20230815161203866](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151612448.png)

编写软件包 str.js

```js
export const checkUserName = uname => uname.length >= 8
export const checkPwd = pwd => pwd.length >= 6
```

编写用户表单验证的项目包 index.js

```js
import { checkUserName, checkPwd } from '../utils/str.js'
/**
 * 目标：用户登录-长度判断案例
 * 3.1 准备用户登录页面
 * 3.2 编写核心JS逻辑代码
 * 3.3 打包并手动复制网页到dist下，引入打包后的js，运行
 */
// 3.2 编写核心JS逻辑代码
document.querySelector('.btn').addEventListener('click', function () {
    const phone = document.querySelector('.login-form [name=mobile]').value
    const code = document.querySelector('.login-form [name=code]').value
    if (!checkUserName(phone)) {
        console.log('手机号长度必须大于8')
        return
    }
    if (!checkPwd(code)) {
        console.log('验证码长度必须大于6')
        return
    }
    console.log('提交到服务器登录...')
})
```

将项目的登录页面拷贝到public目录中将里面的css，js引入全部去掉

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>黑马头条-数据管理平台</title>
</head>

<body>
  <!-- 警告框 -->
  <div class="alert info-box">
    操作结果
  </div>
  <!-- 登录页面 -->
  <div class="login-wrap">
    <div class="title">黑马头条</div>
    <div>
      <form class="login-form">
        <div class="item">
          <input type="text" class="form-control" name="mobile" placeholder="请输入手机号" value="13888888888">
        </div>
        <div class="item">
          <input type="text" class="form-control" name="code" placeholder="默认验证码246810" value="246810">
        </div>
        <div class="item">
          <button type="button" class="btn btn-primary btn">登 录</button>
        </div>
      </form>
    </div>
  </div>
</body>
</html>
```

执行命令：`npm i webpack webpack-cli --save-dev` 下载webpack进行打包

![image-20230815161647591](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151616852.png)

自定义打包命令：

```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
```

修改入口和出口

```js
const path = require('path')

module.exports = {
    entry: path.join(__dirname,'src/login/index.js'),
    output: {
        path: path.join(__dirname,'dist'),
        filename: './login/index.js',
        clean: true
    }
}
```

执行命令进行打包：`npm run build` 注意终端的所在目录需要到项目根目录执行否则报错

![image-20230815161937931](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151619083.png)

将public/index.html登录页面拷贝到dist目录下

![image-20230815162111843](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151621806.png)

在dist下的index.html页面代码中引入dist/login/index.js下的压缩打包后的js文件

```html
<script src="./login/index.js"></script>
```

打开dist下的登录页面进行测试 (简单测试，还待测试 其它判断条件 )

![image-20230815162257063](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151622400.png)

## 自动生成HTML文件

插件 html-webpack-plugin：在webpack打包时生成 html 文件

[查看文档](https://webpack.docschina.org/plugins/html-webpack-plugin/) 

**步骤**：

1.  下载<font title='red'> html-webpack-plugin</font> 本地软件包

    ```sh
    npm i html-webpack-plugin --save-dev
    ```

2.  <font title='red'>配置</font> webpack.config.js 让webpack 拥有插件功能

    ```js
    const HtmlWebpackPlugin = require('html-webpack-plugin')
    
    module.exports = {
       plugins: [
          new HtmlWebpackPlugin({
             template: './public/login.html', //模版文件
             filename: './login/index.html' //输出文件
          })
       ]
    }
    ```

3.  重新<font title='red'>打包</font>观察效果

让webpac拥有更多功能三部曲：找包，下包，配置包

**演示**：

1 下载软件包`npm i html-webpack-plugin --save-dev`

![image-20230815165802909](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151658121.png)

2 <font title='red'>配置</font> webpack.config.js 让webpack 拥有插件功能

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry: path.join(__dirname, 'src/login/index.js'),
    output: {
        path: path.join(__dirname, 'dist'),
        filename: './login/index.js',
        clean: true
    },
    // 插件([给webpack提供更多功能])
    plugins: [
        //实例化上面require导入的对象
        new HtmlWebpackPlugin({
            // 模版文件
            template: path.join(__dirname, 'public/index.html'),
            // 输出文件
            filename: path.join(__dirname, 'dist/login/index.html')
        })
    ]
}
```

3 打包查看效果

![image-20230815165921526](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151659827.png)

查看打包后的index.html文件

![image-20230815170003695](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151700950.png)

测试功能

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151701941.gif)

## 打包CSS代码

**记载器 css-loader**：解析css代码

**注意**：<span alt='solid'>webpack默认只能识别js代码，要想识别更多代码就需要添加另一个软件包</span>.

**加载器 style-loader**：把解析后的css代码插入到DOM

css样式代码打包输出到 index.js 出口当中

[查看文档](https://webpack.docschina.org/loaders/css-loader/) 

**步骤**：

1.  准备css文件<font title='red'>代码</font>引入到 src/login/index.js中 (压缩转译处理等)

    ```css
    // 1.准备css代码，并引入到js中
    import './index.css'
    ```

    1.  如果项目中用到了Bootstrap则下载本地软件包并引入

        ![image-20230815173642332](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151736495.png)

        ```sh
        // 1.准备css代码，并引入到js中
        // 2.页面用到了bootstrap则引入，先引入依赖的样式
        import '../../node_modules/bootstrap/dist/css/bootstrap.min.css'
        import './index.css'
        ```

2.  <font title='red'>下载</font> css-loader 和 style-loader 本地软件包

    ```sh
    npm i css-loader --save-dev
    npm i style-loader --save-dev
    ```

3.  <font title='red'>配置</font> webpack.config.js 让 webpack 拥有该加载器功能

    ```js
    module.exports = {
       module: {
          rules: [
             {
                test: /\.css$/i,
                use: ['style-loader' , 'css-loader']
             }
          ]
       }
    }
    ```

4.  打包后观察效果

1 引入css文件到打包入口

```css
// 1.准备css代码，并引入到js中
// 2.页面用到了bootstrap则引入，先引入依赖的样式
import '../../node_modules/bootstrap/dist/css/bootstrap.min.css'
import './index.css'
```

2 执行命令打包：`npm run build` 

![image-20230815173203005](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151732258.png)

3 打包后的目录结构

![image-20230815174430228](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151744373.png)

3.1 发现在dist中并没有找到打包后的css文件，因为css样式代码打包输出到 index.js 出口当中，查看dist/login/index.js的变化：

3.2 可以看到 里面多了很多代码

```js
(()=>{"use strict";var r={666:(r,o,t)=>{t.d(o,{Z:()=>D});var a=t(81),e=t.n(a),i=t(645),n=t.n(i),s=t(667),l=t.n(s),d=new URL(t(770),t.b),b=new URL(t(711),t.b),m=new URL(t(199),t.b),p=new URL(t(204),t.b),c=new URL(t(931),t.b),g=new URL(t(486),t.b),f=new URL(t(609),t.b),v=new URL(t(469),t.b),u=new URL(t(991),t.b),h=new URL(t(122),t.b),x=new URL(t(144),t.b),w=new URL(t(221),t.b),y=new URL(t(956),t.b),k=new URL(t(460),t.b),z=new URL(t(321),t.b),j=new URL(t(281),t.b),R=new URL(t(254),t.b),B=new URL(t(647),t.b),A=new URL(t(692),t.b),G=n()(e()),L=l()(d),M=l()(b),U=l()(m),$=l()(p),S=l()(c),X=l()(g),Y=l()(f),C=l()(v),T=l()(u),E=l()(h),I=l()(x),Z=l()(w),q=l()(y),N=l()(k),P=l()(z),F=l()(j),O=l()(R),_=l()(B),H=l()(A);G.push([r.id,`@charset "UTF-align:left!important}.text-xl-end{text-align:right!important}.text-xl-center{text-align:center!important}}@media (min-width:1400px){.float-xxl-start{float:left!important}.float-xxl-end{float:right!important}.float-xxl-none{float:none!important}.object-fit-xxl-contain{-o-object-fit:contain!important;object-fit:contain!important}.object-fit-xxl-cover{-o-object-fit:cover!important;object-fit:cover!important}.object-fit-xxl-fill{-o-object-fit:fill!important;object-fit:fill!important}.object-fit-xxl-scale{-o-object-fit:scale-down!important;object-fit:scale-down!important}.object-fit-xxl-none{-o-object-fit:none!important;object-fit:none!important}.d-xxl-inline{display:inline!important}.d-xxl-inline-block{display:inline-block!important}.d-xxl-block{display:block!important}.d-xxl-grid{display:grid!important}.d-xxl-inline-grid{display:inline-grid!important}.d-xxl-table{display:table!important}.d-xxl-table-row{display:table-row!important}.d-xxl-table-cell{displayr=t(379),o=t.n(r),a=t(795),e=t.n(a),i=t(569),n=t.n(i),s=t(565),l=t.n(s),d=t(216),b=t.n(d),m=t(589),p=t.n(m),c=t(666),g={};g.styleTagTransform=p(),g.setAttributes=l(),g.insert=n().bind(null,"head"),g.domAPI=e(),g.insertStyleElement=b(),o()(c.Z,g),c.Z&&c.Z.locals&&c.Z.locals;var f=t(386),v={};v.styleTagTransform=p(),v.setAttributes=l(),v.insert=n().bind(null,"head"),v.domAPI=e(),v.insertStyleElement=b(),o()(f.Z,v),f.Z&&f.Z.locals&&f.Z.locals,document.querySelector(".btn").addEventListener("click",(function(){const r=document.querySelector(".login-form [name=mobile]").value,o=document.querySelector(".login-form [name=code]").value;r.length>=8?o.length>=6?console.log("提交到服务器登录..."):console.log("验证码长度必须大于6"):console.log("手机号长度必须大于8")}))})()})();
```

4 查看页面效果：

![image-20230815174502959](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151745252.png)

## 优化-提取css代码

**好处**：css文件可以被浏览器缓存，减少js文件体积

<span alt='solid'>插件 mini-css-extract-plugin</span>：提取css代码

<blockquote alt='danger'>
	<div>
      <p>
         <span><font title=red>注意</font>：不要同时使用 style-loader 和 mini-css-extract-plugin。</span>
      </p>
   </div>
</blockquote>

**步骤**：

1.  <font title='red'>下载</font> mini-css-extract-plugin 本地软件包

    ```sh
    npm i mini-css-extract-plugin --save-dev
    ```

2.  <font title='red'>配置</font> webpack.config.js 让 webpack 拥有该插件功能

    ```js
    const MiniCssExtractPlugin = require('mini-css-extract-plugin')
    
    module.exports = {
       module: {
          rules: [
             {
                test: /\.css$/i,
                // use: ['style-loader','css-loader'] 使用下面就不要使用这个
                use: [MiniCssExtractPlugin.loader,'css-loader']
             }
          ]
       }
    },
    plugins: [
       new MiniCssExtractPlugin()
       //或者使用
       //new MiniCssExtractPlugin({
       //	filename：指定提取出来的路径到dist目录下
       //})
    ]
    ```

    
    
    执行打包命令：`npm run build` 查看目录变化 多个一个main.css文件而且这个文件已经在index.html中被引入了
    
    ![image-20230815190629194](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151906677.png)

## 优化-压缩过程(在提取的前提下)

在上一章，提取css压缩代码呢，我们自己写的一部分代码是没有被压缩的如下：

```css
/* 提示框 */
.alert{
  width: 400px;
  position: fixed;
  top: 50px;
  left: 50%;
  transform: translateX(-50%);
  display: none;
}
.show{
  display: block !important;
}

/* 登录 */
.login-wrap {
  width: 400px;
  padding: 20px;
  background-color: #fff;
}
.login-wrap .title{
  font-size: 28px;
  text-align: center;
  color: #fc6627;
}
.login-wrap .login-form {
  margin-top: 20px;
}
input::-webkit-input-placeholder{
  color:#dcdfe6 !important;
}
.login-form .btn{
  width: 100%;
  background-color: #66b1ff;
  border: 1px solid #66b1ff;
}
.login-form .item:nth-child(n + 1) {
  margin-top: 20px;
}
```

**问题**：css代码提取后没有压缩

**解决**：使用 css-minimizer-webpack-plugin 插件

步骤：

1.  <font title='red'>下载</font> css-minimizer-webpack-plugin 本地软件包

    ```sh
    npm i css-minimizer-webpack-plugin --save-dev
    ```

2.  <font title='red'>配置</font> webpack.config.js 让 webpack 拥有该功能

    ```js
    const CssMinimizerPlugin = require('css-minimizer-webpack-plugin')
    
    module.exports = {
       //优化
       optimization: {
          minimizer: [
             //在 webpack@5 中，你可以使用 `...` 语法来扩展现有的 minimizer (即 `terser-webpack-plugin`) ，将下一行取消注释 (保证JS代码还能被压缩处理) 如果不用 `...` 则需要自己下载terser-webpack-plugin插件require导入然后new对象
             `...`,
             new CssMinimizerPlugin()
          ]
       }
    }
    ```

3.  重新打包观察效果

**演示**：

1 执行命令安装本地软件：`npm i css-minimizer-webpack-plugin --save-dev` 

![image-20230815204547355](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308152045530.png)

2 <font title='red'>配置</font> webpack.config.js 让 webpack 拥有该功能

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin')

module.exports = {
    entry: path.join(__dirname, 'src/login/index.js'),
    output: {
        path: path.join(__dirname, 'dist'),
        filename: './login/index.js',
        clean: true
    },
    // 插件([给webpack提供更多功能])
    plugins: [
        //实例化上面require导入的对象
        new HtmlWebpackPlugin({
            // 模版文件
            template: path.join(__dirname, 'public/index.html'),
            // 输出文件
            filename: path.join(__dirname, 'dist/login/index.html')
        }),
        // 生成css文件
        new MiniCssExtractPlugin()
    ],
    //加载器 (让webpack 识别更多模块内容)
    module: {
        rules: [
            {
                test: /\.css$/i,
                // use: ['style-loader', 'css-loader']
                use: [MiniCssExtractPlugin.loader, 'css-loader']
            }
        ]
    },
    // 优化
    optimization: {
        // 最小化
        minimizer: [
            //在webpack@5中，你可以使用 `...` 语法来扩展现有的minimizer(即
            // `terser-webpack-plugin`) ，将下一行注释 如果不使用 `...` 则需要自动下载terset-webpack-plugin插件导入并实例化对象
            `...`,//保证js的代码还能压缩
            new CssMinimizerPlugin()
        ]
    }
}
```

3 执行命令打包观察效果：`npm run build` 

![image-20230815204710801](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308152047939.png)

4 查看优化-压缩后的css代码

![image-20230815204745065](./NodeJs%E4%B8%8EWebPack.assets/202308152047192.png)

后面的代码都被压缩了

## 打包less代码

加载器 less-loader：把less代码编译为css代码

**步骤**：

1.  新建less<font title='red'>代码</font> (设置背景图) 并引入到 src/login/index.js中 

    -  <span alt='solid'>注意</span>：引入的less文件不要和其它css文件同名否则打包后就会被替换掉的。

    ```css
    // 1.新建less代码 (设置背景图) 并引入到 src/login/index.js中
    import './indexTest.less'
    ```

2.  <font title='red'>下载</font> less 和 less-loader 本地软件包

    ```sh
    npm i less-loader --save-dev
    ```

3.  <font title='red'>配置</font> webpack.config.js让webpack拥有功能

    ```js
    module.exports = {
       module: {
          rules: [
             {
                test: /\.less$/i,
                use: [MiniCssExtractPlugin.loader,'css-loader','less-loader']
             }
          ]
       }
    }
    ```

    

**演示**：

1 引入less文件到打包入口

```css
// 1.新建less代码 (设置背景图) 并引入到 src/login/index.js中
import './indexTest.less'
```

2 执行命令安装本地软件包：`npm i less-loader --save-dev` 

![image-20230815211518226](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308152115393.png)

3 <font title='red'>配置</font> webpack.config.js让webpack拥有功能

```js
//加载器 (让webpack 识别更多模块内容)
module: {
   rules: [
      {
         test: /\.css$/i,
         // use: ['style-loader', 'css-loader']
         use: [MiniCssExtractPlugin.loader, 'css-loader']
      },
      {
         test: /\.less$/i,
         //compiles Less to CSS
         use: [
               //compiles Less to CSS
            	//'style-loader', MiniCssExtractPlugin.loader与该项不能混用如果上面用MiniCssExtractPlugin.loader则将其写在下面。
            	MiniCssExtractPlugin.loader,
               'css-loader',
               'less-loader'
              ]
      }
   ]
},
```

4 执行命令打包：`npm run build` 

![image-20230815211644125](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308152116347.png)

5 查看目录变化

![image-20230816085710401](./NodeJs%E4%B8%8EWebPack.assets/202308160857869.png)

6 打开压缩后的页面查看效果

![image-20230816090133157](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308160901522.png)

## 打包图片

**资源模块**：Webpack5内置资源模块 (字体，图片等) 打包，无需额外 loader

[查看文档](https://webpack.docschina.org/guides/asset-modules/) 

**步骤**：

1.  <font title='red'>配置</font> webpack.config.js 让 webpack 拥有打包图片功能

    ```js
    module.exports = {
       module: {
          rules: [
             {
                test: /\.(png|jpg|jpeg|gif)$/i,
                type: 'asset',
                generator: {
                   filename: 'assets/[hash][ext][query]'
                }
             }
          ]
       }
    }
    ```

    -  占位符 [hash] 对模块内容做算法计算，得到映射的数字字母组合的字符串
    -  占位符 [ext] 使用当前模块原本的占位符，例如：.png / .jpg 第字符串
    -  占位符 [query] 保留引入文件时代码中查询参数 (只有RUL下生效)

2.  打包观察效果

**注意**：判断临界值默认为 <font title='red'>8KB</font>.

-  大于 8KB 文件：发送一个单独的文件并导出URL地址

![image-20230816091613254](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308160916629.png)

-  小于 8KB 文件：导出一个 data URI (base64字符串)

![image-20230816091732872](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308160917109.png)

**演示**：

1 编写入口文件index.js 引入图片资源的地址，创建一个img标签将src属性设置为该图片然后追加到页面中

```js
/**
 * 目标：打包资源模块(图片处理)
 * 1.创建img标签并动态添加到页面，配置 webpack.config.js
 * 2.打包后观察效果和区别
 */
// 注意：js中引入本地图片资源要用 import 方式 (如果是网络图片http地址，字符串可以直接写)
import imgObj from './assets/logo.png'
const theImg = document.createElement('img')
theImg.src = imgObj
document.querySelector('.login-wrap').appendChild(theImg)
```

2 配置 webpack.config.js 让 webpack  拥有该功能

```js
{
   test: /\.(png|jpg|jpeg|gif)$/i,
      type: 'asset',
         generator: {
            filename: 'asset/[hash][ext][query]'
         }
}
```

3 打包查看效果

![image-20230816093459035](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308160935424.png)

页面效果

![image-20230816093523711](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308160935070.png)

## 用户登录—完成功能

**需求**：完成登录功能的核心流程，以及Alert警告框使用

**步骤**：

1.  使用npm下载axios (体验npm作用在前端项目中)
2.  准备并修改 utils 工具包 源代码 <font title='red'>导出</font> 实现函数
3.  <font title='red'>导入</font>并编写逻辑代码，打包后运行观察效果

<center>核心思路流程图</center>

![image-20230816094207386](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308160942898.png)

**演示**：

request.js

```js
//导入npm安装到本地的axios
//import axios from 'axios' 两种方式可供使用
const axios = require('axios')
// axios 公共配置
// 基地址
axios.defaults.baseURL = 'http://geek.itheima.net'
//设置axios请求拦截器
axios.interceptors.request.use(function (config) {
    const token = localStorage.getItem('token')
    token && (config.headers.Authorization = `Bearer ${token}`)
    return config
}, function (err) {
    return Promise.reject(err)
})
//设置axios响应拦截器
axios.interceptors.response.use(function (config) {
    const result = config.data
    return result
}, function (err) {
    if(err?.response?.status === 401) {
        alert('token过期，请重新登录')
        localStorage.removeItem('token')
        location.href = './login/index.html'
    }
    return Promise.reject(err)
})
//默认导出axios
export default axios
```

alert.js

```js
// 弹窗插件
// 需要先准备 alert 样式相关的 DOM
/**
 * BS 的 Alert 警告框函数，2秒后自动消失
 * @param {*} isSuccess 成功 true，失败 false
 * @param {*} msg 提示消息
 */
//命名导出 函数
export function myAlert(isSuccess, msg) {
  const myAlert = document.querySelector('.alert')
  myAlert.classList.add(isSuccess ? 'alert-success' : 'alert-danger')
  myAlert.innerHTML = msg
  myAlert.classList.add('show')

  setTimeout(() => {
    myAlert.classList.remove(isSuccess ? 'alert-success' : 'alert-danger')
    myAlert.innerHTML = ''
    myAlert.classList.remove('show')
  }, 2000)
}
```

index.js 入口 看不懂str.js往上面的案例看就能看到了

```js
/**
 * 目标：完成登录功能
 * 1.使用npm下载 axios (体验npm作用在前端项目中)
 * 2.准备并修改，utils 工具包资源代码导出实现函数
 * 3.导入并编写逻辑代码，打包后运行观察效果
 */
import { checkUserName, checkPwd } from '../utils/str.js'
// 3.导入并编写逻辑代码，打包后运行观察效果
import myAxios from '../utils/request.js'
import { myAlert } from '../utils/alert.js'

document.querySelector('.btn').addEventListener('click', function () {
    const phone = document.querySelector('.login-form [name=mobile]').value
    const code = document.querySelector('.login-form [name=code]').value
    if (!checkUserName(phone)) {
        myAlert(false, '手机号长度必须大于8')
        console.log('手机号长度必须大于8')
        return
    }
    if (!checkPwd(code)) {
        myAlert(false, '验证码长度必须大于6')
        console.log('验证码长度必须大于6')
        return
    }
    myAxios({
        url: '/v1_0/authorizations',
        method: 'POST',
        data: {
            mobile: phone,
            code: code
        }
    }).then(result => {
        console.log(result)
        localStorage.setItem('token', result.data.token)
        myAlert(true, '登录成功')
    }).catch(err => {
        myAlert(false, err.response.data.message)
    })
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161022213.gif)

使用npm下载的软件包最终是怎么用作在前端的项目里的？

1.  先用import导入最终用webpack打包输出

## 搭建开发环境

**问题**：之前改代码，需要重新打包才能运行查看最终效果，效率很低。

**开发环境**：配置 webpack-dev-server 快速开发应用程序

**作用**：启动 web 服务，<font title='red'>自动</font>检测代码变化，<font title='red'>热更新</font>到网页

<span alt='solid'>注意</span>：dist目录和打包内容是在内存里 (更新快)

<span alt='solid'>注意1</span>：webpack-dev-server 借助 http 模块创建 8080 默认 web 服务

<span alt='solid'>注意2</span>：默认以 public 文件夹作为服务器目录

<span alt='solid'>注意3</span>：webpack-dev-server 根据配置，打包相关代码在内存当中，以output.path的值作为服务根目录(所以可以直接自己拼接访问dist目录下内容)但是注意，这里的dist是内存当中的跟项目结构里面的dist没有一分关系

-  根目录有两个

   1 默认的静态资源：public/login.html

   2 webpack设置时的出口

   ```js
   output: {
      path: path.join(__dirname, 'dist'),
         filename: './login/index.js',
            clean: true
   },
   ```

-  所以相当于也可以访问 dist 下的目录页面 <a href="#演示第二种根目录方式">查看演示</a>

**步骤**：

1.  <font title='red'>下载</font> webpack-dev-server 软件包到当前项目

    ```sh
    npm i webpack-dev-server --save-dev
    ```

2.  设置模式为 <font title='red'>开发模式</font>，并配置 <font title='red'>自定义命令</font>.

    ```js
    module.exports = {
       mode: 'development'
    }
    ```

    

    ```json
    "scripts"： {
       "build": "webpack",
       "dev": "webpack server --open"
    }
    ```

3.  使用 npm run dev 来启动开发服务器，试试热更新效果

    

    **演示**：

    1 安装<font title='red'>软件包</font>到本地执行命令：`npm i webpack-dev-server --save-dev` 

    ![image-20230816104513328](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161045824.png)

    2 <font title='red'>配置</font>自定义命令

    ```json
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "build": "webpack",
        "dev": "webpack serve --open"
      },
    ```

    

    3 <font title='red'>设置打包模式</font>为开发者模式

    ```js
    const path = require('path')
    const HtmlWebpackPlugin = require('html-webpack-plugin')
    const MiniCssExtractPlugin = require('mini-css-extract-plugin')
    const CssMinimizerPlugin = require('css-minimizer-webpack-plugin')
    
    module.exports = {
        //设置打包模式(development 开发者模式-使用相关内置优化)
        mode: 'development',
        entry: path.join(__dirname, 'src/login/index.js'),
        output: {
            path: path.join(__dirname, 'dist'),
            filename: './login/index.js',
            clean: true
        },
        // 插件([给webpack提供更多功能])
        plugins: [
            //实例化上面require导入的对象
            new HtmlWebpackPlugin({
                // 模版文件
                template: path.join(__dirname, 'public/index.html'),
                // 输出文件
                filename: path.join(__dirname, 'dist/login/index.html')
            }),
            // 生成css文件
            new MiniCssExtractPlugin()
        ],
        //加载器 (让webpack 识别更多模块内容)
        module: {
            rules: [
                {
                    test: /\.css$/i,
                    // use: ['style-loader', 'css-loader']
                    use: [MiniCssExtractPlugin.loader, 'css-loader']
                },
                {
                    test: /\.less$/i,
                    //compiles Less to CSS
                    use: [
                        MiniCssExtractPlugin.loader,
                        'css-loader',
                        'less-loader'
                    ]
                },
                {
                    test: /\.(png|jpg|jpeg|gif)$/i,
                    type: 'asset',
                    generator: {
                        filename: 'asset/[hash][ext][query]'
                    }
                }
            ]
        },
        // 优化
        optimization: {
            // 最小化
            minimizer: [
                //在webpack@5中，你可以使用 `...` 语法来扩展现有的minimizer(即
                // `terser-webpack-plugin`) ，将下一行注释 如果不使用 `...` 则需要自动下载terset-webpack-plugin插件导入并实例化对象
                `...`,//保证js的代码还能压缩
                new CssMinimizerPlugin()
            ]
        }
    }
    ```

4 执行命令：`npm run dev` 启动开发服务器，尝试热更新效果

![image-20230816105321644](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161053988.png)

启动成功了，会自动跳转到一个页面 网址为：localhost:8080 的一个页面 效果如下

![image-20230816105417430](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161054035.png)

为什么是这个页面呢 ， 首先看下目录结构：

![image-20230816105448840](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161054221.png)

这个页面会自动查找public目录下的index.html文件，如果有则作为页面打开，如果没有则效果如下，比方说将public中的index.html改名为login.html，并且记得改一下 webpack 的入口名。

webpack.config.js 文件

```js
// 插件([给webpack提供更多功能])
plugins: [
   //实例化上面require导入的对象
   new HtmlWebpackPlugin({
      // 模版文件
      // template: path.join(__dirname, 'public/index.html'),
      template: path.join(__dirname, 'public/login.html'),
      // 输出文件
      // filename: path.join(__dirname, 'dist/login/index.html')
      filename: path.join(__dirname, 'dist/login/login.html')
   }),
   // 生成css文件
   new MiniCssExtractPlugin()
],
```

刷新页面，正好也尝试了一下热更新，不用重新打包直接刷新页面很快 它的默认页面就改变了。

![image-20230816105904974](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161104539.png)

###### 演示第二种根目录方式

演示第二种根目录的请求方式和效果

```js
// 出口
output: {
   path: path.join(__dirname, 'dist'),
      filename: './login/index.js',
         clean: true
},
```

复制出口filename的目录：/login/index.js，到浏览器的地址栏框中粘贴进去回车访问，就可以访问到页面了。

![image-20230816112357119](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161123517.png)

## 打包模式

**打包模式**：告知 webpack 使用相应模式的内置优化

**分类**：

| 模式名称 | 模式名字    | 特点                             | 场景     |
| -------- | ----------- | -------------------------------- | -------- |
| 开发模式 | development | 调试代码，实时加载，模块热替换等 | 本地开发 |
| 生产模式 | production  | 压缩代码，资源优化，更轻量等     | 打包上线 |

**设置**：

<span alt='solid'>方式1</span>：在webpack.config.js配置文件设置mode选项

```js
module.exports = {
     //设置打包模式(development 开发者模式-使用相关内置优化)
    // 在package.json中设置两种不同的打包模式的自定义命令的话这里就可以注释掉了
    mode: 'development',
}
```

<span alt='solid'>方式2</span>：在package.json命令行设置mode参数

```json
"scripts": {
   "test": "echo \"Error: no test specified\" && exit 1",
   "build": "webpack --mode=production",
   "dev": "webpack serve --open --mode=development"
},
```

<font title='red'>注意</font>：命令行设置的优先级高于配置文件中的，推荐用命令行设置

### 打包模式的应用

需求：在开发模式下用 style-loader 内嵌更快，在生产模式下提取css代码

方案1：webpack.config.js 配置导出函数，但是局限性大 (只接受2种模式)

方案2：借助 cross-env (跨平台通用) 包命令，设置参数区分环境

<span alt='solid'>方案3</span>：配置不同的 webpack.config.js (使用多种模式差异性较大情况)

**步骤**：

1.  <font title='red'>下载</font> cross-env 软件包到当前项目

    ```sh
    npm i cross-env --save-dev
    ```

2.  <font title='red'>配置</font>自定义命令，传入参数名和值  (会绑定到 <font title='red'>prcess.env</font> 对象下)

    ```json
    "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "build": "cross-env NODE_ENV=production webpack --mode=production",
       "dev": "cross-env NODE_ENV=development webpack server --open --mode=development"
    },
    ```

3.  在 webpack.config.js 区分不同环境 <font title='red'>使用</font> 不同配置

    ```js
    // 插件([给webpack提供更多功能])
    plugins: [
       //实例化上面require导入的对象
       new HtmlWebpackPlugin({
          // 模版文件
          // template: path.join(__dirname, 'public/index.html'),
          template: path.join(__dirname, 'public/login.html'),
          // 输出文件
          // filename: path.join(__dirname, 'dist/login/index.html')
          filename: path.join(__dirname, 'dist/login/index.html')
       }),
       // 生成css文件
       new MiniCssExtractPlugin()
    ],
       //加载器 (让webpack 识别更多模块内容)
       module: {
          rules: [
             {
                test: /\.css$/i,
                // use: ['style-loader', 'css-loader']
                // use: [MiniCssExtractPlugin.loader, 'css-loader']
                //对css文件处理的loader根据package.json中对 cross-env设置的环境变量做一个区分
                // 在use:配置中使用一个条件表达式
                //process.env是Node.js环境中内置的一个环境变量调用在package.json中配置的属性
                //判断在执行命令的时候给NODE_ENV赋予了哪个模式 比如判断是否为 development 则运行 style-loader
                // 如果是打包，上线的生产模式则 NODE_ENV的值会被赋予 production 则会使用 提取css代码插件 和 css-loader
                use: [process.env.NODE_ENV === 'development' ? 'style-loader' :
                      MiniCssExtractPlugin.loader , 'css-loader']
             },
             {
                test: /\.less$/i,
                //compiles Less to CSS
                // less代码也会被转换为css代码所以也需要设置process.env.NODE_ENV的不同模式的打包配置
                use: [
                   process.env.NODE_ENV === 'development' ? 'style-loader' : MiniCssExtractPlugin.loader,
                   'css-loader',
                   'less-loader'
                ]
             },
    ```

4.  重新打包观察效果

执行命令：npm run dev

![image-20230816135748602](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161357558.png)

项目dist目录中不会产生提取出来的css文件

![image-20230816135826271](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161358831.png)

执行命令：npm run build

![image-20230816135912066](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161359807.png)

项目dist目录中会提取出css代码文件

![image-20230816135937211](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161359621.png)

## 前端—注入环境变量

**需求**：<font title='red'>前端</font>项目中，开发模式下打印语句生效，生产模式下打印语句失效

**问题**：cross-env 设置的只在Node.js 环境生效，前端代码无法访问 process.env.NODE_ENV

<span alt='solid'>解决</span>：使用 webpack 内置的 DefinePlugin 插件

**作用**：在编译时，将前端代码中匹配的变量名，替换为值或表达式

```js
const webpack = require('webpack')

module.exports = {
   plugins: [
      new webpack.DefinePlugin({
         //key 是注入到打包后的前端 JS 代码中作为全局变量
         //value 是变量对应的值 (在 cross-env 注入在 node.js 中的环境变量字符串)
         'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
      })
   ]
}
```

index.js 入口 文件

```js
/**
 * 目标：前端—注入环境变量
 * 需求：前端项目代码中，开发模式下打印语句生效，生产模式下打印语句失效
 */

if (process.env.NODE_ENV === 'production')
    console.log = function () { }
console.log('开发模式下生效，生产模式下失效')
```

执行命令：npm run dev 开发模式下 效果

![image-20230816143347527](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161433974.png)

可以看到console.log正常打印了一段话

---

执行命令：npm run build 生产模式下 效果

![image-20230816143600450](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161436583.png)

可以看到控制台中没有输出 那段话

>  向前端注入环境变量，我们就可以让相同的一份代码，在不同的环境下来出现不同的效果。

## 开发环境调错—source map

**问题**：代码被==压缩==和==混淆==，<span alt='solid'>无法正确定位源代码位置</span> (行数和列数)

**source map**：可以准确追踪 <span alt='solid'>error</span> 和 <span alt='solid'>warning</span> 在==原始代码的位置==。

**设置**：webpack.config.js 配置 devtool 选项

**演示问题**：

index.js 入口文件

```js
/**
 * 目标：source-map 调试代码
 * 问题：error 和 warning 代码的位置和源代码对不上，不方便我们调试
 * 解决：启动webpack 的 source-map 资源地图功能
 * 1.在 webpack.config.js 配置 devtool 选项和值开启功能 (注意：只在开发环境下使用)
 * 2.代码中造成错误，并在开发服务器环境下查看效果
 */
//在consolee中多打一个e让程序报错
consolee.warning('123')
```

执行命令：npm run dev 开发模式下启动

![image-20230816144923391](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161449850.png)

实际代码错误的位置：

![image-20230816144954803](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161449528.png)

被压缩后的js报错后控制台打印出的错误位置不是原本的错误位置很难找到错误的代码对其修改

**解决问题演示**：

配置 webpack.config.js 

资源地图只能在开发模式下使用(development) 我们需要将 所有配置 提取到 config变量 中存储，然后下面判断是否为发开模式(development) 如果是则 调用config 追加一个地图资源的配置项 (config.devtool = 'inline-source-map') 

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin')
const webpack = require('webpack')

const config = {
    //设置打包模式(development 开发者模式-使用相关内置优化)
    // 在package.json中设置两种不同的打包模式的自定义命令的话这里就可以注释掉了
    // mode: 'development',
    // 入口
    entry: path.join(__dirname, 'src/login/index.js'),
    // 出口
    output: {
        path: path.join(__dirname, 'dist'),
        filename: './login/index.js',
        clean: true
    },
    // 插件([给webpack提供更多功能])
    plugins: [
        //实例化上面require导入的对象
        new HtmlWebpackPlugin({
            // 模版文件
            // template: path.join(__dirname, 'public/index.html'),
            template: path.join(__dirname, 'public/login.html'),
            // 输出文件
            // filename: path.join(__dirname, 'dist/login/index.html')
            filename: path.join(__dirname, 'dist/login/index.html')
        }),
        // 生成css文件
        new MiniCssExtractPlugin(),
        // 配置webpack
        new webpack.DefinePlugin({
            'process.env.NDE_ENV': JSON.stringify(process.env.NODE_ENV)
        })
    ],
    //加载器 (让webpack 识别更多模块内容)
    module: {
        rules: [
            {
                test: /\.css$/i,
                // use: ['style-loader', 'css-loader']
                // use: [MiniCssExtractPlugin.loader, 'css-loader']
                //对css文件处理的loader根据package.json中对 cross-env设置的环境变量做一个区分
                // 在use:配置中使用一个条件表达式
                //process.env是Node.js环境中内置的一个环境变量调用在package.json中配置的属性
                //判断在执行命令的时候给NODE_ENV赋予了哪个模式 比如判断是否为 development 则运行 style-loader
                // 如果是打包，上线的生产模式则 NODE_ENV的值会被赋予 production 则会使用 提取css代码插件 和 css-loader
                use: [process.env.NODE_ENV === 'development' ? 'style-loader' :
                 MiniCssExtractPlugin.loader , 'css-loader']
            },
            {
                test: /\.less$/i,
                //compiles Less to CSS
                // less代码也会被转换为css代码所以也需要设置process.env.NODE_ENV的不同模式的打包配置
                use: [
                    process.env.NODE_ENV === 'development' ? 'style-loader' : MiniCssExtractPlugin.loader,
                    'css-loader',
                    'less-loader'
                ]
            },
            {
                test: /\.(png|jpg|jpeg|gif)$/i,
                type: 'asset',
                generator: {
                    filename: 'asset/[hash][ext][query]'
                }
            }
        ]
    },
    // 优化
    optimization: {
        // 最小化
        minimizer: [
            //在webpack@5中，你可以使用 `...` 语法来扩展现有的minimizer(即
            // `terser-webpack-plugin`) ，将下一行注释 如果不使用 `...` 则需要自动下载terset-webpack-plugin插件导入并实例化对象
            `...`,//保证js的代码还能压缩
            new CssMinimizerPlugin()
        ]
    }
}

//开发环境下使用 source-map选项
if(process.env.NODE_ENV === 'development') {//判断运行环境
    config.devtool = 'inline-source-map'
}

module.exports = config
```

执行命令：npm run dev 查看效果

![image-20230816145805071](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161458587.png)

报错信息的最上面的不重要我们直接找 index.js 报错的信息 查看位置

![image-20230816145847231](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161458715.png)

如实际报错位置相同

![image-20230816145916952](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161459869.png)

## 解析别名alias

**解析别名**：配置模块如何解析，创建 import 引入路径的别名，来确保模块引入变得简单

**例如**：原来路径如图，比较长而且相对路径不安全

**解决**：在 webpack.config.js 中配置解析别名 @ 来代表 src 绝对路径

```js
const config = {
   resolve: {
      alias: {
         '@': path.resolve(__dirname, 'src')
      }
   }
}
```

导入时

![image-20230816150515867](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161519277.png)

1 配置 webpack.config.js

```js
// 解析
resolve: {
   // 别名
   alias: {
      '@': path.join(__dirname, 'src')
   }
}
```

2 编写index.js 入口 记得将上面的输出报错给注释了

```js
/**
 * 目标：路径解析别名 设置
 * 作用：让我们前端代码引入路径更简单 (而且使用绝对路径)
 * 1.在 webpack.config.js 中配置 resolve.alias 选项
 * 2.在代码中尝试并在开发环境和生产环境测试效果
 */
import youAxios from '@/utils/request.js'
console.log(youAxios)
```

3 执行命令：npm run dev 查看效果

![image-20230816152126847](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161521155.png)

![image-20230816152153753](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161521615.png)

## 优化—CDN使用 

**CDN定义**：内容分发网络，指的是一组分布在各个地区的服务器

**作用**：把静态资源文件 / 第三方库放在 CDN 网络中各个服务器中，供用户就近请求获取

**好处**：减轻自己服务器请求压力，就近请求物理延迟低，配套缓存策略。

![image-20230816152546133](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161525497.png)

**需求**：<font title='red'>开发模式</font>使用<font title='red'>本地</font>第三方库，<font title='red'>生产模式</font>下使用<font title='red'>CDN加载</font>引入

![image-20230816152842336](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161528215.png)

**步骤**：

1.  在html中引入第三方库的CDN地址并用模版语法判断

    ```jsp
    <% if(htmlWebpackPlugin.options.useCdn) { %>
    	<link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.2.3/css/bootstrap.min.css" rel="stylesheet">
    <% } %>
    ```

2.  配置 webpack.config.js 中 extrnals 外部扩展选项 (防止某些 import 的包被打包)

    ```js
    //生产模式下-使用
    if(process.env.NODE_ENV === 'production') {
       config.externals = {
          // key : 代码中 import from 后面的模块标识字符串
          // value ：替换在原地的变量名 (要和 cdn 暴漏在全局的变量名一致)
          'axios': 'axios'
       }
    }
    ```

    

    ```js
    const config = {
       plugins: [
          new HtmlWebpackPlugin({
             //自定义属性，在html模版中 <%=htmlWebpackPlugin.options.useCdn%> 访问使用
             useCdn: process.env.NODE_ENV === 'production'
          })
       ]
    }
    ```

3.  两种模式下打包观察效果

演示代码：

login.html

```html
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>黑马头条-数据管理平台</title>
  <% if(htmlWebpackPlugin.options.useCnd){ %>
    <link href="https://cdn.bootcdn.net/ajax/libs/twitter-bootstrap/5.2.3/css/bootstrap.min.css" rel="stylesheet">
    <% } %>
</head>
<body>
  <!-- 警告框 -->
  <div class="alert info-box">
    操作结果
  </div>
  <!-- 登录页面 -->
  <div class="login-wrap">
    <div class="title">黑马头条</div>
    <div>
      <form class="login-form">
        <div class="item">
          <input type="text" class="form-control" name="mobile" placeholder="请输入手机号" value="13888888888">
        </div>
        <div class="item">
          <input type="text" class="form-control" name="code" placeholder="默认验证码246810" value="246810">
        </div>
        <div class="item">
          <button type="button" class="btn btn-primary btn">登 录</button>
        </div>
      </form>
    </div>
  </div>
  <% if(htmlWebpackPlugin.options.useCnd){ %>
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.3.6/axios.min.js"></script>
    <% } %>
</body>
```

webpack.config.js

```js
plugins: [
   //实例化上面require导入的对象
   new HtmlWebpackPlugin({
      // 模版文件
      // template: path.join(__dirname, 'public/index.html'),
      template: path.join(__dirname, 'public/login.html'),
      // 输出文件
      // filename: path.join(__dirname, 'dist/login/index.html')
      filename: path.join(__dirname, 'dist/login/index.html'),
      useCnd: process.env.NODE_ENV === 'production' // 生产模式下使用 cdn 引入的地址
   }),
]

//生产环境下使用相关配置
if(process.env.NODE_ENV === 'production') {
    // 外部扩展 (让 webpack 防止 import 的包被打包进来)
    config.externals = {
        //key: import from 语句后面的字符串
        //value: 留在原地的全局变量 (最好和 cdn 在全局暴漏的变量一致)
        'bootstrap/dist/css/bootstrap.min.css': 'bootstrap',
        'axios': 'axios'
    }
}
```

查看两种模式下的效果

执行命令：npm run dev 开发模式

![image-20230816170354276](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161703091.png)

找不到我们在login.html页面中引入的bootstrap和axios的cdn标签

执行命令：npm run build 生产模式

![image-20230816170501879](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161705202.png)

可以找到我们在login.html中引入的cdn标签了。

在index.js出口文件中的axios只有一个引入了

![image-20230816171143155](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161711537.png)

## 多页面打包

**单页面**：<font title='red'>单个</font> html 文件，切换DOM的方式实现不同业务逻辑展示，后续 vue / react 会学到

**多页面**：<font title='red'>多个</font> html 文件，切换页面实现不同业务逻辑展示

**需求**：把黑马头条—数据管理平台—内容页面一起引入打包使用

**步骤**：

1.  准备<font title='red'>源码</font> (html，css，js) 放入相应的位置，并改用模块化语法 导出
2.  下载 form-serialize 包并<font title='red'>导入</font>到核心代码中使用
3.  配置 webpack.config.js <font title='red'>多入口</font>和<font title='red'>多页面</font>的设置

基本结构：

```js
const config = {
   entry: {
      '模块名1': path.join(__dirname , 'src/入口1.js'),
      '模块名2': path.join(__dirname , 'src/入口2.js')
   },
   output: {
      path: path.join(__dirname , 'dist'),
      filename: './[name]/index.js',
      clean: true
   },
	plugins: [
      new htmlWebpackPlugin({
         template: './public/页面1.html',//模版文件
         filename: './路径/index.html',//输出文件
         chunks: ['模块名1']
      })
      new htmlWebpackPlugin({
         template: './public/页面2.html',//模版文件
         filename: './路径/index.html',//输出文件
         chunks: ['模块名2']
      })
   ]
}
```

**代码演示**：

1 准备<font title='red'>源码</font> (html，css，js) 放入相应的位置，并改用模块化语法 导出

![image-20230816194258773](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161943164.png)

将内容页面与它的css，js代码放入到content目录中，配置webpack.config.js

```js
const config = {
    //设置打包模式(development 开发者模式-使用相关内置优化)
    // 在package.json中设置两种不同的打包模式的自定义命令的话这里就可以注释掉了
    // mode: 'development',
    // 入口
    // entry: path.join(__dirname, 'src/login/index.js'),
    //配置多个入口
    entry: {
        'login': path.join(__dirname , 'src/login/index.js'),
        'content': path.join(__dirname , 'src/content/index.js')
    },
    // 出口
    output: {
        path: path.join(__dirname, 'dist'),
        // [name]根据不同的模块名生成不同的位置 (模块名占位符)
        filename: './[name]/index.js',
        clean: true
    },
    // 插件([给webpack提供更多功能])
    plugins: [
        //实例化上面require导入的对象
        new HtmlWebpackPlugin({
            // 模版文件
            // template: path.join(__dirname, 'public/index.html'),
            template: path.join(__dirname, 'public/login.html'),//模版文件
            // 输出文件
            // filename: path.join(__dirname, 'dist/login/index.html')
            filename: path.join(__dirname, 'dist/login/index.html'),//输出文件
            useCnd: process.env.NODE_ENV === 'production', // 生产模式下使用 cdn 引入的地址
            chunks: ['login'] // 引入哪些打包后的模块 (和 entry 和 key 一致)
        }),
        new HtmlWebpackPlugin({
            // 模版文件
            // template: path.join(__dirname, 'public/index.html'),
            template: path.join(__dirname, 'public/content.html'),//模版文件
            // 输出文件
            // filename: path.join(__dirname, 'dist/login/index.html')
            filename: path.join(__dirname, 'dist/content/index.html'),//输出文件
            useCnd: process.env.NODE_ENV === 'production', // 生产模式下使用 cdn 引入的地址
            chunks: ['content']
        }),
        // 生成css文件
        new MiniCssExtractPlugin({
            //输出文件
            //[name]占位符，根据不同的模块名生成不同的目录名
            filename: './[name]/index.css'
        }),
        // 配置webpack
        new webpack.DefinePlugin({
            'process.env.NDE_ENV': JSON.stringify(process.env.NODE_ENV)
        })
    ],
```

在内容页面文件中使用<% %>语法引入cdn第三方库

```html
<% if(htmlWebpackPlugin.options.useCdn) { %>
  <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.3.4/axios.min.js"></script>
  <% } %>
```

在inde.js文件中，是内容页面文件的js文件。里面引入相应的代码

```js
import '@/utils/auth.js'
import axios from '@/utils/request.js'
import './index.css'
```

执行命令：npm run build 生产模式 在dist中的content目录中会生成如下的出口文件

![image-20230816194732999](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161947330.png)

查看效果

![image-20230816195903027](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308161959518.png)

执行命令：npm run dev 查看效果

![image-20230816195947135](./NodeJs%E4%B8%8EWebPack.assets/image-20230816195947135.png)

### 打包publish页面

1.  在项目中创建一个新文件夹命名为publish存储publish的 js，css代码，将publish.html文件放到public中

    ![image-20230816205419242](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308162054625.png)

2.  将publish.html里面的第三方cnd库全部使用<%%>进行判断，开发模式不使用，生产模式使用

    ![image-20230816205522765](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308162055946.png)

3.  配置 webpack.config.js

    ```js
    const config = {
       //设置打包模式(development 开发者模式-使用相关内置优化)
       // 在package.json中设置两种不同的打包模式的自定义命令的话这里就可以注释掉了
       // mode: 'development',
       // 入口
       // entry: path.join(__dirname, 'src/login/index.js'),
       //配置多个入口
       entry: {
          'login': path.join(__dirname , 'src/login/index.js'),
          'content': path.join(__dirname , 'src/content/index.js'),
          'publish': path.join(__dirname, 'src/publish/index.js')
       },
       // 出口
       output: {
          path: path.join(__dirname, 'dist'),
          // [name]根据不同的模块名生成不同的位置 (模块名占位符)
          filename: './[name]/index.js',
          clean: true
       },
       // 插件([给webpack提供更多功能])
       plugins: [
          //实例化上面require导入的对象
          new HtmlWebpackPlugin({
             // 模版文件
             // template: path.join(__dirname, 'public/index.html'),
             template: path.join(__dirname, 'public/login.html'),//模版文件
             // 输出文件
             // filename: path.join(__dirname, 'dist/login/index.html')
             filename: path.join(__dirname, 'dist/login/index.html'),//输出文件
             useCnd: process.env.NODE_ENV === 'production', // 生产模式下使用 cdn 引入的地址
             chunks: ['login'] // 引入哪些打包后的模块 (和 entry 和 key 一致)
          }),
          new HtmlWebpackPlugin({
             // 模版文件
             // template: path.join(__dirname, 'public/index.html'),
             template: path.join(__dirname, 'public/content.html'),//模版文件
             // 输出文件
             // filename: path.join(__dirname, 'dist/login/index.html')
             filename: path.join(__dirname, 'dist/content/index.html'),//输出文件
             useCnd: process.env.NODE_ENV === 'production', // 生产模式下使用 cdn 引入的地址
             chunks: ['content']
          }),
          new HtmlWebpackPlugin({
             template: path.join(__dirname , 'public/publish.html'),
             filename: path.join(__dirname , 'dist/publish/index.html'),
             useCnd: process.env.NODE_ENV === 'production',
             chunks: ['publish']
          }),
          // 生成css文件
          new MiniCssExtractPlugin({
             //输出文件
             //[name]占位符，根据不同的模块名生成不同的目录名
             filename: './[name]/index.css'
          }),
          // 配置webpack
          new webpack.DefinePlugin({
             'process.env.NDE_ENV': JSON.stringify(process.env.NODE_ENV)
          })
       ],
    ```

**安装对应插件**：

前面已经安装过 axios 和 bootstrap了 所以 只剩下这两个

editor：npm i @wangeditor/editor

form：npm i form-serialize

**说明**：

@wangeditor/editor 比较特殊在 editor.js 中必须使用 require 进行导入然后使用 对象名来接受editor里面的两个 同名变量 

```js
const wangEditor = require('@wangeditor/editor')
// 富文本编辑器
// 创建编辑器函数，创建工具栏函数
const { createEditor, createToolbar } = wangEditor
```

配置webpack.config.js 中的externals 让第三方cdn库 不进行打包

```js
//生产环境下使用相关配置
if(process.env.NODE_ENV === 'production') {
    // 外部扩展 (让 webpack 防止 import 的包被打包进来)
    config.externals = {
        //key: import from 语句后面的字符串
        //value: 留在原地的全局变量 (最好和 cdn 在全局暴漏的变量一致)
        'bootstrap/dist/css/bootstrap.min.css': 'bootstrap',
        'axios': 'axios',
        'form-serialize': 'serialize',
        '@wangeditor/editor': 'wangEditor'
    }
}
```

执行命令：npm run build 生产模式

![image-20230816210011003](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308162100290.png)

执行命令：npm run dev 开发模式

![image-20230816210124802](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308162101797.png)

开发模式没有引入第三方的bootstrap库和一些其他的库导致样式发生了改变

## 优化—分割公共代码

**需求**：把2个以上页面引用的公共代码提取

**步骤**：

1.  <font title='red'>配置</font>webpack.config.js的splitChunks分割功能

    ```js
    const config = {
       optimization: {
          //代码分割 
            splitChunks: {
                chunks: 'all', // 所有模块动态非动态移入的都分割分析
                cacheGroups: {// 分隔组
                    commons: {// 抽取公共模块
                        minSize: 0, // 抽取的 Chunk最小 大小字节
                        minChunks: 2,// 最小引用数
                        reuseExistingChunk: true,// 当前 chunk 包含已从主 bundle 中拆分出的模块，则它将被重用
                        name(module , chunks , cacheGroupKey) {// 分离出模块文件名
                            const allChunksNames = chunks.map((item) => item.name).join('~')// 模块1 ~ 模块名2
                            return `./js/${allChunksNames}`// 输出到dist目录下位置
                        }
                    }
                }
            }
       }
    }
    ```

2.  <font title='red'>打包</font>观察效果

执行命令：npm run build

![image-20230816212016525](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308162120945.png)

查看目录变化

![image-20230816212049697](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308162120169.png)

在项目中生成了一个公共的js目录里面都是被拼接成代码片段的使用过的代码 也就是用到的公共代码被提取成了代码片段了

