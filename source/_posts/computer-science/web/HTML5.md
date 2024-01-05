---
title: HTML5
categories: 
    - [计算机学科,web]
tags:
    - web
    - html
    - 计算机学科
---

[TOC]

---

# 基础认知:christmas_tree: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**目标**：认知<font title="red">网页组成</font>和<font title="red">五大浏览器</font>,明确<font title="red">Web标准的构成</font>，使用<font title="red">HTML骨架</font>搭建出一个网页。

## 1.基础概念铺垫(了解):deciduous_tree: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

1.  **认识网页**.

    -  **问题**1：网页由哪些部分组成？<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170950013.gif" alt="星" style="zoom:25%;" align="right"/>
       -  文字，图片，音频，视频，超链接
    -  **问题**2：我们看到的网页背后本质是什么？
       -  前端程序员写的代码
    -  **问题**3：前端的代码是通过什么软件转换成用户眼中的页面的？
       -  通过浏览器转化(解析和渲染)成用户看到的网页

2.  **五大浏览器和渲染引擎**.

    -  浏览器：是<font title="red">网页显示 ，运行的平台</font>，是前端开发必备利器
    -  常见的五大浏览器：
       -  IE浏览器，火狐浏览器(firefox)，谷歌浏览器(Chrome)，Safari浏览器，欧朋浏览器(Opera)

    ![image-20230429181245965](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170951226.png)

    -  **渲染引擎(浏览器内核)**：浏览器中专门<font title="red">对代码进行解析渲染的部分</font>.
    -  浏览器出品的公司不同，内在的渲染引擎也是不同的

    | 浏览器       | 内核    | 备注                                    |
    | ------------ | ------- | --------------------------------------- |
    | IE           | Trident | IE，猎豹安全，360极速浏览器，百度浏览器 |
    | Firefox      | Gecko   | 火狐浏览器内核                          |
    | Safari       | Webkit  | 苹果浏览器内核                          |
    | Chrome/Opera | Blink   | Blink其实是Webkit的分支                 |

    -  :warning:**注意点**：
       -  :clipboard:渲染引擎不同，导致解析相同代码时的 <font title="red">速度</font>，<font title="green">性能</font>，<font title="blue">效果</font>也不同的

3.  **Web标准**.

    -  不同浏览器的渲染引擎不同,对于相同代码解析的效果会存在差异

       -  如果用户想看一个网页,结果用不同浏览器打开效果不同,用户体验极差!

       Web标准: 让不同的浏览器按照相同的标准显示结果,让展示的效果统一!

4.  **HTML的概念**:

    -  HTML(Hyper Text Markup Language): 中文译为: 超文本语言

       -  专门用于网页开发的语言,主要通过<font title="red">HTML标签</font>对网页中的 <font title="blue">文本</font>,<font title="green">图片</font>,<font title="yellow">音频</font>,<font style="background-color:#CC00FF">视频</font>等内容进行描述

       **案例**: 文字变粗案例

       ​	体验构建一个网页,需要在网页中显示一个加粗的文字

![image-20230430114530345](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170951131.png)

-  **Web标准中分成三个构成**::space_invader: 

| 构成 | 语言       | 说明                                                         |
| ---- | ---------- | ------------------------------------------------------------ |
| 结构 | HTML       | <font style="color:red">页面元素</font>和内容                |
| 表现 | CSS        | 网页元素的外观和位置等<font style="color:red">页面样式</font>(如:颜色,大小等) |
| 行为 | JavaScript | 网页模型的定义与<font style="color:red">页面交互</font>.     |



## 2.HTML初体验:deciduous_tree: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

1.  HTML骨架结构

    -  网页类似于一篇文章:

       -  每一页文章内容是由固定的结构的,如: 开头,正文,末尾等等 ...

       网页中也是存在固定的结构的,如: **整体**,**头部**,**标题**,**主题**.

2.  HTML骨架结构由那些标签组成?

    -  html标签: 网页的整体
    -  head标签: 网页的头部
       -  title: 网页的标题
       -  meta: 网页的编码
    -  body:标签: 网页的身体

## 3.语法规范:deciduous_tree: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

1.  HTML的注释

    ```js
    <!--HTML中的注释-->
    ```

2.  HTML标签的构成

    ![image-20230430131016092](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170951924.png)

    -  **结构说明**:

       1.  标签由<,>,/,英文单词或字母组成,并且把标签中<>包括起来的英文单词或字母称为<font title="red">标签名</font>.
       2.  常见标签由两部分组成,我们称之为: <font title="red">双标签</font> . 前部分叫<font title="red">开始标签</font>.后部分叫<font title="red">结束标签</font>,两部分之间包裹内容
       3.  少数标签由一部分组成,我们称之为: <font title="red">单标签</font> . 自成一体,无法包裹内容比如:<font title="green">`<br>`</font>,<font title="green">`<hr>`</font>.

       **总结**: 有开始有结束 使用双标签 , 不需要开始的使用但标签

3.  HTML标签的关系

-  父子关系(嵌套关系)

   ```html
   <head>
      <title></title>
   </head>
   ```

   ![image-20230430132357927](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170951200.png)

-  兄弟关系(并列关系)

   ```html
   <head></head>
   <body></body>
   ```

   ![image-20230430132415714](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170951379.png)

# HTML标签学习:christmas_tree: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

## 排版标签:8ball: 

### 标题标签:yellow_heart: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在新闻和文章的页面中,都离不开**标题** ,用来突出显示**文章主题**.

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170951165.png" alt="image-20230430133550353" style="zoom: 67%;" align="right"/>



**代码**::game_die: h系列标签

````html
<h1>1级标题</h1>
<h2>2级标题</h2>
<h3>3级标题</h3>
<h4>4级标题</h4>
<h5>5级标题</h5>
<h6>6级标题</h6>
````

**语义**: 1~6级标题,重要程度依次递减

**特点**:

1.  文字都有加粗
2.  文字都有变大,并且从h1~h6文字逐渐减小
3.  独占一行

### 段落标签:ocean: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在新闻和文章的页面中,用于分段显示

![image-20230430134733228](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170951462.png)

**代码**::game_die: 

```html
<p>我是一段文字</p>
```

**语义**: 段落

**特点**:

1.  段落之间存在间隙
2.  独占一行

### 换行标签:umbrella: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 让文字强制换行显示

**代码**::game_die: 

```html
<br>
```

**语义**: 换行

**特点**:

1.  单标签
2.  让文字强制换行

### 水平线标签:rabbit: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 分割不同主题的内容的水平线

![image-20230430135019247](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952257.png)

**代码**::game_die: 

```html
<hr>
```

**语义**: 主题的分割转换

**特点**:

1.  单标签
2.  在页面中显示一条水平线

## 文本格式化标签:gear: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

### 文本格式化标签的介绍:ghost: 

**场景**: 需要让文字**加粗** , <u>下划线</u> , <i>倾斜</i> , <s>删除线</s>等效果

**代码**::game_die: <font title="green">普通语境</font>                                                                      <font title="red">强调语境</font> 

| 标签 | 说明   | 标签   | 说明   |
| ---- | ------ | ------ | ------ |
| b    | 加粗   | strong | 加粗   |
| u    | 下划线 | ins    | 下划线 |
| i    | 倾斜   | em     | 倾斜   |
| s    | 删除线 | del    | 删除线 |

```html
<b>加粗</b><br><!--不重要性的-->
<strong>加粗</strong><br><!--重要性的-->
<u>下划线</u><br><!--不重要性的-->
<ins>下划线</ins><br><!--重要性的-->
<i>倾斜</i><br><!--不重要性的-->
<em>倾斜</em><br><!--重要性的-->
<s>删除线</s><br><!--不重要性的-->
<del>删除线</del><!--重要性的-->
```

![image-20230430140740714](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952125.png)

**语义**: 突出重要性的强调语境

### 标签语义化:yum: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

实际项目开发中选择标签的原则: 标签语义化

1.  即: 根据语义选择对应正确的标签
2.  如: 需要写标题,就使用h系列标签
3.  如: 需要写段落,就使用p标签
4.  ...

**好处**: 

1.  对人: 好理解,好记忆
2.  对机器: 有利于机器解析,对搜索引擎(SEO)有帮助

**推荐**:

-  <span alt="shake">strong</span>,<span alt="shake">ins</span>,<span alt="shake">em</span>,<span alt="shake">del</span> , 表示的强调语义更强烈 !

## 媒体标签:medal_military: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

### 图片标签:tumbler_glass: 

**场景**: 在网页中显示图片

**代码**::game_die: 

```html
<img src="" alt="">
```

**特点**:

1.  单标签
2.  img标签需要展示对应的效果,需要借助标签的属性进行设置 !

**标签的完整结构图**:

**![image-20230430143250330](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952613.png)**

:warning:**属性注意点**:

1.  标签的属性写在<font title="red">开始标签内部</font>.
2.  标签上可以同时存在多个属性
3.  属性之间以空格隔开
4.  标签名与属性之间<font title="red">必须以空格隔开</font>.
5.  属性之间没有顺序之分

#### 图片标签的src属性:leaves: 

**属性名**: src

**属性值**: 目标图片的路径

:warning:**注意点**:

1.  当前网页和目标图片在同一个文件夹中,路径直接写目标图片的名字即可(包括后缀名)
2.  路径的情况有很多,稍后会详细介绍

![image-20230430143651123](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952994.png)

**代码**: :game_die: 

```html
<img src="./img/test.gif" alt="二进制云">
```

**效果**:

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952522.gif" alt="test" style="zoom: 33%;" />

#### 图片标签的alt属性:four_leaf_clover: 

**属性名**: alt

**属性值**: 替换文本

1.  当图片加载失败时,才显示alt的文本
2.  当图片加载成功时,不会显示alt的文本

![image-20230430144536028](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952940.png)

#### 图片标签的title属性:film_projector: 

**属性名**: title

**属性值**: 提示文本

-  当鼠标悬停时,才显示的文本

**代码**::game_die: 

```html
<img src="./img/test.gif" alt="二进制云" title="我是云">
```

**效果**:

![image-20230430165020642](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952908.png)

:warning:**注意点**: title属性不仅仅可以用于图片标签,还可以用于其它标签

![image-20230430164533561](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952280.png)

#### 图片标签的width和height属性:wind_chime: 

**属性名**: width和height

**属性值**: 宽度和高度(数字)

**代码**::game_die: 

```html
<img src="./img/test.gif" alt="二进制云" title="我是云" width="150px" height="150px">
```

**效果**:

![image-20230430165106364](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170952831.png)

:warning:**注意点**: 

1.  如果只设置width或height中的一个.另一个没设置的会自动等比例缩放(此时图片不会变形)
2.  如果==同时设置了width和height两个==,若==设置不当此时图片可能会变形== 

### 路径:cake: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 页面需要加载图片,需要先找到对应的图片

**路径可分为**:

1.  <span alt="shake">**绝对路径**</span>.

    -  **绝对路径**: 指目录下的绝对位置,可直接到达目标位置,通常<font title="red">从盘符开始</font>的路径

    -  <font style="color:red">绝对路径在img中浏览器会发送一个请求,response中不会有任何信息,因为</font>==这不是正常的请求==<font style="color:red">图片加载会失败</font>.

       ![image-20230430173945787](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170953083.png)

2.  <span alt="shake">**相对路径**</span>.

    -  **概念普及**:
       -  **当前文件**: 当前的HTML网页文件 (比如text.html文件)
       -  **目标文件**: 要找到的图片
    -  **相对路径**: 从==当前文件开始==出发==找目标文件==的过程

    -  **相对路径分类**:

       -  同级目录
          -  直接找到
       
       
       ```html
       ./test.gif
       ```

       -  下级目录
          -  进入到image目录中找到
       

       ```html
       ./image/test.gif
       ```
       
       -  上级目录
          -  .. /退回到上级目录进入image找到
       
       
       ```html
       ../image/test.gif
       ```

### 音频标签:musical_keyboard: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在页面中插入音频

**代码**::game_die: 

```html
<audio src="./music.mp3" controls loop></audio>
```

![image-20230430175923286](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307170953715.png)

效果：[src中引入一个本地音乐即可]

<audio src="" controls loop></audio>

**常见属性**:

| 属性名   | 功能                       |
| -------- | -------------------------- |
| src      | 音频的路径                 |
| controls | 显示播放的控件             |
| autoplay | 自动播放(部分浏览器不支持) |
| loop     | 循环播放                   |

:warning:**注意点**:

-  音频标签目前支持三种格式: <font title="red">MP3</font> , Wav , Ogg

### 视频标签:video_camera: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在页面中插入视频

**代码**: :game_die: 

```html
<!--谷歌浏览器可以让视频自动播放,但是前提必须是静音状态:muted-->
<video src="./video.mp4" controls autoplay muted loop></video>
```

![image-20230430181804445](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171000537.png)

效果：[src中引入一个本地的视频即可]

<video src="" controls autoplay muted loop></video>

**常见属性**:

| 属性名   | 功能                                            |
| -------- | ----------------------------------------------- |
| src      | 视频的路径                                      |
| controls | 显示播放的空间                                  |
| autoplay | 自动播放(谷歌浏览器中需要配合muted实现静音播放) |
| loop     | 循环播放                                        |

:warning:**注意点**:

-  视频标签目前支持三种格式: <font title="red">MP4</font> , WebM , Ogg

## 链接标签:lantern: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 点击之后,从一个页面跳转到另一个页面

**称呼**：ａ标签，超链接，锚链接

**代码**：:game_die: 

```html
<a href="./目标网页.html">超链接</a>
<!--当开发网站初期,我们还不知道跳转地址的时候,href中可以先写一个#(空链接)-->
<a href="#">超链接</a>
```

![image-20230430182723267](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171001065.png)

<a href="http://www.baidu.com">我是百度的超链接点击即可去百度</a>。

**特点**:

1.  双标签,内部可以包裹内容
2.  如果需要a标签点击之后去指定页面,需要设置a标签的href属性

### 链接标签的target属性

**属性名**: target

**属性值**: 目标网页的打开形式

| 取值              | 效果                                |
| ----------------- | ----------------------------------- |
| `_self`           | 默认值,在当前窗口中跳转(覆盖原网页) |
| `_blank`          | 在新窗口中跳转(保留原网页)          |
| pointer-events: n | 链接不可使用状态 (点击无效)         |

**代码**: :game_die: 

```html
<a href="./目标网页.html" target="_blank">超链接</a>
```

## 列表标签:bookmark_tabs: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

### 列表的应用场景:taco: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在网页中按照行展示关联性的内容,如: 新闻列表,排行榜,账单等

**特点**: 按照行的方式,整齐显示内容

**种类**: <span alt="shake">**无序列表**</span> , <span alt="shake">**有序列表**</span> , <span alt="shake">**自定义列表**</span>.

![image-20230430184906900](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171004467.png)

### 无序列表:paintbrush: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在网页中表示一组无顺序之分的列表,如: 新闻列表

**标签组成**::balance_scale: 

| 标签名 | 说明                                      |
| ------ | ----------------------------------------- |
| ul     | 表示无序列表的整体,用于包裹li标签         |
| li     | 表示无序列表的每一项,用于包含每一行的内容 |

**显示特点**:

-  列表的每一项前默认显示原点标识

:warning:**注意点**:

1.  ul标签中只允许包含li标签
2.  li标签可以包含任意内容

**代码**: :game_die: 

```html
<h1>水果列表</h1>
<ul>
   <li>榴莲</li>
   <li>香蕉</li>
   <li>苹果</li>
   <li>哈密瓜</li>
   <li>火龙果</li>
</ul>
```

**效果**:

![image-20230430190952333](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171005550.png)

**小结**:

无序列表由几个标签组成?分别表示什么?

1.  ul标签: 表示无序列表的整体
2.  li标签: 表示无序列表的每一项

无序列表标签的嵌套规范是什么?

1.  ul标签中只允许嵌套li标签
2.  li标签中可以嵌套任意内容

### 有序列表:factory: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在网页中表示一组有顺序之分的列表

**如**: 排行榜

**标签组成**:

| 标签名 | 说明                                    |
| ------ | --------------------------------------- |
| ol     | 表示有序列表的整体,用于包裹li标签       |
| li     | 表示有序列表的每一项,用于包含一行的内容 |

**显示特点**:

-  列表的每一项前默认显示序号标识

:warning:**注意点**:

1.  ol标签中只允许包含li标签
2.  li标签可以包含任意内容

**代码**:

```html
<h1>有序列表</h1>
<ol>
   <li>张三:100</li>
   <li>李四:90</li>
   <li>刘桑:80</li>
   <li>丽丽:70</li>
</ol>
```

**效果**:

![image-20230430191829654](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171005139.png)

**小结**:

有序列表由几个标签组成? 分别表示什么?

1.  ol标签: 表示无序列表的整体
2.  li标签: 表示无序列表的每一项

有序列表标签的嵌套规范是什么?

1.  ol标签中只允许嵌套li标签
2.  li标签中可以嵌套任意内容

### 自定义列表:kaaba: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在网页的底部导航中通常会使用自定义列表实现

**标签组成**:

| 标签名 | 说明                                   |
| ------ | -------------------------------------- |
| dl     | 表示自定义列表的整体,用于包裹dt/dd标签 |
| dt     | 表示自定义列表的主题                   |
| dd     | 表示自定义列表的针对主题的每一项内容   |

**显示特点**:

-  dd前会默认显示缩进效果

:warning:**注意点**:

1.  dl标签中只允许包含dt/dd标签
2.  dt/dd标签可以包含任意内容

**代码**::game_die:  

```html
<dl>
   <dt>自定义列表</dt>
   <dd>张三</dd>
   <dd>李四</dd>
   <dd>刘桑</dd>
</dl>
```

**效果**:

![image-20230430192729370](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171006331.png)

**小结**:

自定义列表由几个标签组成? 分别表示什么?

1.  dl标签: 表示自定义列表的整体
2.  dt标签: 表示自定义列表的主题
3.  dd标签: 表示对于主题的每一项内容 

自定义列表标签的嵌套规范是什么?

1.  dl标签中只允许嵌套dt/dd标签
2.  dt/dd标签中可以嵌套任意内容

## 表格标签:baby_bottle: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

### 表格的基本标签:jack_o_lantern: 

<span alt="shake">:diamond_shape_with_a_dot_inside: </span> 

**场景**: 在网页中以行+列的单元格的方式整齐展示和数据

**如**: 学生成绩表

**基本标签**:

| 标签名                              | 说明                      |
| ----------------------------------- | ------------------------- |
| <span alt="shake">**table **</span> | 表格整体,可用于包裹多个tr |
| <span alt="shake">**tr**</span>     | 表格每行,可用于包裹td     |
| <span alt="shake">**td**</span>     | 表格单元格,可用于包裹内容 |

:warning:**注意点**:

-  标签的嵌套关系: <font title="red">table</font> <span alt="blink">**>**</span> <font title="red">tr</font> <span alt="blink">**>**</span> <font title="red">td</font>.

### 表格相关属性:hamster: 

<span alt="shake">:diamond_shape_with_a_dot_inside:</span> 

**场景**：设置表格基本展示效果

**常见相关属性**：

| 属性名                              | 属性值                            | 效果                                                         |
| ----------------------------------- | --------------------------------- | ------------------------------------------------------------ |
| <span alt="shake">**border**</span> | <span alt="blink">**数字**</span> | <font title="red"><span alt="blink"><span alt="shake">**边框宽度**</span></span></font> |
| <span alt="shake">**width **</span> | <span alt="blink">**数字**</span> | <font title="blue"><span alt="blink"><span alt="shake">**表格宽度**</span></span></font> |
| <span alt="shake">**height**</span> | <span alt="blink">**数字**</span> | <font title="green"><span alt="blink"><span alt="shake">**表格高度**</span></span></font> |

:warning:**注意点**：

-  实际开发针对于样式效果 <font title="red">推荐用CSS设置</font>.

### 表格标题和表头单元格标签:headphones: 

<span alt="shake">:diamond_shape_with_a_dot_inside:</span> 

**场景**：在表格中表示整体大标题和一列小标题

**其它标签**：

| 标签名                               | 名称                                    | 说明                                                         |
| ------------------------------------ | --------------------------------------- | ------------------------------------------------------------ |
| <span alt="shake">**caption**</span> | <span alt="blink">**表格大标题**</span> | 表示==表格整体大标题==，==默认在表格整体顶部居中位置显示==   |
| <span alt="shake">**th**</span>      | <span alt="blink">**表头单元格**</span> | 表示==一列小标题==，通常用于==表格第一行<br>默认内部文字加粗并居中显示== |

:warning:**注意点**：

1.  <font title="red">caption</font>标签书写在==table==标签内部
2.  <font title="red">th</font>标签书写在==tr==标签内部(用于替换<font title="green">td</font>标签)

**代码**：:game_die: 

```html
<table border="1" cellspacing="0" bgcolor="hotpink">
   <caption><strong>成绩单</strong></caption>
   <tr>
      <th>姓名</th>
      <th>成绩</th>
      <th>评语</th>
   </tr>
   <tr>
      <td>刘桑</td>
      <td>100分</td>
      <td>优良</td>
   </tr>
   <tr>
      <td>张三</td>
      <td>80分</td>
      <td>优</td>
   </tr>
   <tr>
      <td>李四</td>
      <td>60分</td>
      <td>及格</td>
   </tr>
</table>
```

**效果**:

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171009820.png" alt="image-20230430234541305" align="left"/>

### 表格的结构标签:japanese_castle: 

<span alt="shake">:diamond_shape_with_a_dot_inside:</span> 

**场景**：让表格的内容结构分组，突出表格的不同部分(**头部**，**主体**，**底部**)，使==语义更加清晰== 

**结构标签**：

| 标签名                             | 名称                                  |
| ---------------------------------- | ------------------------------------- |
| <span alt="shake">**thead**</span> | <span alt="blink">**表格头部**</span> |
| <span alt="shake">**tbody**</span> | <span alt="blink">**表格主体**</span> |
| <span alt="shake">**tfoot**</span> | <span alt="blink">**表格底部**</span> |

:warning:**注意点**：

1.  <font style="color:red"><span alt="wavy">**表格结构标签内部用于包裹 <span alt="shake">tr</span> 标签**</span></font> 
2.  表格的结构标签可以省略

**代码**：:game_die: 

```html
<table border="1" cellspacing="0" bgcolor="hotpink">
   <caption><strong>成绩单</strong></caption>
   <thead>
      <tr>
         <th>姓名</th>
         <th>成绩</th>
         <th>评语</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>刘桑</td>
         <td>100分</td>
         <td>优良</td>
      </tr>
      <tr>
         <td>张三</td>
         <td>80分</td>
         <td>优</td>
      </tr>
      <tr>
         <td>李四</td>
         <td>60分</td>
         <td>及格</td>
      </tr>
   </tbody>
   <tfoot>
      <tr>
         <td>总结</td>
         <td>240</td>
         <td>优</td>
      </tr>
   </tfoot>
</table>
```

**效果**:

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171010094.png" alt="image-20230501002250423" style="zoom: 80%;" align="left"/>

### 合并单元格:bacon: 

<span alt="shake">:diamond_shape_with_a_dot_inside:</span> 

**场景**：将<span alt="wavy" style="color:red">水平或垂直</span>多个单元格<span alt="wavy" style="color:red">合并成一个单元格</span>.

#### 合并单元格步骤:wolf: 

:diamond_shape_with_a_dot_inside: 

1.  明确合并哪几个单元格
2.  通过左上原则，确定保留谁删除谁
    -  上下合并 -> 只保留最上的，删除其它
    -  左右合并 -> 只保留最左的，删除其它
3.  给保留的单元格设置：跨行合并(rowspan)或者跨列合并(colspan)

| 属性名                               | 属性值                                        | 说明                                                         |
| ------------------------------------ | --------------------------------------------- | ------------------------------------------------------------ |
| <span alt="shake">**rowspan**</span> | <span alt="blink">**合并单元格的个数**</span> | <font title="yellow"><span alt="blink"><span alt="shake">**跨行合并,将多行的单元格垂直合并**</span></span></font> |
| <span alt="shake">**colspan**</span> | <span alt="blink">**合并单元格的个数**</span> | <font title="green"><span alt="blink"><span alt="shake">跨列合并,将多列的单元格水平合并</span></span></font> |

:warning:**注意点**：

-  只有同一个结构标签中的单元格才能合并，不能跨结构标签合并(<span alt="blink">**不能跨**</span><span alt="shake">：</span><span alt="wavy" style="color:red">**thead**，**tbody**，**tfoot**</span>)

**代码**：:game_die: 

```html
<table border="1" cellspacing="0" bgcolor="hotpink">
   <caption><strong>成绩单</strong></caption>
   <thead>
      <tr>
         <th>姓名</th>
         <th>成绩</th>
         <th>评语</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>刘桑</td>
         <td>100分</td>
         <td>优良</td>
      </tr>
      <tr>
         <td>张三</td>
         <!--上下合并，保留最上的删除其它合并几行下面的删除几个单元格-->
         <td rowspan="2">80分</td>
         <td>优</td>
      </tr>
      <tr>
         <td>李四</td>
         <!--上下合并，保留最上的删除其它，将下面的注释掉-->
         <!--<td>60分</td>-->
         <td>及格</td>
      </tr>
   </tbody>
   <tfoot>
      <tr>
         <td>总结</td>
         <!--<td>240</td>-->
         <!--将分数注释掉我们展示评语占2个单元格,字体居中-->
         <td colspan="2" align="center">优</td>
      </tr>
   </tfoot>
</table>
```

**效果**：

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171012517.png" alt="image-20230501002810833" align="left" />

## 表单标签:label: 

:diamond_shape_with_a_dot_inside: 

### form表单

-  autocomplete="off"  禁用提交后存留的信息

### input系列标签:ice_cream: 

:diamond_shape_with_a_dot_inside: 

**场景**：在网页中显示收集用户信息的表单效果，如：登入页，注册页

**标签名**：input

-  input标签可以通过<font title="red">**type属性值的不同**</font>，展示不同效果

**属性**：

-  **required**：input为空提交时会有提示

**type属性值**：

| 标签名                             | type属性值                                                   | 说明                                     |
| ---------------------------------- | ------------------------------------------------------------ | ---------------------------------------- |
| <span alt="shake">**input**</span> | <font title="blue" style="color:red"><span alt="blink">**text**</span></font> | 文本框，用于输入单行文本                 |
| <span alt="shake">**input**</span> | <font title="blue" style="color:red"><span alt="blink">**password**</span></font> | 密码框，用于输入密码                     |
| <span alt="shake">**input**</span> | <font title="blue" style="color:red"><span alt="blink">**radio**</span></font> | 单选框，用于多选一                       |
| <span alt="shake">**input**</span> | <font title="blue" style="color:red"><span alt="blink">**checkbox**</span></font> | 多选框，用于多选多                       |
| <span alt="shake">**input**</span> | <font title="blue" style="color:red"><span alt="blink">**file**</span></font> | 文件选择，用于之后上传文件               |
| <span alt="shake">**input**</span> | <font title="blue" style="color:red"><span alt="blink">**submit **</span></font> | 提交按钮，用于提交                       |
| <span alt="shake">**input**</span> | <font title="blue" style="color:red"><span alt="blink">**reset**</span></font> | 重置按钮，用于重置                       |
| <span alt="shake">**input**</span> | <font title="blue" style="color:red"><span alt="blink">**button**</span></font> | 普通按钮，默认无功能，之后配合js添加功能 |

![image-20230501004727443](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171013061.png)

#### 文本框:pager: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在网页中显示<font title="red">**输入单行文本**</font>的表单控件

**type属性值**: text

**常用属性**:

| 属性名                                   | 说明                          |
| ---------------------------------------- | ----------------------------- |
| <span alt="shake">**placeholder**</span> | 占位符,提示用户输入内容的文本 |

**代码**::game_die: 

```html
<input type="text" placeholder="请输入账号..."/> 
```

**效果**:

![image-20230501220909478](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171014862.png)

#### 单选框:dango: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在网页中显示<font title="red">**多选一的单元**</font>表单控件

**type属性值**: radio

**常用属性**:

| 属性名                               | 说明                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| <span alt="shake">**name**</span>    | 分组,有相同name属性值的单选框为一组<br>一组中同时只能有一个被选中 |
| <span alt="shake">**checked**</span> | 默认选中                                                     |

:warning:**注意点**:

1.  name属性对于单选框有==分组功能== (比如：男，女和爱好分别分一组为两组不然这四个就只能选一个)
2.  有相同name属性值的单选框为一组,一组中只能同时有一个被选中

**代码**::game_die: 

```html
<input type="radio" name="gender" checked/> 男<br>
<input type="radio" name="gender"/> 女<br>
```

**效果**:

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171016393.png" alt="image-20230501222356158" style="zoom: 80%;" align="left"/><img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171016406.png" alt="image-20230501222422950" style="zoom:80%;" align=""/>

#### 文件选择:fallen_leaf: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在网页中显示<font  title="red">**文件选择的**</font>表单控件

**type属性值**: file

**常用属性**:

| 属性名   | 说明       |
| -------- | ---------- |
| multiple | 多文件选择 |

**代码**::game_die: 

```html
<input type="file" multiple/>
```

**效果**:

![image-20230501223548578](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171016086.png)

#### 按钮:name_badge: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在网页中显示<font title="red">**不同功能的按钮**</font>表单控件

**type属性值**:

| 标签名                         | type属性值                          | 说明                                   |
| ------------------------------ | ----------------------------------- | -------------------------------------- |
| <font title="red">input</font> | <span alt="shake">**submit**</span> | 提交按钮,点击之后提交数据给后端服务器  |
| <font title="red">input</font> | <span alt="shake">**reset**</span>  | 重置按钮,点击之后恢复表单默认值        |
| <font title="red">input</font> | <span alt="shake">**button**</span> | 普通按钮,默认无功能,之后配合js添加功能 |

:warning:**注意点**:

1.  如果需要实现以上按钮功能,需要配合form标签使用
2.  **form使用方法**: 用form标签把表单标签一起包裹起来即可

**代码**::game_die: 

```html
<form action="" method="get">
   账号: <input type="text" name="username" placeholder="请输入账号..."/><br><br>
   密码: <input type="password" name="password" placeholder="请输入密码..."/><br><br>
   <!--按钮-->
   <input type="submit" value="提交信息"/>
   <input type="reset" value="重置信息"/>
   <input type="button" value="普通按钮"/>
</form>
```

**效果**:

![image-20230501225242197](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171016949.png)

### button按钮标签:baby_chick: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在网页中显示用户点击的按钮

**标签名**: button

**type属性值**(同input的按钮系列)

| 标签名                              | type属性值                          | 说明                                   |
| ----------------------------------- | ----------------------------------- | -------------------------------------- |
| <font title="red">**button**</font> | <span alt="shake">**submit**</span> | 提交按钮,点击之后提交数据给后端服务器  |
| <font title="red">**button**</font> | <span alt="shake">**reset**</span>  | 重置按钮,点击之后恢复表单默认值        |
| <font title="red">**button**</font> | <span alt="shake">**button**</span> | 普通按钮,默认无功能,之后配合js添加功能 |

:warning:**注意点**:

1.  谷歌浏览器中button默认是提交按钮
2.  button标签是双标签,更便于包裹其它内容: 文字,图片等

**代码**::game_die: 

```html
<form action="" method="get">
   <input type="text" placeholder="请输入账号..."/><br><br>
   <button type="submit">我是提交按钮</button>
   <button type="reset">我是重置按钮</button>
   <button>我是普通按钮,点击后刷新页面</button>
   <button type="button">我是普通按钮,没有任何功能</button>
</form>
```

**效果**:

![image-20230502101751764](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171017721.png)

### select下拉菜单标签:sake: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在网页中提供多个选择项的下拉菜单表单控件

![image-20230502102501823](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171017748.png)

**标签组成**:

1.  select标签: 下拉菜单的整体
2.  option标签: 下拉菜单的每一项

**常见属性**:

-  selected: 下拉菜单默认选中

**代码**::game_die: 

```html
<strong>所在城市:</strong>
<select>
   <option selected>上海</option>
   <option>北京</option>
   <option>深圳</option>
</select> 
```

### textarea文本域标签:aerial_tramway: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在网页中提供可输入多行文本的表单控件

![image-20230502103611318](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171017506.png)

**标签名**: textarea

**常见属性**:

1.  cols: 规定了文本域内可见宽度
1.  rows: 规定了文本域内可见行数

:warning:**注意点**:

1.  右下角可以拖拽改变大小
2.  实际开发时针对于样式效果<font title="red">推荐使用CSS设置</font>.

**代码**::game_die: 

```html
<textarea cols="30px" rows="10px">这是内容</textarea> 
```

### label标签:ear_of_rice:  

:diamond_shape_with_a_dot_inside: 

**场景**: 常用于绑定内容与表单标签的关系

**标签名**: label

**使用方法①**:

1.  使用label标签把内容(如: 文本)包裹起来
2.  在表单标签上添加id属性
3.  在label标签的for属性中设置对应的id属性值

```html
<input type="checkbox" id="chek"/> <label for="chek">是否选择</label>
```

**使用方法②**:

1.  直接使用label标签把内容(如: 文本)和表单标签一起包裹起来
2.  需要把label标签的for属性删除即可

```html
<label><input type="checkbox"/> 是否选择</label>
```

## 语义化标签:yin_yang: 

:diamond_shape_with_a_dot_inside: 

### 没有语义的布局标签(<span alt="shake">div</span>和<span alt="shake">span</span>):paintbrush: 

:diamond_shape_with_a_dot_inside: 

**场景**: 实际开发网页时会大量频繁的使用到div和span这两个没语义的布局标签

**div标签**: 一行只显示一个(独占一行)

-  :warning:**注意点**: <span alt="wavy" style="color:red">尽量不要在div上使用id选择器因为会大量使用尽可能使用class选择器</span>.

**span标签**: 一行可以显示多个(不换行)

### 有语义的布局标签:octopus: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在HTML5新版本中,推出了一些有语义的布局标签供开发者使用

![image-20230502110427195](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171018799.png)

**标签**:

| 标签名  | 语义       |
| ------- | ---------- |
| header  | 网页头部   |
| nav     | 网页导航   |
| footer  | 网页底部   |
| aside   | 网页侧边栏 |
| section | 网页区块   |
| article | 网页文章   |

:warning:**注意点**:

-  以上标签显示特点和div一致,但是比div多了不同的语义

## 字符实体:sandal: 

:diamond_shape_with_a_dot_inside: 

### HTML中的空格和并现象:ocean: 

:diamond_shape_with_a_dot_inside: 

**场景**: 如果在html代码中同时并列出现多个空格,换行,缩进等,最终浏览器只会解析出一个空格

### 常见字符实体:key: 

:diamond_shape_with_a_dot_inside: 

**场景**: 在网页中展示特殊符号效果时,需要使用字符实体替代

**结构**: &英文;

**常见字符实体**:

![image-20230502112351299](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171019906.png)

## 综合案例-表单:baby_chick: 

:diamond_shape_with_a_dot_inside: 

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
    <form action="" method="get">
   <h1>博客</h1> 
   <hr>
   昵称: <input type="text" placeholder="请输入昵称..."/><br><br>
   性别: <input type="radio" name="gender" id="male"/> <label for="male">男</label>
        <input type="radio" name="gender" id="female"/> <label for="female">女</label><br><br>
    所在城市: <select>
                <option>上海</option>
                <option selected>北京</option>
                <option>深圳</option>
            </select><br><br>
    个人状况: <input type="radio" checked name="status" id="o1"/> <label for="o1">初级工程师</label>
            <input type="radio" name="status" id="o2"/> <label for="o2">中级工程师</label>
            <input type="radio" name="status" id="o3"/> <label for="o3">高级工程师</label><br><br>
    擅长语言: <input type="checkbox" checked id="oo1"/> <label for="oo1">Java</label>
                <input type="checkbox" id="oo2"/> <label for="oo2">C</label>
                <input type="checkbox" id="oo3"/> <label for="oo3">C++</label>
                <input type="checkbox" id="oo4"/> <label for="oo4">Rust</label>
                <input type="checkbox" id="oo5"/> <label for="oo5">php</label>
                <input type="checkbox" id="oo6"/> <label for="oo6">JavaScript</label><br><br>
    个人介绍:<br><br>
    <textarea cols="55px" rows="10px"></textarea><br><br>
    <strong>我承诺</strong>
    <ul>
        <li>年满18岁</li>
        <li>抱着严肃的态度</li>
        <li>认真对待</li>
    </ul>
    <label><input type="checkbox"/>我同意所有条款</label><br><br>
    <input type="submit" value="免费注册"/>
    <button type="reset">重置</button>
    </form>
</body>
</html>
```

**效果**:

![image-20230502124301094](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171019353.png)

