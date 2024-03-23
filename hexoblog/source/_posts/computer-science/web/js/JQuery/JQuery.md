---
title: jQuery概述
categories:
    - [计算机学科,web,js,JQuery]
tags:
    - 计算机学科
    - jquery
---

## jQuery概述

### 1.1 JavaScript库

仓库：可以把很多东西放到这个仓库里面。找东西只需要到仓库里面查找到就可以了。

**JavaScript库**：即library，是一个==封装==好的特定的==集合== (方法和函数) 。从封装一大推函数的角度理解库，就是在这个库中，封装很多预先定义好的函数在里面，比如动画 animation，hide，show，比如获取元素等。

>  简单理解：
>
>  就是JS文件，里面对我们原生JS代码进行了封装，存放到里面。这样我们可以快速高效的使用这些封装好的功能了。
>
>  比如jQuery，就是为了快速方便的操作DOM，里面基本都是函数 (方法)

#### 常见的JavaScript库

-  jQuery
-  Prototype
-  YUI
-  Dojo
-  Ext JS
-  移动端的zepto

这些库都是对原生JavaScript的封装，内部都是使用==JavaScript实现的==，我们主要学习的是==jQuery==。

### 1.2 jQuery的概念

==jQuery==是一个快速，简洁的==JavaScript库==，其设计的宗旨是 “write Less，Do More” ，即倡导写更少的代码，做更多的事情。

j 就是 JavaScript；Query 查询；意思就是查询 js ，把 JS 中的DOM操作做了封装，我们可以快速的查询使用里面的功能。

==JQuery 封装了 JavaScript 常用的功能代码==，优化了DOM操作，事件处理，动画设计和 Ajax交互。

<span alt=solid>学习JQuery本质：就是学习调用这些函数 (方法)</span>。

==JQuery 出现的目的是加快前端人员的开发速度，我们可以非常方便的调用和使用它==，从而提高开发效率。

>  ==jQuery==是一个快速，简洁的==JavaScript库==，其设计的宗旨是 “write Less，Do More” ，即倡导写更少的代码，做更多的事情。

![image-20230811103556095](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111035140.png)

### 1.3 jQuery的优点

**优点**：

-  轻量级，核心文件才几十kb，不会影响页面加载速度
-  跨浏览器兼容，基本兼容了现在主流的浏览器
-  链式编程，隐式迭代
-  对事件，样式，动画支持，大大简化了DOM操作
-  支持插件扩展开发。有着丰富的第三方的插件，例如：
   -  树形菜单，日期控件，轮播图等
-  免费，开源

---

## 2 jQuery的基本使用

**官网地址**：https://jquery.com/

**版本**：

-  1x：兼容 IE 678 等低版本浏览器，官网不再更新
-  2x：不兼容 IE 678 等低版本浏览器，官网不再更新
-  3x：不兼容 IE 678 等低版本浏览器，是官方主要更新维护的版本

各个版本的下载：https://code.jquery.com/

---

### 2.1 jQuery使用步骤

###### 1 将jQuery文件放入到项目的js文件中

![image-20230811104933522](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111049910.png)

###### 2 引入jQuery文件到页面文件中

-  写在head标签内，注意JS文件要在CSS文件下面引入 因为页面加载顺序是 CSS -> HTML -> JS
-  明明JS在最后面为什么写在head里面而不是`</body>`里面呢，因为它内部做了 延迟加载所以没有关系

```js
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
   <script src="./js/jquery-3.3.1.min.js"></script> 
</head>
```



###### 3 编写JQuery代码隐藏div盒子

```js
<body>
    <div></div>
    <script src="./js/index.js"></script>
</body>
```



**效果**：

![image-20230811105957953](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111059250.png)

但是将 JS代码 和 HTML代码的顺序 置换一下

```js
<body>
    <script src="./js/index.js"></script>
    <div></div>
</body>
```



**效果**：

![image-20230811110054609](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111100991.png)

在JS中可以使用load和DOMContentLoaded实现页面加载完后执行 JS 代码，而在jQuery中也有入口函数

### 2.2 jQuery入口函数 （页面结构加载)

有两种写法 效果相同：页面DOM结构加载完成执行jQuery代码

```js
$(function() {
   ... // 此处是页面DOM结构加载完成的入口
});
```



```js
$((document).ready(function() {
   ... // 此处是页面DOM结构加载完成的入口
})
```



>  1.  等着DOM 结构渲染完毕即可执行内部代码，不必等到所有外部资源加载完毕，jQuery帮我们完成了封装。
>  2.  相当于原生 js 中的 DOMContentLoaded
>  3.  不同于原生js中的load事件是等页面文档，外部的js文件，css文件，图片/音频/视频 全部加载完毕才执行内部JS代码

```js
// 等着页面DOM结构加载完毕再去执行JQuery代码
$(document).ready(function() {
    $('div').hide()
})
// 等着页面DOM结构加载完毕再去执行JQuery代码
$(function() {
    $('div').hide()
})
```



**效果**：

![image-20230811112634574](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111126737.png)

### 2.3 jQuery 的顶级对象 `$` 

1 `$`是jQuery的别称，在代码中可以使用jQuery代替`$` ，但一般为了方便，通常都直接使用 `$`.

2 `$`是jQuery的顶级对象，相当于原生JavaScript中的window。把元素利用`$`包装成jQuery对象，就可以调用jQuery的方法。

```js
// 它们两个实现的功能是一样的
// 1.$是jQuery的别称 (另外的名字)
// 2.$同时也是jQuery的 顶级对象
$(function() {
    alert('111')
})
jQuery(function() {
    alert('222')
})
```



### 2.4 jQuery对象和DOM对象

1.  用原生JS获取来的对象就是==DOM==对象
2.  jQuery方法获取的元素就是==jQuery==对象
3.  jQuery对象本质是：利用`$`对DOM对象包装后产生的对象 (<span alt=wavy>伪数组</span>形式存储)

```html
<body>
    <div>123</div>
</body>
```



```js
// 1.DOM对象：用原生js获取过来的对象就是DOM对象
const div = document.querySelector('div')
console.log(div)//div元素对象
// 2.jQuery对象：用jQuery方式获取过来的对象就是jQuery对象。本质：通过$把DOM元素进行了包装
$('div') //$('div')是一个jQuery对象 里面包装了div元素对象
console.log($('div'))
// 3.jQuery对象只能使用jQuery方法， DOM对象则只能使用原生的JavaScript属性和方法
// div.style.color = 'red' 正确写法
// div是一个dom对象不能使用jQuery里面的hide方法
// div.hide() 此写法错误：div.hide is not a function
// $('div')是一个jQuery对象不能使用原生JavaScript的属性和方法
// $('div').style.color = 'green' 此写法错误:$(...).style is undefined
```



**效果**：

![image-20230811115619301](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111156000.png)

### 2.5 jQuery对象和DOM对象—转换

DOM对象于jQuery对象之间是可以<span alt=solid>相互转换</span>的。

>  因为原生js比jQuery更大，原生的一些属性和方法jQuery没有给我们封装，要想使用这些属性和方法需要把jQuery对象转换为DOM对象才能使用。

##### 1 DOM对象转换为jQuery对象：`$(DOM对象)` 

```js
const div = document.querySelector('div')
$(div) // 将DOM对象放入到$()中，不需要带引号
```



##### 2 jQuery对象转换为DOM对象 (两种方式)

```js
$('div')[index] // index是索引号
```



```js
$('div').get(index) // index是索引号
```



**代码**：

```js
<body>
    <video src="mov.mp4" muted></video>
    <script>
        // 1.DOM对象转换为jQuery对象
        // (1)我们直接获取视频，得到的就是jQuery对象
        $('video')
        // (2)我们已经使用原生js 获取过来DOM对象
        const myVideo = document.querySelector('video')
        // $(myVideo).play() jQuery里面没有 play() 方法
        // 2.jQuery对象转换为DOM对象 (两种方式)
        $(myVideo)[0].play()
        $(myVideo).get(0).play()
    </script>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111336163.gif)

## jQuery常用API

### 1 jQuery选择器

#### 1.1 jQuery基础选择器

原生 JS 获取元素方式很多，很杂，而且兼容性情况不一致，因此jQuery给我们做了封装，使获取元素统一标准。

```js
$('选择器') // 里面选择器直接写 CSS 选择器即可，但是要加引号
```



| 名称       | 用法              | 描述                     |
| ---------- | ----------------- | ------------------------ |
| ID选择器   | `$('#id')`        | 获取指定ID的元素         |
| 全选择器   | `$('*')`          | 匹配所有元素             |
| 类选择器   | `$('.class')`     | 获取同一类class的元素    |
| 标签选择器 | `$('div')`        | 获取同一类标签的所有元素 |
| 并集选择器 | `$('div,p,li')`   | 获取多个元素             |
| 交集选择器 | `$('li.current')` | 交集元素                 |

---

#### 1.2 jQuery层级选择器

| 名称       | 用法           | 描述                                                         |
| ---------- | -------------- | ------------------------------------------------------------ |
| 子代选择器 | `$('ul > li')` | 使用>号，获取亲儿子层级的元素；注意，并不会获取孙子层级的元素 |
| 后代选择器 | `$('ul li')`   | 使用空格隔开，代表后代选择器，获取ul下的所有li元素，包括孙子元素等。 |

---

#### 1.3 隐式迭代

###### 知识铺垫

jQuery设置样式

```js
$('div').css('属性','值')
```



==遍历内部DOM元素== (==伪数组==形式存储) 的过程叫做==隐式迭代==。

**简单理解**：给匹配到的所有元素进行==循环遍历==，执行==相应的方法==，而==不用我们再进行循环==，==简化==我们的==操作==，方便我们调用

**代码**：

```js
<body>
    <div>惊喜不，意外不</div>
    <div>惊喜不，意外不</div>
    <div>惊喜不，意外不</div>
    <div>惊喜不，意外不</div>
    <ul>
        <li>相同的操作</li>
        <li>相同的操作</li>
        <li>相同的操作</li>
    </ul>
    <script>
        // 1.获取四个div元素
        console.log($('div')) //Object { 0: div, 1: div, 2: div, 3: div, length: 4, prevObject: {…} }
        // 2.给四个div设置背景颜色为粉色，jQuery对象不能使用style
        // jQuery设置css样式属性名称可以以驼峰命名法也可以正常写css格式
        $('div').css('background-color', 'pink')
        // $('div').css('backgroundColor', 'pink')
        // 3.隐式迭代就是把匹配的所有元素内部进行遍历循环，给每一个元素添加css这个方法
        // [后代选择器] 选择ul中所有的li 添加css属性为 color: red
        $('ul li').css('color', 'red')
    </script>
</body>
```



**效果**：

![image-20230811140841698](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111408129.png)

---

#### 1.4 jQuery筛选选择器

| 语法       | 用法            | 描述                                                        |
| ---------- | --------------- | ----------------------------------------------------------- |
| :first     | `$('li:first')` | 获取第一个li元素                                            |
| :last      | `$('li:last')`  | 获取最后一个li元素                                          |
| :eq(index) | `$('li:eq(2)')` | 获取到的li元素中，选择索引号为2的元素，索引号index从0开始。 |
| :odd       | `$('li:odd')`   | 获取到的li元素中，选择索引号为奇数的元素                    |
| :even      | `$('li:even')`  | 获取到的li元素中，选择索引号为偶数的元素                    |

**代码**：

```js
<body>
    <ul>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
    </ul>
    <ol>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
    </ol>
    <script>
        // 获取ul中第一个li 并设置css属性
        $('ul li:first').css('color', 'red')
        // 使用eq获取ul中第三个li 索引号从0开始计算 并设置css属性
        $('ul li:eq(2)').css('color', 'blue')
        // 获取ol中为奇数的li 并设置css属性
        $('ol li:odd').css('color', 'pink')
        // 获取ol中为偶数的li 并设置css属性
        $('ol li:even').css('color', 'yellow')
        // 获取ol中最后一个li 并设置css属性
        $('ol li:last').css('color', 'green')
    </script>
</body>
```



---

#### 1.5 jQuery筛选方法 (重点)

| 语法               | 用法                             | 说明                                                 |
| ------------------ | -------------------------------- | ---------------------------------------------------- |
| parent()           | `$('li').parent()`               | 获取父级元素，最近的父级也就是亲爸爸                 |
| children(selector) | `$('ul').children('li')`         | 相当于`$('ul > li')`，最近一级 (亲儿子)              |
| find(selector)     | `$('ul').find('li')`             | 相当于`$('ul li')`，后代选择器                       |
| siblings(selector) | `$('.first').siblings('.elem')`  | 查找兄弟节点，不包括自己本身                         |
| nextAll([expr])    | `$('.first').nextAll()`          | 查找当前元素之后所有的同辈元素                       |
| prevtAll([expr])   | `$(.last).prevAll()`             | 查找当前元素之前所有的同辈元素                       |
| hasClass(class)    | `$('div').hasClass('protected')` | 检查当前元素是否含有某个特定的类，如果有，则返回true |
| eq(index)          | `$('li').eq(2)`                  | 相当于`$('li:eq(2)')`，index从0开始                  |

**重点记住**：parent()，children()，find()，siblings()，eq()

**代码**：

```js
<body>
    <!--siblings(selector)-->
    <ol>
        <p>siblings(selector)</p>
        <li>我是ol的li</li>
        <li>我是ol的li</li>
        <li class="item">我是ol的li</li>
        <li>我是ol的li</li>
        <li>我是ol的li</li>
    </ol>

    <!--nextAll([expr])-->
    <ol>
        <p>nextAll([expr])</p>
        <li>我是ol的li</li>
        <li>我是ol的li</li>
        <li class="next">我是ol的li</li>
        <li>我是ol的li</li>
        <li>我是ol的li</li>
    </ol>

    <!--prevtAll([expr])-->
    <ol>
        <p>prevtAll([expr])</p>
        <li>我是ol的li</li>
        <li>我是ol的li</li>
        <li class="pre">我是ol的li</li>
        <li>我是ol的li</li>
        <li>我是ol的li</li>
    </ol>

    <!--eq(index)-->
    <ul>
        <p>eq(index)</p>
        <li>我是ul的li</li>
        <li>我是ul的li</li>
        <li>我是ul的li</li>
        <li>我是ul的li</li>
        <li>我是ul的li</li>
    </ul>

    <div class="yeye">
        <div class="father">
            <div class="son">儿子</div>
        </div>
    </div>

    <div class="nav">
        <p>我是屁</p>
        <p class="p1">我是屁1</p>
        <div>
            <p>我是p</p>
            <p class="p1">我是p1</p>
        </div>
    </div>
    <div class="current">我有current</div>
    <div>我没有current</div>
    <script>
        // 1. 父 parent()返回最近一级的父元素 也就是亲爸爸
        console.log($('.son').parent())
        // 2. 子 children('p') 相当于子代选择器 div > p
        $('.nav').children('p').css('color', 'red')
        // 2.1 子/孙 find() 获取素有的子孙后代 相当于 div p 后代选择器
        $('.nav').find('.p1').css('color', 'red')
        // 3. 兄 siblings('elem') 获取除了自身以外所有的亲兄弟
        $('ol .item').siblings('li').css('color', 'red')
        // 4. 前同辈
        $('ol .next').nextAll('li').css('color', 'red')
        // 4.1 后同辈
        $('ol .pre').prevAll('li').css('color', 'red')
        // 5. 利用方法方式选择第n个 设置css属性 eq <-两个eq更推荐使用
        $('ul li').eq(2).css('color', 'red')
        // 5.1 利用选择器方式
        $('ul li:eq(3)').css('color', 'red')
        // 6.判断某个元素是否包含某个特定的类
        // 倒数第二个div
        console.log($('div:eq(5)').hasClass('current'))
        // 倒数第一个div
        console.log($('div:eq(6)').hasClass('current'))
    </script>
</body>
```



**效果**：

![image-20230811150614419](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111506743.png)

#### 案例—新浪下拉菜单

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" href="./css/新浪下拉菜单.css"/>
    <script src="./js/jquery-3.3.1.min.js"></script>
   <script src="./js/新浪下拉菜单.js"></script>
</head>
<body>
    <ul class="nav">
        <li>
            <a href="#">微博</a>
            <ul>
                <li>
                    <a href="">私信</a>
                </li>
                <li>
                    <a href="">评论</a>
                </li>
                <li>
                    <a href="">@我</a>
                </li>
            </ul>
        </li>
        <li>
            <a href="#">微博</a>
            <ul>
                <li>
                    <a href="">私信</a>
                </li>
                <li>
                    <a href="">评论</a>
                </li>
                <li>
                    <a href="">@我</a>
                </li>
            </ul>
        </li>
        <li>
            <a href="#">微博</a>
            <ul>
                <li>
                    <a href="">私信</a>
                </li>
                <li>
                    <a href="">评论</a>
                </li>
                <li>
                    <a href="">@我</a>
                </li>
            </ul>
        </li>
        <li>
            <a href="#">微博</a>
            <ul>
                <li>
                    <a href="">私信</a>
                </li>
                <li>
                    <a href="">评论</a>
                </li>
                <li>
                    <a href="">@我</a>
                </li>
            </ul>
        </li>
    </ul>
</body>
</html>
```



```less
* {
    margin: 0;
    padding: 0;
}

.nav {
    list-style: none;
    margin: 100px;

    li {
        float: left;
        width: 100px;
        height: 41px;
        text-align: center;
        position: relative;

        a {
            // 需要将 a 标签转换为block否则鼠标移动到文字就会触发 鼠标移出事件
            display: block;
            width: 100%;
            height: 100%;
            text-decoration: none;
            color: #000;
            line-height: 2.5;
        }

        &:hover {
            background-color: #ccc;
        }

        ul {
            display: none;
            list-style: none;
            position: absolute;
            top: 41px;
            border: 1px yellow solid;

            li {
                border-bottom: 1px yellow solid;
                width: 100px;
                height: 41px;

                a {
                    display: block;
                    text-decoration: none;
                    width: 100%;
                    height: 100%;
                    line-height: 2.5;

                    &:hover {
                        color: pink;
                    }
                }

                &:hover {
                    background-color: #d7bbbb;
                }
            }

        }
    }
}
```



```js
<script>
   $(function() {
   // 鼠标经过
   $(".nav li").mouseover(function() {
      // $(this) jQuery 当前元素  this不要加引号
      // show() 显示元素  hide() 隐藏元素
      $(this).children("ul").show();
   });
   // 鼠标离开
   $(".nav li").mouseout(function() {
      $(this).children("ul").hide();
   })
})
</script>
```



#### 1.6 jQuery里面的排他思想

想要多选一的效果，排他思想：<span alt=solid>当前元素设置样式，其余的兄弟元素清除样式</span>。

>  思路：
>
>  1.  获取button对象绑定点击事件
>  2.  通过this获取当前对象button并设置背景颜色
>  3.  通过this获取当前对象button使用siblings排除自己并查找其余所有button并设置背景颜色

```js
<body>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <script>
        $(function () {
            // 1.隐式迭代 给所有的按钮都绑定了点击事件
            $('button').click(function () {
                // 2.当前的元素变化背景颜色
                $(this).css('background-color', 'pink')
                // 3.其余兄弟去掉背景颜色 第三步还含有 隐式迭代 查找除了自己的所有button元素对象 设置背景颜色为空
                $(this).siblings('button').css('background-color', '')
            })
        })
    </script>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111620159.gif)



#### 案例：淘宝服饰精品案例分析

1.  核心原理：鼠标经过左侧盒子某个小li，就让内容盒子相对应图片显示，其余的图片隐藏
2.  需要得到当前小li的索引号，就可以显示对应索引号的图片
3.  jQuery得到当前元素索引号`$(this).index()` 

```css
<style type="text/css">
* {
   margin: 0;
   padding: 0;
   font-size: 12px;
}
ul {
   list-style: none;
}
a {
   text-decoration: none;
}
.wrapper {
   width: 250px;
   height: 248px;
   margin: 100px auto 0;
   border: 1px solid pink;
   border-right: 0;
   overflow: hidden;
}
#left,
#content {
   float: left;
}
#left li {
   background: url(images/lili.jpg) repeat-x;
}
#left li a {
   display: block;
   width: 48px;
   height: 27px;
   border-bottom: 1px solid pink;
   line-height: 27px;
   text-align: center;
   color: black;
}
#left li a:hover {
   background-image: url(images/abg.gif);
}
#content {
   border-left: 1px solid pink;
   border-right: 1px solid pink;
}
</style>
```



```html
<body>
    <div class="wrapper">
        <ul id="left">
            <li><a href="#">女靴</a></li>
            <li><a href="#">雪地靴</a></li>
            <li><a href="#">冬裙</a></li>
            <li><a href="#">呢大衣</a></li>
            <li><a href="#">毛衣</a></li>
            <li><a href="#">棉服</a></li>
            <li><a href="#">女裤</a></li>
            <li><a href="#">羽绒服</a></li>
            <li><a href="#">牛仔裤</a></li>
        </ul>
        <div id="content">
            <div>
                <a href="#"><img src="images/女靴.jpg" width="200" height="250" /></a>
            </div>
            <div>
                <a href="#"><img src="images/雪地靴.jpg" width="200" height="250" /></a>
            </div>
            <div>
                <a href="#"><img src="images/冬裙.jpg" width="200" height="250" /></a>
            </div>
            <div>
                <a href="#"><img src="images/呢大衣.jpg" width="200" height="250" /></a>
            </div>
            <div>
                <a href="#"><img src="images/毛衣.jpg" width="200" height="250" /></a>
            </div>
            <div>
                <a href="#"><img src="images/棉服.jpg" width="200" height="250" /></a>
            </div>
            <div>
                <a href="#"><img src="images/女裤.jpg" width="200" height="250" /></a>
            </div>
            <div>
                <a href="#"><img src="images/羽绒服.jpg" width="200" height="250" /></a>
            </div>
            <div>
                <a href="#"><img src="images/牛仔裤.jpg" width="200" height="250" /></a>
            </div>

        </div>
    </div>
</body>
```



```js
<script>
    $(function () {
        // 1.鼠标经过左侧的小li
        $('#left li').mouseenter(function () {
            let index = $(this).index()
            // 这里使用到了链式编程
            $('#content div').eq(index).show().siblings().hide()
        })
    })
</script>
```



**效果**：



#### 1.7 jQuery链式编程

链式编程是为了节省代码量，看起来更优雅。

```js
$(this).css('color','red').siblings().css('color','')
```



优化前面的<a href="#1.6 jQuery里面的排他思想">排他思想案例</a> 使用==链式编程==来完成

```js
<body>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <script>
        $(function () {
            // 1.隐式迭代 给所有的按钮都绑定了点击事件
            $('button').click(function () {
                // 2.当前的元素变化背景颜色
                $(this).css('background-color', 'pink').siblings().css('background-color', '')
            })
        })
    </script>
</body>
```



**效果**：

![202308111620159](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308120835334.gif)

>  <span alt=wavy>使用链式编程一定注意是哪个对象执行样式</span>.

```js
<body>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
    <button>快速</button>
<script>
   $(function () {
   // 1.隐式迭代 给所有的按钮都绑定了点击事件
   $('button').click(function () {
      // 2.除了我们自己以外的都添加背景颜色
      $(this).siblings().css('background-color', 'pink')
      // 除了我之外的父级元素添加背景颜色 父级是body
      $(this).siblings().parent().css('background-color', 'blue')
   })
})
</script>
</body>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111742400.gif)

### 2 jQuery样式操作

#### 2.1 操作css方法

jQuery可以使用css方法来修改简单元素样式；也可以操作类，修改多个样式

###### 1 参数只写属性名，则是返回属性值

```js
$(this).css('color')
```



###### 2 参数是==属性名==，==属性值==，==逗号分隔==，是设置一组样式，属性必须加引号，值如果是数字可以不用跟单位和引号。

```js
$(this).css('color','red')
```



###### 3 参数可以是==对象形式==，方便设置==多组样式==。属性名和属性值用==冒号隔开==，属性可以不用加引号。

```js
$(this).css({'color':'white','fonst-size':'20px'})
```



**代码**：

```js
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <div></div>
    <script>
        $(function() {
       		// 1 参数只写属性名，则是返回属性值
            const width = $('div').css('width')
            // 2 参数是属性名，属性值，逗号分隔，是设置一组样式，属性必须加引号，值如果是数字可以不用跟单位和引号。
            console.log(width)
            // $('div').css('width','300px')
            // 属性值，如果是数字的话可以不用加 引号和单位
            $('div').css('width',300)
       		//3 参数可以是对象形式，方便设置多组样式。属性名和属性值用冒号隔开，属性可以不用加引号。
            $('div').css({
                height: 100,
                // 如果是复合属性必须采用小驼峰命名法，如果数值不是数字，则需要加引号
                backgroundColor: 'red'
            })
        })
    </script>
</body>
```



**效果**：

![image-20230811180243654](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111802039.png)

#### 2.2 设置类样式方法

作用等同于以前的 ==classList==，可以==操作类样式==，注意操作类里面的参数==不要加点==。

###### 1 添加类

```js
$('div').addClass('current') // 添加class类名不需要加 前面的点直接写名称就行解析自带点的
```



###### 2 移出类

```js
$('div').removeClass('current') 
```



###### 3 切换类

```js
$('div').toggleClass('current')
```



**代码**：

```js
    <style>
        div {
            width: 150px;
            height: 150px;
            background-color: pink;
            margin: 100px auto;
            transition: all 0.5s;
        }

        .current {
            background-color: red;
            transform: rotate(360deg);
        }
    </style>
    <script src="./js/jquery-3.3.1.min.js"></script>
</head>

<body>
    <div></div>
    <div class="current"></div>
    <script>
        $(function () {
            // 1. 添加类 addClass
            $('div').click(function () {
                $(this).addClass('current')
            })
            // 2. 删除类 removeClass
            $('div:last').click(function() {
                $(this).removeClass('current')
            })
            // 3. 切换类toggleClass
            // $('div').click(function() {
            //     $(this).toggleClass('current')
            // })
        })
    </script>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111845305.gif)

##### 案例：tab栏切换

1 点击上部的li，当前li添加current类，其余兄弟移除类

2 点击的同时，得到当前li的索引号

3 让下部里面相应索引号的item显示，其余的item隐藏

**代码**：

```js
<script>
   $(function() {
   // 1.点击上部的li，当前li 添加current类，其余兄弟类移除类
   $('.tab_list li').click(function() {
      // 链式编程操作
      $(this).addClass('current').siblings().removeClass('current')
      // 2.点击同时，得到当前li的索引号
      let index = $(this).index()
      // 3.让下部里面相应索引号的item显示，其余item隐藏
      $('.tab_con .item').eq(index).show().siblings().hide()
   })
})
</script>
```



```css
<style>
* {
   margin: 0;
   padding: 0;
}

li {
   list-style-type: none;
}

.tab {
   width: 978px;
   margin: 100px auto;
}

.tab_list {
   height: 39px;
   border: 1px solid #ccc;
   background-color: #f1f1f1;
}

.tab_list li {
   float: left;
   height: 39px;
   line-height: 39px;
   padding: 0 20px;
   text-align: center;
   cursor: pointer;
}

.tab_list .current {
   background-color: #c81623;
   color: #fff;
}

.item_info {
   padding: 20px 0 0 20px;
}

.item {
   display: none;
}
</style>
```



```html
<body>
   <div class="tab">
      <div class="tab_list">
         <ul>
            <li class="current">商品介绍</li>
            <li>规格与包装</li>
            <li>售后保障</li>
            <li>商品评价（50000）</li>
            <li>手机社区</li>
         </ul>
      </div>
      <div class="tab_con">
         <div class="item" style="display: block;">
            商品介绍模块内容
         </div>
         <div class="item">
            规格与包装模块内容
         </div>
         <div class="item">
            售后保障模块内容
         </div>
         <div class="item">
            商品评价（50000）模块内容
         </div>
         <div class="item">
            手机社区模块内容
         </div>
      </div>
   </div>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111857358.gif)

#### 2.3 类操作与className区别

<span alt=solid>原生JS中className会覆盖元素原先里面的类名</span>.

jQuery里面类操作只是==对指定类进行操作，不影响原先的类名==。

```js
    <style>
        .one {
            width: 200px;
            height: 200px;
            background-color: pink;
            transition: all .3s;
        }
        
        .two {
            transform: rotate(360deg);
        }
    </style>
    <script src="./js/jquery-3.3.1.min.js"></script>
</head>
<body>
    <div class="one"></div>
    <script>
        // var one = document.querySelector(".one");
        // one.className = "two";
        $(".one").addClass("two");  //这个addClass相当于追加类名 不影响以前的类名
        // $(".one").removeClass("two");
    </script>
</body>
```



### 3 jQuery效果

jQuery给我们封装了很多动画效果，最为常见的如下：

![image-20230811190546579](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111905236.png)

#### 3.1 显示隐藏效果

##### 1 显示语法规范

```js
show([speed,[easing],[fn]])
```



##### 2 显示参数

1.  参数都可以省略，无动画直接显示。
2.  speed：三种预定速度之一的字符串 (‘slow’，‘normal’，‘or’，‘fast’) 或表示动画时长的毫秒数值 (如：1000)。
3.  easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear` 
4.  fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

---

##### 1 隐藏语法规范

```js
hide([speed,[easing],[fn]])
```



##### 2 隐藏参数

1.  参数都可以省略，无动画直接显示。
2.  speed：三种预定速度之一的字符串 (‘slow’，‘normal’，‘or’，‘fast’) 或表示动画时长的毫秒数值 (如：1000)。
3.  easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear` 
4.  fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

---

##### 1 切换语法规范

```js
toggle([speed,[easing],[fn]])
```



##### 2 切换参数

1.  参数都可以省略，无动画直接显示。
2.  speed：三种预定速度之一的字符串 (‘slow’，‘normal’，‘or’，‘fast’) 或表示动画时长的毫秒数值 (如：1000)。
3.  easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear` 
4.  fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

---

**代码**：

```js
    <style>
        div {
            width: 150px;
            height: 300px;
            background-color: pink;
        }
    </style>
    <script src="./js/jquery-3.3.1.min.js"></script>
</head>

<body>
    <button>显示</button>
    <button>隐藏</button>
    <button>切换</button>
    <div></div>
    <script>
        $(function() {
            $('button:first').click(function() {
                // 参数解释：
                // 1.执行时间 ， 2.动画效果 默认swing前慢中快后慢 linear线性(均速) , 3.回调函数执行其它操作
                $('div').show(500,'linear',function() {
                    console.log('显示了')
                })
            })
            $('button').eq(1).click(function() {
                // 参数解释：
                // 1.执行时间 ， 2.动画效果 默认swing前慢中快后慢 linear线性(均速) , 3.回调函数执行其它操作
                $('div').hide(500,'linear',function() {
                    console.log('隐藏了')
                })
            })
            $('button:last').click(function() {
                $('div').toggle(500)
            })
        })
    </script>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308111928049.gif)

---

#### 3.2 滑动效果

##### 1 下滑效果语法规范

```js
slideDown([speed,[easing],[fn]])
```



##### 2 下滑效果参数

1.  参数都可以省略。
2.  speed：三种预定速度之一的字符串(‘slow’，‘normal’，‘or’，‘fast’) 或表示动画时长的毫秒值 (如：1000)。
3.  easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear` 
4.  fn：回调函数，在动画完成时执行的函数，每个元素执行一次。

---

##### 1 上滑效果语法规范

```js
slideUp([speed,[easing],[fn]])
```



​	参数同上

---

##### 1 切换上下滑动效果语法规范

```js
slideToggle([speed,[easing],[fn]])
```



参数同上

---

**代码**：

```js
    <style>
        div {
            width: 150px;
            height: 300px;
            background-color: pink;
            display: none;
        }
    </style>
    <script src="./js/jquery-3.3.1.min.js"></script>
</head>

<body>
    <button>下拉滑动</button>
    <button>上拉滑动</button>
    <button>切换滑动</button>
    <div></div>
    <script>
        $(function () {
            $('button:first').click(function () {
                // 上滑动效果
                // 参数解释:
                // 1.执行时间， 2.效果 ， 3.回调函数
                $('div').slideDown(500, 'linear', function () {
                    console.log('上啦')
                })
            })
            $('button').eq(1).click(function () {
                // 下滑动效果
                // 参数解释:
                // 1.执行时间， 2.效果 ， 3.回调函数
                $('div').slideUp(500, 'linear', function () {
                    console.log('下啦')
                })
            })
            $('button:last').click(function() {
                // 切换效果
                // 参数解释:
                // 1.执行时间， 2.效果 ， 3.回调函数
                $('div').slideToggle(500)
            })
        })
    </script>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308112019255.gif)

#### 3.3 事件切换

**参数说明**：

over：鼠标经过

out：鼠标离开

```js
hover([over,]out)
```



**代码**：

```js
    <script>
        $(function () {
            // 事件切换hover就是鼠标经过和离开的复合写法
            // $('.nav li').hover(function() {
            //     $(this).children('ul').slideDown(500)
            // },function() {
            //     $(this).children('ul').slideUp(500)
            // })
            // 切换事件hover可以只写一个参数 为鼠标经过和离开都会调用这个函数
            $('.nav li').hover(function() {
                $(this).children('ul').slideToggle(100)
            })
        })
    </script>
</body>
```



```html
<body>
    <ul class="nav">
        <li>
            <a href="#">微博</a>
            <ul>
                <li>
                    <a href="">私信</a>
                </li>
                <li>
                    <a href="">评论</a>
                </li>
                <li>
                    <a href="">@我</a>
                </li>
            </ul>
        </li>
        <li>
            <a href="#">微博</a>
            <ul>
                <li>
                    <a href="">私信</a>
                </li>
                <li>
                    <a href="">评论</a>
                </li>
                <li>
                    <a href="">@我</a>
                </li>
            </ul>
        </li>
        <li>
            <a href="#">微博</a>
            <ul>
                <li>
                    <a href="">私信</a>
                </li>
                <li>
                    <a href="">评论</a>
                </li>
                <li>
                    <a href="">@我</a>
                </li>
            </ul>
        </li>
        <li>
            <a href="#">微博</a>
            <ul>
                <li>
                    <a href="">私信</a>
                </li>
                <li>
                    <a href="">评论</a>
                </li>
                <li>
                    <a href="">@我</a>
                </li>
            </ul>
        </li>
    </ul>
</body>
```



```css
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        li {
            list-style-type: none;
        }

        a {
            text-decoration: none;
            font-size: 14px;
        }

        .nav {
            margin: 100px;
        }

        .nav>li {
            position: relative;
            float: left;
            width: 80px;
            height: 41px;
            text-align: center;
        }

        .nav li a {
            display: block;
            width: 100%;
            height: 100%;
            line-height: 41px;
            color: #333;
        }

        .nav>li>a:hover {
            background-color: #eee;
        }

        .nav ul {
            display: none;
            position: absolute;
            top: 41px;
            left: 0;
            width: 100%;
            border-left: 1px solid #FECC5B;
            border-right: 1px solid #FECC5B;
        }

        .nav ul li {
            border-bottom: 1px solid #FECC5B;
        }

        .nav ul li a:hover {
            background-color: #FFF5DA;
        }
    </style>
```



#### 3.4 动画队列及其停止排队方法

##### 1 动画或效果队列

动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行。

<font title=red>谁做动画！给谁前面加stop</font>！

**续上集案例中—解决如下出现的问题**：

![test-1691759599856-3](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308112118597.gif)

##### 2 停止排队

```js
stop()
```



1.  stop() 方法用于停止动画或效果
2.  注意：stop() 写到动画或者效果的<span alt=solid>前面，相当于停止结束上一次的动画</span>.

```js
// 切换事件hover可以只写一个参数 为鼠标经过和离开都会调用这个函数
$('.nav li').hover(function() {
   // stop() 方法必须写到动画的前面
   $(this).children('ul').stop().slideToggle(100)
})
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308112116618.gif)

---

#### 3.5 淡入淡出效果

##### 1 淡入效果语法规范

```js
fadeIn([speed,[easing],[fn]])
```



##### 2 淡入效果参数

1.  参数都可以省略。
2.  speed：三种预定速度之一的字符串 (‘slow’，‘normal’，‘or’，‘fast’) 或表示动画时长的毫秒值 (如：1000)
3.  easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear` 

---

##### 1 淡出效果语法规范

```js
fadeOut([speed,[easing],[fn]])
```



##### 2 淡出效果参数

1.  参数都可以省略。
2.  speed：三种预定速度之一的字符串 (‘slow’，‘normal’，‘or’，‘fast’) 或表示动画时长的毫秒值 (如：1000)
3.  easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear` 

---

##### 1 淡入淡出切换效果语法规范

```js
fadeToggle([speed,[easing],[fn]])
```



##### 2 淡入淡出效果参数

1.  参数都可以省略。
2.  speed：三种预定速度之一的字符串 (‘slow’，‘normal’，‘or’，‘fast’) 或表示动画时长的毫秒值 (如：1000)
3.  easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear` 

---

##### 1 渐进方式调整到指定的不透明度

```js
fadeTo([[speed],opacity,[easing],[fn]])
```



##### 2 效果参数

1.  <span alt=solid>opacity：透明度必须写，取值 0 ~ 1 之间</span>.
2.  <span alt=solid>speed</span>：三种预定速度之一的字符串 (‘slow’，‘noram’，‘or’，‘fast’) 或表示动画时长的毫秒值 (如：1000) 。<font title=red>必须写!!!</font>.
3.  easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear` 

---

#### 3.6 自定义动画animation

**语法**：

```js
animate(params,[speed],[easing],[fn])
```



**参数**：

(1) <span alt=solid>params：想要更改的样式属性，以对象形式传递，必须写。属性名可以不用带引号，如果是复合属性则需要采取驼峰命名法borderLeft</span>。其余参数都可以省略。

(2) speed：三种 预定速度之一的字符串 (‘slow’，‘normal’，‘or’，‘fast’) 或表示动画时长的毫秒值 (如：1000)

(3) easing：(Optional) 用来指定切换效果，默认是 `swing`，可用参数 `linear`。

```js
    <style>
        div {
            position: absolute;
            width: 200px;
            height: 200px;
            background-color: pink;
            transition: all 1s;
        }
    </style>
</head>
<body>
    <button>动起来</button>
    <div></div>
    <script>
        $(function () {
            $('button').click(function () {
                $('div').animate({
                    left: 180,
                    opacity: .5,
                    width: 400,
                }, function () {
                    console.log('执行回调函数')
                    console.log(this)
                    $(this).css({
                        'transform': 'rotate(180deg)'
                    })
                })
            })
        })
    </script>
</body>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308120857613.gif)

#### 手风琴案例

代码：

```css
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
        }
        
        img {
            display: block;
        }
        
        ul {
            list-style: none;
        }
        
        .king {
            width: 852px;
            margin: 100px auto;
            background: url(images/bg.png) no-repeat;
            overflow: hidden;
            padding: 10px;
        }
        
        .king ul {
            overflow: hidden;
        }
        
        .king li {
            position: relative;
            float: left;
            width: 69px;
            height: 69px;
            margin-right: 10px;
        }
        
        .king li.current {
            width: 224px;
        }
        
        .king li.current .big {
            display: block;
        }
        
        .king li.current .small {
            display: none;
        }
        
        .big {
            width: 224px;
            display: none;
        }
        
        .small {
            position: absolute;
            top: 0;
            left: 0;
            width: 69px;
            height: 69px;
            border-radius: 5px;
        }
    </style>
```



```html
<body>
   <div class="king">
        <ul>
            <li class="current">
                <a href="#">
                    <img src="images/m1.jpg" alt="" class="small">
                    <img src="images/m.png" alt="" class="big">
                </a>
            </li>
            <li>
                <a href="#">
                    <img src="images/l1.jpg" alt="" class="small">
                    <img src="images/l.png" alt="" class="big">
                </a>
            </li>
            <li>
                <a href="#">
                    <img src="images/c1.jpg" alt="" class="small">
                    <img src="images/c.png" alt="" class="big">
                </a>
            </li>
            <li>
                <a href="#">
                    <img src="images/w1.jpg" alt="" class="small">
                    <img src="images/w.png" alt="" class="big">
                </a>
            </li>
            <li>
                <a href="#">
                    <img src="images/z1.jpg" alt="" class="small">
                    <img src="images/z.png" alt="" class="big">
                </a>
            </li>
            <li>
                <a href="#">
                    <img src="images/h1.jpg" alt="" class="small">
                    <img src="images/h.png" alt="" class="big">
                </a>
            </li>
            <li>
                <a href="#">
                    <img src="images/t1.jpg" alt="" class="small">
                    <img src="images/t.png" alt="" class="big">
                </a>
            </li>
        </ul>

    </div>
</body>
```



```js
    <script type="text/javascript">
        $(function() {
            // 1.鼠标经过某个li
            $('.king li').mouseenter(function() {
                // 2.当前小li，宽度设置为224px，同时里面的小图片淡出，大图片淡入
                // 谁做动画给谁加stop()
                $(this).stop().animate({
                    width: 224,
                    // 使用find找到li的孙子元素img class=".small"的添加淡出效果然后他的兄弟.big添加淡入效果
                }).find('.small').stop().fadeOut(10).siblings('.big').stop().fadeIn(10)
                // 找到li的其它兄弟添加动画
                $(this).siblings('li').stop().animate({
                    // 设置宽度
                    width: 69,
                    // 找到li的孙子节点元素img class=".small"添加淡入效果 找到他的兄弟.big添加淡出效果
                }).find('.small').stop().fadeIn(10).siblings('.big').stop().fadeOut(10)
                // 3.其余兄弟小li宽度设置为69px,小图片淡入，大图片淡出
            })
        })
    </script>
```



**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308120933276.gif)

### 4 jQuery属性操作

##### 4.1 设置或获取元素固有属性值 prop()

所谓元素固有属性就是元素本身自带的属性，比如<a>元素里面的href，比如<input>元素里面的type

###### 1 获取属性语法

```js
prop('属性')
```



###### 2 设置属性语法

```js
prop('属性','属性值')
```



**代码**：

```js
<a href="http://www.itcast.cn" title="都挺好">都挺好</a>
<input type="checkbox" name="" id="" checked>
   // prop获取/设置的是 预先定义好的属性 也就是固有元素属性
   // 获取属性值
   console.log($('a').prop('href'))
// 设置属性值
$('a').prop('title', '我们都挺好')
// 绑定单击事件
$('input').change(function () {
   // 获取当前ckecked的状态
   console.log($(this).prop('checked'))
})
```



**效果**：

![image-20230812095547810](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308120955161.png)

##### 4.2 设置或获取元素自定义属性值 attr()

用户自己给元素添加的属性，我们称为自定义属性。比如给div添加 index = ‘1’。

###### 1 获取属性语法

```js
attr('属性') // 类似原生 getAttribute()
```



###### 2 设置属性语法

```js
attr('属性','属性值') // 类似原生 setAttribute()
```

<span alt=solid>该方法也可以获取H5自定义属性</span>.



**代码**：

```js
<div index="1" data-index="2">我是div</div>            
// 使用prop只能获取固有元素属性不能获取自定义的属性否则是undefined
console.log($('div').prop('index'))// undefined
// 获取自定义属性的值
console.log($('div').attr('index'))// 1
// 设置自定义属性 的值
$('div').attr('index', 4) // <div index="4" data-index="2">我是div</div>
// 获取自定义属性的值 支持H5的自定义属性
console.log($('div').attr('data-index'))// 2
```



**效果**：

![image-20230812100224911](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121002357.png)

#### 4.3 数据缓存 data()

data() 方法可以在指定的元素上存取数据，并不会修改DOM元素结构。一旦页面刷新，之前存放的数据都将被移除。



##### 1 附加数据语法

```js
data('uname' , 'value')// 向被选元素附加数据
```



##### 2 获取数据语法

```js
data('uname') // 向被选元素获取数据
```

<span alt=solid>同时，还可以读取HTML5自定义属性 data-index，得到的是数字型</span>.



**代码**：

```js
<div index="1" data-index="2">我是div</div>
<span>123</span>
// 数据缓存 data() 这里面的数据是存放在元素的内存里面当一个变量来看了
// 在DOM中看不到
$('span').data('uname', 'andy')
// 通过使用data存储到内存里面的变量名uname获取值
console.log($('span').data('uname'))// andy
// 使用data数据缓存函数 获取data-index H5自定义属性 1.不用写data- 而且返回的是数字型的值
console.log($('div').data('index'))// 2
```



**效果**：

![image-20230812101233307](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121012699.png)

### 5 jQuery内容文本值

主要针对元素的==内容==还是==表单的值==操作。

#### 1 普通元素内容html() (相当于原生innerHTML)

```js
html() // 获取元素的内容
```



```js
html('内容') // 设置元素的内容
```



**代码**：

```js
<div>
   <span>我是内容</span>
</div>
<input type="text" value="请输入内容">
// 1.获取元素内容 html()
console.log($('div').html())// <span>我是内容</span>
// 1.1 设置元素内容 html() 相当于js中innerHTML = '123'
$('div').html('123')// <div>123</div>
```

**效果**：

![image-20230812103933293](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121039526.png)

![image-20230812103951419](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121039732.png)

#### 2 普通元素文本内容 text() (相当于原生innerText)

```js
text() // 获取元素的文本内容
```



```js
text('文本内容') // 设置元素的内容
```



**代码**：

```js
<div>
   <span>我是内容</span>
</div>
<input type="text" value="请输入内容">
// 2.获取元素文本内容
console.log($('div').text())// 123
// 2.1 设置元素文本内容 相当于js中innerText = '123'
$('div').text('456') // 456
```

**效果**：

![image-20230812104224709](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121042043.png)

![image-20230812104237050](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121042558.png)

#### 3 表单元素内容 val() (相当于原生value)

```js
val()// 获取表单的value值
val('内容')// 设置表单的value值
```



**代码**：

```js
<div>
   <span>我是内容</span>
</div>
<input type="text" value="请输入内容">
// 3. 获取表单值 val()
console.log($('input').val())// 请输入内容
// 3.1 设置表单值 val()
$('input').val('123')
```

**效果**：

![image-20230812104545403](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121045189.png)

### 6 jQuery元素操作

#### 获取元素的索引号

**语法**：

```js
$('ul').click(function() {
   $(this).index()// 获取当前
})
```

-  获取的是兄弟节点的索引号，如果是孙子节点只能获取到 0

**代码**：

```js
<body>
    <div>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </div>
    <ul>
        <li><a style="text-decoration:none" href="javaScript:;">1</a></li>
        <li><a style="text-decoration:none" href="javaScript:;">2</a></li>
        <li><a style="text-decoration:none" href="javaScript:;">3</a></li>
    </ul>
   <script>
    $(function() {
        $('div li').click(function() {
            console.log($(this).index())// 0 1 2
        })
        $('ul li a').click(function() {
            console.log($(this).index()) // 获取了3个0
        })
    })
   </script> 
</body>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308122124527.gif)

---

**学习内容**：

主要是 ==遍历==，==创建==，==添加==，==删除元素==等操作。

#### 6.1 遍历元素

>  jQuery隐式迭代是对同一类元素做了==同样的操作==。如果想要给同一类元素做==不同操作==，就==需要用到遍历==。

##### 语法1：

```js
$('div').each(function(inde,domEle) {xxx;})
```

1.  each() 方法遍历匹配的每一个元素。主要用DOM处理。each每一个
2.  里面的回调函数有2个参数：index是每个元素的索引号；demEle是每个==DOM元素对象==，==不是jQuery对象==.
3.  所以想要使用jQuery方法，需要给这个DOM元素转换为jQuery对象 `$(domEle)` 

##### 语法2：

```js
$.each(Object,function(index,element) {xxx;})
```

1.  `$.each()`方法可用于遍历任何对象。主要用于数据处理，比如数组，对象
2.  里面的函数有2个参数：index是每个元素的索引号；element 遍历内容

###### 遍历数组

```js
// 遍历数组
$.each(['a', 'b', 'c'], function (index, ele) {
   console.log(index)// 0 1 2
   console.log(ele)// a b c 
})
```

###### 遍历对象

```js
// 遍历对象
$.each({
   uname: '张三',
   age: 18,
   sex: '男'
}, function (index, ele) {
   console.log(index)// uname age sex
   console.log(ele)// 张三 18 男
})
```

###### 给三个div设置不同的颜色

**代码**：

```js
<div>1</div>
<div>2</div>
<div>3</div>
// 隐式迭代 会将全部的div都改变背景颜色
$('div').css('color', 'red')
// 如果针对于同一个元素做不同操作，需要用到遍历元素(类似for,但是比for强大 没错就是each)
// 1.each(function(index,domEle) {})方法遍历元素 each中第二个参数是DOM元素中的jQuery不能使用
// 定义一个颜色数组
const arr = ['red', 'green', 'blue']
// $('div').each(function (index, domEle) {
//     // 回调函数第一个参数一定是索引号
//     console.log(index)
//     // 回调函数第二个参数一定是DOM元素对象 jQuery不能使用
//     console.log(domEle)
//     // domEle.css('color')// dom对象没有css方法
//     $(domEle).css('color', arr[index])
// })
console.log('----------------------------')
// 遍历div元素 ， 回调函数 参数1；索引好 参数2；元素
$.each($('div'), function (index, ele) {
   console.log(index)// 0 1 2
   console.log(ele)// div div div
   // 给三个div设置不用的颜色
   $(this).css('color',arr[index])
})
```

**效果**：

![image-20230812112431229](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121124131.png)

### 6.2 创建元素

##### 语法：

```js
$('<li></li>')
```

动态的创建了一个 <li> 标签

**代码**：

```js
// 1.创建元素
const li = $('<li>我是后来创建的小li</li>')
```

-  光是创建了一个标签是没有用处的 也看不到 我们需要将创建好的标签添加到一个DOM节点上来展示

### 6.3 添加元素

##### 1 内部添加 最后面

```js
element.append(标签对象)
```

把内容放入匹配元素内部最<font title=red>后面</font>，类似原生appendChild

##### 1.1 内部添加 最前面

```js
element.prepend(标签对象)
```

把内容放入匹配元素内部最<font title=red>前面</font>.

**代码**：

```js
<ul>
   <li>原先的li</li>
</ul>
// 1.创建元素
const li = $('<li>我是后来创建的小li</li>')
// 2.添加元素
// 2.1内部添加
//$('ul').append(li) //内部添加并且放到内容的最后面
$('ul').prepend(li) //内部添加并且放到内容最前面
```

**效果**：

![image-20230812113754726](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121137199.png)

##### 2 外部添加

```js
element.after(标签对象) // 把内容放入目标元素后面
```



```js
element.before(标签对象) // 把内容放入目标元素前面
```

**代码**：

```js
<div class="test">我是原先的div</div>
// 2.2外部添加
$('div').before('<div>我是前面的div</div>')
// $('div').after('<div>我是后面的div</div>')
```

**效果**：

![image-20230812114212533](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121142542.png)

>  <span alt=solid>总结</span>：
>
>  1.  内部添加元素，生成之后，它们是父子关系
>  2.  外部添加元素，生成之后，它们是兄弟关系

### 6.4 删除元素

```js
element.remove() // 删除匹配的元素(本身)
```



```js
element.empty() // 删除匹配的元素集合中所有的子节点
```



```js
element.html('') // 清空匹配的元素内容
```



**代码**：

```js
<body>
    <ul>
        <li>原先的li</li>
    </ul>
    <div class="test">我是原先的div</div>
    <script>
        $(function() {
            // 1.创建元素
            const li = $('<li>我是后来创建的小li</li>')
            // 2.添加元素
            // 2.2外部添加
            $('div').before('<div>我是前面的div</div>')
            // 3.删除元素
            $('ul').remove() //将自身从DOM结构中删除
            // 下面两个基本上等价 都是干掉自己里面的所有子元素
            $('div').empty()
            $('div').html('')
        })
    </script>
</body>
```

**效果**：

![image-20230812114933879](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121149650.png)

### 7 jQuery尺寸，位置操作

#### 7.1 jQuery尺寸

| 语法                                 | 用法                                                 |
| ------------------------------------ | ---------------------------------------------------- |
| width() / height()                   | 取得匹配元素宽度和高度值 只算 width / height         |
| innerWidth() / innerHeight()         | 取得匹配元素宽度和高度值 包含padding                 |
| outerWidth() / outerHeight()         | 取得匹配元素宽度和高度值 包含padding，border         |
| outerWidth(true) / outerHeight(true) | 取得匹配元素宽度和高度值 包含padding，border，margin |

-  以上参数为空，则是获取相应值，返回的是数字型
-  如果参数为数字，则是修改相应值
-  参数可以不必写单位

**代码**：

```js
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
            padding: 10px;
            border: 15px solid red;
            margin: 20px;
        }
    </style>
    <script src="./js/jquery-3.3.1.min.js"></script>
</head>
<body>
    <div></div>
    <script>
        $(function () {
            // 1.width() / height() 获取/设置元素 width和height大小
            console.log($('div').width())
            $('div').width(300)
            // 2.innerWidth() / innerHeight() 获取/设置元素width + padding和height + padding大小
            // innerWidth() / innerHeight包含padding
            console.log($('div').innerWidth())
            // 3.outerWidth() / outerHeight() 获取/设置元素width + padding + booder和height + padding + border大小
            console.log($('div').outerWidth())
            // 4. outerWidth(true) / outerHeight(true) 获取/设置 元素width/height + paddinng + border + margin大小
            console.log($('div').outerWidth(true))
        })
    </script>
</body>
```

**效果**：

![image-20230812131641410](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121316126.png)

#### 2 jQuery位置

位置主要有三个：`offset()`，`position()`，`scrollTop()`/`scrollLeft()` 

##### 1 offset( ) 设置或获取元素偏移

1.  offset() 方法设置或返回被选元素相对于<font title=red>文档</font>的偏移坐标，跟父级没有关系。
2.  该方法有2个属性`left`,`top`。`offset().top`用于获取距离文档顶部的距离，`offset().left`用于获取距离文档左侧的距离。
3.  可以设置元素的偏移：`offset({top: 10，left: 30})` 

**代码**：

```css
<style>
* {
   margin: 0;
   padding: 0;
}
.father {
   width: 400px;
   height: 400px;
   background-color: pink;
   margin: 100px;
   overflow: hidden;
   position: relative;
}
.son {
   width: 150px;
   height: 150px;
   background-color: purple;
   position: absolute;
   left: 10px;
   top: 10px;
}
</style>
```



```js
<body>
    <div class="father">
        <div class="son"></div>
    </div>
    <script>
        // 1.获取/设置距离文档的位置(偏移) offset
        console.log($('.son').offset())
        // 2.获取距离带有定位父级位置(偏移)position如果没有带定位的父级，则以文档为准
        // 这个方法只能获取不能设置偏移
        console.log($('.son').position())
    </script>
</body>
```

**效果**：

![image-20230812134026323](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121351996.png)

#### 案例—带有动画的返回顶部

1 **核心原理**：使用animate动画返回顶部

2 animate动画函数里面有个scrollTop属性，可以设置位置

3 但是元素做动画，因此`$('body,html').animate({scrollTop:0})` 

3.1 ==需要注意==：animate做动画是针对于元素做动画，让某个元素变大变大移动是元素才有的不能整个文档来该的。所以我们要将body，html 对象修改为DOM元素才行也就是`$('body,html')`包装成DOM元素然后再执行操作 <span alt=solid>说白了就是动画操作操作的都是元素而不是文档对象</span>.

**代码**：

```css
    <style>
        body {
            height: 2000px;
        }

        .back {
            position: fixed;
            width: 50px;
            height: 50px;
            background-color: pink;
            right: 30px;
            bottom: 100px;
            display: none;
        }

        .container {
            width: 900px;
            height: 500px;
            background-color: skyblue;
            margin: 400px auto;
        }
    </style>
```



```js
<body>
    <div class="back">返回顶部</div>
    <div class="container">
    </div>
    <script>
        $(function () {
            // 被卷去的头部scrollTop() / 被卷去的左侧 scrollLeft()
            // 页面滚动事件
            let val = $('.container').offset().top
            $(window).scroll(function () {
                console.log($(document).scrollTop())
                if ($(document).scrollTop() >= val) {
                    $('.back').fadeIn(100)
                } else {
                    $('.back').fadeOut(100)
                }
            })
            // 返回顶部
            $('.back').click(function () {
                $('body,html').stop().animate({
                    scrollTop: 0,
                })
                // $(document).stop().animate({
                //     scrollTop: 0,
                // })不能是文档对象，而是body,html元素做动画
            })
        })
    </script>
</body>
```

**效果**：

![test-1691820152418-6](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121416443.gif)

## jQuery事件

### 1 jQuery事件注册

#### 单个事件注册

###### 语法：

```js
element.事件(function() {})
```

**比如**：

```js
$('div').click(function() {事件处理程序})
```

其它事件和原本基本一致

比如`mouseover`，`mouseout`，`blur`，`focus`，`change`，`keydown`，`keyup`，`resize`，`scroll`等

**代码**：

```js
<script>
   $(function () {
   // 给div盒子绑定点击事件 点击后改变背景颜色
   $('div').click(function () {
      $(this).css('background-color', 'red')
   })
   // 给div盒子绑定鼠标经过事件 点击后改变背景颜色
   $('div').mouseenter(function () {
      $(this).css('background-color', 'yellow')
   })
})
</script>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121428482.gif)

### 2 jQuery事件处理

#### 2.1 事件处理 on( ) 绑定事件

on( ) 方法在匹配元素上绑定一个或多个事件的事件处理函数

###### 语法：

```js
element.on(events,[selector],fn)
```

1.  events：一个或多个用空格分隔的事件类型，如`click`或`keydown` 
2.  selector：元素的子元素选择器
3.  fn：回调函数 即 绑定在元素身上的侦听函数

**代码**：

```js
// 2. 事件处理on
$('div').on({
   mouseenter: function () {
      $(this).css('background-color', 'red')
   },
   mouseout: function () {
      $(this).css('background-color', 'pink')
   },
   click: function() {
      $(this).css('background-color', 'skyblue')
   }
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121444297.gif)

##### on( ) 方法优势1

可以绑定多个事件，多个处理事件程序。

```js
$('div').on({
   mouseenter:function(){},
   mouseout:function(){},
   click:function(){}
})
```

同时添加多个事件使用==空格==隔开

```js
$('div').on('mouseenter mouseout click',function() {
   $(this).toggleClass('current')
})
```

##### on( ) 方法优势2

可以事件委派操作。事件委派的定义就是，把原来加给子元素身上的事件绑定在父元素身上，就是把事件委派给父元素。

<span alt=solid>给父元素绑定事件但是触发的是里面指定的子元素</span>.

###### 语法：

```js
$('ul').on('click','li',function() {
   alert('hello world!')
})
```

在此之前有`bind()`，`live()`，`delegate()`等方法来处理事件绑定或者事件委派，最新版本的请用<span alt=solid>on</span>替代他们。

**代码**：

```js
// 获取ul对象.on(绑定事件类型，选择子元素，回调函数(执行操作))
$('ul').on('click','li',function() {
   console.log(this)// 当前被点击的li
   console.log($(this).index())// 索引号
   alert('好的')
})
// click是绑定在ul身上的，但是触发的对象是ul里面的小li
```

>click是绑定在ul身上的，但是触发的对象是ul里面的小li

**效果**：

![test-1691823408117-12(1)](./JQuery.assets/test-1691823408117-12(1).gif)

##### on( ) 方法优势3

动态创建的元素，click( ) 没有办法绑定事件，on( ) 可以给动态生成的元素绑定事件

**代码**：

```html
<o></o>
```



```js
// click是绑定在ul身上的，但是触发的对象是ul里面的小li
// on可以给动态创建的元素绑定事件
// $('ol li').click(function() {
//     alert('123')
// })
$('ol').on('click','li',function() {
   alert('123')
})
let lis = $('<li>我是后来创建的</li>')
$('ol').append(lis)
```

**效果**：

![image-20230812151259696](./JQuery.assets/image-20230812151259696.png)

### 发布微博案例

分析：

1.  点击发布按钮，动态创建一个小li，放入文本框的内容和删除按钮，并且添加到ul中
2.  点击的删除按钮，可以删除当前的微博留言

**代码**：

```js
<script>
   $(function () {
   // 1.点击发布按钮，动态创建一个小li，放入文本框的内容和删除按钮，并且添加到ul中
   $('.btn').on('click', function () {
      let lis = $('<li></li>')
      lis.html($('.txt').val() + '<a href="javascript:">删除</a>')
      $('ul').prepend(lis)
      lis.slideDown()
      $('.txt').val('')
   })
   // 2.点击删除按钮，可以删除当前的微博发言
   $('ul').on('click', 'a', function () {
      // this=a它的父级元素上滑动隐藏然后在回调函数中移除条li
      $(this).parent().slideUp(function () {
         // this指向li
         $(this).remove()
      })
   })
})
</script>
```



```css
    <style>
        * {
            margin: 0;
            padding: 0
        }
        ul {
            list-style: none
        }
        .box {
            width: 600px;
            margin: 100px auto;
            border: 1px solid #000;
            padding: 20px;
        }
        textarea {
            width: 450px;
            height: 160px;
            outline: none;
            resize: none;
        }
        ul {
            width: 450px;
            padding-left: 80px;
        }
        ul li {
            line-height: 25px;
            border-bottom: 1px dashed #cccccc;
            display: none;
        }
        input {
            float: right;
        }
        ul li a {
            float: right;
        }
    </style>
```



```html
<body>
    <div class="box" id="weibo">
        <span>微博发布</span>
        <textarea name="" class="txt" cols="30" rows="10"></textarea>
        <button class="btn">发布</button>
        <ul>
        </ul>
    </div>
</body>
```

**效果**：

![test](./JQuery.assets/test-1691827444708-15.gif)

### 2.2 事件处理 off( ) 解绑事件

off( ) 方法可以移除通过 on( ) 方法添加的事件处理程序。

```js
$('p').off() // 解除p元素所有事件处理程序
$('p').off('click') // 解除p元素上面的点击事件 后面的 click 是监听函数名称
$('ul').off('click','li') // 解除事件委托
```

<span alt=solid>如果有的事件只想触发一次，可以使用one( ) 来绑定事件</span>.

```js
// one() 只触发一次,下次再点都不会触发点击事件
$('p').one('click', function () {
   alert('你好我是p')
})
```

**代码**：

```js
    <style>
        div {
            width: 100px;
            height: 100px;
            background-color: pink;
        }
    </style>
    <script src="./js/jquery-3.3.1.min.js"></script>
    <script>
        $(function () {
            $('div').on({
                mouseenter: function () {
                    console.log('我来啦')
                },
                mouseout: function () {
                    console.log('我走了')
                },
                click: function () {
                    console.log('我被点击了-------------')
                }
            })
            $('ul').on('click', 'li', function () {
                alert('ul li 123')
            })
            // 1.事件解除 off()
            // $('div').off() // 解除div中的所有事件
            $('div').off('click') // 解除div中的点击事件click
            $('ul').off('click', 'li') // 解除事件委托
        })
    </script>
</head>
<body>
    <div></div>
    <ul>
        <li>我们都是好孩子</li>
        <li>我们都是好孩子</li>
        <li>我们都是好孩子</li>
    </ul>
    <p>我是屁</p>
</body>
```

### 2.3 自动触发事件 trigger( )

有些事件希望自动触发，比如轮播图自动播放功能跟点击右侧按钮一致。可以利用定时器自动触发右侧按钮点击事件，不必鼠标点击触发。

```js
element.click() // 第一种简写形式
```



```js
element.trigger('type') // 第二种自动触发模式
```



```js
<script>
    $(function () {
        $('div').on('click', function () { // 不需要手动点击下面执行了 相应自动事件操作
            console.log('123')
        })
        //自动调用click事件点击div盒子触发上面的事件打印出123
        $('div').click()
        // 自动根据传入的事件执行 一次事件
        $('div').trigger('click')
    });
</script>
```

**效果**：

![image-20230812163137654](./JQuery.assets/image-20230812163137654.png)

有些事件希望自动触发，比如轮播图自动播放功能跟点击右侧按钮一致。可以利用定时器自动触发右侧按钮点击事件，不必鼠标点击触发。

```js
element.triggerHandler(type) // 第三种自动触发模式 并且不会触发事件的默认行为
```

-  <span alt=solid>自动触发模式，不会触发事件的默认行为</span>.

```js
$('input').on('focus',function() {
   $(this).val('你好吗')
})
$('input').triggerHandler('focus') //自动触发模式 没有默认行为
$('input').trigger('focus') // 自动触发模式 有默认行为
$('input').focus() // 自动触发模式  有默认行为
```

**三种效果演示**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121646854.gif)

### 2.4 事件对象

事件被触发，就会有事件对象的产生。

```js
element.on(events,[selector],function(event) {})
```

-  阻止默认行为：`event.preventDefault()` 或者 `return false` 
-  阻止冒泡：`event.stopPropagation()` 

## jQuery其它方法

### 1 jQuery拷贝对象

如果想要把某个对象拷贝 (合并) 给另外一个对象使用，此时可以使用 `$.extend()` 方法

###### 语法：

```js
$.extend([deep],target,object1,[objectN])
```

1.  deep：如果设定为true为深拷贝，默认为false 浅拷贝
2.  target：要拷贝的目标对象
3.  object1：待拷贝到第一个对象的对象
4.  objectN：待拷贝到第N个对象的对象
5.  浅拷贝是把被拷贝的对象<font title=red>复杂数据类型中的地址</font>拷贝给目标对象，修改目标对象<font title=red>会影响</font>被拷贝对象。
6.  深拷贝，前面加true，完全克隆 (拷贝的对象，而不是地址) ，修改目标对象<font title=red>不会影响</font>被拷贝对象。

**代码**：

```js
$(function () {
   let targetObj = {
      id: 0, // 属性名与下面对象冲突
      idi: 0, // 不冲突
      msg: { // msg 冲突了 被下面所覆盖
         uname: '小pink',
         sex: '小pink' //覆盖msg即使属性名不冲突 这里也不是小pink了而是pink
      }
   }
   let obj = {
      id: 1,
      uname: 'andy',
      msg: {
         uname: 'pink',
         age: 10
      }
   }
   // 2.深拷贝把里面的数据完全复制一份给目标对象，如果里面有不冲突属性则合并，冲突则覆盖
   $.extend(true,targetObj, obj) //会覆盖targetObj原来的数据的属性名称冲突的数据
   // 1.浅拷贝把原来对象里面的复杂数据类型地址拷贝给目标对象
   targetObj.msg.uname = '老pink'
   console.log(targetObj)
   console.log(obj)
})
```

### 2jQuery多库共存

**问题概述**：

jQuery使用`$`作为标识符，随着jQuery的流行，其它js库也会用`$`作为标识符，这样一起使用会引起冲突。

**客观需求**：

需要一个解决方案，让jQuery和其它的 js 库不存在冲突，可以同时存在，这就叫==多库共存==

**jQuery解决方案**：

1.  把里面的`$`符号 统一改为jQuery。比如 `jQuery(‘div’)` 
2.  jQuery变量规定新的名称：`let xx = jQuery.noConflict();` 

**代码**：

```js
    <script>
        $(function() {
            // 自定义获取元素对象函数名称是 $
            function $(ele) {
                // 返回获取的元素对象
                return document.querySelector(ele)
            }
            // 调用自定义的函数
            console.log($('div'))
            // $.each()// 这里的$符号是上面函数的名称它没有each方法于jQuery的$发生冲突调用后报错
            // 解决方案1：将$符号改为jQuery
            jQuery.each()
            // 解决方案2：重新规定名称
            let dkx = jQuery.noConflict()
            // 获取span元素对象
            const span = dkx('span')
            console.log(span)
        })
    </script>
</head>
<body>
    <div></div>
    <span></span>
</body>
```

**效果**：

![image-20230812190759362](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308121908592.png)

### 3 jQuery插件

jQuery功能比较有限，想要更复杂的特效效果，可以借助于 jQuery 插件完成。

注意：这些插件也是依赖于 jQuery 来完成的，所以必须要先引入 jQuery 文件，因此也称为 jQuery 插件。

jQuery插件常用的网站：

1.  jQuery插件库：http://www.jq22.com/
2.  jQuery之家：http://www.htmleaf.com/

## 综合案例—toDoList待办事项

<span alt=solid>toDoList分析</span>.

1.  刷新页面不会丢失数据，因此需要使用到本地存储localStorage
2.  **核心思路**：<span alt=wavy>不管按下回车，还是点击复选框，都是把本地存储的数据加载到页面中个，这样保证刷新关闭页面不会丢失数据</span>.
3.  存储的数据格式：`vartodolist = [{title: 'xxx',done: false}]`

```js
$(function () {
    let arr = [{
        title: '吃饭',
        done: false
    }, {
        title: '睡觉',
        done: false
    }]
    localStorage.setItem('todo', JSON.stringify(arr))
    const data = JSON.parse(localStorage.getItem('todo'))
    console.log(JSON.parse(localStorage.getItem('todo')))
})
```

<span alt=solid>toDoList按下回车把数据添加到本地存储里面</span>.

1.  切记：页面中的数据，都要从本地存储里面获取，这样刷新页面不会丢失数据，所以先要把数据保存到本地存储里面。
2.  利用事件对象.keyCode判断用户按下回车键 (13)。
3.  声明一个数组，保存数据。
4.  先要读取本地存储原来的数据 (声明函数getData( )) ，放到这个数组里面
5.  之后把最新从表单获取过来的数据，追加到数组里面。
6.  最后把数组存储给本地存储 (声明函数savaDate( ))

```js
// 1.按下回车，把完整数据存储到本地存储里面
$('#title').on('keydown', function (event) {
   // 判断用户按下的键位是否是回车键
   if (event.keyCode === 13) {
      // 先读取本地存储原来的数据
      const local = getData()
      // 给localStorage数组中追加数据
      local.push({ title: $(this).val(), done: false })
      // 调用保存函数将追加的数据保存到本地存储里面
      saveData(local)
   }
})
// 封装常用的函数
// 读取本地存储的数据
function getData() {
   return localStorage.getItem('todolist') === null ? [] : JSON.parse(localStorage.getItem('todolist'))
}
// 保存本地存储
function saveData(data) {
   localStorage.setItem('todolist', JSON.stringify(data))
}
```

<span alt=solid>toDoList本地存储数据渲染加载到页面</span>.

1.  因为后面也会经常渲染加载操作，所以声明一个函数load，方便后面调用
2.  先要读取本地存储数据。(数据不要忘记转换为对象格式)
3.  之后遍历这个数据 (`$.each()`) ，有几条数据，就生成几个小li添加到ol里面。
4.  每次渲染之前，先把原先里面 ol 的<span alt=solid>内容</span> 清空，然后渲染加载最新的数据。

```js
// 渲染加载数据
function load() {
   // 读取本地存储的数据
   const data = getData()
   // 遍历本地数据之前先清空ol里面的元素内容防止重复显示成倍的内容
   $('ol').empty()
   $.each(data, function (index, ele) {
      $('ol').prepend(`<li><input type="checkbox"><p>${ele.title}</p><a href="javascript:;"></a></li>`)
   })
}
```

<span alt=solid>toDoList删除操作</span>.

1.  点击里面的a链接，不是删除的li，而是删除本地存储对应的数据。
2.  **核心原理**：先获取本地存储数据，删除对应的数据，保存给本地存储，重新渲染列表 li
3.  我们可以给链接自定义属性记录当前的索引号
4.  根据这个索引号删除相关的数据—数组的 splice(i,1) 方法
5.  存储修改后的数据，然后存储给本地存储
6.  重新渲染加载数据列表
7.  因为 a 是动态创建的，我们使用 on 方法绑定事件

```js
// 删除toDoList数据操作
$('ol').on('click', 'a', function () {
   // 获取本地存储
   const data = getData()
   // 修改数据
   const id = $(this).prop('id')
   data.splice(id,1)
   // 保存到本地
   saveData(data)
   // 重新渲染页面
   load()
})
```

<span alt=solid>toDoList 正在进行和已经完成选项操作</span>.

1.  <span alt=solid>当我们点击了小的复选框，修改本地存储数据，再重新渲染数据列表</span>.
2.  点击之后，获取本地存储数据
3.  修改对应数据属性 done 为当前复选框的 checked状态。
4.  之后保存数据到本地存储
5.  重新渲染加载数据列表
6.  load 加载函数里面，新增一个条件，如果当前数据的 done 为 true 就是已经完成的，就把列表渲染到ul里面
7.  如果当前数据的 done 为false，则是待办事项，就把列表渲染加载到 ol 里面

```js
$('ol,ul').on('click', 'input', function () {
   // 先获取本地存储数据
   const data = getData()
   const index = $(this).siblings('a').prop('id')
   // 修改数据
   data[index].done = $(this).prop('checked')
   // 保存到本地
   saveData(data)
   // 重新渲染页面
   load()
})
```

<span alt=solid>toDoList统计正在进行个数和已经完成个数</span>.

1.  在我们load函数里面操作
2.  声明2个变量：todoCount 待办个数 doneCount 已完成个数
3.  进行遍历本地存储数据的时候，如果数据done为false，则todoCount + +，否则 doneCount + +
4.  最后修改相应的元素text()



完整代码：

```js
$(function () {
    // 每次打开页面就加载数据
    load()
    // 1.按下回车，把完整数据存储到本地存储里面
    $('#title').on('keydown', function (event) {
        // 判断用户按下的键位是否是回车键
        if (event.keyCode === 13) {
            // 先读取本地存储原来的数据
            const local = getData()
            // 给localStorage数组中追加数据
            local.push({ title: $(this).val(), done: false })
            // 调用保存函数将追加的数据保存到本地存储里面
            saveData(local)
            load()
        }
    })
    // 删除toDoList数据操作
    $('ol,ul').on('click', 'a', function () {
        // 获取本地存储
        const data = getData()
        // 修改数据
        const id = $(this).prop('id')
        data.splice(id, 1)
        // 保存到本地
        saveData(data)
        // 重新渲染页面
        load()
    })
    $('ol,ul').on('click', 'input', function () {
        // 先获取本地存储数据
        const data = getData()
        const index = $(this).siblings('a').prop('id')
        // 修改数据
        data[index].done = $(this).prop('checked')
        // 保存到本地
        saveData(data)
        // 重新渲染页面
        load()
    })
    // 封装常用的函数
    // 读取本地存储的数据
    function getData() {
        return localStorage.getItem('todolist') === null ? [] : JSON.parse(localStorage.getItem('todolist'))
    }
    // 保存本地存储
    function saveData(data) {
        localStorage.setItem('todolist', JSON.stringify(data))
    }
    // 渲染加载数据
    function load() {
        // 读取本地存储的数据
        const data = getData()
        // 遍历本地数据之前先清空ol里面的元素内容防止重复显示成倍的内容
        $('ol,ul').empty()
        let todoCount = 0; // 正在进行的个数
        let doneCount = 0; // 已经完成的个数
        $.each(data, function (index, ele) {
            if (ele.done) {
                $('ul').prepend(`<li><input type="checkbox" checked="checked"><p>${ele.title}</p><a id=${index} href="javascript:;"></a></li>`)
                doneCount++;
            } else {
                $('ol').prepend(`<li><input type="checkbox"><p>${ele.title}</p><a id=${index} href="javascript:;"></a></li>`)
                todoCount++;
            }
        })
        $('#todocount').text(todoCount)
        $('#donecount').text(doneCount)
    }
})
```



