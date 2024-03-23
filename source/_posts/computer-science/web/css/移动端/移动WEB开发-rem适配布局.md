---
title: 移动端WEB-rem适配布局
categories: 
    - [计算机学科,web,css,移动端]
tags:
    - web
    - 计算机学科
---

## 移动WEB开发之rem适配布局

### rem基础

#### rem单位

rem(root em)是一个相对单位，类似于em，em是父元素字体大小。

不同的是<font color="red">rem</font>的基准是相对于html元素的<font color="red">字体大小</font>。

比如，根元素(html)设置font-size=12px; 非根元素设置width:2rem; 则换成 px 表示就是24px。

**优点**：

-  所有使用rem单位的盒子都通过html元素字体大小统一被控制
-  等比例缩放。

**em**:

```html
    <style>
        div {
            font-size: 12px;
        }
        p {
            width: 10em;
            height: 10em;
            background-color: pink;
        }
    </style>
</head>
<body>
   <div>
    <p></p>
   </div> 
</body>
```

效果：

![image-20230725200748180](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307252007717.png)

**rem**:

```html
    <style>
        html {
            /* font-size: 14px; */
            /* html字体大小变小后控制所有rem单位盒子也变小 */
            font-size: 10px;
        }
        div {
            width: 15rem;
            height: 15rem;
            font-size: 12px;
            background-color: purple;
        }
        p {
            /* 1.em相对于父元素的字体大小来说的 */
            /* width: 10em;
            height: 10em; */
            /* 2.rem相对于html元素字体大小来说的 */
            width: 10rem;
            height: 10rem;
            background-color: pink;
            /* 3.rem的优点就是可以通过修改html里面的文字大小来改变页面中元素的大小可以整体控制 */
        }
    </style>
</head>
<body>
   <div>
    <p></p>
   </div> 
</body>
```

![image-20230725201735129](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307252017624.png)

### 媒体查询

#### 什么是媒体查询

媒体查询(Media Query)是CSS3新语法。

-  使用`@media`查询，可以针对不同的媒体类型定义不同的样式
-  <font color="red">`@media`可以针对不同的屏幕尺寸设置不同的样式</font>.
-  当你重置浏览器大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面
-  目前针对很多苹果手机，Android手机，平板等设备都用得到 <font color="red">多媒体查询</font>.

#### 语法规范

```css
@media mediatype and|not|only (media feature) {
   CSS-Code;
}
```

-  用`@media`开头 注意`@`符号
-  `mediatype`媒体类型
-  关键字`and  not  only`.
-  `media feature`媒体特性 必须有小括号包含

#### 1 mediatype 查询类型

将不同的终端设备划分成不同的类型，成为媒体类型

| 值    | 解释说明                           |
| ----- | ---------------------------------- |
| all   | 用于所有设备                       |
| print | 用于打印机和打印预览               |
| scree | 用于电脑屏幕，平板电脑，智能手机等 |

#### 2 关键字

关键字将媒体类型或多个媒体特性连接到一起作为媒体查询的条件

-  `and`：可以将多个媒体特性连接到一起，相当于 "且" 的意思
-  `not`：排除某个媒体类型，相当于 "非" 的意思，可以省略
-  `only`：指定某个特定的媒体类型，可以==省略==。

#### 3 媒体特性

每种媒体类型都具体各自不同的特性，根据不同媒体类型的媒体特性设置不同的展示风格。==暂且了解三个== <font color="red">注意它们要加小括号</font>。

| 值        | 解释说明                           |
| --------- | ---------------------------------- |
| width     | 定义输出设备中页面可见区域的宽度   |
| min-width | 定义输出设备中页面最小可见区域宽度 |
| max-width | 定义输出设备中页面最大可见区域宽度 |

代码：

```html
<style>
   /* 这句话的意思就是:在我们屏幕上 并且 最大的宽度是800像素 设置我们想要的样式
   max-width 小于等于800像素 */
   @media screen and (max-width: 800px) {
      body {
         background-color: pink;
      }
   }
   /* min-width大于等于800像素 */
   @media screen and (min-width: 800px) {
      body {
         background-color: purple;
      }
   }
</style>
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307252048745.gif)

#### 媒体查询 + rem 实现元素动态大小变化

rem单位是跟着html来走的，有了rem页面元素可以设置不同大小尺寸

媒体查询可以根据不同设备宽度来修改样式

媒体查询+rem 就可以实现不同设备宽度，实现页面元素大小的动态变化

代码：

```html
    <style>
        * {
            margin: 0;
        }
        @media screen and (min-width: 320px) {
            html {
                font-size: 50px;
            }
        }
        @media screen and (min-width: 640px) {
            html {
                font-size: 100px;
            }
        }
        @media screen and (min-width: 800px) {
            html {
                font-size: 120px;
                background-color: red;
                text-align: center;
                line-height: 4rem;
                animation: move 1s linear infinite;
                &::after {
                    content: '警告!';
                    font-size: 1rem;
                    display: block;
                    width: 3rem;
                    margin: 0 auto;
                    animation: dou 1s infinite;
                    text-shadow: 3px 3px rgba(17, 1, 1, 1);
                }
            }
            @keyframes dou {
                0%{}
                45% {
                    transform: translateX(2px);
                }
                50% {
                    transform: translateX(0);
                }
                55% {
                    transform: translateX(-2px);
                }
                70% {
                    transform: translateX(0);
                }
                85% {
                    transform: translateX(-3px);
                }
                90% {
                    transform: translateX(0);
                }
                100% {
                    transform: translateX(3px);
                }
            }
            @keyframes move {
                from {
                    background-color: purple;
                }
                to {
                    background-color: red;
                }
            }
        }
        .top {
            height: 1rem;
            font-size: .5rem;
            background-color: green;
            color: #fff;
            text-align: center;
            line-height: 1rem;
        }
    </style>
</head>
<body>
   <div class="top">购物车</div> 
</body>
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307252155640.gif)

#### 引入资源(理解)

当样式比较繁多的时候，我们可以针对不同的媒体使用不同stylesheets(样式表)

原理，就是直接在link中判断设备的尺寸，然后引用不同的css文件达到预期的样式效果。

##### 1 语法规范

```css
<link rel="stylesheet" media="mediatype and|not|only (media feature)" href="mystylesheet.css">
```

### less基础

#### 维护CSS的弊端

CSS是一门非程序式语言，没有变量，函数，SCOPE(作用域)等概念。

-  CSS需要书写大量看似没有逻辑的代码 ，CSS冗余度比较高的。
-  不方便维护及扩展，不利于复用
-  CSS没有很好的计算能力
-  非前端开发工程师来讲，往往会因为缺少CSS编写经验而很难写出组织良好且易于维护的CSS代码项目

#### less介绍

Less(Leaner Style Sheets的 缩写)是一门CSS扩展语言，也称为CSS预处理器

做为CSS的一种形式的扩展，它并没有减少CSS的功能，而是在现有的CSS语法上，为CSS加入程序式语言的特性。

它在CSS的语法基础之上，引入了变量，Mixin(混入)，运算以及函数等功能，大大简化了CSS的编写，并且降低了CSS的维护成本，就像它的名称所说的那样，Less可以让我们用更多的代码做更多的事情。

Less中文网站：http://lesscss.cn/    or     https://less.nodejs.cn/

常见的CSS预处理器：Sass，Less，Stylus

**一句话**：<strong style="color:red">Less是一门CSS预处理语言，它扩展了CSS的动态特性</strong>。

#### 1 Less使用

新建一个后缀名为less的文件，在这个less文件里面书写less语句。

#### 2 Less变量

变量是指没有固定的值，可以改变的。因为我们CSS中的一些颜色和数值等经常使用。

```css
@变量名:值;
```

例如：

```less
/*@变量名:值;*/
@color:pink;
```

##### 1.1 变量命名规范

-  必须有@为前缀
-  不能包含特殊字符
-  不能以数字开头
-  大小写敏感

##### 2.1 Less注释

-  使用`//`来进行注释

#### 3 Less编译

本质上，Less包含一套自定义的语法及一个解析器，用户根据这些语法定义自己的样式规则，这些规则最终会通过解析器，编译生成对应的CSS文件。

所以，我们需要把我们的less文件，编译生成为css文件，这样我们的html页面才能使用。

##### 3.1 vscode Less插件:star: 

Esay Less插件用来把less文件编译为css文件

安装完毕插件，重新加载下vscode。

只要把存一下Less文件，会自动生成CSS文件。

![image-20230726101358393](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261014082.png)

编写好less后 保存后目录结构发生的变化

![image-20230726101454114](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261014113.png)

生成了一个my.css文件，将这个文件link引入大html文件中就可以使用了。每次修改less保存就可以修改my.css文件的内容了。

#### 4 Less嵌套

在less中将子级的元素写在父元素css作用域中

##### 4.1 Less嵌套写法

```less
.header {
    width: 200px;
    height: 200px;
    background-color: pink;
    /*1.less嵌套 子元素的样式直接写到父元素里面就好了*/
    a {
        color: purple;
    }
}
```

经过编译后结果如下：

```css
.header {
  width: 200px;
  height: 200px;
  background-color: pink;
}
.header a {
  color: purple;
}
```

##### 4.2 如果遇见(交集|伪类|伪元素选择器)

-  内层选择器的前面没有&符号，则它被解析为选择器的后代；
-  如果有&符号，他就被解析为父元素自身或父元素的伪类；
-  如果有伪类，交集选择器，伪元素选择器 内层选择器的前面需要加&连接起来.

伪类选择器

CSS

```css
a:hover {
   color:red;
}
```

Less嵌套写法

```less
a {
   &:hover {
      color: @变量名;
   }
}
```

伪元素

CSS

```css
.nav .log {
  color: green;
}
.nav::after {
  content: '你也好啊,我是伪元素';
  color: green;
}
```

Less嵌套写法

```less
.nav {
    .log {
        color: @colorLv;
    }
    &::after {
        content: '你也好啊,我是伪元素';
        color: @colorLv;
    }
}
```

交集选择器

CSS

```css
div.nav {
  background-color: pink;
}
div.nav .log {
  color: green;
}
```

Less嵌套写法

```less
div&.nav{
    background-color: @colorP;
    .log{
        color: @colorLv;
    }
}
```

#### 5 Less运算

任何数字，颜色或者变量都可以参与运算。就是Less提供了 加(+) ，减 (-) ，乘 (*) ，除 (/) 算术运算。

<strong style="color:red">注意</strong>：

-  <strong style="color:red">如果 `/` 运算符没有被解析则 在表达式中带上小括号抱起来</strong>。
-  乘号(*) 和除号(/) 的写法
-  运算符中间左右有个空格隔开 1px + 5
-  对于两个不同的单位的值之间的运算，运算结果的值取第一个值的单位
-  如果两个值之间只有一个值有单位，则运算结果就取该单位

代码：

html

```html
    <link rel="stylesheet" href="./css/count.css">
</head>
<body>
    <div></div>
    <img src="../imgage/hot图标.png" alt=""/>
</body>
```

less

```less
@baseFont: 50px;
@border: 5px + 5;
@colorRed: red;
html {
    font-size: @baseFont;
}
div {
    width: (200px - 50) * 2;
    height: 200px * 2;
    border: @border @colorRed solid;
    //运算结果：#44444
    background-color: #666 - #222;
}
img {
    //两个数都有单位结果为第一个单位为准 为rem单位
    width: (82rem / @baseFont);
    height: (82rem / @baseFont);
}
```

css

```css
html {
  font-size: 50px;
}
div {
  width: 300px;
  height: 400px;
  border: 10px red solid;
  background-color: #444444;
}
img {
  width: 1.64rem;
  height: 1.64rem;
}
```

效果：

![image-20230726111516598](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261115141.png)

#### 6 导入其它less文件到当前less文件中

将common.less引入到index.less中 语法如下：

```less
//在index.less中导入 common.less文件
@import "common";
```

common.less文件

```les
html {
    font-size: 50px;
}
// 设置常见的屏幕尺寸修改html里面的文字大小
// 320
// 我们此次定义的划分的份数为15
@no: 15;
@media screen and (min-width: 320px) {
    html {
        font-size: (320px / @no);
    }
}
// 360px
@media screen and (min-width: 360px) {
    html {
        font-size: (360px / @no);
    }
}
// 375px
@media screen and (min-width: 375px) {
    html {
        font-size: (375px / @no);
    }
}
// 384px
@media screen and (min-width: 384px) {
    html {
        font-size: (384px / @no);
    }
}
// 400px
@media screen and (min-width: 400px) {
    html {
        font-size: (400px / @no);
    }
}
// 414px
@media screen and (min-width: 414px) {
    html {
        font-size: (414px / @no);
    }
}
// 424px
@media screen and (min-width: 424px) {
    html {
        font-size: (424px / @no);
    }
}
// 480px
@media screen and (min-width: 480px) {
    html {
        font-size: (480px / @no);
    }
}
// 540px
@media screen and (min-width: 540px) {
    html {
        font-size: (540px / @no);
    }
}
// 720px
@media screen and (min-width: 720px) {
    html {
        font-size: (720px / @no);
    }
}
// 750px
@media screen and (min-width: 750px) {
    html {
        font-size: (750px / @no);
    }
}
```

导入到index.less中

```les
@import "common";
```

生成的index.css查看如下：

```css
html {
  font-size: 50px;
}
@media screen and (min-width: 320px) {
  html {
    font-size: 21.33333333px;
  }
}
@media screen and (min-width: 360px) {
  html {
    font-size: 24px;
  }
}
@media screen and (min-width: 375px) {
  html {
    font-size: 25px;
  }
}
@media screen and (min-width: 384px) {
  html {
    font-size: 25.6px;
  }
}
@media screen and (min-width: 400px) {
  html {
    font-size: 26.66666667px;
  }
}
@media screen and (min-width: 414px) {
  html {
    font-size: 27.6px;
  }
}
@media screen and (min-width: 424px) {
  html {
    font-size: 28.26666667px;
  }
}
@media screen and (min-width: 480px) {
  html {
    font-size: 32px;
  }
}
@media screen and (min-width: 540px) {
  html {
    font-size: 36px;
  }
}
@media screen and (min-width: 720px) {
  html {
    font-size: 48px;
  }
}
@media screen and (min-width: 750px) {
  html {
    font-size: 50px;
  }
}
```

**结果**：会将common.less中的内容全部导入到index.css中

### rem适配方案

1.  让一些不能等比例自适应的元素，达到当设备尺寸发生改变的时候，等比例适配当前设备。
2.  使用媒体查询根据不同设备按比例设置html的字体大小，然后页面元素使用rem做尺寸单位，当html字体大小变化元素尺寸也会发生变化，从而达到等比例缩放的适配。

#### 1 rem实际开发适配方案

1.  按照设计稿与设备宽度的比例，动态计算并设置html跟标签的font-size大小；(媒体查询)
2.  CSS中，设计稿元素的宽，高，相对位置等取值，按照同等比例换算为rem为单位的值；

#### 2 rem适配方案技术使用(市场主流)

##### 技术方案1

-  less
-  媒体查询
-  rem

##### 技术方案2 (推荐)

-  flexible.js
-  rem

#### 总结：

1.  两种方案现在都存在。
2.  方案2更简单，现阶段大家无需了解里面的 js 代码。

#### 2.1 rem实际开发适配方案1

rem + 媒体查询 + less 技术

##### 1 设计稿常见尺寸宽度

| 设备         | 常见宽度                                                     |
| ------------ | ------------------------------------------------------------ |
| iphone 4.5   | 640px                                                        |
| iphone 6/7/8 | 750px                                                        |
| Android      | 常见320px，360px，375px，384px，400px，414px，500px，720px<br><font color="red">大部分4.7 ~ 5寸的安卓设备为720px</font>. |

一般情况下，我们以一套或两套效果图适应大部分的屏幕，放弃极端屏或对其优雅降级，牺牲一些效果<font color="red">现在基本以750为准</font>。

#### 2 动态设置html标签font-size大小

1.  假设设计稿是==750px==。
2.  假设我们把==整个屏幕划分为15等份==(划分标准不一定可以是20份 也可以是10等份)
3.  每一份作为html字体大小，这里就是==50px==。
4.  那么在==320px设备==的时候，==字体大小为320 / 15==就是==21.33px==。
5.  用我们==页面元素的大小== <strong style="color:red">除以</strong> ==不同的html字体大小==会发现它们==比例还是相同==的
6.  比如我们以==750px==为标准设计稿
7.  一个100 * 100像素的页面元素在 750 屏幕下，就是100 / 50转换为2rem * 2rem比例是1 比 1
8.  320屏幕下，html字体大小为21.33则2rem = 42.66px 此时宽和高都是 42.66 但是 宽和高的比例还是 1比1
9.  但是已经能实现不同屏幕下，页面元素盒子等比例缩放的效果

#### 3 元素大小取值方法

1.  最后的公式：页面元素的rem值 = 页面元素值(px) / (屏幕宽度 / 划分的份数)
    -  比如：2rem = 100 / 50 (750 / 15 = 50)
2.  屏幕宽度 / 划分的份数 就是html font-size的大小
3.  或者：页面元素的rem值 = 页面元素值(px) / html font-size字体大小
    -  比如：2rem = 100 / 50 (50怎么来的看上面步骤1)

**代码**：

```html
    <style>
        @media screen and (min-width: 320px) {
            html {
                /*320px屏幕划分15等份计算公式：320 / 15 = 21.33*/
                font-size: 21.33px;
            }
        }
        @media screen and (min-width: 750px) {
            html {
                /*750px屏幕划分15等份计算公式：750 / 15 = 50*/
                font-size: 50px;
            }
        }
        div {
            /*100 / 50 = 2rem
            页面元素的rem值 = 页面元素值(px) / (屏幕宽度 / 划分的份数)*/
            width: 2rem;
            height: 2rem;
            background-color: pink;
        }
    </style>
</head>
<body>
    <div></div>
</body>
```

### 苏宁网移动端案例

#### 1 技术选型

**方案**：采取单独制作移动页面方案

**技术**：布局采取rem适配布局(less + rem + 媒体查询)

**设计图**：本设计图采用750px设计尺寸

#### 2 文件夹结构

![image-20230726134031571](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261340554.png)

#### 3 设置公共common.less文件

1.  新建common.less 设置好最常见的屏幕尺寸，利用媒体查询设置不同的html字体大小，因为除了首页其它页面也需要
2.  我们关心的尺寸有 320px，360px，375px，384px，400px，414px，424px，480px，540px，720px，750px
3.  划分的份数我们定位15等份
4.  因为我们PC端也可以打开我们苏宁移动端首页，我们默认html字体大小为50px，<strong style="color:red">注意这句话写到最上面</strong>.

#### 4 新建index.less文件

1.  新建index.less 这里面写首页的样式
2.  将刚才设置好的common.less引入到index.less里面 语法如下：

```less
//在index.less中导入 common.less文件
//将公共样式导入到首页样式中节省代码空间看起来特别简洁
@import "common";
```

查看index.css文件内容：

```css
html {
  font-size: 50px;
}
@media screen and (min-width: 320px) {
  html {
    font-size: 21.33333333px;
  }
}
@media screen and (min-width: 360px) {
  html {
    font-size: 24px;
  }
}
@media screen and (min-width: 375px) {
  html {
    font-size: 25px;
  }
}
@media screen and (min-width: 384px) {
  html {
    font-size: 25.6px;
  }
}
@media screen and (min-width: 400px) {
  html {
    font-size: 26.66666667px;
  }
}
@media screen and (min-width: 414px) {
  html {
    font-size: 27.6px;
  }
}
@media screen and (min-width: 424px) {
  html {
    font-size: 28.26666667px;
  }
}
@media screen and (min-width: 480px) {
  html {
    font-size: 32px;
  }
}
@media screen and (min-width: 540px) {
  html {
    font-size: 36px;
  }
}
@media screen and (min-width: 720px) {
  html {
    font-size: 48px;
  }
}
@media screen and (min-width: 750px) {
  html {
    font-size: 50px;
  }
}
```

直接将index.css引入到index.html页面中编写index.less这样就节省了很多的代码空间了

```html
<link rel="stylesheet" href="./css/index.css">
```

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
    <link rel="stylesheet" href="./css/normalize.css">
    <link rel="stylesheet" href="./css/index.css">
</head>
<body>
    <!--顶部搜索框-->
    <div class="search-content">
        <a href="#" class="classify"></a>
        <div class="search">
            <form action="" method="get">
                <input type="search" id="" name="" placeholder="请输入..."/>
            </form>
        </div>
        <a href="#" class="login">登录</a>
    </div>
    <!--banner部分-->
    <div class="banner">
        <img src="./upload/banner.gif" alt=""/>
    </div>
    <!--广告部分-->
    <div class="ad">
        <a href="#"><img src="./upload/ad1.gif" alt=""/></a>
        <a href="#"><img src="./upload/ad2.gif" alt=""/></a>
        <a href="#"><img src="./upload/ad3.gif" alt=""/></a>
    </div>
    <!--nav模块-->
    <nav>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
        <a href="#">
            <img src="./upload/nav1.png" alt=""/>
            <span>爆款手机</span>
        </a>
    </nav>
</body>
</html>
```

index.less

```less
@import "common";
body {
    min-width: 320px;
    // 为什么是15rem 因为要沾满等份显示内容如下：
    // 750 / 50 = 15 总共分15等份宽度沾满15等份
    width: 15rem;
    margin: 0 auto;
    line-height: 1.5;
    font-family: Arial, Helvetica;
    background: #F2F2F2;
}
// 顶部搜索框
@baseFont: 50;
.search-content {
    display: flex;
    position: fixed;
    top: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 15rem;
    height: (88rem / @baseFont);
    background-color: #FFC001;
    .classify {
        width: (44rem / @baseFont);
        height: (70rem / @baseFont);
        // background-color: pink;
        margin: (11rem / @baseFont) (25rem / @baseFont) (7rem / @baseFont) (24rem / @baseFont);
        background: url(../image/classify.png) no-repeat;
        background-size: 100% auto;
    }
    .search {
        flex: 1;
        input {
            outline: none;
            border: 0;
            width: 100%;
            height: (66rem / @baseFont);
            border-radius: (33rem / @baseFont);
            background-color: #FFF2CC;
            margin-top: (12rem / @baseFont);
            font-size: (25rem / @baseFont);
            text-indent: 1em;
        }
    }
    .login {
        width: (75rem / @baseFont);
        height: (70rem / @baseFont);
        // background-color: purple;
        margin: (10rem / @baseFont);
        font-size: (25rem / @baseFont);
        text-align: center;
        color: #fff;
        line-height: (70rem / @baseFont);
        text-decoration: none;
    }
}
// banner部分
.banner {
    width: (750rem / @baseFont);
    height: (368rem / @baseFont);
    img {
        width: 100%;
        height: 100%;
    }
}
// 广告部分
.ad {
    display: flex;
    a {
        flex: 1;
        img {
            width: 100%;
            height: 100%;
        }
    }
}
// nav模块
nav {
    width: (750rem / @baseFont);
    a {
        text-decoration: none;
        float: left;
        width: (150rem / @baseFont);
        height: (140rem / @baseFont);
        text-align: center;
        line-height: .8;
        img {
            display: block;
            width: (82rem / @baseFont);
            height: (82rem / @baseFont);
            margin: (10rem / @baseFont) auto 0;
        }
        span {
            font-size: (25rem / @baseFont);
            color: #333;
        }
    }
}
```

对应index.css

```css
html {
  font-size: 50px;
}
@media screen and (min-width: 320px) {
  html {
    font-size: 21.33333333px;
  }
}
@media screen and (min-width: 360px) {
  html {
    font-size: 24px;
  }
}
@media screen and (min-width: 375px) {
  html {
    font-size: 25px;
  }
}
@media screen and (min-width: 384px) {
  html {
    font-size: 25.6px;
  }
}
@media screen and (min-width: 400px) {
  html {
    font-size: 26.66666667px;
  }
}
@media screen and (min-width: 414px) {
  html {
    font-size: 27.6px;
  }
}
@media screen and (min-width: 424px) {
  html {
    font-size: 28.26666667px;
  }
}
@media screen and (min-width: 480px) {
  html {
    font-size: 32px;
  }
}
@media screen and (min-width: 540px) {
  html {
    font-size: 36px;
  }
}
@media screen and (min-width: 720px) {
  html {
    font-size: 48px;
  }
}
@media screen and (min-width: 750px) {
  html {
    font-size: 50px;
  }
}
body {
  min-width: 320px;
  width: 15rem;
  margin: 0 auto;
  line-height: 1.5;
  font-family: Arial, Helvetica;
  background: #F2F2F2;
}
.search-content {
  display: flex;
  position: fixed;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 15rem;
  height: 1.76rem;
  background-color: #FFC001;
}
.search-content .classify {
  width: 0.88rem;
  height: 1.4rem;
  margin: 0.22rem 0.5rem 0.14rem 0.48rem;
  background: url(../image/classify.png) no-repeat;
  background-size: 100% auto;
}
.search-content .search {
  flex: 1;
}
.search-content .search input {
  outline: none;
  border: 0;
  width: 100%;
  height: 1.32rem;
  border-radius: 0.66rem;
  background-color: #FFF2CC;
  margin-top: 0.24rem;
  font-size: 0.5rem;
  text-indent: 1em;
}
.search-content .login {
  width: 1.5rem;
  height: 1.4rem;
  margin: 0.2rem;
  font-size: 0.5rem;
  text-align: center;
  color: #fff;
  line-height: 1.4rem;
  text-decoration: none;
}
.banner {
  width: 15rem;
  height: 7.36rem;
}
.banner img {
  width: 100%;
  height: 100%;
}
.ad {
  display: flex;
}
.ad a {
  flex: 1;
}
.ad a img {
  width: 100%;
  height: 100%;
}
nav {
  width: 15rem;
}
nav a {
  text-decoration: none;
  float: left;
  width: 3rem;
  height: 2.8rem;
  text-align: center;
  line-height: 0.8;
}
nav a img {
  display: block;
  width: 1.64rem;
  height: 1.64rem;
  margin: 0.2rem auto 0;
}
nav a span {
  font-size: 0.5rem;
  color: #333;
}
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261745722.gif)

### 简洁高效的rem适配方案flexible.js

#### 技术方案1

-  less
-  媒体查询
-  rem

#### 技术方案2 (推荐)

-  flexible.js
-  rem

手机淘宝团队出的简洁高效移动端适配库

我们再也不需要再写不同屏幕的媒体查询，因为里面js做了处理

它的原理是把当前设备划分为10等份，但是不同设备下，比例还是一致的。

我们要做的，就是确定好我们当前设备的html字体大小就可以了。

比如当前设计稿是750px，那么我们只需要把html字体大小设置为75px(750px / 10)就可以

里面页面元素rem值：页面元素的px值 / 75

剩余的，让flexible.js来计算

github地址：https://github.com/amfe/lib-flexible

修改等份需要下载好flexible.js现在版本应该名为index.js了打开在里面修改如下的值：

```js
// set 1rem = viewWidth / 10
  function setRemUnit () {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
  }
```

修改10这个参数就好了。

#### 使用适配方案2制作苏宁移动端首页

##### 1 技术选型

方案：采取单独制作移动页面方案

技术：布局采取rem适配布局2(flexible.js + rem)

设计图：本设计图采用750px设计尺寸

##### 2 文件结构

![image-20230726183903544](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261839864.png)

##### VSCode px转换rem 插件==cssrem== 

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261859115.gif)

px与rem单位之间使用alt+z快速切换

设置html字体大小基准值

1.  打开 设置 快捷键是ctrl + 逗号

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261930044.gif)

或者

![image-20230726193953193](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307261939598.png)

代码：

html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
    <link rel="stylesheet" href="./css/normalize.css">
    <link rel="stylesheet" href="./css/index.css">
</head>
<body>
    <div class="search-content">
        <a href="#" class="classify"></a>
        <div class="search">
            <form action="" method="get">
                <input type="search" id="" name="" placeholder="请输入..."/>
            </form>
        </div>
        <a href="#" class="login">登录</a>
    </div>
    <script src="./js/index.js"></script>
</body>
</html>
```

css

```css
body {
    min-width: 320px;
    max-width: 750px;
    width: 10rem;
    margin: 0 auto;
    line-height: 1.5;
    font-family: Arial, Helvetica;
    background-color: #F2F2F2;
}
@media screen and (min-width: 750px) {
    html {
        /*flexible.js 中有一个权重比较高的页面字体大小我们要设置为自己想要的需要加权重!important*/
        font-size: 75px !important;
    }
}
.search-content {
    display: flex;
    position: fixed;
    top: 0;
    width: 10rem;
    height: 1.1733rem;
    background-color: #FFC001;
}
.classify {
    width: .5867rem;
    height: .9333rem;
    background-color: pink;
    margin: .1467rem .3333rem .1333rem;
    background: url(../image/classify.png) no-repeat;
    background-size: .5867rem  auto;
}
.search {
    flex: 1;
    /* background-color: purple; */
}
.search input {
    outline: none;
    font-size: .3333rem;
    border: 0;
    width: 100%;
    height: .88rem;
    background-color: #FFF2CC;
    margin-top: .1333rem;
    border-radius: .44rem;
    text-indent: 1em;
}
.login {
    width: 1rem;
    height: .9333rem;
    margin: .1333rem;
    text-align: center;
    line-height: .9333rem;
    text-decoration: none;
    color: #fff;
    font-size: .3333rem;
}
```

normalize.css

```css
/*! normalize.css v8.0.1 | MIT License | github.com/necolas/normalize.css */

/* Document
   ========================================================================== */

/**
 * 1. Correct the line height in all browsers.
 * 2. Prevent adjustments of font size after orientation changes in iOS.
 */

html {
  line-height: 1.15; /* 1 */
  -webkit-text-size-adjust: 100%; /* 2 */
}

* {
  margin: 0;
  -webkit-top-higlight-color: transparent;
}

/* Sections
   ========================================================================== */

/**
 * Remove the margin in all browsers.
 */

body {
  margin: 0;
}

/**
 * Render the `main` element consistently in IE.
 */

main {
  display: block;
}

/**
 * Correct the font size and margin on `h1` elements within `section` and
 * `article` contexts in Chrome, Firefox, and Safari.
 */

h1 {
  font-size: 2em;
  margin: 0.67em 0;
}

/* Grouping content
   ========================================================================== */

/**
 * 1. Add the correct box sizing in Firefox.
 * 2. Show the overflow in Edge and IE.
 */

hr {
  box-sizing: content-box; /* 1 */
  height: 0; /* 1 */
  overflow: visible; /* 2 */
}

/**
 * 1. Correct the inheritance and scaling of font size in all browsers.
 * 2. Correct the odd `em` font sizing in all browsers.
 */

pre {
  font-family: monospace, monospace; /* 1 */
  font-size: 1em; /* 2 */
}

/* Text-level semantics
   ========================================================================== */

/**
 * Remove the gray background on active links in IE 10.
 */

a {
  background-color: transparent;
}

/**
 * 1. Remove the bottom border in Chrome 57-
 * 2. Add the correct text decoration in Chrome, Edge, IE, Opera, and Safari.
 */

abbr[title] {
  border-bottom: none; /* 1 */
  text-decoration: underline; /* 2 */
  text-decoration: underline dotted; /* 2 */
}

/**
 * Add the correct font weight in Chrome, Edge, and Safari.
 */

b,
strong {
  font-weight: bolder;
}

/**
 * 1. Correct the inheritance and scaling of font size in all browsers.
 * 2. Correct the odd `em` font sizing in all browsers.
 */

code,
kbd,
samp {
  font-family: monospace, monospace; /* 1 */
  font-size: 1em; /* 2 */
}

/**
 * Add the correct font size in all browsers.
 */

small {
  font-size: 80%;
}

/**
 * Prevent `sub` and `sup` elements from affecting the line height in
 * all browsers.
 */

sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
  vertical-align: baseline;
}

sub {
  bottom: -0.25em;
}

sup {
  top: -0.5em;
}

/* Embedded content
   ========================================================================== */

/**
 * Remove the border on images inside links in IE 10.
 */

img {
  border-style: none;
}

/* Forms
   ========================================================================== */

/**
 * 1. Change the font styles in all browsers.
 * 2. Remove the margin in Firefox and Safari.
 */

button,
input,
optgroup,
select,
textarea {
  font-family: inherit; /* 1 */
  font-size: 100%; /* 1 */
  line-height: 1.15; /* 1 */
  margin: 0; /* 2 */
}

/**
 * Show the overflow in IE.
 * 1. Show the overflow in Edge.
 */

button,
input { /* 1 */
  overflow: visible;
}

/**
 * Remove the inheritance of text transform in Edge, Firefox, and IE.
 * 1. Remove the inheritance of text transform in Firefox.
 */

button,
select { /* 1 */
  text-transform: none;
}

/**
 * Correct the inability to style clickable types in iOS and Safari.
 */

button,
[type="button"],
[type="reset"],
[type="submit"] {
  -webkit-appearance: button;
}

/**
 * Remove the inner border and padding in Firefox.
 */

button::-moz-focus-inner,
[type="button"]::-moz-focus-inner,
[type="reset"]::-moz-focus-inner,
[type="submit"]::-moz-focus-inner {
  border-style: none;
  padding: 0;
}

/**
 * Restore the focus styles unset by the previous rule.
 */

button:-moz-focusring,
[type="button"]:-moz-focusring,
[type="reset"]:-moz-focusring,
[type="submit"]:-moz-focusring {
  outline: 1px dotted ButtonText;
}

/**
 * Correct the padding in Firefox.
 */

fieldset {
  padding: 0.35em 0.75em 0.625em;
}

/**
 * 1. Correct the text wrapping in Edge and IE.
 * 2. Correct the color inheritance from `fieldset` elements in IE.
 * 3. Remove the padding so developers are not caught out when they zero out
 *    `fieldset` elements in all browsers.
 */

legend {
  box-sizing: border-box; /* 1 */
  color: inherit; /* 2 */
  display: table; /* 1 */
  max-width: 100%; /* 1 */
  padding: 0; /* 3 */
  white-space: normal; /* 1 */
}

/**
 * Add the correct vertical alignment in Chrome, Firefox, and Opera.
 */

progress {
  vertical-align: baseline;
}

/**
 * Remove the default vertical scrollbar in IE 10+.
 */

textarea {
  overflow: auto;
}

/**
 * 1. Add the correct box sizing in IE 10.
 * 2. Remove the padding in IE 10.
 */

[type="checkbox"],
[type="radio"] {
  box-sizing: border-box; /* 1 */
  padding: 0; /* 2 */
}

/**
 * Correct the cursor style of increment and decrement buttons in Chrome.
 */

[type="number"]::-webkit-inner-spin-button,
[type="number"]::-webkit-outer-spin-button {
  height: auto;
}

/**
 * 1. Correct the odd appearance in Chrome and Safari.
 * 2. Correct the outline style in Safari.
 */

[type="search"] {
  -webkit-appearance: textfield; /* 1 */
  outline-offset: -2px; /* 2 */
}

/**
 * Remove the inner padding in Chrome and Safari on macOS.
 */

[type="search"]::-webkit-search-decoration {
  -webkit-appearance: none;
}

/**
 * 1. Correct the inability to style clickable types in iOS and Safari.
 * 2. Change font properties to `inherit` in Safari.
 */

::-webkit-file-upload-button {
  -webkit-appearance: button; /* 1 */
  font: inherit; /* 2 */
}

/* Interactive
   ========================================================================== */

/*
 * Add the correct display in Edge, IE 10+, and Firefox.
 */

details {
  display: block;
}

/*
 * Add the correct display in all browsers.
 */

summary {
  display: list-item;
}

/* Misc
   ========================================================================== */

/**
 * Add the correct display in IE 10+.
 */

template {
  display: none;
}

/**
 * Add the correct display in IE 10.
 */

[hidden] {
  display: none;
}
```

index.js也就是大名鼎鼎的 flexible.js

```js
(function flexible (window, document) {
  var docEl = document.documentElement
  var dpr = window.devicePixelRatio || 1

  // adjust body font size
  function setBodyFontSize () {
    if (document.body) {
      document.body.style.fontSize = (12 * dpr) + 'px'
    }
    else {
      document.addEventListener('DOMContentLoaded', setBodyFontSize)
    }
  }
  setBodyFontSize();

  // set 1rem = viewWidth / 10
  function setRemUnit () {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
  }

  setRemUnit()

  // reset rem unit on page resize
  window.addEventListener('resize', setRemUnit)
  window.addEventListener('pageshow', function (e) {
    if (e.persisted) {
      setRemUnit()
    }
  })

  // detect 0.5px supports
  if (dpr >= 2) {
    var fakeBody = document.createElement('body')
    var testElement = document.createElement('div')
    testElement.style.border = '.5px solid transparent'
    fakeBody.appendChild(testElement)
    docEl.appendChild(fakeBody)
    if (testElement.offsetHeight === 1) {
      docEl.classList.add('hairlines')
    }
    docEl.removeChild(fakeBody)
  }
}(window, document))
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307262118398.gif)

非常的<strong style="color:red">丝滑</strong>。

## 移动WEB开发响应式布局

### 1 响应式开发原理

就是使用==媒体查询==针对不同宽度的设备进行布局和样式的设置，从而适配不同设备的目的。

| 设备划分               | 尺寸区间            |
| ---------------------- | ------------------- |
| 超小屏幕(手机)         | < 768px             |
| 小屏设备(平板)         | >= 768px ~ < 992px  |
| 中等屏幕(桌面显示器)   | >= 992px ~ < 1200px |
| 宽屏设备(大桌面显示器) | >= 1200px           |

### 1.2 响应式布局容器

响应式需要一个父级作为布局容器，来配合子级元素来实现变化效果。

原理就是在不同屏幕下，通过媒体查询来改变这个布局容器的大小，再改变里面子元素的排列方式和大小，从而实现不同屏幕下，看到不同的页面布局和样式变化。

**平时我们的响应式尺寸划分** 

-  超小屏幕(手机，小于768px)：设置宽度为 100%
-  小屏幕(平板，大于等于768px)：设置宽度为 750px
-  中等屏幕(桌面显示器，大于等于992px)：设置宽度为 970px
-  大屏幕(大桌面显示器，大于等于1200px)：设置宽度为 1170px

布局容器比页面尺寸稍微小一点点就是为了居中对齐 两侧留空白看起来舒服些。

### 响应式导航——案例

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307262349513.gif)

#### 需求分析

1.  当我们屏幕大于等于768px，我们给布局容器container宽度为750px。
2.  container里面包含了8个小li盒子，每个盒子的宽度定为93.75px(750 / 8)，高度为30px，浮动一行显示。
3.  当我们屏幕缩放，宽度小于768px的时候，container盒子宽度修改为100%宽度。
4.  此时里面的8个小li，宽度修改为33.33%，这样一行就只能显示3个小li，剩余下行显示。

代码：

```html
    <style>
        * {
            margin: 0;
            padding: 0;
            -webkit-tap-higlight-color: transparent;
        }
        .container {
            width: 750px;
            margin: 0 auto;
        }
        .container ul li {
            float: left;
            width: 93.75px;
            height: 30px;
            background-color: green;
            list-style: none;
        }
        @media screen and (max-width: 767px) {
            .container {
                width: 100%;
            }
            .container ul li {
                width: 33.33%;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <ul>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
            <li>导航栏</li>
        </ul>
    </div>
</body>
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307270012729.gif)
