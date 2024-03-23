---
title: 移动端WEB-Bootstrap
categories: 
    - [计算机学科,web,css,移动端]
tags:
    - web
    - 计算机学科
---

## Bootstrap

### Bootstrap简介

Bootstrap来自Twitter(推特)，是目前最受欢迎的前端框架。Bootstrap是基于HTML，CSS和Javascript的， 它简介灵活，==使得WEB开发更加快捷==。

官网：http://bootstrap.css88.com/

**框架**：顾名思义就是一套架构，它有一套比较完整的网页功能解决方案，而且控制权在框架本身，有预制样式库，组件和插件。使用者要按照框架所规定的某种规范进行开发。

![image-20230727001749284](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271143556.png)

### Bootstrap使用

Bootstrap使用四部曲：1 创建文件夹结构   2 创建html骨架结构  3 引入相关样式文件   4 书写内容

#### 1 创建文件夹结构

![image-20230727114621731](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271146262.png)

bootstrap

文件夹内的文件都是在 Bootstrap离线文档中找到

其中html5shiv和respond是 为了兼容性导入的

![image-20230727124552732](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271245317.png)

css

![image-20230727124608940](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271246663.png)

image 看项目需求了

#### 2 创建html骨架结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
</head>
<body>
</body>
</html>
```

#### 3 引入相关样式文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Document</title>
    <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
    <link rel="stylesheet" href="./css/index.css">
    <script src="./bootstrap/js/bootstrap.min.js"></script>
    <script src="./bootstrap/js/jquery-3.2.1.min.js"></script>
    <script src="./bootstrap/html5shiv.min.js"></script>
    <script src="./bootstrap/respond.min.js"></script>
</head>
<body>
</body>
</html>
```

#### 4 书写内容

-  直接拿Bootstrap预先定义好的样式来使用
-  修改Bootstrap原来的样式，注意==权重问题==.
-  学号Bootstrap的关键在于知道<font color=red>它定义了哪些样式，以及这些样式能实现什么样的效果</font>。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
    <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
    <link rel="stylesheet" href="./css/index.css">
    <script src="./bootstrap/js/bootstrap.min.js"></script>
    <script src="./bootstrap/js/jquery-3.2.1.min.js"></script>
    <script src="./bootstrap/html5shiv.min.js"></script>
    <script src="./bootstrap/respond.min.js"></script>
</head>	
<body>
    <div class="glyphicon glyphicon-cloud"></div>
    <button type="button" class="btn btn-danger">（危险）Danger</button>
</body>
</html>
```

效果：

![image-20230727123024586](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271230286.png)

### Bootstrap布局容器

Bootstrap需要为页面==内容==和==栅格系统==包裹一个==.container容器==，Bootstrap<font color=red>预</font>先<font color=red>定义</font>好了这个<font color=red>类</font>叫container它提供了==两个作此用处的类==。

#### 1 container类

-  响应式布局的容器 固定宽度
-  大屏(>= 1200px) 宽度定为 1170px
-  中屏(>= 992px) 宽度定为 970px
-  小屏(>= 768px) 宽度定为 750px
-  超小屏(100%)

**代码**：

```html
<body>
     <!--响应式布局容器-->
    <div class="container">
        <div class="glyphicon glyphicon-cloud">123</div>
    </div>
</body>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271240278.gif)

#### 2 container-fluid类

-  流式布局容器 百分百宽度
-  占据全部视口(viewport)的容器
-  适合于单独做移动端开发

**代码**：

```html
<body>
    <!--响应式布局容器-->
    <div class="container">
        <div class="glyphicon glyphicon-cloud">123</div>
    </div>
    <!--流式布局(百分比)容器-->
    <div class="container-fluid">
        <div class="glyphicon glyphicon-cloud">123</div>
    </div>
</body>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271245710.gif)

### Bootstrap栅格系统

<font color=red>栅格系统</font>英文为 "grid systems" ，也有人翻译为 "网格系统" ，它是指将布局划分为等宽的列，然后通过列数的定义来模块化页面布局。

![image-20230727125012343](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271250640.png)

Bootstrap提供了一套响应式，移动设备优先的流式栅格系统，随着屏幕或视口(viewport)尺寸的增加，系统会自动分为最多<font color=red>12列</font>。

Bootstrap里面container==宽度==是==固定==的，但是==不同屏幕下==，container的==宽度不同==，我们再把container划分为==12等份==。

**在Bootstrap官网的CSS中的概述中可以看到Bootstrap初始化采取了normalize.css省去我们导入这个文件的时间了**。

![image-20230727130110172](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271301103.png)

#### 栅格选项参数

栅格系统用于通过一系列的==行==(row)与==列==(column)的==组合来创建页面布局==，你的==内容就可以放入这些创建好的布局中==。

|                     | ==超小==屏幕(手机)<br>< 768px | ==小==屏设备(平板)<br>>= 768px | ==中等==屏幕(桌面显示器)<br>>= 992px | ==宽==屏设备(大桌面显示器)<br>>= 1200px |
| ------------------- | ----------------------------- | ------------------------------ | ------------------------------------ | --------------------------------------- |
| .container 最大宽度 | 自动(100%)                    | 750px                          | 970px                                | 1170px                                  |
| 类前缀              | .col-xs-列数                  | .col-sm-列数                   | .col-md-列数                         | .col-lg-列数                            |
| 列(column)数        | 12                            | 12                             | 12                                   | 12                                      |

-  <font color=red>按照不同屏幕划分为 1 ~ 12 等份</font>.

-  ==行==(row) <font color=red>必须放到container布局容器里面</font>.

   -  ==行==(row) 可以==除去父容器作用15px的边距==.

      ![image-20230727140223736](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271402119.png)

-  我们实现列的平均划分，需要给列添加<font color=red>类前缀</font>.

-  全称：xs-extra small：超小；sm-small：小；md-medium：中等；lg-large：大；

-  列(column)==大于12==，多余的 "列(column)" 所在的元素将被作为一个整体==另起一行排列==.

-  ==每一列==默认有左右==15(px)像素==的==padding==.

   ![image-20230727135830074](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271358468.png)

-  可以同时为==一列==指定==多个设备的类名==，以便==划分不同份数==，例如 class="col-md-4 col-sm-6"

##### 划分等份演示

代码：

```html
    <style>
        div[class ^= 'col-lg'] {
            text-align: center;
            border: 2px pink solid;
        }
    </style>
</head>
<body>
    <!--响应式布局容器-->
    <div class="container">
        <h1>第一列</h1>
        <p>演示效果</p>
        <div class="row">
            <!--一行最多12等份4个每个划分3分-->
            <div class="col-lg-3">1</div>
            <div class="col-lg-3">2</div>
            <div class="col-lg-3">3</div>
            <div class="col-lg-3">4</div>
        </div>
        <!--如果孩子的份数相加等于12则孩子能沾满整个container的宽度-->
        <h1>第二列</h1>
        <p>测试：如果孩子的份数相加等于12则孩子能沾满整个container的宽度</p>
        <div class="row">
            <!--一行最多12等份4个第一个6分其它2分相加为12分-->
            <div class="col-lg-6">1</div>
            <div class="col-lg-2">2</div>
            <div class="col-lg-2">3</div>
            <div class="col-lg-2">4</div>
        </div>
        <!--如果孩子的份数相加 小于12则会 占不满整个container的宽度会有空白-->
        <h1>第三列</h1>
        <p>测试：如果孩子的份数相加 小于12则会 占不满整个container的宽度会有空白</p>
        <div class="row">
            <!--一行最多12等份 每个2分 一共4个相加 占了8份 剩余4分为空白部分-->
            <div class="col-lg-2">1</div>
            <div class="col-lg-2">2</div>
            <div class="col-lg-2">3</div>
            <div class="col-lg-2">4</div>
        </div>
        <!--如果孩子的份数相加 大于12等份 则会另起一行显示-->
        <h1>第四列</h1>
        <p>测试：如果孩子的份数相加 大于12等份 则会另起一行显示</p>
        <div class="row">
            <!--一行最多12等份 两个2分一个3分 其中一个6分 占了13份 多余的另起一行显示-->
            <div class="col-lg-6">1</div>
            <div class="col-lg-2">2</div>
            <div class="col-lg-2">3</div>
            <div class="col-lg-3">4</div>
        </div>
    </div>
</body>
```

效果：

![image-20230727133821560](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271338863.png)

##### 设备类名演示

代码：

```html
    <style>
        div[class ^= 'col-'] {
            text-align: center;
            border: 2px pink solid;
        }
    </style>
</head>
<body>
    <!--响应式布局容器-->
    <div class="container">
        <h1>第一列</h1>
        <p>宽屏 >= 1200px</p>
        <div class="row">
            <div class="col-lg-3">1</div>
            <div class="col-lg-3">2</div>
            <div class="col-lg-3">3</div>
            <div class="col-lg-3">4</div>
        </div>
        <h1>第二列</h1>
        <p>中等 >= 992px</p>
        <div class="row">
            <div class="col-lg-3 col-md-4">1</div>
            <div class="col-lg-3 col-md-4">2</div>
            <div class="col-lg-3 col-md-4">3</div>
            <div class="col-lg-3 col-md-4">4</div>
        </div>
        <h1>第三列</h1>
        <p>小屏 >= 768px</p>
        <div class="row">
            <div class="col-lg-3 col-md-4 col-sm-6">1</div>
            <div class="col-lg-3 col-md-4 col-sm-6">2</div>
            <div class="col-lg-3 col-md-4 col-sm-6">3</div>
            <div class="col-lg-3 col-md-4 col-sm-6">4</div>
        </div>
        <h1>第四列</h1>
        <p>超小 < 768px</p>
        <div class="row">
            <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">1</div>
            <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">2</div>
            <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">3</div>
            <div class="col-lg-3 col-md-4 col-sm-6 col-xs-12">4</div>
        </div>
    </div>
</body>
```

### 列嵌套

栅格系统内置的栅格系统将内容再次==嵌套==。简单理解就是一个==列内再分成若干份小列==。我们可以通过==添加==一个新的 ==.row 元素==和一系列 ==.col-sm-* 元素到已经存在的 .col-sm-* 元素内==。

![image-20230727140658819](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271407492.png)

<font color=red>**注意**</font>：使用列嵌套 ==里面最好加一个row==行 来==消除==父盒子默认==15px的padding值== 而且==高度自动和父级一样高==。

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271437092.gif)

**代码**：

html

```html
<body>
   <div class="container">
        <div class="row">
            <div class="col-md-4">
                <!--使用row消除父盒子默认15px的padding值-->
                <!--使用列嵌套 最好加一个行 来消除父盒子的padding值-->
                <div class="row">
                    <div class="col-md-6">a</div>
                    <div class="col-md-6">b</div>
                </div>
            </div>
            <div class="col-md-4">2</div>
            <div class="col-md-4">3</div>
        </div>
   </div> 
</body>
```

less

```less
body {
    min-width: 320px;
    width: 10rem;
}
@media screen and (min-width: 750px) {
    html {
        font-size: 75px;
    }
}
.row > div {
    height: .5333rem;
    background-color: pink;
}
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271441360.gif)

### 列偏移

使用 .col-md-offset-number(必须为整数) 类可以将==列向右侧偏移==。这些类实际是通过使用  *  选择器为==当前元素增加了左侧的边距==(margin)。

![image-20230727144324568](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271443927.png)

![image-20230727145950689](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271459465.png)

代码：

```html
<body>
   <div class="container">
        <div class="row">
            <div class="col-md-4">左侧</div>
            <div class="col-md-4 col-md-offset-4">右侧</div>
        </div>
        <div class="row">
            <div class="col-md-8 col-md-offset-2">中间盒子</div>
        </div>
   </div> 
</body>
```

less

```less
body {
    min-width: 320px;
    width: 10rem;
}
@media screen and (min-width: 750px) {
    html {
        font-size: 75px;
    }
}
.row div {
    height: .5333rem;
    background-color: pink;
}
```

效果：

![image-20230727150253375](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271502291.png)

### 列排序

通过使用 .col-md-push-* (==推==) 和 .col-md-pull-* (==拉==) 类就可以很容易的改变列(column)的顺序。

![image-20230727150451918](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271504740.png)

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271507399.gif)

代码：

```html
<body>
   <div class="container">
    <h1>原版</h1>
    <div class="row">
        <div class="col-md-4">左侧</div>
        <div class="col-md-2">中间</div>
        <div class="col-md-6">右侧</div>
    </div>
    <br>
    <h1>交换位置后</h1>
    <div class="row">
        <div class="col-md-4 col-md-push-8">左侧</div>
        <div class="col-md-2 col-md-push-2">中间</div>
        <div class="col-md-6 col-md-pull-6">右侧</div>
    </div>
   </div> 
</body>
```

less

```less
.row div {
    text-align: center;
    line-height: .5333rem;
    &:nth-child(1) {
        height: .5333rem;
        background-color: pink;
    }
    &:nth-child(2) {
        height: .5333rem;
        background-color: #fff;
    }
    &:nth-child(3) {
        height: .5333rem;
        background-color: purple;
    }
}
```

效果：

![image-20230727165404621](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271654478.png)

### 响应式工具

为了加快对移动设备友好的页面开发工作，利用媒体查询功能，并使用这些工具类可以方便针对不同设备展示或隐藏页面内容。

| 类名       | 超小屏 | 小屏 | 中屏 | 大屏 |
| ---------- | ------ | ---- | ---- | ---- |
| .hidden-xs | 隐藏   | 可见 | 可见 | 可见 |
| .hidden-sm | 可见   | 隐藏 | 可见 | 可见 |
| .hidden-md | 可见   | 可见 | 隐藏 | 可见 |
| .hidden-lg | 可见   | 可见 | 可见 | 隐藏 |

与之相反的，是 visible-xs，visible-sm，visible-md，visible-lg 是显示某个页面内容的。

代码：

```html
<body>
   <div class="container">
    <div class="row">
        <div class="col-xs-3">
            <span class="visible-lg">哦哦哦,的新之助</span>
            <p>class= "visible-lg"</p>
        </div>
        <div class="col-xs-3">2</div>
        <div class="col-xs-3 hidden-md hidden-xs">
            我会变魔术哦
            <p>class= "hidden-md hidden-xs"</p>
        </div>
        <div class="col-xs-3">4</div>
    </div>
   </div> 
</body>
```

less

```less
.row div {
    height: .5333rem;
    background-color: purple;
    &:nth-child(3) {
        background-color: pink;
        overflow: hidden;
    }
    &:nth-child(-n+3) {
        border-right: 3px #fff solid;
    }
    span {
        font-size: .25rem;
        color: #fff;
        line-height: .5333rem;
    }
    p {
        color: purple;
    }
}
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271812699.gif)

### 清除浮动

Bootstrap要清除浮动只需要在该盒子上添加class="clearfix"即可

### 阿里百秀案例移动端首页

#### 技术选型

**方案**：采取响应式页面开发方案

**技术**：Bootstrap框架

**设计图**：本设计图采用1280px设计尺寸

![image-20230727184308529](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307271843967.png)

#### 屏幕划分分析

1.  屏幕缩放发现，中屏幕和大屏幕布局是一致的。因此我们列定义为 col-md-就可以了，md是大于等于970px以上的
2.  屏幕缩放发现，小屏幕布局发生变化，因此我们需要为小屏幕根据需求改变布局
3.  屏幕缩放发现，超小屏幕布局又发生变化，因此我们需要为超小屏幕根据需求改变布局
4.  测了：我们先布局md以上的PC端布局，最后根据实际需求再修改小屏幕和超小屏幕的特殊布局样式

#### container宽度修改

因为本效果图采取1280宽度，而Bootstrap里面container宽度 最大为1170px，因此我们需要手动修改xia

代码：

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Document</title>
    <link rel="stylesheet" href="./bootstrap/css/bootstrap.min.css">
    <link rel="stylesheet" href="./css/index.css">
    <script src="./bootstrap/js/bootstrap.min.js"></script>
    <script src="./bootstrap/html5shiv.min.js"></script>
    <script src="./bootstrap/respond.min.js"></script>
    <script src="./js/jquery.min.js"></script>
    <script src="./js/index.min.js"></script>
</head>
<body>
    <div class="container">
        <div class="row">
            <header class="col-md-2">
                <div class="logo">
                    <a href="#">
                        <!--1.我们如果进入了超小屏幕下 logo里面的图片就是隐藏起来-->
                        <img src="./image/logo.png" class="hidden-xs" alt=""/>
                        <!--2.我们事先准备好一个盒子，在logo里面，它平时是隐藏起来的，只有在超小屏幕下显示-->
                        <span class="visible-xs">阿里百秀</span>
                    </a>
                </div>
                <div class="nav">
                    <ul>
                        <li><a href="#" class="glyphicon glyphicon-camera">生活馆</a></li>
                        <li><a href="#" class="glyphicon glyphicon-picture">自然汇</a></li>
                        <li><a href="#" class="glyphicon glyphicon-phone">科技潮</a></li>
                        <li><a href="#" class="glyphicon glyphicon-gift">奇趣事</a></li>
                        <li><a href="#" class="glyphicon glyphicon-glass">美食节</a></li>
                    </ul>
                </div>
            </header>
            <article class="col-md-7">
                <!-- 新闻模块 -->
                <!--Bootstrap清除浮动直接在类中类clearfix-->
                <div class="news clearfix">
                    <ul>
                        <li>
                            <a href="#">
                                <img src="./upload/lg.png" alt=""/>
                                <p>阿里百秀</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <img src="./upload/1.jpg" alt=""/>
                                <p>奇了!成都一小区护卫长得像马云,市民纷纷求合影</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <img src="./upload/2.jpg" alt=""/>
                                <p>奇了!成都一小区护卫长得像马云,市民纷纷求合影</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <img src="./upload/3.jpg" alt=""/>
                                <p>奇了!成都一小区护卫长得像马云,市民纷纷求合影</p>
                            </a>
                        </li>
                        <li>
                            <a href="#">
                                <img src="./upload/4.jpg" alt=""/>
                                <p>奇了!成都一小区护卫长得像马云,市民纷纷求合影</p>
                            </a>
                        </li>
                    </ul>
                </div>
                <!--发表模块-->
                <div class="pulish">
                    <div class="row">
                        <div class="col-sm-9">
                            <h3>生活馆 关于指甲的10个健康知识 你知道几个？</h3>
                            <p class="text-muted hidden-xs">alibaixiu 发布于 2015-11-23</p>
                            <p class="hidden-xs">指甲是经常容易被人们忽略的身体部位， 事实上从指甲的健康
                                状况可以看出一个人的身体健康状况， 
                                快来看看10个暗藏在指甲里知识吧！</p>
                            <p class="text-muted">阅读(2417)评论(1)赞 (18)<span class="hidden-xs">标签：健康 / 感染 / 指甲
                                 / 疾病 / 皮肤 / 营养 / 趣味生活</span></p>
                        </div>
                        <div class="col-sm-3 pic hidden-xs">
                            <img src="./upload/1.jpg" alt=""/>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-sm-9">
                            <h3>生活馆 关于指甲的10个健康知识 你知道几个？</h3>
                            <p class="text-muted hidden-xs">alibaixiu 发布于 2015-11-23</p>
                            <p class="hidden-xs">指甲是经常容易被人们忽略的身体部位， 事实上从指甲的健康
                                状况可以看出一个人的身体健康状况， 
                                快来看看10个暗藏在指甲里知识吧！</p>
                            <p class="text-muted">阅读(2417)评论(1)赞 (18)<span class="hidden-xs">标签：健康 / 感染 / 指甲
                                 / 疾病 / 皮肤 / 营养 / 趣味生活</span></p>
                        </div>
                        <div class="col-sm-3 pic hidden-xs">
                            <img src="./upload/1.jpg" alt=""/>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-sm-9">
                            <h3>生活馆 关于指甲的10个健康知识 你知道几个？</h3>
                            <p class="text-muted hidden-xs">alibaixiu 发布于 2015-11-23</p>
                            <p class="hidden-xs">指甲是经常容易被人们忽略的身体部位， 事实上从指甲的健康
                                状况可以看出一个人的身体健康状况， 
                                快来看看10个暗藏在指甲里知识吧！</p>
                            <p class="text-muted">阅读(2417)评论(1)赞 (18)<span class="hidden-xs">标签：健康 / 感染 / 指甲
                                 / 疾病 / 皮肤 / 营养 / 趣味生活</span></p>
                        </div>
                        <div class="col-sm-3 pic hidden-xs">
                            <img src="./upload/1.jpg" alt=""/>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-sm-9">
                            <h3>生活馆 关于指甲的10个健康知识 你知道几个？</h3>
                            <p class="text-muted hidden-xs">alibaixiu 发布于 2015-11-23</p>
                            <p class="hidden-xs">指甲是经常容易被人们忽略的身体部位， 事实上从指甲的健康
                                状况可以看出一个人的身体健康状况， 
                                快来看看10个暗藏在指甲里知识吧！</p>
                            <p class="text-muted">阅读(2417)评论(1)赞 (18)<span class="hidden-xs">标签：健康 / 感染 / 指甲
                                 / 疾病 / 皮肤 / 营养 / 趣味生活</span></p>
                        </div>
                        <div class="col-sm-3 pic hidden-xs">
                            <img src="./upload/1.jpg" alt=""/>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-sm-9">
                            <h3>生活馆 关于指甲的10个健康知识 你知道几个？</h3>
                            <p class="text-muted hidden-xs">alibaixiu 发布于 2015-11-23</p>
                            <p class="hidden-xs">指甲是经常容易被人们忽略的身体部位， 事实上从指甲的健康
                                状况可以看出一个人的身体健康状况， 
                                快来看看10个暗藏在指甲里知识吧！</p>
                            <p class="text-muted">阅读(2417)评论(1)赞 (18)<span class="hidden-xs">标签：健康 / 感染 / 指甲
                                 / 疾病 / 皮肤 / 营养 / 趣味生活</span></p>
                        </div>
                        <div class="col-sm-3 pic hidden-xs">
                            <img src="./upload/1.jpg" alt=""/>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-sm-9">
                            <h3>生活馆 关于指甲的10个健康知识 你知道几个？</h3>
                            <p class="text-muted hidden-xs">alibaixiu 发布于 2015-11-23</p>
                            <p class="hidden-xs">指甲是经常容易被人们忽略的身体部位， 事实上从指甲的健康
                                状况可以看出一个人的身体健康状况， 
                                快来看看10个暗藏在指甲里知识吧！</p>
                            <p class="text-muted">阅读(2417)评论(1)赞 (18)<span class="hidden-xs">标签：健康 / 感染 / 指甲
                                 / 疾病 / 皮肤 / 营养 / 趣味生活</span></p>
                        </div>
                        <div class="col-sm-3 pic hidden-xs">
                            <img src="./upload/1.jpg" alt=""/>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-sm-9">
                            <h3>生活馆 关于指甲的10个健康知识 你知道几个？</h3>
                            <p class="text-muted hidden-xs">alibaixiu 发布于 2015-11-23</p>
                            <p class="hidden-xs">指甲是经常容易被人们忽略的身体部位， 事实上从指甲的健康
                                状况可以看出一个人的身体健康状况， 
                                快来看看10个暗藏在指甲里知识吧！</p>
                            <p class="text-muted">阅读(2417)评论(1)赞 (18)<span class="hidden-xs">标签：健康 / 感染 / 指甲
                                 / 疾病 / 皮肤 / 营养 / 趣味生活</span></p>
                        </div>
                        <div class="col-sm-3 pic hidden-xs">
                            <img src="./upload/1.jpg" alt=""/>
                        </div>
                    </div>
                </div>
            </article>
            <aside class="col-md-3">
                <a href="#" class="banner">
                    <img src="./upload/zgboke.jpg" alt=""/>
                </a>
                <a href="#" class="hot">
                    <span class="btn btn-primary">热搜</span>
                    <h4 class="text-primary">欢迎加入中国博客联盟</h4>
                    <p>这里收录国内各个领域的优秀博客，是一个全人工编辑的开放式博客联盟交流和展示平台......</p>
                </a>
            </aside>
        </div>
    </div>
</body>
</html>
```

index.less

```less
// 修改container的最大宽度为1280根据设计稿来走的
@media screen and (min-width: 1280px) {
    .container {
        width: 1280px;
    }
}

// 当我们进入 小屏幕 还有 超小屏幕的时候 我们nav里面的li浮动起来并且宽度为 20% 文字并居中
@media screen and (max-width: 991px) {
    .nav {
        margin-bottom: 10px;
        li {
            float: left;
            width: 20%;
            a {
                text-align: center;
            }
        }
    }
}

// 进入超小屏幕后文字变小并居中
@media screen and (max-width: 767px) {
    .nav {
        li {
            a {
                font-size: 6px !important;
                text-align: center;
            }
        }
    }
    // 当我们处于超小屏幕 第一个li宽度为100% 剩下的小li 各50%
    article {
        .news {
            li {
                width: 50% !important;
                &:nth-child(1) {
                    width: 100% !important;
                }
            }
        }
        .pulish .row div h3 {
            font-size: 18px;
        }
    }
}
body {
    background-color: #f5f5f5;
}
.container {
    background-color: #fff;
}
// header
header {
    padding-left: 0 !important;
    // logo模块
    .logo {
        background-color: #3c94d7;
        // <!--1.我们如果进入了超小屏幕下 logo里面的图片就是隐藏起来-->
        // <!--2.我们事先准备好一个盒子，在logo里面，它平时是隐藏起来的，只有在超小屏幕下显示-->
        img {
            // logo图片不需要缩放
            display: block;
            max-width: 100%;
            margin: 0 auto;
        }
        span {
            display: block;
            height: 50px;
            line-height: 50px;
            color: #fff;
            font-size: 20px;
            text-align: center;
        }
    }
    //nav模块
    .nav {
        background-color: #eee;
        border-bottom: 1px #ccc solid;
        ul {
            list-style: none;
            margin: 0;
            padding: 0;
            li a {
                display: block;
                height: 51.198px;
                text-decoration: none;
                color: #666;
                line-height: 48.3042px;
                text-indent: 2em;
                font-size: 15.5748px;
                &:hover {
                    background-color: #fff;
                    color: #333;
                }
                &:before {
                    vertical-align: middle;
                    padding-right: 5.0032px;
                }
            }
        }
    }
}
article {
    .news {
        ul {
            list-style: none;
            margin: 0;
            padding: 0;
            li {
                float: left;
                width: 25%;
                height: 128px;
                padding-right: 10px;
                margin-bottom: 10px;
                &:nth-child(1) {
                    width: 50%;
                    height: 266px;
                    a {
                        p {
                            line-height: 41px;
                            font-size: 20px;
                            padding: 0 10px;
                        }
                    }
                }
                a {
                    display: block;
                    width: 100%;
                    height: 100%;
                    position: relative;
                    img {
                        width: 100%;
                        height: 100%;
                        vertical-align: middle;
                        border: 0;
                    }
                    p {
                            position: absolute;
                            bottom: -10px;
                            left: 0px;
                            background: rgba(0,0,0,.5);
                            font-size: 10px;
                            width: 100%;
                            color: #fff;
                            height: 41px;
                            padding: 5px 10px;
                            overflow: hidden;
                    }
                }
            }
        }
    } 
    .pulish {
        border-top: 1px red solid;
        .pic {
            margin-top: 10px;
            img {
                width: 100%;
            }
        }
        .row {
            border-bottom: 1px #ccc solid;
            padding: 10px 0;
        }
    }
}
aside {
    .banner {
        img {
            width: 100%;
        }
    }
    .hot {
        display: block;
        margin-top: 20px;
        padding: 0 20px 20px;
        border: 1px #ccc solid;
        text-decoration: none;
        span {
            border-radius: 0;
            margin-bottom: 20px;
        }
        p {
            color: #333;
            font-size: 12px;
            &:hover {
                color: #23527c;
            }
        }
    }
}
```

效果：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307280017724.gif)

查看项目：[点击查看](./案例/阿里百秀-响应式布局案例)
