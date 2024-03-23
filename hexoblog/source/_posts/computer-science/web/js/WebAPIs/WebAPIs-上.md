---
title: WebAPIs-上
categories:
    - [计算机学科,web,js,WebAPIs]
tags:
    - 计算机学科
    - js
---

# webAPIs:christmas_tree: 

## 变量声明:diamonds: 

:diamond_shape_with_a_dot_inside: 

变量声明有三个var,let和const

我们应该用哪个呢？

首先var先排除，老派写法，问题很多，可以淘汰掉。。。

<font title="red">let</font> <span alt="blink"><span alt="shake">or</span></span> <font  title="green">const</font>?

建议：<font title="red"><span alt="blink">**const优先**</span></font>，尽量使用const,原因是：

1.  const语义化更好
2.  很多变量我们声明的时候就知道它不会被更改了，那为什么不用const呢?
3.  实际开发中也是，比如react框架，基本const

**如果你还在纠结，那么我建议**:

-  <span style="color:red">有了变量先给const,如果发现它后面是要被修改的，再改为let</span>.

const声明的值不能更改,而且const声明变量的时候需要里面进行初始化

但是对于引用数据类型,const声明的变量,里面存的<span style="color:red"><font color="green">不是值</font>,<font color="yellow">不是值</font>,<font title="red">不是值</font>,<font color="blue">是地址值</font></span>.

```js
const names = []
names = [1,2,3]
```

```js
const obj = {}
obj = {
   uname: '刘桑'
}
```

<font style="color:red"><span alt="wavy">错误,它们的地址值是不一样的</span></font>

```js
const names = []
names[0] = 1
names[1] = 2
names[2] = 3
```

```js
const obj = {}
obj.uname = '刘桑'
```

<font title="green">这是正确的</font>.

**总结**:

1.  以后声明变量我们优先使用哪个?
    -  const
    -  有了变量先给const,如果发现它后面是要被修改的,再改为let
2.  为什么const声明的对象可以修改里面的属性?
    -  因为对象是引用类型,里面存储的是地址,只要地址不变,就不会报错
    -  <font color="red">建议数组和对象使用const来声明</font>.
3.  什么时候使用let声明变量?
    -  如果基本数据类型的值或者引用类型的地址发生变化的时候,需要用let
    -  比如 一个变量进行加减运算,比如for循环中的i++这个值在循环时一直在改变

# WebAPI基本认知:christmas_tree: 

:diamond_shape_with_a_dot_inside: 

## 作用和分类:tangerine: 

**作用**：就是使用js去操作html和浏览器

**分类**：<font title="red">DOM</font>(文档对象模型)，<font title="red">BOM</font>(浏览器对象模型)

![image-20230428214813135](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211922376.png)

## 什么是DOM:dagger: 

:diamond_shape_with_a_dot_inside: 

DOM(Document Object Model-<font style="color:red">文档对象模型</font>)是用来呈现以及与任意HTML或XML文档交互的API

**白话**：DOM是浏览器提供的一套专门用来<font style="color:red">操作网页内容</font>的功能

**DOM作用**：:game_die: 

-  开发网页内容特效和实现用户交互  

1.  Web API阶段我们学习哪两部分？
    -  DOM
    -  BOM
2.  DOM是什么？有什么作用？
    -  DOM是<strong style="color:red">文档对象模型</strong>.
    -  操作网页内容，可以开发网页内容特效和实现用户交互

## DOM树:christmas_tree: 

:diamond_shape_with_a_dot_inside: 

**DOM树是什么**.

-  将HTML文档以树状结构直观的表现出来，我们称之为文档树或DOM树
-  描述网页内容关系的名词
-  **作用**：<font color="red">文档树直观的体现了标签与标签之间的关系</font>.

![image-20230428215945870](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211043407.png)

<font title="red">最大的是document对象然后就是html，html下有两个head和body就这样依次推理</font>.

## DOM对象:gem: <span alt="shake">(重要)</span> 

**DOM对象**：浏览器根据html标签生成的<font color="red">JS对象</font>,任何一个标签都是一个<font color="red">对象</font>,只要是<font color="red">对象</font> 就有 **属性和方法**.

-  所有的标签属性都可以在这个对象上面找到
-  修改这个对象的属性会自动映射到标签上

![image-20230428220655045](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211926698.png)

```js
const div = document.querySelector('div')
console.dir(div)//dom对象
console.log(typeof div)//object
```

## dir专门用于打印对象的

在DOM中所有的标签都是一个对象

![image-20230428221737537](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211926821.png)

**DOM的核心思想**.

-  把网页内容当作<font color="red">对象</font>来处理

**document对象**.

-  是DOM里提供的一个<font color="red">对象</font>.

-  所以它提供的属性和方法都是<font color="red">用来访问和操作网页内容的</font>.

   -  例：document.write()

   网页所有内容都在document里面

**总结**：

1.  **DOM树是什么**？
    -  将HTML文档以树状结构直观的表现出来，我们称之为文档树或DOM树
    -  **作用**：文档树==直观的体现==了==标签==与==标签之间的关系==。
2.  **DOM对象怎么创建的**？
    -  ==浏览器根据html标签生成的JS对象==(DOM对象).
    -  DOM的核心就是把<font color="red">内容</font>当<font color="red">对象</font>来处理
3.  **document是什么**？
    -  是DOM里提供的一个<font title="red">对象 </font>.
    -  网页所有内容都在document里面

# 获取DOM对象:christmas_tree: 

:diamond_shape_with_a_dot_inside: 

**目标**：能查找/获取DOM对象

## 根据CSS选择器来获取DOM元素(重点):deciduous_tree: 

**提问**：我们想要操作某个标签首先做什么?

-  肯定首先选中这个标签，跟CSS选择器类似，选中标签才能操作比如：

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .box{
            width: 50px;
            height: 50px;
            background: red;
        }
    </style>
</head>
<body>
<div class="box"></div>
</body>
</html>
```

-  JS也是同样的
   -  查找元素DOM元素就是利用JS选择页面中标签元素

## 1 选择匹配第一个元素querySelector

**语法**：

```js
document.querySelector('css选择器')
```

代码：

```js
<body>
    <div>123</div>
    <div class="box">456</div>
    <div class="box">789</div>

    <span id="u">你好</span>

    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    <script>
        //querySelector 获取匹配的第一个元素
        const div = document.querySelector('div');
        console.dir(div);//123
        const box = document.querySelector('.box');
        console.dir(box);//456
        const span = document.querySelector('#u');
        console.dir(span);//你好
        const l1 = document.querySelector('ul li:first-child');
        console.dir(l1);//1
        const l2 = document.querySelector('ul li:nth-child(2)');
        console.dir(l2);//2
        const l3 = document.querySelector('ul li:last-child');
        console.dir(l3);//3
    </script>
</body>
```

**结果**：

![image-20230721200859120](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307212009408.png)



```
div：innerText: "123"

div.box：innerText: "456"

span#u：innerText: "你好"

li：innerText: "1"

li：innerText: "2"

li：innerText: "3"
```



**修改样式**：

```js
const span = document.querySelector('#u');
span.style.color = 'red';
```

**结果**：

![image-20230721202352663](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307212024422.png)

querySelector==默认匹配指定元素的第一个被匹配到的元素==，但是想要指定第2个或者其它则需要使用==其它的选择器来完成==，比如 ==结构伪类选择器==。

**参数**：

包含==一个==或==多个有效==的CSS选择器 <font color="red">字符串</font>.

**返回值**：

CSS选择器匹配的<font color="red">第一个元素</font>，一个HTMLElement对象。

如果没有匹配到，则返回null。

## 2 选择匹配的多个元素querySelectorAll

**语法**：

```js
document.querySelectorAll('css选择器');
```

得到的是一个<font color="red">伪数组</font>：

-  有长度有索引号的数组
-  但是没有 pop()，push()等数组方法

想要得到里面的每一个对象，则需要遍历(for)的方式来获得。

**注意事项**：

-  哪怕只有一个元素，通过querySelectorAll()获取过来的也是一个<font color="red">伪数组</font>，里面只有一个元素而已。

**代码**：

```js
<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    <script>
        // querySelectorAll选择匹配多个元素
        const lis = document.querySelectorAll('ul li');
        console.dir(lis);
    </script>
</body>
```

**结果**：

![image-20230721202116807](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307212021817.png)

```
li：1

li：2

li：3
```



**修改样式**：

如果先要修改其中某一个改变颜色 则只需要加一个判断每个元素的值通过innerHTML来获取等于该值后就改变颜色

```js
<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    <script>
        // querySelectorAll选择匹配多个元素
        const lis = document.querySelectorAll('ul li');
        for (let i = 0; i < lis.length; i++) {
            if(lis[i].innerHTML == '2'){
                lis[i].style.color = 'red';
            }
        }
    </script>
</body>
```

**效果**：

![image-20230721213841229](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307212138515.png)

**参数**：

包含一个或多个有效的CSS选择器 <font color="red">字符串</font>。

**返回值**：

CSS选择器匹配的 <font color="red">NodeList 对象集合</font>。

>   **总结**：
>
>  1.  获取一个DOM元素我们使用谁？能直接操作修改吗？
>
>      -   querySelector()
>
>      -   可以
>  2.  获取多个DOM元素我们使用谁？能直接修改吗？如果不能可以怎么做到修改？
>      -  querySelectorAll()
>      -  不可以，只能通过遍历的方式一次给里面的元素做修改但是也可以做一些判断来达到预期
>  3.  <strong style="color:red">因为获取的是标签对象改变的是标签里的属性，标签是不变的所以可以使用const来声明</strong>。
>  4.  获取页面中的标签我们最终常用的哪两种方式？
>      -  querySelectorAll();
>      -  querySelector();
>  5.  它们两者的区别是什么？
>      -  querySelector(); 只能选择一个元素，可以直接操作
>      -  querySelectorAll(); 可以选择多个元素，得到的是伪数组，需要遍历得到每一个元素对象
>  6.  它们两者小括号里面的参数有什么注意事项？
>      -  里面写css选择器
>      -  <strong style="color:red">必须是字符串，也就是必须加引号</strong>。

## 其它获取DOM元素方法(了解):deciduous_tree: 

```js
//根据id获取一个元素
document.getElementById('nav');
//根据 标签获取一类元素 获取页面 所有div
document.getElementsByTagName('div');
//根据 类名获取元素 获取页面 所有类名为w的
document.getElementsByClassName('w');
```

后面getElements带s的都是 <font color="red">伪数组</font> id获取的是<font color="red">单个</font>。

## 操作元素内容

**目标**：能够修改元素的文本更换内容

==DOM对象==都是根据==标签生成==的，所以操作标签，本质上就是==操作DOM对象==。

就是操作对象使用的点语法。

如果想要修改标签元素的里面的<font color="red">内容</font>，则可以使用如下几种方式：

1.  对象.innerText属性
2.  对象.innerHTML属性

<strong style="color:red">注意</strong>：属性不加小括号.innerXXX后面没有小括号。

### 对象.innerText属性

将文本内容==添加==/==更新==到任意标签位置

<strong style="color:red">显示纯文本，不解析标签</strong>。

代码：

```js
<body>
   <div class="box">我是文字的内容</div> 
   <script>
    // 1.获取元素
    const box = document.querySelector('.box')
    // 修改文字内容 对象.innerText属性
    console.log(box.innerText)
    box.innerText = '<strong>我是盒子!</strong>' //修改盒子内容 并且 它是不解析内容的标签的
   </script>
</body>
```

效果：

![image-20230721221212629](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307212212949.png)

### 对象.innerHTML属性

将文本内容添加/更新到任意标签位置

<strong style="color:red">会解析标签，多标签建议使用模板字符</strong>。

代码：

```js
<body>
   <div class="box">我是文字的内容</div> 
   <script>
    // 1.获取元素
    const box = document.querySelector('.box')
    // 修改文字内容 对象.innerHTML属性
    console.log(box.innerHTML)
    box.innerHTML = '<strong>我是盒子!</strong>'//修改盒子内容并且会解析内容标签
   </script>
</body>
```

效果：

![image-20230721221535486](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307212215853.png)



### 总结：

1.  设置/修改 DOM 元素内容有哪2种方式？

    -  元素.innerText属性
    -  元素.innerHTML属性

2.  二者的区别是什么？

    -  元素.innerText属性  只识别文本，不能解析标签
    -  元素.innerHTML属性  能识别文本，能够解析标签


### 随机抽奖案例

需求：从数组随机抽取一等奖，显示到对应的标签里面。抽取到的人就从数组中删除不能重复抽到一个人。

代码：

```html
<body>
    <div class="wrapper">
        <strong>年会抽奖</strong>
        <h1>一等奖:<span id="one">???</span></h1>
    </div>
    <script>
        const personArr = ['周杰伦','刘德华','周星驰','pink老师','张学友']
        // 随机生成数组下标
        const random = Math.floor(Math.random() * personArr.length)
        const one = document.querySelector('#one')
        one.innerHTML = personArr[random]
        personArr.splice(random,1)
    </script>
</body>
```

效果：

![image-20230728165532034](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281655457.png)

## 操作元素属性

-  操作元素常用属性
-  操作元素样式属性
-  操作表单元素属性
-  自定义属性

### 1 操作元素常用属性

-  还可以通过JS==设置==/==修改==标签元素==属性==，比如通过src更换 图片
-  最常见的属性比如：href，title，src等
-  **语法**：

```js
对象.属性 = 值
```

#### 举例说明

```js
//1.获取元素
const pic = document.querySelector('img')
//2.操作元素
pic.src = './image/b02.jpg'
pic.title = '刘德华'
```

代码：

```html
<body>
   <img src="./image/pic.jpg" alt=""/> 
   <script>
    const img = document.querySelector('img')
    img.src = './image/scale.jpg'
    img.title = '空旷的城市'
   </script>
</body>
```

效果：

![image-20230728193600139](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281936672.png)

### 2 操作元素==样式==属性

-  还可以通过JS==设置==/==修改==标签元素的==样式属性==.
   -  比如通过 轮播图小圆点自动更换颜色样式
   -  点击按钮可以滚动图片，这是移动的图片的位置left等等

学习路径：

1.  通过style属性操作CSS
2.  操作类名(className)操作CSS
3.  通过classList操作类控制CSS

#### 通过==style==属性操作CSS

**语法**：

```js
对象.style.样式属性 = 值
```

##### 举例说明

```js
const box = document.querySelector('.box')
box.style.width = '200px'
box.style.marginTop = '15px'
box.style.backgroundColor = 'pink'
```

**注意**：

1.  修改样式通过<font color=red>style</font>属性引出
2.  如果属性有 - 连接符(多组单词) ，需要转换为 <font color=red>小驼峰</font> 命名法
3.  赋值的时候，需要的时候不要忘记加 <font color=red>CSS 单位</font>.
4.  样式属性，一定别忘记，大部分数字后面都需要加单位
5.  生成的是行内样式表，权重比较高

代码：

```html
<body>
   <div class="box"></div> 
   <script>
    const div = document.querySelector('.box')
    div.style.width = '300px'
    div.style.height = '300px'
    // 多组单词 采用 小驼峰命名法
    div.style.backgroundColor = 'red'
    div.style.borderRadius = '13px'
    div.style.border = '2px blue solid'
    div.style.borderTop = '2px green solid'
   </script>
</body>
```

效果：

![image-20230728200408528](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307282004348.png)

#### 3 操作类名(==className==)操作CSS

-  如果修改的样式比较多，直接通过style属性修改比较繁琐，我们可以通过借助于css类名的形式。

语法：

```js
//active 是一个css类名
元素.className = 'active'
```

-  **注意**：
   1.  由于class是关键字，所以使用className去代替
   2.  className是使用新值<font color=red>换</font>旧值，如果需要添加一个类，需要保留之前的类名。
       -  元素.className = 'active nav box'
   3.  用 className js添加的属性会把css写的给替换掉无论权重多高 想使用多个就在js继续加

代码：

```html
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
        }
        .box {
            width: 300px;
            height: 300px;
            background-color: skyblue;
            margin: 100px auto;
            padding: 10px;
            border: 1px #000 solid;
        }
        .nav {
            background-color: red !important;
        }
    </style>
</head>
<body>
   <div></div> 
   <script>
    // 1.获取元素
    const div = document.querySelector('div')
    // 2.添加类名 class 是个关键字所以用 className js添加的属性会把css写的给替换掉无论权重多高 想使用多个就在js继续加
    div.className = 'nav box'
   </script>
</body>
```

效果：

![image-20230728203520206](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307282035602.png)

#### 4 通过==classList==操作类控制CSS:star: 

为了解决className容易覆盖以前的类名，我们可以通过classList方式追加和删除类名

**语法**：

```js
//追加一个类
元素.classList.add('类名')
//删除一个类
元素.classList.remove('类名')
//切换一个类
元素.classList.toggle('类名')
//判断是否存在该属性 存在返回true 不存在返回false
元素.classList.contains('类名')
```

代码：

```js
    <style>
        .box {
            width: 200px;
            height: 200px;
            color: #333;
        }
        .active {
            color: red;
            background-color:pink;
        }
        .nav {
            border: 3px blue solid;
            border-radius: 15px;
        }
    </style>
</head>
<body>
   <div class="box">文字</div> 
   <script>
    // 1.获取元素
    const box = document.querySelector('.box')
    // 在没有删除box类名的时候不添加也会保留box的属性的className则会替换掉js中添加的类名之前的就会没有
    // 2.修改样式 
    //追加类 add() 类名不加点，字符串
    box.classList.add('active')
    box.classList.add('nav')
    //删除类 remove() 类名不加点，字符串
    box.classList.remove('box')
    //切换类 toggle() 有还是没有. 有就删除，没有就加上
    box.classList.toggle('box')
   </script>
</body>
```

效果：

![image-20230728205438686](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307282054031.png)

#### 使用className和classList的==区别== 

-  修改==大量样式==的更方便
-  修改==不多样式==的时候方便
-  classList是==追加==和==删除====不影响以前类名==.

### 3 操作==表单元素==属性

-  表单很多情况，也需要修改属性，比如点击眼睛，可以看到密码，本质是把表单类型转换为文本框

   ![image-20230728225249025](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307282252285.png)

-  正常的有属性有取值的 跟其它的==标签属性没有任何区别==.

-  ==获取==：DOM对象.属性名

-  ==设置==/==修改==：DOM对象.属性名 = 新值

```js
表单.value = '用户名'
表单.type = 'password'
```

-  表单属性中==添加就有效果==，==移除就没有效果==，一律使用==布尔值表示==如果为==true代表添加了该属性==，如果是==false代表移除==了该属性
-  比如：==disabled==，==checked==，==selected==

![image-20230728232114501](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307282321890.png)

代码：

html

```html
<body>
    <input type="text" value="电脑"/> 
    <button id="qie">切换变成圆点的密码形式</button>
    <input type="radio" id="dan" name="dan">
    <input type="radio" id="dan1" name="dan">
    <button id="jin">禁用/启动=></button>
    <button id="yong">按钮，点我 有话对你说!</button>
    <select>
        <option>好吃</option>
        <option>不好吃</option>
    </select>
    <script src="./js/修改表单属性.js"></script>
</body>
```

js

```js
// 1.获取元素
const input = document.querySelector('input')
// 2.获取表单的值
let val = input.value
console.log(`input = ${val}`)
// 3.修改表单的值
input.value = '密码点击右侧会隐藏展示哦'
// 3.1 获取修改后的表单的值
let val1 = input.value
console.log(`修改后的input = ${val1}`)
//修改input的type属性值
const qie = document.querySelector('#qie')
qie.addEventListener('click',function() {
    if(input.type == 'text')
        input.type = 'password'
    else
        input.type = 'text'
})
//修改单选框属性值
const radio = document.querySelector('#dan')
// 修改默认为选中状态
// radio.checked = true
// 这样的也会默认选中，因为除了空字符串为fals其它都为true(隐式转换)
//因为checked只能接受一个布尔值所以隐式转换为一个对应的布尔值a字符串为true
radio.checked = 'a'
const jin = document.querySelector('#jin')
const yong = document.querySelector('#yong')
jin.addEventListener('click',function() {
    if(yong.disabled == false){
        yong.disabled = true
        alert('右边的按钮被我禁用啦')
    } else{
        yong.disabled = false
        alert('右边按钮被解救了')
    }
})
yong.addEventListener('click',function() {
    alert('当心我左边的按钮它会禁用我的，我不想被禁用')
})
// select元素获取过来的是一个数组形式的对象通过下标来访问option
const select = document.querySelector('select')
console.log(select[0])
// 设置默认选中下标为1的option
select[1].selected = true
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307290134319.gif)

### 自定义属性

-  **标准属性**：==标签天生自带的属性==比如==class== ==id== ==title==等，可以==直接使用点语法操作==比如：disabled，checked，selected
-  **自定义属性**：
   -  在==html5==中推出来了专门的==data-自定义属性==.
   -  在==标签上==一律以==data-开头==.
   -  在DOM对象上一律以==dataset==对象方式获取

示例：

```html
<body>
   <div class="box" data-id="10">盒子</div>
   <script>
   	const box = document.querySelector('.box')
      console.log(box.dataset.id)
   </script>
</body>
```

代码：

html

```html
<body>
    <!-- data-自定义属性 -->
   <div data-id="1" data-spm="不知道">1</div>
   <div data-id="2">2</div>
   <div data-id="3">3</div>
   <div data-id="4">4</div>
   <div data-id="5">5</div> 
   <script src="./js/自定义属性.js"></script>
</body>
```

js

```js
const div = document.querySelector('div')
// 得到data数据的集合
console.log(div.dataset)
// 得到data中的id值
console.log(div.dataset.id)
// 得到data中的spm值
console.log(div.dataset.spm)
```

效果：

![image-20230729121649983](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291216521.png)

### 多余的小知识

标签选择body，因为body是唯一的标签，可以直接写document.body.style

## 定时器-间歇函数

**目标**：能够使用定时器函数重复执行代码

定时器函数可以==开启==和==关闭==定时器

##### 定时器函数介绍

-  网页中经常会需要一种功能：每个一段时间需要<font color=red>自动</font>执行一段代码，不需要我们手动去触发
-  例如：网页中的倒计时
-  要实现这种需求，需要定时器函数
-  定时器函数有两种，今天先学习间歇函数

![image-20230729122320318](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291223580.png)

![image-20230729122336607](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291223949.png)

### 1 开启定时器

**语法**：

```js
setInterval(函数,间隔时间)
```

-  作用：每隔一段时间调用这个函数
-  间隔时间单位是毫秒

举例说明：

```js
function repeat() {
   console.log('前端程序员')
}
// 每隔一秒调用repeat函数
setInterval(repeat,1000)
```

**注意**：

-  函数名字不需要加括号
-  定时器返回的是一个id数字(序号 每个定时器都是唯一的)
-  定时器要==开==要==关==会有==重新赋值==的操作，接收返回的id数字==建议使用let来声明==而不是<font color=red>const</font>.
   -  比如说：`const num = 10     num = 10    常量再次赋值就会报错` 

**代码**：

html

```html
<body>
   <script src="./js/定时器.js"></script> 
</body>
```

js

```js
// 直接执行一个函数体
// 定时器返回使用let来声明变量因为返回的是数字型，可能会发生变化的,定时器要开要关会有重新赋值的操作
let num = setInterval(()=>{
    console.log('我被执行了')
},1000)
console.log(num)
function fun() {
    console.log('我也被执行了哦')
}
// 代码从上往下执行，当到了下面的setInterval时 等待1000毫秒后才执行的函数体 并不是立刻执行的
// 调用函数名执行函数体
// 定时器返回使用let来声明变量因为返回的是数字型，可能会发生变化的,定时器要开要关会有重新赋值的操作
let num1 = setInterval(fun,1000)
console.log(num1)
// 错误写法
// setInterval('fun',1000)
```

效果：

定时器返回的序号

![image-20230729124210057](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291242525.png)

定时器的间歇执行函数体 

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291238381.gif)

### 2 关闭定时器

**语法**：

```js
let 变量名 = setInterval(函数,间隔时间)
clearInterval(变量名)
```

**代码**：

html

```html
<body>
   <script src="./js/定时器.js"></script> 
</body>
```

js

```js
// 定时器1
let num = setInterval(()=>{
    console.log('我被执行了')
},1000)
// 定时器2
// 定时器返回使用let来声明变量因为返回的是数字型，可能会发生变化的
let num1 = setInterval(fun,1000)
function fun() {
    console.log('我也被执行了哦')
}
// 关闭第二个定时器,将变量名传入即可
clearInterval(num1)
```

效果：

![image-20230729125401506](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291254911.png)

一半情况下不会刚创建定时器就关闭而是触发某个事件后才关闭的。

### 轮播图定时器案例

需求：每隔一秒 切换一个 图片

分析：

1.  准备一个数组对象，里面包含详细信息(素材包含)
2.  获取元素
3.  设置定时器函数
    1.  设置一个变量 ++
    2.  找到变量对应的对象
    3.  更改图片，文字信息
    4.  激活小圆点：移除上一个高亮的类名，当前变量对应的小圆点添加类
4.  处理图片自动复原从头播放(放到变量++后面，紧挨)
    1.  如果把图片播放到最后一张，就是大于等于数组的长度
    2.  则把变量重置为0

**代码**：

html

```html
  <style>
    * {
      box-sizing: border-box;
    }

    .slider {
      width: 560px;
      height: 400px;
      overflow: hidden;
    }

    .slider-wrapper {
      width: 100%;
      height: 320px;
    }

    .slider-wrapper img {
      width: 100%;
      height: 100%;
      display: block;
    }

    .slider-footer {
      height: 80px;
      background-color: rgb(100, 67, 68);
      padding: 12px 12px 0 12px;
      position: relative;
    }

    .slider-footer .toggle {
      position: absolute;
      right: 0;
      top: 12px;
      display: flex;
    }

    .slider-footer .toggle button {
      margin-right: 12px;
      width: 28px;
      height: 28px;
      appearance: none;
      border: none;
      background: rgba(255, 255, 255, 0.1);
      color: #fff;
      border-radius: 4px;
      cursor: pointer;
    }

    .slider-footer .toggle button:hover {
      background: rgba(255, 255, 255, 0.2);
    }

    .slider-footer p {
      margin: 0;
      color: #fff;
      font-size: 18px;
      margin-bottom: 10px;
    }

    .slider-indicator {
      margin: 0;
      padding: 0;
      list-style: none;
      display: flex;
      align-items: center;
    }

    .slider-indicator li {
      width: 8px;
      height: 8px;
      margin: 4px;
      border-radius: 50%;
      background: #fff;
      opacity: 0.4;
      cursor: pointer;
    }

    .slider-indicator li.active {
      width: 12px;
      height: 12px;
      opacity: 1;
    }
  </style>
</head>

<body>
  <div class="slider">
    <div class="slider-wrapper">
      <img src="./image/slider01.jpg" alt="" />
    </div>
    <div class="slider-footer">
      <p>对人类来说会不会太超前了？</p>
      <ul class="slider-indicator">
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
      <div class="toggle">
        <button class="prev">&lt;</button>
        <button class="next">&gt;</button>
      </div>
    </div>
  </div>
  <script src="./js/轮播图定时器版.js"></script>
</body>
```

js

```js
// 1. 初始数据
const sliderData = [
    { url: './image/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)' },
    { url: './image/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)' },
    { url: './image/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)' },
    { url: './image/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)' },
    { url: './image/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)' },
    { url: './image/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)' },
    { url: './image/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)' },
    { url: './image/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)' },
]
// 1.获取元素对象
const img = document.querySelector('.slider-wrapper img')
const p = document.querySelector('.slider-footer p')
const bgColor = document.querySelector('.slider-indicator')
const beijing = document.querySelector('.slider-footer')
let i = 0
let num = setInterval(fun, 1000)
function fun() {
    // 无缝衔接位置
    // img,p,li这些标签原本初始化就带有了样式的第一次从1开始往后从0开始循环遍历  可以打开console来看下区别
    i++
    if (i >= sliderData.length){
        i = 0
    }
    console.log(i)
    let obj = sliderData[i]
    img.src = obj.url
    p.innerHTML = obj.title
    // 选中之前的active并删除
    document.querySelector(`.slider-indicator .active`).classList.remove('active')
    // 在当前的li中添加active
    document.querySelector(`.slider-indicator li:nth-child(${(i + 1)})`).classList.add('active')
   beijing.style.backgroundColor = sliderData[i].color
    // 每次都循环一整个 可以打开console来看下区别
    // console.log(i)
    // if (i == sliderData.length - 1){
    //     i = 0
    //     return
    // }
    // i++
}
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291532874.gif)

## 事件监听(绑定)

-  事件监听
-  扩展阅读-事件监听版本

### 什么是事件？

事件是在编程时系统内发生的==动作==或者发生的事情

比如用户在网页上==单击==一个按钮

### 什么是事件监听？

就是让==程序检测==是否有==事件产生==，一旦有==事件触发==，就立即==调用一个函数做出响应==，也成为 ==绑定事件==或者==注册事件==.

比如鼠标经过显示下拉菜单，比如点击可以播放轮播图等等

**语法**：

```js
元素对象.addEvenListener('事件类型',要执行的函数)
```

-  事件监听三要素
   -  事件源：哪个dom元素被事件触发了，要获取dom元素 (谁被触发了)
   -  事件类型：用什么方式触发，比如鼠标单击click，鼠标经过mouseover等  (用什么方式触发的)
   -  事件调用的函数：要做什么事

**举例说明**：

```js
<button class="btn">按钮</button>
<script>
	const btn = document.querySelector('.btn') 
	btn.addEventListener('click',function(){
      alert('被点击了~')
   })
</script>
```

**注意**：

1.  事件类型要加==单引号==.
2.  函数是点击之后再去执行，每次点击都会执行一次 ==触发一次就执行一次==.

### 案例-随机抽取一个人

代码：

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        h2 {
            text-align: center;
        }

        .box {
            width: 600px;
            margin: 50px auto;
            display: flex;
            font-size: 25px;
            line-height: 40px;
        }

        .qs {

            width: 150px;
            height: 40px;
            color: red;

        }

        .btns {
            text-align: center;
        }

        .btns button {
            width: 120px;
            height: 35px;
            margin: 0 50px;
        }
    </style>
</head>
<body>
    <h2>随机点名</h2>
    <div class="box">
        <span>名字是：</span>
        <div class="qs">这里显示姓名</div>
        <span class="gong">&nbsp;</span>
    </div>
    <div class="btns">
        <button class="start">开始</button>
        <button class="end">结束</button>
    </div>
    <script src="./js/随机点名案例.js"></script>
</body>
</html>
```

js

```js
// 数据数组
const arr = ['马超', '黄忠', '赵云', '关羽', '张飞']
// 开始按钮
const start = document.querySelector('.start')
// 显示抽到的人的名字
const qs = document.querySelector('.qs')
// 结束按钮
const end = document.querySelector('.end')
// 显示祝贺
const gong = document.querySelector('.gong')
// 对按钮进行禁用的操作保证不出现人为的故障
// 默认禁用结束按钮
end.disabled = true;
let val = 0
let num = 0
start.addEventListener('click', function() {
    if (end.disabled == true)
        end.disabled = false
    start.disabled = true
    val = setInterval(function() {
        num = parseInt(Math.random() * arr.length)
        // 随机展示
        qs.innerHTML = arr[num]
    }, 30)
})
end.addEventListener('click', function() {
    if (start.disabled == true)
        start.disabled = false
    end.disabled = true
    // 抽取一个删除一个
    arr.splice(num, 1)
    clearInterval(val)
    console.log(arr)
    if (arr.length == 1){
        start.disabled = end.disabled = true
        // 最终结果
        qs.innerHTML = arr[0]
        gong.innerText = '恭喜你! ❀'
    }
})
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291851005.gif)

### 扩展阅读-事件监听版本

-  DOM L0
   -  事件源.on事件 = function(){}
   -  比如：事件源.onclick = 函数体 也就是给事件源绑定了一个单击事件，但是多个on事件会被覆盖
-  DOM L2
   -  事件源.addEventListener(事件，事件处理函数)
-  区别：
   -  on方式会==被覆盖==，addEventLiestener方式==可绑定多次==，拥有事件更多特性，推荐使用
-  发展史：
   -  DOM L0：是DOM的发展的第一个版本；L：level
   -  DOM L1：DOM级别1于1998年10月1日成为W3C推荐标准
   -  DOM L2：使用addEventListener注册事件
   -  DOM L3：DOM3级事件模块在DOM2级事件的基础上重新定义了这些事件，也添加了一些新事件类型

## 事件类型

![image-20230729190054985](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307291900580.png)

| 事件    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| change  | 当文本框内容发送==改变==并且==离焦==后执行                   |
| loadend | 资源的加载进度停止之后被触发(例如，触发`error`，`abort`，`load`事件之后) |

**注意**：事件类型全部都是==小写的== 没有大写的！

### 鼠标经过事件的区别

-  **鼠标经过事件**：
   -  mouseover和mouseout会有冒泡效果
   -  mouseenter和mouseleave 没有冒泡效果(==推荐==)
   -  关于冒泡详细查看<a href='#事件流'>点击</a>.

**代码**：

html

```html
<body>
    <div class="father">
        <div class="son"></div>
    </div>
    <script src="./js/鼠标经过事件区别.js"></script> 
</body>
```

js

```js
const box = document.querySelector('.father')
const box1 = document.querySelector('.son')
box.addEventListener('mouseover',function () {
    console.log('father 鼠标经过')
})
box.addEventListener('mouseout',function () {
    console.log('father 鼠标离开')
})
```

less

```less
.father {
    width: 400px;
    height: 400px;
    background-color: pink;
}
.son {
    width: 200px;
    height: 200px;
    background-color: purple;
}
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307311534665.gif)

### 鼠标事件之-完整的轮播图案例

**代码**：

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>轮播图点击切换</title>
  <style>
    * {
      box-sizing: border-box;
    }

    .slider {
      width: 560px;
      height: 400px;
      overflow: hidden;
    }

    .slider-wrapper {
      width: 100%;
      height: 320px;
    }

    .slider-wrapper img {
      width: 100%;
      height: 100%;
      display: block;
    }

    .slider-footer {
      height: 80px;
      background-color: rgb(100, 67, 68);
      padding: 12px 12px 0 12px;
      position: relative;
    }

    .slider-footer .toggle {
      position: absolute;
      right: 0;
      top: 12px;
      display: flex;
    }

    .slider-footer .toggle button {
      margin-right: 12px;
      width: 28px;
      height: 28px;
      appearance: none;
      border: none;
      background: rgba(255, 255, 255, 0.1);
      color: #fff;
      border-radius: 4px;
      cursor: pointer;
    }

    .slider-footer .toggle button:hover {
      background: rgba(255, 255, 255, 0.2);
    }

    .slider-footer p {
      margin: 0;
      color: #fff;
      font-size: 18px;
      margin-bottom: 10px;
    }

    .slider-indicator {
      margin: 0;
      padding: 0;
      list-style: none;
      display: flex;
      align-items: center;
    }

    .slider-indicator li {
      width: 8px;
      height: 8px;
      margin: 4px;
      border-radius: 50%;
      background: #fff;
      opacity: 0.4;
      cursor: pointer;
    }

    .slider-indicator li.active {
      width: 12px;
      height: 12px;
      opacity: 1;
    }
  </style>
</head>

<body>
  <div class="slider">
    <div class="slider-wrapper">
      <img src="./image/slider01.jpg" alt="" />
    </div>
    <div class="slider-footer">
      <p>对人类来说会不会太超前了？</p>
      <ul class="slider-indicator">
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
      <div class="toggle">
        <button class="prev">&lt;</button>
        <button class="next">&gt;</button>
      </div>
    </div>
  </div>
  <script src="./js/轮播图定时器版.js"></script>
</body>
</html>
```

js

```js
// 1. 初始数据
const sliderData = [
    { url: './image/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)' },
    { url: './image/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)' },
    { url: './image/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)' },
    { url: './image/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)' },
    { url: './image/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)' },
    { url: './image/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)' },
    { url: './image/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)' },
    { url: './image/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)' },
]
// 1.获取元素对象
const img = document.querySelector('.slider-wrapper img')
const p = document.querySelector('.slider-footer p')
const bgColor = document.querySelector('.slider-footer')
const prev = document.querySelector('.prev')
const next = document.querySelector('.next')
const boos = document.querySelector('.slider')
const lis = document.querySelectorAll('.slider-indicator li')
let ibr = 0
next.addEventListener('click', function() {
    ibr++
    ibr = ibr >= sliderData.length ? 0:ibr
    fun(ibr)
})
prev.addEventListener('click', function() {
    ibr--
    ibr = ibr < 0 ? sliderData.length - 1:ibr
    fun(ibr)
})
// 函数体写重复的代码减少代码量
function fun(i) {
    i = i | 0
    img.src = sliderData[i].url
    p.innerText = sliderData[i].title
    bgColor.style.backgroundColor = sliderData[i].color
    document.querySelector('.slider-indicator .active').classList.remove('active')
    document.querySelector(`.slider-indicator li:nth-child(${(i + 1)})`).classList.add('active')
}
// 自动播放模块
let num = setInterval(function() {
    // 利用js自动调用点击事件
    next.click()
},1000)
// 鼠标经过停止
boos.addEventListener('mouseenter',function() {
    clearInterval(num)
})
// 鼠标移出开启
boos.addEventListener('mouseleave',function() {
    // 要开启定时器时先关闭一下这是一个好习惯
    clearInterval(num)
    // 这里再次开启定时器一定写num而不是直接写定时器
    // 因为直接写就是又开启了一个新的没有名字的定时器
    // 这样会重复开启好多个 轮播速度就会越来越快
    // 也不要带声明因为就变成局部变量了和成员变量就是两回事儿了
    num = setInterval(function() {
    // 利用js自动调用点击事件
    next.click()
},1000)
})
// 点击小圆点切换图片
for(let i = 0;i < sliderData.length;i ++){
    lis[i].addEventListener('click',function() {
        // 将指针的位置重置
        ibr = i
        // 打印当前点击小圆点的位置
        console.log(i)
        fun(i)
    })
}
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307292213653.gif)

### 焦点事件之-小米搜索下拉框

**需求**：当表单得到焦点，显示下拉菜单，失去焦点隐藏下拉菜单

![image-20230729233158349](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307292332157.png)

代码：

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        ul {

            list-style: none;
        }

        .mi {
            position: relative;
            width: 223px;
            margin: 100px auto;
        }

        .mi input {
            width: 223px;
            height: 48px;
            padding: 0 10px;
            font-size: 14px;
            line-height: 48px;
            border: 1px solid #e0e0e0;
            outline: none;
        }

        .mi .search {
            border: 1px solid #ff6700;
        }

        .result-list {
            display: none;
            position: absolute;
            left: 0;
            top: 48px;
            width: 223px;
            border: 1px solid #ff6700;
            border-top: 0;
            background: #fff;
        }

        .result-list a {
            display: block;
            padding: 6px 15px;
            font-size: 12px;
            color: #424242;
            text-decoration: none;
        }

        .result-list a:hover {
            background-color: #eee;
        }
    </style>

</head>

<body>
    <div class="mi">
        <input type="search" placeholder="小米笔记本">
        <ul class="result-list">
            <li><a href="#">全部商品</a></li>
            <li><a href="#">小米11</a></li>
            <li><a href="#">小米10S</a></li>
            <li><a href="#">小米笔记本</a></li>
            <li><a href="#">小米手机</a></li>
            <li><a href="#">黑鲨4</a></li>
            <li><a href="#">空调</a></li>
        </ul>
    </div>
    <script src="./js/小米搜索框.js"></script>
</body>
</html>
```

js

```js
// 获取元素对象
const input = document.querySelector('input[type="search"]')
const lis = document.querySelector('.result-list')
// 绑定得到焦点事件
input.addEventListener('focus',function() {
    // 以块级元素显示
    lis.style.display = 'block'
    // 添加类名
    input.classList.add('search')
})
// 绑定失去焦点事件
input.addEventListener('blur',function() {
    // 隐藏
    lis.style.display = 'none'
    // 删除类名
    input.classList.remove('search')
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307292349658.gif)

## 事件对象

-  获取事件对象
-  事件对象常用属性

### 获取事件对象

##### 事件对象是什么

-  也是个==对象==，这个对象有==事件触发时的相关信息==.
-  例如：鼠标点击事件中，事件对象就存了鼠标点在哪个位置等信息

##### 使用场景

-  可以判断用户按下哪个键，比如按下回车键可以发布消息
-  可以判断鼠标点击了哪个元素，从而做相应的操作

**语法**：如何获取

-  在事件绑定的回调函数的第一个参数就是事件对象
-  一般命名为`event`，`ev`，`e` 写什么都行但是这些是规范event就是事件的意思嘛

```js
元素.addEventListener('click',function (e) {//e就是事件对象
   函数体 (要做的事儿)
})
```

#### 部分常用属性

-  type
   -  获取当前的事件类型
-  clientX/clientY
   -  获取光标相对于浏览器可见窗口左上角的位置
-  offsetX/offsetY
   -  获取光标相对于当前DOM元素左上角的位置
-  key
   -  用户按下的键盘的值
   -  现在不提倡使用keyCode

代码：

html

```html
<body>
    <button class="btn">按钮</button>
    <input type="text"/>
   <script src="./js/事件对象.js"></script> 
</body>
```

js

```js
// 获取元素对象 按钮
const button = document.querySelector('.btn')
// 绑定点击事件 () => {} 匿名写法 参数列表中获取事件对象
button.addEventListener('click', function(e) {
    console.log(`click事件对象:${e}`)
    console.log(e)
    console.log(`鼠标点击X轴:${e.clientX},鼠标点击Y轴${e.clientY}`)
    console.log(`事件类型: ${e.type}`)
})
// 获取元素对象 输入文本框
const input = document.querySelector('input[type="text"]')
// 绑定键盘按下事件 () => {} 匿名写法 参数列表中获取事件对象
input.addEventListener('keydown', function(e) {
    console.log(`keydown事件对象:${e}`)
    console.log(e)
    console.log(`事件类型:${e.type}`)
    console.log(`键盘按下的key:${e.key}`)
    if (e.key == 'Enter' || e.key == 'A')
        alert('您输入的回<车键>或者输入大写字母<A>,小写无效,输入其它的也无效哦')
})
```

效果：

![image-20230730120237862](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307301202410.png)

![image-20230730120254486](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307301202724.png)

### 案例—评论回车发布

需求：按下回车键，可以发布消息

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307301206094.gif)

分析：

1.  用到按下键盘事件keydown或者keyup 但是建议使用keyup因为keydown一直按一直触发并不合理。
2.  如果用户按下回车键，发布消息
3.  让留言信息模块显示，把拿到的数据渲染到对应标签内部

**代码**：

html

```html
  <style>
    .wrapper {
      min-width: 400px;
      max-width: 800px;
      display: flex;
      justify-content: flex-end;
    }

    .avatar {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      overflow: hidden;
      background: url(./image/avatar.jpg) no-repeat center / cover;
      margin-right: 20px;
    }

    .wrapper textarea {
      outline: none;
      border-color: transparent;
      resize: none;
      background: #f5f5f5;
      border-radius: 4px;
      flex: 1;
      padding: 10px;
      transition: all 0.5s;
      height: 30px;
    }

    .wrapper textarea:focus {
      border-color: #e4e4e4;
      background: #fff;
      height: 50px;
    }

    .wrapper button {
      background: #00aeec;
      color: #fff;
      border: none;
      border-radius: 4px;
      margin-left: 10px;
      width: 70px;
      cursor: pointer;
    }

    .wrapper .total {
      margin-right: 80px;
      color: #999;
      margin-top: 5px;
      opacity: 0;
      transition: all 0.5s;
    }

    .list {
      min-width: 400px;
      max-width: 800px;
      display: flex;
    }

    .list .item {
      width: 100%;
      display: flex;
    }

    .list .item .info {
      flex: 1;
      border-bottom: 1px dashed #e4e4e4;
      padding-bottom: 10px;
    }

    .list .item p {
      margin: 0;
    }

    .list .item .name {
      color: #FB7299;
      font-size: 14px;
      font-weight: bold;
    }

    .list .item .text {
      color: #333;
      padding: 10px 0;
    }

    .list .item .time {
      color: #999;
      font-size: 12px;
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <i class="avatar"></i>
    <textarea id="tx" placeholder="发一条友善的评论" rows="2" maxlength="200"></textarea>
    <button>发布</button>
  </div>
  <div class="wrapper">
    <span class="total">0/200字</span>
  </div>
  <div class="list">
    <div class="item" style="display: none;">
      <i class="avatar"></i>
      <div class="info">
        <p class="name">清风徐来</p>
        <p class="text">大家都辛苦啦，感谢各位大大的努力，能圆满完成真是太好了[笑哭][支持]</p>
        <p class="time">2022-10-10 20:29:21</p>
      </div>
    </div>
  </div>
  <script src="./js/评论回车发布.js"></script>
</body>
```

js

```js
const textArea = document.querySelector('#tx')
const total = document.querySelector('.total')
const item = document.querySelector('.item')
const text = document.querySelector('.text')
const info = document.querySelector('.info')
// 当我们文本域获得了焦点，就让total显示
textArea.addEventListener('focus', function() {
    total.style.opacity = 1
})
// 当我们文本域失去了焦点，就让total隐藏
textArea.addEventListener('blur', function() {
    total.style.opacity = 0
})
textArea.addEventListener('input', function() {
    let max = textArea.value.length
    console.log(max)
    if (max > 200) {
        max = 200
        alert('内容满了')
    }
    total.innerText = `${max}/200字`
})
textArea.addEventListener('keyup', function(e) {
    if (e.key === 'Enter') {
        if (!textArea.value.trim()) {
            alert('内容为空')
        } else {
            item.style.display = 'block'
            text.innerText = textArea.value
            // 按下回车发送消息后清空文本框内容
        }
        textArea.value = ''
        let max = textArea.value.length
        total.innerText = `${max}/200字`
    }
})
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307301434507.gif)

## 环境对象

目标：能够分析判断==函数==运行在==不同环境中this==所指代的==对象==.

**环境对象**：指的是函数内部特殊的==变量this==，它代表着当前函数运行时所处的环境

**作用**： 弄清楚 this 的指向，可以让我们代码更简洁

-  函数 的调用方式不同，this指代的对象也不同
-  [<font color=red>谁调用，this就是谁</font>] 是判断this指向的粗略规则

直接调用函数，其实相当于是window.函数，所以this指代window

代码：

html

```html
<body>
   <button>点击</button>
   <script src="./js/环境对象.js"></script> 
</body>
```

js

```js
// 每个函数都有this环境对象普通函数里面this指向的是window
function fun() {
    console.log(this)
}
fun()
window.fun()
const button = document.querySelector('button')
// 使用function函数this就是 谁调用我，我就指向谁 也就是现在指向了button
button.addEventListener('click', function () {
    console.log(this)
})
const button1 = document.querySelector('button')
// 使用箭头函数this指向的就是window
button1.addEventListener('click',() => {
    console.log(this)
})
```

效果：

![image-20230730175732623](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307301757741.png)

## 回调函数

如果将函数A做为参数传递给函数B时，我们称函数A为==回调函数==.

回调函数本质还是函数，只不过把它当做参数使用

使用匿名函数做为回调函数比较常见

简单理解：当一个函数当做参数来传递给另外一个函数的时候，这个函数就是==回调函数==.

-  常见的使用场景

```js
function fun() {// 函数A
   console.log('我是回调函数')
}
// fun传递给了setInterval,fun就是回调函数
setInterval(fun,1000) //函数B，每过一秒调用一次函数
//setInterval第一个参数就是回调函数
```

```js
box.addEventListener('click',function () {
   console.log('我也是回调函数')
})
```

## 综合案例-Table栏切换

需求：鼠标经过不同的选项卡，底部可以显示 不同的内容

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307301847996.gif)

代码：

html

```html
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    .tab {
      width: 590px;
      height: 340px;
      margin: 20px;
      border: 1px solid #e4e4e4;
    }

    .tab-nav {
      width: 100%;
      height: 60px;
      line-height: 60px;
      display: flex;
      justify-content: space-between;
    }

    .tab-nav h3 {
      font-size: 24px;
      font-weight: normal;
      margin-left: 20px;
    }

    .tab-nav ul {
      list-style: none;
      display: flex;
      justify-content: flex-end;
    }

    .tab-nav ul li {
      margin: 0 20px;
      font-size: 14px;
    }

    .tab-nav ul li a {
      text-decoration: none;
      border-bottom: 2px solid transparent;
      color: #333;
    }

    .tab-nav ul li a.active {
      border-color: #e1251b;
      color: #e1251b;
    }

    .tab-content {
      padding: 0 16px;
    }

    .tab-content .item {
      display: none;
    }

    .tab-content .item.active {
      display: block;
    }
  </style>
</head>
<body>
  <div class="tab">
    <div class="tab-nav">
      <h3>每日特价</h3>
      <ul>
        <li><a class="active" href="javascript:;">精选</a></li>
        <li><a href="javascript:;">美食</a></li>
        <li><a href="javascript:;">百货</a></li>
        <li><a href="javascript:;">个护</a></li>
        <li><a href="javascript:;">预告</a></li>
      </ul>
    </div>
    <div class="tab-content">
      <div class="item active"><img src="./image/tab00.png" alt="" /></div>
      <div class="item"><img src="./image/tab01.png" alt="" /></div>
      <div class="item"><img src="./image/tab02.png" alt="" /></div>
      <div class="item"><img src="./image/tab03.png" alt="" /></div>
      <div class="item"><img src="./image/tab04.png" alt="" /></div>
    </div>
  </div>
  <script src="./js/tab栏切换.js"></script>
</body>
```

js

```js
// 获取元素对象
const as = document.querySelectorAll('.tab-nav a')
// 给5个as对象绑定鼠标经过事件
for(let i = 0;i < as.length;i ++)
    as[i].addEventListener('mouseenter',function () {
        // 排它思想
        // 移除类 active
        document.querySelector('.tab-nav .active').classList.remove('active')
        // 添加类 active
        // document.querySelector(`.tab-nav li:nth-child(${(i + 1)}) a`).classList.add('active')
        // 简写方式
        this.classList.add('active')
        document.querySelector('.tab-content .active').classList.remove('active')
        document.querySelector(`.tab-content .item:nth-child(${(i + 1)})`).classList.add('active')
    })
```

## 全选文本框案例1

**需求**：用户点击全选，则下面复选框全部选择，取消全选则全部取消

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307302052012.gif)

**分析**：

1.  全选复选框点击，可以得到当前按钮的checked
2.  把下面所有的小复选框状态checked，改为和全选复选框一致

代码：

html

```html
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    table {
      border-collapse: collapse;
      border-spacing: 0;
      border: 1px solid #c0c0c0;
      width: 500px;
      margin: 100px auto;
      text-align: center;
    }

    th {
      background-color: #09c;
      font: bold 16px "微软雅黑";
      color: #fff;
      height: 24px;
    }

    td {
      border: 1px solid #d0d0d0;
      color: #404060;
      padding: 10px;
    }

    .allCheck {
      width: 80px;
    }
    body {
      perspective: 300px
    }
    input {
      transform-style: preserve-3d;
    }
    input:checked {
      animation: move 1s;
    }
    @keyframes move {
      from {
        transform: rotate(0deg)
      }
      to {
        transform: rotateY(360deg)
      }
    }
  </style>
</head>

<body>
  <table>
    <tr>
      <th class="allCheck">
        <input type="checkbox" name="" id="checkAll"> <span class="all">全选</span>
      </th>
      <th>商品</th>
      <th>商家</th>
      <th>价格</th>
    </tr>
    <tr>
      <td>
        <input type="checkbox" name="check" class="ck">
      </td>
      <td>小米手机</td>
      <td>小米</td>
      <td>￥1999</td>
    </tr>
    <tr>
      <td>
        <input type="checkbox" name="check" class="ck">
      </td>
      <td>小米净水器</td>
      <td>小米</td>
      <td>￥4999</td>
    </tr>
    <tr>
      <td>
        <input type="checkbox" name="check" class="ck">
      </td>
      <td>小米电视</td>
      <td>小米</td>
      <td>￥5999</td>
    </tr>
  </table>
  <script src="./js/全选反选案例.js"></script>
</body>
```

js

```js
// 获取元素对象
const checkAll = document.querySelector('#checkAll')
// 获取元素对象集合
const ck = document.querySelectorAll('.ck')
// 给大check绑定单击事件
// let num = 1
checkAll.addEventListener('click', function () {
    // 循环 遍历小check集合并设置checked属性为大check的属性状态 完成 全选和全不选
    for (let i = 0; i < ck.length; i++) {
        ck[i].checked = this.checked
        // if (this.checked === true)
        //     num++
        // else
        //     num--
    }
})
for (let i = 0; i < ck.length; i++)
    ck[i].addEventListener('click', function () {
        // 每次触发点击事件后获取全部的ck中被选中的ck然后获取长度来判断是否满足条件
        checkAll.checked = document.querySelectorAll('.ck:checked').length === ck.length
        // if (ck[i].checked === false)
        //     checkAll.checked = false
        // if (ck[i].checked === true)
        //     num++
        // else
        //     num--
        // if (num > ck.length)
        //     checkAll.checked = true
        // console.log(num)
    })
```

## 事件流

-  事件流与两个阶段说明
-  事件捕获
-  事件冒泡
-  阻止冒泡
-  解绑事件

### 1 事件流和两个阶段说明

-  ==事件流==指的==是==事件==完整执行过程==中的==流动路径==.

![image-20230730211440938](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307302114338.png)

-  说明：假设页面里面有个div，当触发事件时，会经历两个阶段，分别是==捕获阶段==，==冒泡阶段==。

-  简单来说：捕获阶段是 ==从父到子== 冒泡阶段是  ==从子到父==.

-  <font color=red>实际工作都是使用事件冒泡为主</font>.

### 1.2 事件捕获

-  **事件捕获概念**：

从DOM的根元素开始去执行对应的事件(从外到里)

-  事件捕获需要写对应代码才能看到效果

代码：

```js
DOM.addEventListener(事件类型,事件处理函数,是否使用捕获机制)
```

-  **说明**：
   -  addEventListener第三个参数传入布尔值(true / false^默认^)代表是否捕获阶段触发(很少使用)
   -  addEventListener默认不使用捕获机制 除非在第三个参数中开启

**代码**：

html

```html
<body>
    <div class="father">
        <div class="son"></div>
    </div>
    <script src="./js/事件流.js"></script>
</body>
```

js

```js
const fa = document.querySelector('.father')
const son = document.querySelector('.son')
document.addEventListener('click', function () {
    alert('我是爷爷')
})
fa.addEventListener('click', function () {
    alert('我是爸爸')
})
son.addEventListener('click', function () {
    alert('我是儿子')
})
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307302204437.gif)

#### 使用捕获机制

js

```js
const fa = document.querySelector('.father')
const son = document.querySelector('.son')
document.addEventListener('click', function () {
    alert('我是爷爷')
},true)
fa.addEventListener('click', function () {
    alert('我是爸爸')
},true)
son.addEventListener('click', function () {
    alert('我是儿子')
},true)//开启捕获机制
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307302206142.gif)

#### 注意

若是用L0事件监听，则只有冒泡阶段，没有捕获

### 1.3 事件冒泡

**事件冒泡概念** 

当==一个元素的事件被触发时==，==同样的事件==将会在==该元素==的==所有祖先元素中依次被触发==。这一过程称为事件冒泡

-  **简单理解**：当一个==元素触发事件后==，会==依次向上调用所有父级元素==的 <font color=red>同名事件</font>.
   -  ![image-20230731002517078](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307310025638.png)
   -  都是点击才会给你冒泡，同名事件
-  <font color=red>事件冒泡是默认存在的</font>.
-  L2事件监听第三个参数是false，或者默认都是冒泡(默认就是false，true为捕获机制)
-  如果==没有冒泡==^指的是小盒子开启了e.stopPropagation()阻止冒泡^，给==大盒子注册点击事件==，==点击的是==里面的==小盒子==，会==导致大盒子==的==点击无法执行==.

### 1.4 阻止冒泡

-  **问题**：因为==默认就有冒泡模式的存在==，所以==容易导致事件影响到父级元素==.
-  **需求**：若想把==事件==就==限制在当前元素内==，就==需要阻止事件冒泡==.
-  **前提**：==阻止事件冒泡==需要拿到==事件对象==.

**语法**：

```js
事件对象.stopPropagation()
```

-  **注意**：此方法可以阻断事件流动==传播==，不光在==冒泡阶段有效==，==捕获阶段==也==有效==.

**代码**：

html

```html
<body>
    <div class="father">
        <div class="son"></div>
    </div>
    <script src="./js/事件流.js"></script>
</body>
```

js

```js
const fa = document.querySelector('.father')
const son = document.querySelector('.son')
document.addEventListener('click', function () {
    alert('我是爷爷')
})
fa.addEventListener('click', function () {
    alert('我是爸爸')
})
son.addEventListener('click', function (e) {
    alert('我是儿子')
    // 阻止冒泡
    e.stopPropagation()
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307311433790.gif)

### 1.5 阻止事件默认行为

在事件流的任何阶段调用 `preventDefault()` 都会取消事件，这意味着任何通常被该实现触发并作为结果的默认行为都不会发生。

你可以使用 [`Event.cancelable`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/cancelable) 来检查该事件是否支持取消。为一个不支持 cancelable 的事件调用 `preventDefault()` 将没有效果。

**语法**：

```js
event.preventDefault();
```

**代码**：

html

```html
<body>
    <p>请单击复选框控件。</p>
    <form>
        <label for="id-checkbox">Checkbox:</label>
        <input type="checkbox" id="id-checkbox" />
    </form>
    <div id="output-box"></div>
    <script src="./js/阻止默认行为.js"></script>
</body>
```

js

```js
document.querySelector("#id-checkbox").addEventListener("click", function (event) {
    document.getElementById("output-box").innerHTML +=
        "抱歉! <code>preventDefault()</code> 不会让你检查的！<br>";
    //阻止事件的默认行为 也就是 阻止复选框点击后的勾选状态可不是阻止点击事件哦
    event.preventDefault();
});
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307311912257.gif)

**代码案例2** 阻止input输入的默认行为

**代码**：

html

```html
<body>
    <p>请仅使用小写字母输入您的姓名。</p>
        <label for="id-text">input:</label>
        <input type="text" id="id-text" />
    <script src="./js/阻止默认行为.js"></script>
</body>
```

less

```less
.warning {
  border: 2px solid #f39389;
  border-radius: 2px;
  padding: 10px;
  position: absolute;
  background-color: #fbd8d4;
  color: #3b3c40;
}
```

js

```js
//beforeinput在input或select或textArea的值即将被修改时触发事件
const myText = document.querySelector("#id-text")
myText.addEventListener("keydown", function (e) {
    console.log(e)
    let val = +e.keyCode
    console.log(val)
    if (val !== 0)
        if (val < 65 || val > 90) {
            e.preventDefault()
            msgFun("请仅使用小写字母。\n" + "keyCode:" + val + "!\n")
        }
});
let warningtimeout;
const warningBox = document.createElement('div')
warningBox.classList.add('warning')
function msgFun(msg) {
    // 盒子的内容
    warningBox.innerText = msg

    //判断body中是否包含了warningBox元素
    if (document.body.contains(warningBox)) {
        // 包含了就删除上一次的定时器
        window.clearTimeout(warningtimeout);
    } else {
        console.log(myText.parentNode)//body标签
        // 将wanringBox插入到myText的父盒子的前面
        // insertBefore()参数一：用于插入的节点，参数二：将要插在这个节点之前
        myText.parentNode.insertBefore(warningBox, myText.nextSibling);
    }
    // 第二次执行 满足if删除了上一次的定时器 下面执行又创建了一个新的定时器 也就是 刷新定时器
    // 创建定时器2秒后执行操作
    warningtimeout = setTimeout(function () {
        // 删除wanringBox盒子
        warningBox.parentNode.removeChild(warningBox);
    }, 2000);
}
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307312035157.gif)

### 1.6 解绑事件

##### 解绑事件分两个方式

1.  on事件方式，直接使用==null==覆盖就可以实现事件的解绑了

**语法**：

```js
//绑定事件
btn.onclick = function(){
   alert('点击了')
}
//解绑事件
btn.onclick = null
```

**代码**：

html

```html
<body>
   <button>点击</button> 
   <script src="./js/事件解绑.js"></script>
</body>
```

js

```js
const button = document.querySelector('button')
button.onclick = function() {
    // L0 事件移除解绑方式 也可以写在函数体中点击后执行弹出后就解绑了后面点击无效 只有 页面加载后 点击一次有效
    button.onclick = null
    alert('点击了')
}
// L0 事件移除解绑方式 上来就解绑
// button.onclick = null
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307311449027.gif)

2.  addEventListener方式，必须使用：removeEventListener(事件类型，事件处理函数，[获取捕获或者冒泡阶段])
    -  addEventListener参数列表中 第三个参数是[]抱起来的说明可以省略的。

**语法**：

```js
function fun() {
   alert('点击了')
}
//绑定事件
btn.addEventListener('click',fun)
//解绑事件
btn.removeEventListener('click',fun)
```

**注意**：<font color=red>匿名函数无法被解绑</font>.

**代码**：

html

```html
<body>
   <button>点击</button> 
   <script src="./js/事件解绑.js"></script>
</body>
```

js

```js
const button = document.querySelector('button')
function fun () {
    alert('点击了')
}
//绑定事件
button.addEventListener('click',fun)
//解绑事件
button.removeEventListener('click',fun)
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307311452745.gif)

## 两种注册事件的区别

-  传统on注册(L0)
   -  ==同一个对象==，后面注册的事件会==覆盖==前面注册 (同一个事件)
   -  直接使用==null==覆盖就可以实现==事件的解绑==.
   -  都是==冒泡阶段==执行的，没有捕获机制
-  事件监听注册(L2)
   -  语法：==addEventListener==(事件类型，事件处理函数，是否使用捕获)
   -  后面注册的事件==不会覆盖==前面注册的事件(同一个事件)
   -  可以通过==第三个参数==去确定是在==冒泡==或者==捕获阶段执行==.
   -  必须使用==removeEventListener==(事件类型，事件处理函数，获取捕获或者冒泡阶段) ==解绑事件==，<font color=red>无法解绑匿名函数事件</font>.
   -  <font color=red>匿名函数无法被解绑</font>.

## 事件委托

### 思考

1.  如果同时给多个元素注册事件，我们怎么做？

    -  for循环注册事件

    **代码**：

    ```html
    <ul>
       <li>我是第1个小li</li>
       <li>我是第2个小li</li>
       <li>我是第3个小li</li>
       <li>我是第4个小li</li>
       <li>我是第5个小li</li>
    </ul>
    ```

    **绑定事件**：

    ```js
    const lis = document.querySelectorAll('ul li')
    for (let i = 0;i < lis.length;i ++)
       lis[i].addEventListener('click',function () {
          alert('点击了')
       })
    ```

2.  有没有一种<font color=red>技巧</font> 注册一次事件就1能完成以上效果呢？

    -  使用 <font color=red>事件委托</font>.

==事件委托==是利用==事件流==的特征解决一些开发需求的知识<font color=red>技巧</font>.

-  **优点**：减少注册次数，提高程序性能
-  **原理**：==事件委托==其实是==利用事件冒泡的特点==.

-  给<font color=red>父元素注册事件</font>，当我们==触发子元素==的时候，会==冒泡到父元素身上==，从而==触发父元素的事件==.
-  **实现**：==事件对象.target.tagName==可以获得==真正触发事件的元素==^大写命名的^.
-  **e.target 这个对象可以实时获取当前页面点击的 DOM 元素对象** 

![image-20230731155558181](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307311556107.png)

**代码**：

html

```html
<body>
    <ul>
        <li>我是第1个li</li>
        <li>我是第2个li</li>
        <li>我是第3个li</li>
        <li>我是第4个li</li>
        <li>我是第5个li</li>
        <p>我是p标签，我不想变红色，我想变黄色</p>
    </ul>
   <script src="./js/事件委托.js"></script> 
</body>
```

js

```js
// 点击ul 中的子元素li 点击哪个哪个字体变为红色
// 按照事件委托的方式 委托给父级，事件写到父级身上
// 获取li父级元素对象
const ul = document.querySelector('ul')
// 绑定单击事件
ul.addEventListener('click', function (e) {
    console.dir(e.target)//事件对象.target就是点击的对象 点了哪个li对象就是哪个li
    // target中有一个属性是tagName是该标签的大写命名可以用来进行判断点击的是哪个标签
    if (e.target.tagName === 'LI')
        e.target.style.color = 'red'
    else
        //注意，如果直接else判断被点击的父元素ul中子元素都会变成黄色的，所以不想让点击p时却所有的ul子元素都跟着改变一定记得做判断！
        e.target.style.color = 'yellow'
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307311729003.gif)

**总结**：

1.  事件委托的好处是什么？
    -  减少注册次数，提高了程序性能
2.  事件委托是委托给了谁？父元素还是子元素？
    -  父元素
3.  如何找到真正触发的元素
    -  事件对象.target.tagName

### 案例—table栏切换—事件委托方式

**需求**：优化程序，将tab切换案例改为<font color=red>事件委托</font>写法 

![test](./WebAPIs.assets/202307301847996.gif)

**代码**：

html

```html
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    .tab {
      width: 590px;
      height: 340px;
      margin: 20px;
      border: 1px solid #e4e4e4;
    }

    .tab-nav {
      width: 100%;
      height: 60px;
      line-height: 60px;
      display: flex;
      justify-content: space-between;
    }

    .tab-nav h3 {
      font-size: 24px;
      font-weight: normal;
      margin-left: 20px;
    }

    .tab-nav ul {
      list-style: none;
      display: flex;
      justify-content: flex-end;
    }

    .tab-nav ul li {
      margin: 0 20px;
      font-size: 14px;
    }

    .tab-nav ul li a {
      text-decoration: none;
      border-bottom: 2px solid transparent;
      color: #333;
    }

    .tab-nav ul li a.active {
      border-color: #e1251b;
      color: #e1251b;
    }

    .tab-content {
      padding: 0 16px;
    }

    .tab-content .item {
      display: none;
    }

    .tab-content .item.active {
      display: block;
    }
  </style>
</head>
<body>
  <div class="tab">
    <div class="tab-nav">
      <h3>每日特价</h3>
      <ul>
        <!--为每个a添加自定义属性 便于展示下面的div img-->
        <li><a class="active" href="javascript:;" data-id="0">精选</a></li>
        <li><a href="javascript:;" data-id="1">美食</a></li>
        <li><a href="javascript:;" data-id="2">百货</a></li>
        <li><a href="javascript:;" data-id="3">个护</a></li>
        <li><a href="javascript:;" data-id="4">预告</a></li>
      </ul>
    </div>
    <div class="tab-content">
      <div class="item active"><img src="./image/tab00.png" alt="" /></div>
      <div class="item"><img src="./image/tab01.png" alt="" /></div>
      <div class="item"><img src="./image/tab02.png" alt="" /></div>
      <div class="item"><img src="./image/tab03.png" alt="" /></div>
      <div class="item"><img src="./image/tab04.png" alt="" /></div>
    </div>
  </div>
  <script src="./js/tab栏切换.js"></script>
</body>
```

js

```js
// 获取元素对象
const as = document.querySelector('.tab .tab-nav ul')
// img显示第二种写法
const items = document.querySelectorAll('.tab .tab-content .item')
// 给5个as对象绑定鼠标经过事件
as.addEventListener('mouseover',function (e) {
    if(e.target.tagName === 'A'){
        // 排它思想
        // 移除类 active
        document.querySelector('.tab-nav .active').classList.remove('active')
        // 添加类 active
        // document.querySelector(`.tab-nav li:nth-child(${(i + 1)}) a`).classList.add('active')
        // 简写方式
        console.dir(e.target)
        e.target.classList.add('active')
        // this.classList.add('active')
        //通过e.target.dataset.id来获取自定义属性的值 但是需要注意获取到的是字符串类型的
        console.log(e.target.dataset.id)
        document.querySelector('.tab-content .active').classList.remove('active')
        // e.target.dataset.id获取为字符串类型值 + 1后就是拼接字符串了所以使用+来隐式转换数据类型
        // 第一种写法
        // document.querySelector(`.tab-content .item:nth-child(${(+(e.target.dataset.id) + 1)})`).classList.add('active')
        // 第二种写法
        console.log(items)
        items[+(e.target.dataset.id)].classList.add('active')
    }
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307311811238.gif)

## 其它事件

-  页面加载事件
-  元素滚动事件
-  页面尺寸事件

目标：掌握新的事件，做更强交互

### 1 页面加载事件

-  等待外部资源<font color=red>**全部**</font>(如 图片，外联CSS和JavaScript等)==加载完毕==时触发的事件

-  为什么要学？
   -  有些时候需要==等页面资源全部处理完==了做一些事情
   -  老代码喜欢把script写在==head==中 ，这时候直接找==dom==元素找不到，因为文档==从上往下==先css然后html然后script

-  **事件名**：load (翻译：等待)

-  **监听页面所有资源加载完毕**：
   -  给window^最大的对象^添加load事件

**语法**：

```js
//页面加载事件
window.addEventListener('load',function() {
   //执行操作
})
```

**注意**：<font color=red>不光可以监听整个页面资源加载完毕，也可以针对某个资源绑定load事件</font>.

```js
img.addEvnetListener('load',function () {
   //等待图片加载完毕，再执行操作
})
```

**代码**：

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
    <script src="./js/load.js"></script>
</head>
<body>
   <button>按钮</button> 
</body>
</html>
```

js

```js
// 等待页面资源加载完后再执行回调函数
window.addEventListener('load', function () {
    const button = document.querySelector('button')
    button.addEventListener('click', function () {
        alert('点击了')
    })
})
```

效果：

![image-20230731231853992](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307312318456.png)

### 1.1 HTML结构加载事件

-  当==初始HTML文档被完全加载和解析完成之后==，DOMContentLoaded事件被触发，而==无需等待样式表，图像等完全加载==.
-  **事件名**：DOMContentLoaded
-  监听页面DOM加载完毕：
   -  给document^文档对象^添加DOMContentLoaded事件

**语法**：

```js
document.addEventListener('DOMContentLoaded',function() {
   //执行的操作
})
```

**代码**：

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
    <script src="./js/load.js"></script>
</head>
<body>
   <button>按钮</button> 
</body>
</html>
```

js

```js
// 等待HTML结构加载完成后执行回调函数
document.addEventListener('DOMContentLoaded', function () {
    const button = document.querySelector('button')
    button.addEventListener('click', function () {
        alert('点击了')
    })
})
```

**效果**：

![image-20230731231853992](./WebAPIs.assets/202307312318456.png)

### 2 页面滚动事件

==滚动条在滚动==的时候持续触发的事件

-  使用场景
   -  我们想要页面滚动一段距离，比如100px，就让某些元素 显示 / 隐藏
   -  可以使用scroll来检测滚动的距离

-  为什么要学？
   -  很多网页需要检测用户把页面滚动到某个区域后做一些处理，比如固定导航栏，比如返回顶部
-  **事件名**：scroll
-  监听整个页面滚动：

**语法**：

```js
//页面滚动事件
window.addEventListener('scroll',function() {
   //执行的操作
})
//页面滚动事件
document.addEventListener('scroll',function() {
   //执行的操作
})
```

-  给window或document添加scroll事件都可以  <font color=red>**建议给window添加**</font>.

**代码**：

html

```html
    <link rel="stylesheet" href="./css/scroll.css"/>
</head>
<body>
   <script src="./js/scroll.js"></script> 
</body>
```

less

```less
body {
    height: 3000px;
}
```

js

```js
window.addEventListener('scroll',function() {
    console.log('滚动了')
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307312341121.gif)

<font color=red>**注意细节**</font>：<font color=red>只要滚动条</font>==不在原位==<font color=red>上刷新页面也算是触发了scroll事件了</font>.

### 2.1 页面滚动事件—获取位置

scrollLeft和scrollTop(属性)

-  获取被卷去的大小
-  获取元素内容往左，网上滚出去看不到的距离
-  这两个值是可<font color=red>读写</font>的

![image-20230731234502165](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307312345121.png)

**代码**：

html

```html
    <title>Document</title>
    <link rel="stylesheet" href="./css/scroll.css"/>
</head>
<body>
    <div>
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
    </div>
   <script src="./js/scroll.js"></script> 
</body>
```

less

```less
body {
    height: 3000px;
}
div {
    overflow: auto;
    margin: 100px auto;
    width: 300px;
    height: 300px;
    border: 2px #000 solid;
}
```

js

```js
const div = document.querySelector('div')
div.addEventListener('scroll',function() {
    // 打印往上滚动的距离
    console.log(this.scrollTop)
})
```

**效果**：

![image-20230801000541744](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308010005630.png)

### 2.2 页面滚动事件—滚动到指定的坐标

-  scrollTop()方法可把内容滚动到指定的坐标
-  语法：元素.scrollTo(x,y)

```js
//让页面滚动到y轴1000像素的位置
window.scrollTo(0,1000)
```

### 2.3 获取html标签的写法

document.documentElement HTML文档返回对象为

![image-20230801092907559](./WebAPIs.assets/image-20230801092907559.png)

-  开发中，我们经常检测页面滚动的距离，比如页面滚动100像素，就可以显示一个元素，或者固定一个元素

**语法**：

```js
//页面滚动事件
window.addEventListener('scroll',function() {
   //document.documentElement 是 html 元素获取方式
   const n = document.documentElement.scrollTop
   console.log(n)
})
```

**代码**：

html

```html
    <link rel="stylesheet" href="./css/scroll.css"/>
</head>
<body>
    <div class="box" style="display:none">
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
        里面有很多很多的文字
    </div>
    <div class="top" style="display:none">返回顶部</div>
   <script src="./js/scroll.js"></script> 
</body>
```

less

```less
body {
    height: 3000px;

    .box {
        position: fixed;
        overflow: auto;
        left: 50%;
        top: 10px;
        transform: translate(-50%);
        width: 300px;
        height: 300px;
        border: 2px #000 solid;
    }

    .top {
        position: fixed;
        right: 10px;
        bottom: 50px;
        width: 33px;
        height: 43px;
        background-color: #ccc;
    }
}
```

js

```js
// 获取元素对象
const box = document.querySelector('.box')
const top1 = document.querySelector('.top')
window.addEventListener('scroll', function () {
    // 打印往上滚动的距离
    const n = document.documentElement.scrollTop
    console.log(n)
    // 满足条件后以块级元素显示 不满足后不展示
    if (n >= 300)
        box.style.display = 'block'
    else
        box.style.display = 'none'
    // 满足条件后以块级元素显示 不满足后不展示
    if (n >= 1236)
        top1.style.display = 'block'
    else
        top1.style.display = 'none'
})
// 绑定点击事件
top1.addEventListener('click',function() {
    // 滚动条到0的位置 也就是页面的顶部
    document.documentElement.scrollTop = 0
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308011023890.gif)

#### 总结

1.  被卷去的头部或者左侧用哪个属性？是否可以读取和修改？
    -  scrollTop/scrollLeft
    -  可以读取，也可以修改(赋值)
2.  检测页面滚动的头部距离(被卷去的头部)用哪个属性?
    -  document.documentElement.scrollTop

### 3页面尺寸事件

-  会在==窗口尺寸改变==的时候==触发事件==：

-  resize

```js
window.addEventListener('resize',function() {
   //执行代码
})
```

-  检测屏幕宽度大小：
-  document.documentElement.clientWidth

```js
window.addEventListener('resize',function () {
   let w = document.documentElement.clientWidth
   console.log(w)
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308011516138.gif)

### 3.1 页面尺寸事件—获取元素宽高

-  **获取宽高**：
   -  获取元素的==可见部分宽高==(不包含border，margin，scroll等)
   -  ==clientWidth==和==clientHeight==. 

![image-20230801144134635](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308011441893.png)

**代码**：

html

```html
    <link rel="stylesheet" href="./css/clientWidth和clientHeight.css"/>
</head>
<body>
    <div class="box">123123123123</div>
   <script src="./js/clientWidth和clientHeight.js"></script> 
</body>
```

less

```less
.box {
    display: inline-block;
    height: 300px;
    background-color: pink;
    padding: 10px;
    border: 20px red solid;
}
```

js

```js
const box = document.querySelector('.box')
console.log(`boxWidth:${box.clientWidth}`)
console.log(`boxHeight:${box.clientHeight}`)
```

**效果**：

![image-20230801151208476](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308011512956.png)

## 元素尺寸与位置

-  **使用场景** 
   -  前面案例滚动多少距离，都是我们自己算的，最好是页面滚动到==某个元素==，就可以==做某些事==。
   -  简单说，就是通过js的方式，得到<font color=red>元素在页面中的位置</font>.
   -  这样我们可以做，页面滚动到这个位置，就可以做某些操作，省去计算了

![image-20230801144921204](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308011449092.png)

### 1 元素尺寸与位置—尺寸

-  **获取宽高**：
   -  获取元素的==自身宽高==，==包含==元素==自身设置的宽高==，==padding==，==border==.
   -  offsetWidth和offsetHeight
   -  获取出来的是==数值==，方便计算
   -  **注意**：获取的是==可视宽高==，如果盒子是==隐藏==的，获取的==结果是0==.
-  **获取元素位置**：
   -  获取元素距离自己==定位父级元素^有定位的父级元素以父级元素为准^==的==左==，==上==距离
   -  **offsetLect**和**offsetTop** 注意是<font color=red>只读属性</font>.

**代码**：

html

```html
    <link rel="stylesheet" href="./css/offsetLeft.css"/>
</head>
<body>
    <div class="box">
        <p class="p"></p>
    </div>
   <script src="./js/offsetLeft.js"></script> 
</body>
```

less

```less
.box {
  width: 200px;
  height: 200px;
  background-color: pink;
  margin: 100px;
}
.box .p {
  width: 100px;
  height: 100px;
  background-color: purple;
  margin: 50px;
}
```

js

```js
const box = document.querySelector('.box')
const p = document.querySelector('.p')
console.log(`boxWidth:${box.offsetWidth}`)
console.log(box.offsetHeight)
// 获取元素位置
console.log(box.offsetLeft)
console.log(box.offsetTop)
// 检测盒子的位置，最近一级带有定位的祖先元素
console.log(p.offsetTop)
```

**效果**：

![image-20230801155521088](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308011555542.png)

**总结**：

1.  offsetWidth和offsetHeight是得到元素什么的宽高？
    -  内容 + padding + border
2.  offsetTop和offsetLeft得到位置以谁为准？
    -  带有==定位==的==父级==.
    -  如果都没有则以，文档左上角为准

offset与client获取元素宽高的区别效果：

![image-20230801155726430](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308011557760.png)

### 2 元素尺寸与位置-尺寸

-  **获取位置**：

element.getBoundingClientRect()

方法返回元素的大小及其相对于==视口的位置==.

![image-20230801233349199](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308012333704.png)

**代码**：

html

```html
    <link rel="stylesheet" href="./css/getBoundingClientRect.css"/>
</head>
<body>
   <div class="box"></div> 
   <script src="./js/getBoundingClientRect.js"></script>
</body>
```

less

```less
body {
    height: 2000px;
}
.box {
    width: 300px;
    height: 300px;
    background-color: pink;
    margin: 100px auto;
}
```

js

```js
const box = document.querySelector('.box')
console.log(box.getBoundingClientRect())
```

**效果**：

![image-20230801234220843](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308012342842.png)

将滚动条向上卷去

![image-20230801234236697](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308012342951.png)

## 总结

| 属性                      | 作用                                        | 说明                                                         |
| ------------------------- | ------------------------------------------- | ------------------------------------------------------------ |
| scrollLeft和scrollTop     | 被卷去的头部和左侧                          | 配合页面滚动来用，<font color=red>可读写</font>.             |
| clientWidth和clientHeight | 获得元素宽度和高度                          | ==不包含==border，margin，scroll 用于js获取元素大小，==只读属性==。 |
| offsetWidth和offsetHeight | 获得元素高度和宽度                          | ==包含==border，padding，scroll等，==只读属性==.             |
| offsetLeft和offsetTop     | 获取元素距离自己定位父级元素的左，上距离    | 获取元素位置的时候使用，==只读属性==.                        |
| getBoundingClientRect     | 方法返回元素的大小及其相对于==视口的位置==. | 不受有定位的父元素的影响，得到的是相对于==视口的坐标==.      |
