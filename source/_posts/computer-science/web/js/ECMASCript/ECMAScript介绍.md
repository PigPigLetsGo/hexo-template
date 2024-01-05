---
title: JavaScript语言基础|JavaScript介绍
categories:
    - [计算机学科,web,js,ECMASCript]
tags:
    - 计算机学科
    - web
    - js
    - ECMASCript
    - 介绍
---

# JavaScript介绍:zap: 

## 什么是JavaScript:question: 

### JavaScript权威网站: [😉](https://developer.mozilla.org/zh-CN/docs/web/Javascript) 点击表情进入

**官方定义**:

-  是一种==运行在客户端==（浏览器）的==编程语言==，实现==人机交互效果==。

**作用**：

-  页面特效（监听用户的一些行为让网页做出对应的反馈）
-  表单验证（针对表单数据的合法性进行判断）
-  数据交互（获取后台的数据，渲染到前端）
-  服务端编程（node.js）

![image-20230419213457889](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211043391.png)

## JavaScript的组成:clamp: 

### JavaScript可以分成两大类:leaves: 

#### ECMAScript:star: 

-  JavaScript语言基础
   -  JavaScript遵循的是ECMAScript的规范
   -  规定了js基础语法核心知识
      -  **比如**: ==变量==，==分支语句==，==循环语句==，==对象==等等

#### Web APIs :star:

#### 又分为 : DOM，BOM 

##### DOM:star2: 

页面文档对象模型

-  操作文档，比如对==页面元素进行移动，大小，添加，删除==等操作

##### BOM:star2: 

浏览器对象模型

-  操作浏览器，比如==页面弹窗，检测窗口宽度，存储数据到浏览器==等等

## JavaScript书写位置:guitar: 

### 1.内部JavaScript:cocktail: 

直接写在html文件里，用Script标签包住

规范：Script标签写在</body>上面

扩展：alert('你好,js');页面弹出警告对话框

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--内部js-->
<script>
    <!--弹出警示框-->
    alert('你好,js');
</script>
</body>
<!--内部js的错误写法-->
<!--<script></script>-->
</html>
```

<font style="color:red">**注意事项**</font> :warning: 

>  我们将<script>放在html文件的底部附近的原因是浏览器会按照代码在文件中的<font style="color:red">顺序加载HTML</font>。
>  如果先加载的JavaScript期望修改其下方的HTML,那么它可能由于HTML尚未被加载而失效。
>  因此，<font style="color:green">将JavaScript代码放在HTML页面的底部附近通常是最好的策略</font>。:green_book: 

### 2.外部JavaScript:wastebasket: 

代码写在以.js结尾的文件里

:video_game: <font style="color:red">语法</font>:通过Script标签，引入到html页面中。

###### 目录结构:scorpion: 

![image-20230420074424033](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211049478.png)

-  在其它文件夹下创建一个我们要引入的js文件

```js
alert('我是外部.js')
```

-  在index.html文件中引入外部的js文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--引入外部js-->
<script src="./static/My.js"><!--这里不能写内容--></script>
</body>
</html>
```

<font style="color:red">**注意事项**</font>: :warning: :red_circle: 

>  1.  script标签中间无需代码，否则会被忽略。
>  2.  外部JavaScript会使代码更加有序，更易于复用，且没有了脚本的混合，HTML也会更加易读，因此这是个好的习惯。

### 3.行内JavaScript:four_leaf_clover: 

代码写在标签内部

语法：:space_invader: 

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--行内JavaScript-->
<button onclick="alert('我被点击了')">我是按钮</button>
</body>
</html>
```

## JavaScript注释:christmas_tree: 

**JavaScript中可以把注释分为两种:** :dart: 

### 第一种:  单行注释:evergreen_tree: 

符号：//

作用：//右边这一行的代码会被忽略

```js
<script>
    //单行注释
</script>
```

### 第二种：块注释:palm_tree: 

符号：/* */

作用：在/* 和 */之间的所有内容都会被忽略

```js
<script>
    /*多行注释/块注释*/
</script>
```

## 结束符:stopwatch: 

:funeral_urn: **作用**：使用英文的 `;` 代表语句结束

:clipboard: **实际情况**：实际开发中，可写可不写，浏览器（JavaScript引擎）可以自动推断语句的结束位置

:clipboard: **现状**：在实际开发中，越来越多的人主张，书写JavaScript代码时省略结束符

<font style="color:red">**约定：为了风格统一，结束符要么每句都写，要么每句都不写（按照团队要求）**</font> :small_red_triangle: 

## 输入和输出的语法:radio_button: 

**什么是语法** :question: 

-  人和计算机打交道的规则约定

-  我们要按照这个规则去写

==输出==和==输入可理解为人和计算机的交互==，==用户通过键盘==，==鼠标==等==向计算机输入信息==，==计算机处理后再展示结果给用户，这便是一次输入和输出的过程==。

### 输出语法:outbox_tray: 

-  **语法1** :game_die: 
   -  **作用**：向body内输出内容

```js
<script>
    /*多行注释/块注释*/
    document.write('这是写出的内容');
</script>
```

:warning: **注意**：如果输出的内容写的是==标签==，也会被解析==成网页元素==。

```js
<script>
    /*多行注释/块注释*/
    document.write('这是写出的内容');
    document.write('<h1>我是标题</h1>');
</script>
```

![image-20230420163244224](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211110439.png)

-  **语法2** :game_die: 
   -     :clipboard: **作用**：页面弹出警告对话框

```js
<script>
   alert('你好,js');
</script>
```

-  **语法3**  :game_die: 
   -  :clipboard: **作用**：控制台输出语法，程序员调式使用

```js
<script>
   console.log('控制台打印:你好,js');
</script>
```

![image-20230420164733555](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211110254.png)

#### 写出一个99乘法表打印到页面上:stars: 

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #table td{
            text-align:center;
            border:red 2px solid;
            border-radius:35%;
            background-color: blueviolet;
        }
    </style>
</head>
<body>
   <script>
    document.write('<table id="table">');
    for(let i = 1;i <= 9;i++){
        document.write('<tr>');
        for(let j = 1;j <= i;j++){
            document.write('<td>'+i+'*'+j+'='+i*j+'</td>');
        }
        document.write('</tr>');
    }
    document.write('</table>');
   </script> 
</body>
</html>
```

效果

![image-20230423210725873](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211110230.png)

### 输入语法:inbox_tray: 

-  **语法**::game_die: 
   -  :clipboard: **作用**: 显示一个对话框，对话框中包含一条文字信息，用来提示用户输入文字

```js
<script>
    <!--显示一个带有提示的对话框,让用户输入-->
    prompt('请输入您的姓名:');
</script>
```

![image-20230420175305554](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307211110015.png)

## JavaScript代码执行顺序:fleur_de_lis: 

按HTML文档流顺序执行JavaScript代码

==alert==和==prompt==它们会==跳过页面渲染先被执行==:detective: 

```js
<script>
    /*执行顺序:
    prompt:第一个执行
    write:第二个执行
    console:第三个执行*/
    //显示一个带有提示的对话框,让用户输入
    let name = prompt('请输入您的姓名:');
    document.write('你好'+name);
    console.log(name+'打开了控制台');
</script>
```

## 字面量:christmas_tree: 

:desert: 

<u>在计算机科学中，字面量（literal）是在计算机中描述  事/物</u>。

比如：

-  我们工资是：<u>1000</u>此时<u>1000</u>就是数字字面量
-  ‘<u>乾坤未定</u>’ 字符串字面量
-  还有接下来我们学的 [ ] 数组字面量 { } 对象字面量 等等