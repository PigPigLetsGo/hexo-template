---
title: CSS3
categories: 
    - [计算机学科,web,css]
tags:
    - web
    - 计算机学科
---

# CSS:canoe: 

:diamond_shape_with_a_dot_inside: 

## 基础认知:jack_o_lantern: 

:diamond_shape_with_a_dot_inside: 

### CSS初识:facepunch: 

:diamond_shape_with_a_dot_inside: 

#### 1.CSS的介绍:octopus: 

:diamond_shape_with_a_dot_inside: 

**CSS**: <font title="red">**层叠样式表**</font>(Cascading style sheets)

**CSS作用是什么**?

-  给页面中的HTML标签设置样式

#### CSS语法规则:candy: 

:diamond_shape_with_a_dot_inside: 

**写在哪里**?

-  css写在style标签中,style标签一般写在head标签里面,title标签下面

**CSS中的注释**: `/*内容*/` 

**怎么写**?

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /*选择器*/
        p{
          /*css属性*/  
            color:red
            /*属性名*/ /*属性值*/
        }
    </style>
</head>
<body>
   <p>hello,world 你好世界</p> 
</body>
</html>
```

**效果**:

![image-20230502125327890](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171019975.png)

### CSS引入方式:mailbox_closed: 

:diamond_shape_with_a_dot_inside: 

1.  **内嵌式**: CSS写在style标签中
    -  **提示**: style标签虽然可以写在页面任意位置,但是通常约定写在<span alt="wavy" style="color:red">head</span>标签中
2.  **外联式**: CSS写在一个单独的.css文件中
    -  **提示**: 需要那个过<span alt="wavy" style="color:red">link</span>标签在网页中引入
3.  **行内式**: CSS写在标签的style属性中
    -  **提示**: 不推荐,可配合js使用

**CSS常见三种引入方式的特点区别(书写位置,作用范围,使用场景)**?

| 引入方式                              | 书写位置                            | 作用范围 | 使用场景   |
| ------------------------------------- | ----------------------------------- | -------- | ---------- |
| <span alt="shake">**内嵌式**</span>   | CSS写在style标签中                  | 当前页面 | 小案例     |
| <span alt="shake">**外联联式**</span> | CSS写在单独的css文件中,通过标签引入 | 多个页面 | 项目中     |
| <span alt="shake">**行内式**</span>   | CSS写在标签的style属性中            | 当前标签 | 配合js使用 |

## 基础选择器:sailboat: 

:diamond_shape_with_a_dot_inside: 

**选择器的作用**:

-  选择页面中对应的标签(找它),方便后续设置样式(改它)

### 标签选择器:panda_face: 

:diamond_shape_with_a_dot_inside: 

**结构**: <span alt="shake">**标签名**</span>{<span alt="blink">**css属性名**</span> <span alt="shake">**:**</span> <span alt="blink">**属性值;**</span>}

**作用**: 通过标签名,找到页面中所有这类标签,设置样式

:warning:**注意点**:

1.  标签选择器选中的是一类标签,而不是单独某一个
2.  标签选择器无论嵌套关系有多深,都能找到对应的标签

### 类选择器:lantern: 

:diamond_shape_with_a_dot_inside: 

**结构**: <span alt="blink">**.**</span><span alt="shake" style="color:red">**类名**</span>{<span alt="blink">**css属性名**</span> <span alt="shake">**:**</span> <span alt="blink">**属性值;**</span>}

**作用**: 通过类名,找到页面中所有带有这个类名的标签,设置样式

:warning:**注意点**:

1.  所有标签上都有class属性,class属性的属性值称为<font title="blue">类名(类似于名字)</font>.
2.  类名可以由<font title="red">数字</font>,<font title="red">字母</font>,<font title="red">下划线</font>,<font title="red">中划线</font> 组成,但<span alt="wavy" style="color:red">不能以数字或者中划线开头</span>.
3.  一个标签可以同时有多个类名,类名之间以空格隔开
4.  类名可以重复,一个类选择器可以同时选中多个标签

**代码**::game_die: 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .red{
            color:red;
        }
        .size{
            font-size: 66px;
        }
    </style>
</head>
<body>
   <p>111</p> 
   <!--定义多个类名使用空格隔开-->
   <p class="red size">222</p>
   <div class="red">这个标签文字也要变红</div>
</body>
</html>
```



### id选择器:ice_cream: 

:diamond_shape_with_a_dot_inside: 

**说明**: <span alt="rainbow">**id选择器设置的初衷不是设置样式的,而是为了配合js找标签使用的**</span>.

**结构**: <span alt="blink">**#**</span><span alt="shake" style="color:red">**id属性值**</span>{<span alt="blink">**css属性名**</span> <span alt="shake">**:**</span> <span alt="blink">**属性值;**</span>}

**作用**: 通过id属性值,找到页面中带有这个id属性值的标签,设置样式

:warning:**注意点**:

1.  所有标签上都有id属性
2.  id属性值类似于身份证号码,在一个页面中是唯一的,不可重复的!
3.  一个标签上只有一个id属性值
4.  一个id选择器只能选中一个标签

### 通配符选择器:taco:  

:diamond_shape_with_a_dot_inside: 

**结构**: <span alt="shake">*****</span>{<span alt="blink">**css属性名**</span> <span alt="shake">**:**</span> <span alt="blink">**属性值;**</span>}

**作用**: 找到页面中所有的标签,设置样式

:warning:**注意点**:

1.  开发中使用极少,只会在特殊情况下才会用到
2.  可用于清除所有标签之间自带的外边距和内边距

**代码**::game_die: 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        *{
            color:red;
            margin:0px;
            padding:0px;
        }
    </style>
</head>
<body>
    <p>123</p>
    <p>345</p>
    <div>567</div>
    <a href="#">超链接</a>
</body>
</html>
```

**效果**:

![image-20230502195938929](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171020056.png)


## 文字和文本样式:yum:

:diamond_shape_with_a_dot_inside: 

### 字体样式:label: 

:diamond_shape_with_a_dot_inside: 

1.  <span alt="shake">字体大小</span>: <span alt="rainbow">**font-size**</span>.
2.  <span alt="shake">字体粗细</span>: <span alt="rainbow">**font-weight**</span>.
3.  <span alt="shake">字体样式</span>: <span alt="rainbow">**font-style**</span>.
4.  <span alt="shake">字体类型</span>: <span alt="rainbow">**font-family**</span>.
5.  <span alt="shake">字体类型</span>: <span alt="rainbow">**font属性连写**</span>.

#### 字体大小:oil_drum: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**font-size**</span>.

**取值**: <span alt="shake">数字</span> <span alt="blink">+</span> <span alt="modern">px</span>.

:warning:**注意点**:

1.  谷歌浏览器默认文字大小是16px
2.  <font style="color:red">单位需要设置,否则无效</font> .

#### 字体加粗:camping: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**font-weight**</span>.

**取值**:

-  <span alt="glow">**关键字**</span> : normal , bold

| <font style="color:black">正常</font> | <font style="color:black">normal</font> |
| ------------------------------------- | --------------------------------------- |
| <font style="color:black">加粗</font> | <font style="color:black">bold</font>   |

-  <span alt="glow">**纯数字**</span>: 100~900的整百数:

| <font style="color:black">正常</font> | <font style="color:black">400</font> |
| ------------------------------------- | ------------------------------------ |
| <font style="color:black">加粗</font> | <font style="color:black">700</font> |

:warning:**注意点**:

1.  <font style="color:red">不是所有字体都提供了九种粗细,因此部分取值页面中无变化</font>.
2.  实际开发中: 以 <font title="red">正常</font>,<font title="blue">加粗</font> 两种取值使用最多

**代码**: :game_die: 

```html
    <style>
        p {
            /*font-weight: bold;*/
            /*上面或者下面*/
            font-weight: 700;
        }
    </style>
</head>
<body>
   <p>123</p> 
</body>
```

#### 是否倾斜:zipper_mouth_face: 

:diamond_shape_with_a_dot_inside: 

**属性名**: **<span alt="rainbow">font-style</span>**.

**取值**:

1.  **正常**(默认值): normal
2.  **倾斜**: italic (翻译: 斜体的)

**代码**::game_die: 

```html
    <style>
        div{
            /*倾斜*/
            font-style: italic;
        }
        em{
            /*正常,不倾斜*/
            font-style: normal;
        }
    </style>
</head>
<body>
    <div>123</div>
    <em>345</em>
</body>
```

**效果**:

![image-20230502202705987](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171023454.png)

#### font-family:pager: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**font-family**</span>.

**常见取值**: 具体字体1,具体字体2,具体字体3,具体字体4,...,字体系列

-  具体字体: "Microsoft YaHei" , 微软雅黑,黑体,宋体,楷体等等...
-  字体系列: <font title="red">sans-serif</font>,<font title="red">serif</font>,<font title="red">monospace</font>等等...

**渲染规则**:

1.  从左往右按照顺序查找,如果电脑中未安装该字体,则显示下一个字体
2.  如果都不支持,此时会根据操作系统,显示最后字体系列的默认字体

:warning:**注意点**:

1.  如果字体名称中存在多个单词,推荐使用<span alt="glow">引号</span>包裹
2.  最后一项<span alt="wavy" style="color:red">字体系列不需要引号包裹</span>.
3.  网页开发时,尽量使用系统常见自带字体,保证不同用户浏览网页都可以正确显示

##### 常见字体系列:o: 

:diamond_shape_with_a_dot_inside: 

###### 无衬线字体(sans-serif)

1.  **特点**: 文字笔画粗细均匀,并且首尾无装饰
2.  **场景**: 网页中大多采用无衬线字体
3.  **常见该系列字体**: 黑体,Arial

###### 衬线字体(serif)

1.  **特点**: 文字笔画粗细不均,并且首尾有笔锋装饰
2.  **场景**: 报刊书籍中应用广泛
3.  **常见该系列字体**: 宋体,Times New Roman

###### 等宽字体(monospace)

1.  **特点**: 每个字母或文字的宽度相等
2.  **场景**: 一般用于程序代码编写,有利于代码的阅读和编写
3.  **常见该系列字体**: Consolas,fira code

#### font相关属性的连写:lantern: 

**作用**: 简写

**属性名**: <span alt="rainbow">font</span>(复合属性)

**取值**:

-  **font**: style weight size family;

**省略要求**:

-  <span alt="wavy" style="color:red">只能省略前两个,如果省略了相当于设置了默认值,后面的省略或者缺少一个或者顺序混乱效果不会生效</span>.

:warning:<font style="color:red">**注意点**</font>: 如果需要同时设置<span alt="shake">单独和连写</span>形式

1.  要么把单独的样式写在连写的下面,<span alt="wavy" style="color:red">因为样式层叠的问题</span>.
2.  要么把单独的样式写在连写的里面

**代码**::game_die: 

```html
    <style>
        p{
            /*
            font-size: ;
            font-style: ;
            font-weight: ;
            font-family: ;
            font: style weight size 字体;
            */
            font:italic 700 66px 宋体;
            /*覆盖上面样式,只能省略前两个*/
            font: 100px 微软雅黑;
        }
    </style>
</head>
<body>
   <p>123</p> 
</body>
```

**效果**:

![image-20230503111519889](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171025631.png)

#### 样式的层叠问题:watch: 

:diamond_shape_with_a_dot_inside: 

**问题**:

-  给同一个标签色设置了相同的样式,此时浏览器会如何渲染呢?

**结果**:

- 如果给==同一个标签设置了相同的属性==,此时==样式会层叠==(覆盖),写在==最下面的会生效==.

**TIP**:

-  CSS(Cascding style sheets) **层叠样式表** 
-  所谓的==层叠==即叠加的意思,表示==样式可以一层一层的层叠覆盖==.

### 文本样式:jack_o_lantern: 

:diamond_shape_with_a_dot_inside: 

1.  **文本缩进**: <span alt="rainbow">**text-indent**</span>.
2.  **文本水平对齐方式**: <span alt="rainbow">**text-align**</span>.
3.  **文本修饰**: <span alt="rainbow">**text-decoration**</span>.

#### 文本缩进:sake: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**text-indent**</span>.

**取值**:

1.  数字 + px
2.  数字 + em (推荐: 1em = 当前标签的font-size的大小)

**代码**::game_die: 

```html
    <style>
        p{
            /*需求:设置首行缩进两个字*/
            /*字体默认大小16px,相加就是32*/
            text-indent:32px;
            /*但是如果将字体大小改了那么首行缩进就不是两个字了*/
            font-size:20px;
            /*单位:em为一个字的大小*/
            text-indent:2em;
        }
    </style>
</head>
<body>
   <p>哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈
    哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈
    哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈
    哈哈</p> 
</body>
```

**效果**:

![image-20230503112640076](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171027016.png)

#### 文本水平对齐方式:japan: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**text-align**</span>.

**取值**:

| 属性值                                | 效果     |
| ------------------------------------- | -------- |
| <span alt="rainbow">**left**</span>   | 左对齐   |
| <span alt="rainbow">**center**</span> | 居中对齐 |
| <span alt="rainbow">**right**</span>  | 右对齐   |

:warning:**注意点**:

-  如果需要让==文本水平居中==,text-align属性给<font title="red">**文本所在标签(文本的父元素)**</font>设置

**text-align: center 能让哪些元素水平居中**?

1.  文本
2.  span标签,a标签
3.  input标签,img标签

:warning:**注意点**:

-  如果需要让==以上元素水平居中==,text-align: center 需要给以上元素的==父元素==设置

#### 文本修饰:waning_crescent_moon: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**text-decoration**</span>.

**取值**:

| 属性值                                      | 效果             |
| ------------------------------------------- | ---------------- |
| <span alt="rainbow">**underline**</span>    | 下划线(常用)     |
| <span alt="rainbow">**line-through**</span> | 删除线(不常用)   |
| <span alt="rainbow">**overline**</span>     | 上划线(几乎不用) |
| <span alt="rainbow">**none**</span>         | 无装饰线(常用)   |

:warning:**注意点**:

-  开发中会使用<span alt="rainbow">**text-decoration:none;**</span>清除a标签默认的下划线

### line-height行高:gem: 

**作用**: 控制一行的==上下间距==

**属性名**: <span alt="rainbow">**line-height**</span>.

**取值**:

-  数字 + px
-  倍数(当前标签font-size的倍数)

**应用**:

1.  让<font style="color:red">单行文本</font>垂直居中可以设置<font style="color:red">line-height: 文字父元素高度</font>.
2.  网页精准布局时,会设置<font style="color:red">line-height: 1</font>可以取消上下间距

:warning:**行高与font连写的注意点**:

1.  <font style="color:red">如果同时设置了行高和font连写,注意覆盖问题</font>。
2.  **书写顺序**: font: style weight size<span alt="shake">/</span><span alt="rainbow">**line-height**</span> family;
3.  <span alt="wavy" style="color:red">需要写在size的后面否则不生效</span>.

![image-20230717103357363](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171033800.png)

**代码**::game_die: 

```html
    <style>
        div{
            /*只能省略前两个,少了family下面这个不会生效*/
            /*style weight size*/
            font: italic 700 20px;
            /*size/line-heigth family ，只省略了前两个以下会生效*/
            font: 20px/1.5 YaHei;
        }
    </style>
</head>
<body>
   <div>123<br>123</div> 
</body>
```

**效果**:

![image-20230503153812847](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171031414.png)

## Chrome调式工具:gem: 

:diamond_shape_with_a_dot_inside: 

**代码**::game_die: 

```html
    <style>
        div{
            /*只能省略前两个,下面这个不会生效*/
            /*style weight size*/
            font: italic 700 20px;
            /*size line-heigth family*/
            font:20px/1.5 YaHei;
        }
    </style>
</head>
<body>
   <div>123<br>123</div> 
</body>
```

通过f12开打Chrome的控制台查看Styles的div标签中有一个font被划上了删除线,这表明这个代码没有生效,它在源码里被上面的font覆盖了

![image-20230503155524992](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171031689.png)

我们可以点击最后一个font的属性值但后点击Enter(回车)就可以添加属性了,当然这是暂时的刷新后效果消失,除非到源码里设置

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171031923.gif)

可以选中font属性值然后按↑ ↓键 来控制数值的加减,用来看什么数值是我们需要的然后再到源码里设置,方便快速设置我们预期的效果

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171034068.gif)

将对勾去掉后者勾选可以选择性的添加或者去除样式

![image-20230503160318613](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307200901378.png)

如果代码写错了导致后面的代码没有生效可以查看报错的具体位置在哪里然后到编译器中查看修改即可

![image-20230503160407106](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171034721.png)

## 扩展 颜色常见取值:ice_hockey: 

:diamond_shape_with_a_dot_inside: 

**属性名**:

-  如: **文字颜色**: color
-  如: **背景颜色**: background-color

**属性值**:

| 颜色表示方式   | 表示含义                                   | 属性值                                       |
| -------------- | ------------------------------------------ | -------------------------------------------- |
| 关键词         | 预定义的颜色名                             | red,green,blue,yellow...                     |
| rgb表示法      | 红绿蓝三原色,每项取值范围:0~255            | rgb(0,0,0),rgb(255,255,255),rgb(255,0,0),... |
| rgba表示法     | 红绿三三原色+==a表示透明度==,取值范围是0~1 | rgba(255,255,255,0.5),rgba(255,0,0,0.3),...  |
| 十六进制表示法 | ==#开头==,将数字转换成十六进制表示         | #000000,#ff0000,#e92322,简写: #000,#f00      |

![image-20230503162307852](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171035906.png)

## 扩展 标签水平居中方法总结: <span alt="rainbow">**margin: 0 auto**</span>:package: 

:diamond_shape_with_a_dot_inside: 

**如果需要让div,p,h(大盒子)水平居中**?

-  可以通过<span alt="rainbow">**margin: 0 auto;**</span>实现

:warning:**注意点**:

1.  如果需要让div,p,h(大盒子)水平居中,直接给<font style="color:red">**当前元素本身**</font>设置即可
2.  <span alt="rainbow">**margin: 0 auto;**</span> 一般针对于==固定宽度的盒子==,如果==大盒子没有设置宽度==,此时会==默认占满父元素的宽度==。

## 新闻网页案例:pager: 

:diamond_shape_with_a_dot_inside: 

**要求**: 对于大小,颜色等具体样式取值,参考效果

**代码**: :game_die: 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div{
            margin: 0 auto;
            line-height: 1.5;
            width: 800px;
            height: 500px;
        }
        .p{
            text-indent:2em;
        }
        h1 , p:nth-child(2){
            text-align:center;
        }
        p > span:first-child{
            color:darkgray;
        }
        p > span:nth-child(2){
            font-weight: bold;
            color:hotpink;
        }
        p > a{
            text-decoration:none;
        }
    </style>
</head>
<body>
    <div>
        <h1>朱自清《荷塘月色》</h1>
        <p>
            <span>
            2023-05-3 4:00
            </span>
            <span>
            知乎
            </span>
            <a href="#">
            收藏文本
            </a>
        </p>
        <hr>
        <p class="p">路上只我一个人，背着手踱着。这一片天地好像是我的;
            我也像超出了平常旳自己，到了另一世界里。我爱热闹，
            也爱冷静;爱群居，也爱独处。像今晚上，一个人在这苍
            茫旳月下，什么都可以想，什么都可以不想，便觉是个
            自由的人。白天里一定要做的事，一定要说的话，现在
            都可不理。这是独处的妙处，我且受用这无边的荷香月色好了。</p>
        <p class="p">曲曲折折的荷塘上面，弥望旳是田田的叶子。叶子出水很高
            ，像亭亭旳舞女旳裙。层层的叶子中间，零星地点缀着些白花
            ，有袅娜(niǎo,nuó)地开着旳，有羞涩地打着朵儿旳;正如一
            粒粒的明珠，又如碧天里的星星，又如刚出浴的美人。微风过处，
            送来缕缕清香，仿佛远处高楼上渺茫的歌声似的。这时候叶子与
            花也有一丝的颤动，像闪电般，霎时传过荷塘的那边去了。叶子
            本是肩并肩密密地挨着，这便宛然有了一道凝碧的波痕。叶子底
            下是脉脉(mò)的流水，遮住了，不能见一些颜色;而叶子却更见风
            致了。</p>
        <p class="p">月光如流水一般，静静地泻在这一片叶子和花上。薄薄的青雾浮起在
            荷塘里。叶子和花仿佛在牛乳中洗过一样;又像笼着轻纱的梦。虽然
            是满月，天上却有一层淡淡的云，所以不能朗照;但我以为这恰是到了
            好处——酣眠固不可少，小睡也别有风味的。月光是隔了树照过来的，
            高处丛生的灌木，落下参差的斑驳的黑影，峭楞楞如鬼一般;弯弯的杨
            柳的稀疏的倩影，却又像是画在荷叶上。塘中的月色并不均匀;但光与
            影有着和谐的旋律，如梵婀(ē)玲(英语violin小提琴的译音)上奏着的
            名曲。</p>
        <p class="p">荷塘的四面，远远近近，高高低低都是树，而杨柳最多。这些树将一片
            荷塘重重围住;只在小路一旁，漏着几段空隙，像是特为月光留下的。
            树色一例是阴阴的，乍看像一团烟雾;但杨柳的丰姿，便在烟雾里也辨
            得出。树梢上隐隐约约的是一带远山，只有些大意罢了。树缝里也漏着
            一两点路灯光，没精打采的，是渴睡人的眼。这时候最热闹的，要数树上
            的蝉声与水里的蛙声;但热闹是它们的，我什么也没有。</p>
    </div>
</body>
</html>
```

**效果**:

![image-20230503172454060](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171039229.png)

## 选择器的进阶:cake: 

:diamond_shape_with_a_dot_inside: 

### 1.复合选择器:pancakes: 

:diamond_shape_with_a_dot_inside: 

#### 后代选择器: <span alt="rainbow">空格</span>:taco: 

:diamond_shape_with_a_dot_inside: 

**作用**: 根据HTML标签的嵌套关系,选择父元素<font style="color:red">**后代中**</font>满足条件的元素

**选择器语法**: <font style="color:red">**选择器1 选择器2{css}**</font> 

**结果**:

-  在选择器1所找到标签的后代(<span alt="wavy" style="color:red">儿子,孙子,重孙子,曾孙子,...</span>)中,找到满足选择器2的标签,设置样式

:warning:**注意点**:

1.  后代包括: 儿子,孙子,重孙子,曾孙子,...
2.  后代选择器中,选择器与选择器之间通过<span alt="rainbow">**空格**</span>隔开

#### 子代选择器: <span alt="shake">**>**</span>:hammer: 

:diamond_shape_with_a_dot_inside: 

**作用**: 根据HTML标签的嵌套关系,选择父元素<font style="color:red">**子代中**</font>满足条件的元素

**选择器语法**: <font style="color:red">**选择器1 > 选择器2{css}**</font> 

**结果**:

-  在选择器1所找到标签的子代(儿子)中,找到满足选择器2的标签,设置样式

:warning:**注意点**:

1.  子代只包括: 儿子
2.  子代选择器中,选择器与选择器之间通过 <font style="color:red">**>**</font> 隔开

**代码**: :game_die: 

```html
    <style>
        /*后代选择器会选中,儿子,孙子,重孙子,曾孙子...*/
        /* div a {
            color:red;
        } */
        div > a {
            color:red;
        }
    </style>
</head>
<body>
   <div>
    父级
    <a href="#">这是div中的a标签</a>
    <p>
        <a href="#">这是div中的p中的a标签</a>
    </p>
   </div> 
</body>
```

**效果**:

![image-20230503200315035](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171040687.png)

### 2.并集选择器<span alt="shake">**,**</span>:badminton: 

:diamond_shape_with_a_dot_inside: 

**作用**: 同时选择多组标签,设置相同样式

**选择器语法**: <font style="color:red">**选择器1 <span alt="shake">,</span> 选择器2{css}**</font> 

**结果**:

-  找到 <font style="color:red">**选择器1**</font>和<font style="color:red">**选择器2**</font>选中的标签,设置样式

:warning:**注意点**:

1.  并集选择器中的每组选择器之间通过<span alt="shake">**,**</span>分隔
2.  并集选择器中的每组选择器可以是基础选择器或者复合选择器
3.  并集选择器中的每组选择器通常一行写一个,提高代码的可读性

**代码**: :game_die: 

```html
    <style>
       /*一行写一个提高代码的可读性*/
        p,
        div,
        span,
        h1 {
            color:red;
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <p>ppp</p>
    <div>div</div>
    <span>span</span>
    <h1>h1</h1>

    <h2>h2</h2>
</body>
```

**效果**:

![image-20230503202045369](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171042403.png)

### 3.交集选择器:<span alt="rainbow">紧挨着</span>:jeans: 

:diamond_shape_with_a_dot_inside: 

**作用**: 选中页面中<font style="color:red">**同时满足**</font>多个选择器的标签

**选择器语法**: <font style="color:red">**选择器1选择器2{css}**</font> 

**结果**:

-  (即又原则) 找到页面中 <span alt="rainbow">**即**</span> 能被选择器1选中, <span alt="rainbow">**又**</span>能被选择器2选中的标签,设置样式

:warning:**注意点**:

1.  交集选择器中的==选择器之间是紧挨着==的,==没有东西分隔==。
2.  交集选择器中如果有标签选择器,==标签选择器必须写在最前面==。

**代码**: :game_die: 需求只设置p标签带有box类的标签设置为红色

```html
<body>
   <!--将带有box类的p标签设置为红色-->
    <p class="box">p中有box</p>
    <div class="box">div中有box</div>
    <p>普通的p</p>
</body>
```

```html
<style>
   p {
      color:red;
   }
</style>
```

**效果**:

![image-20230503203936186](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171045827.png)

```html
<style>
   .box {
      color:red;
   }
</style>
```

**效果**:

![image-20230503204013169](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171045920.png)

```html
<style>
   /*同时满足多个条件的选择器就是交集选择器*/
   p.box {
      color:red;
   }
</style>
```

**效果**:

![image-20230503204154208](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171045564.png)

### 4.hover伪类选择器:watermelon: 

:diamond_shape_with_a_dot_inside: 

**作用**: 选中鼠标<font style="color:red">**悬停**</font>在元素上的<font style="color:red">**状态**</font>,设置样式

**选择器语法**: <font style="color:red">**选择器:hover{css}**</font> 

:warning:**注意点**:

-  伪类选择器选中的元素的<font style="color:red">**某种状态**</font> 

**代码**::game_die: 

```html
<style>
        a:hover {
            color:chartreuse;
            background-color:blueviolet;
        }
        div:hover {
            color:red;
        }
    </style>
</head>
<body>
    <a href="#">这是链接</a>
    <div>这是div</div>
</body>
```

**效果**:

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171045919.gif)

## <span alt="rainbow">Emmet语法</span>:ear_of_rice: 

:diamond_shape_with_a_dot_inside: 

**作用**: 通过简写语法,快速生成代码

**语法**:

-  类似于刚刚学习的选择器的写法

| 记忆       | 示例                | 效果                                   |
| ---------- | ------------------- | -------------------------------------- |
| 标签名     | div                 | `<div></div>`                          |
| 类选择器   | .red                | `<div class="red"></div>`              |
| id选择器   | #one                | `<div id="one"></div>`                 |
| 交集选择器 | p.red#one           | `<p class="red" id="one"></p>`         |
| 子代选择器 | ul>li               | `<ul><li></li></ul>`                   |
| 内部文本   | ul>li{我是li的内容} | `<ul><li>我是li的内容</li></ul>`       |
| 创建多个   | ul>li*3             | `<ul><li></li><li></li><li></li></ul>` |

不能直接生成id前面需要跟着class比如:

```html
.red#one
```

## 背景相关属性:panda_face: 

:diamond_shape_with_a_dot_inside: 

### 1.背景颜色:octopus: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**background-color**</span> (简写: bgc)

**属性值**:

-  颜色取值: <font title="red">关键字</font>,<font title="red">rgb表示法</font>,<font title="red">rgba表示法</font>,<font title="red">十六进制...</font>.

![image-20230504091922877](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171046058.png)

:warning:**注意点**:

1.  背景颜色默认值是**透明**:rgba(0,0,0,0) , transparent
2.  背景颜色不会影响盒子大小,并且还能看清盒子的大小和位置,一般在布局中会习惯先给盒子设置背景颜色

**代码**::game_die: 

```html
    <style>
        div {
            width: 300px;
            height: 300px;
            background-color:red;
            background-color: #ccc;
            /*红绿蓝 三原色,a表示透明度.5是0.5的简写*/
            background-color: rgba(0,0,0,.5);
        }
    </style>
</head>
<body>
   <div>div</div> 
</body>
```

**效果**:

![image-20230504092916331](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171047956.png)

### 2.背景图片:large_blue_diamond: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**background-image**</span> (简写: bgi)

**属性值**: `background-image: url('图片路径')` 

:warning:**注意点**:

1.  背景图片中url中可以省略引号
2.  背景图片默认是在水平和垂直方向平铺的
3.  ==背景图片仅仅是==指给盒子==起到装饰效果==,类似于背景颜色,==是不能撑开盒子的==。

**代码**: :game_die: 

```html
<style>
        div {
            width: 400px;
            height: 400px;
            background-image:url('./img/test_224x200.gif')
        }
    </style>
</head>
<body>
   <div>div</div> 
</body>
```

**效果**:

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171048187.gif)

**造成4个图片的原因是**: <span alt="wavy" style="color:red">图片为224x200尺寸,div盒子为400x400所以一行可以占2个一列也可以占两个都造成了4个图的情况</span>.

如果图片尺寸过大也就会只能展示一个不全的图片

**效果**: 图片尺寸为: 778x694

![image-20230504095130550](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171049332.png)

### 3.背景平铺:page_with_curl: 

**属性名**: <span alt="rainbow">**background-repeat**</span> (简写: bgr)

**属性值**:

| 取值                                     | 效果                          |
| ---------------------------------------- | ----------------------------- |
| <span alt="rainbow">**repeat**</span>    | (默认值) 水平和垂直方向都平铺 |
| <span alt="rainbow">**no-repeat**</span> | 不平铺                        |
| <span alt="rainbow">**repeat-x**</span>  | 沿着水平方向(x轴)平铺         |
| <span alt="rainbow">**repeat-y**</span>  | 沿着垂直方向(y轴)平铺         |

**代码**::game_die: 

#### no-repeat:o: 

```html
<style>
        div {
            width: 400px;
            height: 400px;
            background-color: pink;
            background-image:url('./img/test_224x200.gif');
            background-repeat: no-repeat;
        }
    </style>
</head>
<body>
   <div>div</div> 
</body>
```

**效果**:

![image-20230504095729643](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171050910.png)

#### repeat-x:o: 

```html
background-repeat: repeat-x;
```

**效果**:

![image-20230504095948918](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171050043.png)

#### repeat-y:o: 

```html
background-repeat: repeat-y;
```

**效果**:

![image-20230504100016397](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171050346.png)

### 4.背景位置:icecream: 

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**background-position** </span> (简写: gbp)

**属性值**: `background-position: 水平方向位置 垂直方向位置;` 

**属性值**:

1.  **方位名词**(最多只能表示9个位置)
    -  水平方向
       1.  left
       2.  center
       3.  right
    -  垂直方向
       1.  top
       2.  center
       3.  bottom
2.  **数字 + px**(坐标)
    -  坐标系: 原点(0,0) 盒子的左上角
       1.  x轴: 水平向右
       2.  y轴: 垂直向下
    -  操作:
       -  将图片左上角与坐标点重合即可

:warning:**注意点**:

-  ==方位名词取值和坐标取值可以混使用==,==第一个取值==表示==水平==,==第二个取值==表示==垂直==。

**代码**::game_die: 

```html
<style>
        div {
            width: 400px;
            height: 400px;
            background-color: pink;
            background-image:url('./img/test_224x200.gif');
            background-repeat: no-repeat;
            background-position: 85px 85px;
            background-position: right top;
            background-position: left bottom;
            /*两个center可以简写成一个*/
            background-position: center center;
            /*负数就会向反方向移动如果太大会移除背景可视范围*/
            background-position: -60px -100px;
            background-position: 290px 100px;
        }
    </style>
</head>
<body>
   <div>div</div> 
</body>
```

**效果**:

![image-20230504102533434](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171051611.png)

<span alt="wavy" style="color:red">**将方向名词的两个方向颠倒**</span>.

**结果**: 水平方向(x轴)没有top和bottom它会自动按照垂直方向(y轴)显示

如果颠倒的是数值的方式不建议这么干因为它是数据的不是方向固定的

```html
<style>
        div {
            width: 400px;
            height: 400px;
            background-color:pink;
            background-image:url('./img/test_224x200.gif');
            background-repeat: no-repeat;
            background-position: center bottom;
            /*水平方向(x轴)没有top和bottom它会自动按照垂直方向(y轴)显示*/
            background-position: bottom center;
        }
    </style>
</head>
<body>
   <div>123</div> 
</body>
```

**效果**:

![image-20230504112846468](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171053857.png)

### 5.背景大小

| 属性        | 说明     |
| ----------- | -------- |
| **conver**  | **铺满** |
| **contain** | **包含** |



### 6.背景相关属性连写:large_blue_diamond:

:diamond_shape_with_a_dot_inside: 

**属性名**: <span alt="rainbow">**background**</span> (bg)

**属性值**:

-  单个属性值的合写,取值之间以空格隔开

**书写顺序**:(顺序可以乱写,但是推荐这么写)

-  **推荐**: `background: color image repeat position` 

**省略问题**:

1.  可以按照需求省略
2.  **特殊情况**: 在PC端,如果盒子大小合背景图片大小一样,此时可以直接写`background:url('图片地址')` 

:warning:**注意点**:

1.  如果需要设置单独的样式和连写
2.  ①要么把单独的样式写在连写的下面,(<font style="color:red">防止覆盖样式</font>)
3.  ②要么把单独的样式写在连写的里面

```html
   <style>
        div {
            width: 400px;
            height: 400px;
            background: pink url('./img/test_224x200.gif') no-repeat center bottom;
            /*水平方向(x轴)没有top和bottom它会自动按照垂直方向(y轴)显示*/
            background: pink url('./img/test_224x200.gif') no-repeat bottom center;
        }
    </style>
</head>
<body>
   <div>123</div> 
</body>
```

**效果**:

![image-20230504113222498](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171054771.png)

## 扩展 img标签和背景图片的区别:first_quarter_moon_with_face: 

:diamond_shape_with_a_dot_inside: 

**需求**: 需要在网页中展示一张图片的效果

**方法一**: 直接写上img标签即可

-  img标签是一个标签,不设置宽高默认会以原尺寸显示

**方式二**: div标签 + 背景图片

-  **需要设置div的宽高**,<span alt="wavy" style="color:red">因为背景图片只是装饰的CSS样式,不能撑开div标签</span>.

**代码**: :game_die: 

```html
<style>
        div {
            width: 400px;
            height: 0;
            background: pink url('./img/test_224x200.gif') no-repeat center center;
        }
    </style>
</head>
<body>
   <div>123</div> 
</body>
```

**效果**:

![image-20230504114928982](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171055739.png)

<font style="color:red">重要的使用img</font>.

<font style="color:red">装饰性让网页变好看的用background</font>.

## 元素显示模式:ice_cream: 

:diamond_shape_with_a_dot_inside: 

### 1.块级元素:baby: 

:diamond_shape_with_a_dot_inside: 

**显示特点**:

1.  ==独占一行==(一行只能显示一个)
2.  <font style="color:red">宽度默认是父元素的宽度</font>,==高度默认由内容撑开==。
3.  ==可以设置宽高==。

![image-20230504144246797](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171058237.png)

**代表标签**:

-  **div**,**p**,**h系列**,ul,li,dl,dt,dd,form,header,nav,footer...

**代码**: :game_die: 

```html
<style>
        div {
            /*块: 独占一行;宽度默认是父级100%,添加宽高都生效*/
            width: 300px;
            height: 300px;
            background-color: pink;
        }
    </style>
</head>
<body>
   <div>123</div> 
   <hr>
   <div>321</div>
</body>
```

**效果**:

![image-20230504145309996](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171058752.png)

### 2.行内元素:rabbit2: 

:diamond_shape_with_a_dot_inside: 

**显示特点**:

1.  ==一行可以显示多个==。
2.  ==宽度和高度默认由内容撑开==。
3.  <span alt="wavy" style="color:red">不可以设置宽高</span>.(==不会生效== 等于 <span alt="shake">白写</span>)

**代表标签**:

-  **a**,**span**,b,u,i,s,strong,ins,em,del...

**代码**: :game_die: 

```html
<style>
        /*行内: 不换行,设置宽度和高度不生效, 尺寸和内容大小相同*/
        span {
           width: 300px;
           height: 300px; 
           background-color: pink;
        }
        .span {
            font-size: 20px;
        }
    </style>
</head>
<body>
   <span>span1</span> 
   <span class="span">span2</span>
</body>
```

**效果**:

![image-20230504151223885](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171100198.png)

### 3.行内块元素:sailboat: 

:diamond_shape_with_a_dot_inside: 

**显示特点**:

1.  ==一行可以显示多个==。
2.  ==可以设置宽高==。

**代表标签**:

-  **img**,**input**,**textarea**,button,select...
-  **特殊情况**: img标签有行内块元素特点,但是Chrome调式工具中显示结果是inline[行内元素]

![image-20230504151515965](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171101097.png)

**代码**: :game_die: 

```html
<style>
        /*行内块:一行显示多个,加宽加高生效*/
        img {
            width: 100px;
            height: 100px;
        }
    </style>
</head>
<body>
   <img src="./img/test_224x200.gif" alt="金知云"/> 
   <img src="./img/test_224x200.gif" alt="金知云"/> 
</body>
```

**效果**:

![image-20230504152149846](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171102630.png)

### 4.元素显示模式转换:zap: 

:diamond_shape_with_a_dot_inside: 

**目的**: 改变元素默认的显示特点,让元素符合布局要求

**语法**:

| 属性                                                | 效果             | 使用频率 |
| --------------------------------------------------- | ---------------- | -------- |
| **<span alt="rainbow">display:block</span>**        | 转换成块级元素   | 较多     |
| <span alt="rainbow">**display:inline-block**</span> | 转换成行内块元素 | 较多     |
| <span alt="rainbow">**displa:inline**</span>        | 转换成行内元素   | 较少     |

**代码**: :game_die: 

```html
    <style>
        span {
            width: 100px;
            height: 100px;
            background-color: pink;
            /*转换行内块: 宽高生效,不看内同大小*/
            display: inline-block;
            /*转换块级 换行,宽高生效,不看内容大小*/
            display: block;
            /*转换行内 无,看内容大小*/
            display: inline;
        }
        .span {
            font-size: 130px;
        }
    </style>
</head>
<body>
    <span>123</span>
    <span class="span">321</span>
</body>
```

**效果**:

<span alt="rainbow">**inline-block**</span>.

![image-20230504153929037](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171103485.png)

<span alt="rainbow">**block**</span>.

![image-20230504154006106](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171104674.png)

<span alt="rainbow">**inline**</span>.

![image-20230504154111301](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171104250.png)

## 扩展: HTML嵌套规范注意点:small_red_triangle: 

:diamond_shape_with_a_dot_inside: 

1.**块级元素一般作为大容器,可以嵌套: 文本,块级元素,行内元素,行内块元素等等...**.

-  **但是**: <span alt="wavy" style="color:red">p标签中不要嵌套div,p,h等块级元素</span>.

不要在==小的盒子套大的盒子==不现实,<font style="color:red">p是独占一行的h也是浏览器会以为上面的p少了结尾下面的p少了开始就自动补上了</font>。

2.**a标签内部可以嵌套任意元素**.

-  **但是**: <span alt="wavy" style="color:red">a标签不能嵌套a标签</span>.

a标签里面可以是一个div或者其它点击后跳转，但是不能是自己,浏览器会解析出两个a分开的但是点击a嵌套了a的字样它会之确定跳转到哪里

**代码**: :game_die: 

```html
<body>
   <!--p和h标签不能相互嵌套--> 
   <p>
    这是p
    <h1>这是h1</h1>
   </p>
   <!--p里面不能包含div-->
   <p>
    这是p
    <div>这是div</div>
   </p>
   <a href="#">外层a<a href="#">内层a</a></a>
</body>
```

**效果**:

![image-20230504160432792](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171106592.png)

## CSS三大特性:factory: 

:diamond_shape_with_a_dot_inside: 

### 继承性:unicorn: 

:diamond_shape_with_a_dot_inside: 

#### 继承性的介绍:kaaba: 

**特性**: 子元素有默认继承父元素样式的特点

**可以继承的常见属性**(<font style="color:red">**文字控制属性都可以继承**</font>)

1.  color
2.  font-style,font-weight,font-size,font-family
3.  text-indent,text-align
4.  line-height
5.  ...

:warning:**注意点**:

-  可以通过调式工具判断样式是否可以继承

**代码**: :game_die: 

```html
    <style>
        /*控制文字的属性都能继承;不是控制文字的属性都不能继承*/
        div {
            /*能继承*/
            color: pink;
            font: italic bold 12px/1.5 YaHei;
            text-indent: 2em;
            /*不能继承*/
            height: 500px;
        }
    </style>
</head>
<body>
   <div>
    这是div
    <span>这是div中的span</span>
    <!--a标签继承color会失效,需要单独设置-->
   <a href="#">这是链接</a>
   <!--h1~6不能完美继承font-size-->
   <h1>这是标题1</h1>
   </div> 
</body>
```

**效果**:

![image-20230504163841831](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171107467.png)

在京东中打开控制台可以看到在body中设置了字体的样式大小和颜色,==继承性的其它标签也会继承自body的字体属性值,这样就减少了很多代码==。

#### 扩展 继承失效的特殊情况:o: 

:diamond_shape_with_a_dot_inside: 

**如果元素有浏览器默认样式,此时继承性依然存在,但是优先显示浏览器的默认样式**.

1.  a标签的color会继承失效
2.  h系列标签的font-size会继承失效

**代码**: :game_die: 

```html
<style>
        /*控制文字的属性都能继承;不是控制文字的属性都不能继承*/
        div {
            /*能继承*/
            color: pink;
            font: italic bold 12px/1.5 YaHei;
            text-indent: 2em;
            /*不能继承*/
            height: 500px;
        }
    </style>
</head>
<body>
   <div>
    这是div
    <span>这是div中的span</span>
    <!--a标签继承color会失效,需要单独设置-->
   <a href="#">这是链接</a>
   <!--h1~6不能完美继承font-size-->
   <h1>这是标题1</h1>
   </div> 
</body>
```

**效果**:

![image-20230504174316959](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171108190.png)

### 层叠性:unamused: 

:diamond_shape_with_a_dot_inside: 

#### 层叠属性的介绍:japanese_castle: 

:diamond_shape_with_a_dot_inside: 

**特性**:

1.  给同一个标签设置==不同的样式== → 此时样式会==层叠叠加== → 会==共同作用在标签上==。
2.  给同一个标签设置==相同的样式== → 此时样式会==层叠覆盖== → 最终写在==最后的样式会失效==。

:warning:**注意点**:

-  当==样式冲突时==,只有当选==择器优先级相同时==,才能==通过层叠性判断结果==。

**代码**::game_die: 

```html
<style>
        p {
            color:pink;
        }
        .p {
            font-size: 20px;
        }
    </style>
</head>
<body>
   <p class="p">这是p</p> 
</body>
```

**效果**:

![image-20230504174645450](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171110534.png)

### 优先级:yellow_heart: 

:diamond_shape_with_a_dot_inside: 

#### 优先级的介绍:cactus: 

:diamond_shape_with_a_dot_inside: 

**特性**: 不同选择器具有不同的优先级,优先级高的选择器样式会覆盖优先级低的选择器样式

**优先级公式**:

-  <span alt="rainbow">**继承**</span> <span alt="blink">**<**</span> <span alt="rainbow">**通配符选择器**</span> <span alt="blink">**<**</span> <span alt="rainbow">**标签选择器**</span> <span alt="blink">**<**</span> <span alt="rainbow">**类选择器**</span> <span alt="blink">**<**</span> <span alt="rainbow">**id选择器**</span> <span alt="blink">**<**</span> <span alt="rainbow">**行内样式^直接写在标签里的^**</span> <span alt="blink">**<**</span> <span alt="rainbow">**!important**</span>.
-  **秒记法则**(谁最精准谁的优先级最高)

:warning:**注意点**:

1.  !important写在==属性值的后面,分号的前面==!
2.  !important==不能提升==<strong style="color:red">继承</strong>==的优先级==,<font style="color:red">**只要是继承优先级就是最低**</font>!
3.  <font style="color:red">实际开发中不建议使用 !important</font>。
4.  <span alt="wavy" style="color:red">!important不要给继承的添加,自己有样式无法继承父级样式</span>.

**代码**: :game_die: 

```html
    <style>
        #box {
            color:orange;
        }
        .box {
            color:blue !important;
        }
        /*优先级更高的使用!important当前的就会被覆盖了*/
        div {
            color:green /*!important*/;
        }
        /*继承使用!important不生效*/
        body {
            color:red /*!important*/;
        }
       /*!important不要给继承的添加,自己有样式无法继承父级样式*/
    </style>
</head>
<body>
    <!--意义: 当一个标签使用了多个选择器,样式冲突的时候,到底谁生效-->
   <div class="box" id="box" style="color:pink">测试优先级</div> 
</body>
```

**效果**:

![image-20230504191833298](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171113531.png)

#### 权重叠加计算:japan: 

:diamond_shape_with_a_dot_inside: 

**场景**: 如果是==复合选择器==,此时需要通过==权重叠加计算方法==,==判断最终哪个选择器优先级最高会生效==。

**权重叠加计算公式**: (<span alt="wavy" style="color:red">每一级之间不存在进位</span>)

![image-20230504192510181](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171114391.png)

**比较规则**:

1.  先比较第一级数字,如果比较出来了,之后的统统不看
2.  如果第一级数字相同,此时再去比较第二级数组,如果比较出来了,之后的统统不看
3.   ...
4.  ==如果==最终所有==数字都相同==,表示==优先级相同==,则==比较层叠性==(**谁写在下面,谁说了算**!)

:warning:**注意点**:

-  <span alt="wavy" style="color:red">!important如果不是继承,则权重最高,天下第一!</span>.

**代码**: :game_die: 

```html
<style>
        /* (行内,id,类,标签) */
        /* (0,1,0,1) */
        div #one {
            color:orange;
        }
        /* (0,0,2,0) */
        .father .son {
            color:skyblue;
        }
        /* (0,0,1,1) */
        .father p {
            color:purple;
        }
        /* (0,0,0,2) */
        div p {
            color:pink;
        }
    </style>
</head>
<body>
   <div class="father">
    <p class="son" id="one">我是一个标签</p>
   </div> 
</body>
```

**效果**:

![image-20230504194216831](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171117942.png)

## PxCook的基本使用:rabbit: 

:diamond_shape_with_a_dot_inside: 

打开创建一个web项目

将图片拖进去后双击开打

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171117920.png" alt="image-20230509113952030" style="zoom:33%;" />

图片使用设计模式

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171117162.png" alt="image-20230509114047482" style="zoom:33%;" />

alt + 鼠标滚轮可以缩放 ， 按住空格 + 鼠标左键 可以移动 ， 使用尺子可以测量距离

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171117759.png" alt="image-20230509114303620" style="zoom:33%;" />

## 盒子模型:lantern: 

:diamond_shape_with_a_dot_inside: 

### 1.盒子模型的介绍:jack_o_lantern: 

:diamond_shape_with_a_dot_inside: 

1.  **盒子的概念**.
    1.  页面中的每一个标签，都可看做是一个<font style="color:red">"盒子"</font>,通过盒子的视角更方便的进行布局
    2.  浏览器在渲染(显示)网页时，会将网页中的元素看做是一个个的矩形区域，我们也形象的称之为<font  style="color:red">"盒子"</font>.
2.  **盒子模型**.
    -  CSS中规定每个盒子分别由：<font style="color:red">内容区域(content)</font>,<font style="color:red">内边距区域(padding)</font>,<font style="color:red">边框区域(border)</font>,<font style="color:red">外边距区域(margin)</font>构成，这就是 <font style="color:red">盒子模型</font>.
3.  **记忆**：可以联想现实中的包装盒
    -  在送物品的时候必须有盒子里面有防磕碰泡沫，然后就是电脑，在盒子模型中也是具备的
4.  <span alt="wavy" style="color:red">**padding和border都会撑大盒子模型**</span>.

![image-20230509122854553](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171118248.png)

**代码**：:game_die: 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        /*纸箱子,填充泡沫*/
        div {
            width: 300px;
            height: 300px;
            background-color: pink;
            /*边框线 == 纸箱子*/
            border: 1px #000 solid;
            /*内边距 == 填充泡沫:出现在内容和盒子边缘之间*/
            padding: 25px;
            /*外边距:出现在两个盒子之间,出现在盒子的外面*/
            margin: 25px;
        }
    </style>
</head>
<body>
    <div>内容=-=电脑</div>
    <div>内容=-=电脑</div>
</body>
</html>
```

![image-20230509125038586](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171120836.png)

### 2.内容区域的宽度和高度:package: 

:diamond_shape_with_a_dot_inside: 

**作用**：利用<font style="color:red">width</font>和<font style="color:red">height</font>属性默认设置是盒子<font style="color:red">内容区域</font>的大小

**属性**：width / height

**常见取值**：数字 + px

![image-20230509125322998](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171120878.png)

![image-20230509144055249](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171120537.png)

### 3.边框(border)-连写形式:baby_bottle: 

:diamond_shape_with_a_dot_inside: 

**属性名**：border

**属性值**：单个取值的连写，取值之间以==空格隔开==。

-  **如**：border: 10px red solid
-  顺序任意
-  除了solid还有其它的边框线的样式可查看[样式](./border.md).

<strong style="color:red">会撑大盒子模型</strong>。

#### 边框(border)-单方向设置:ice_cream: 

:diamond_shape_with_a_dot_inside: 

**场景**：只给盒子的某个方向单独设置边框

**属性名**：border - 方位名词

-  上: top 下:bottom 左:left 右:right

**属性值**：连写的取值

**代码**：:game_die: 

```html
    <style>
        div {
            width: 300px;
            height: 300px;
            background-color: pink;
            border-top: 5px #000 dotted;
            border-bottom: 5px #000 double;
        }
    </style>
</head>
<body>
    <div>内容=-=电脑</div>    
</body>
```

**效果**：

![image-20230509130954704](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171124912.png)

#### 边框(border) - 单个属性:dagger: 了解就行 工作没必要

:diamond_shape_with_a_dot_inside: 

**作用**：给设置边框粗细，边框样式，边框颜色效果

**单个属性**：

| 作用     | 属性名       | 属性值                             |
| -------- | ------------ | ---------------------------------- |
| 边框粗细 | border-width | 数字 + px                          |
| 边框样式 | border-style | 实现 dolid,虚线 dashed,点线 dotted |
| 边框颜色 | border-color | 颜色取值                           |

**代码**：:game_die: 

```html
    <style>
        div {
            width: 300px;
            height: 300px;
            background-color: pink;
            border-width:5px;
            border-style:solid;
            border-color:#000;
        }
    </style>
</head>
<body>
    <div>内容=-=电脑</div>    
</body>
```

**效果**：

![image-20230509131448561](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171124288.png)

#### 父元素设置border-radius时影响子元素圆角

在父元素中使用标签：

```
overflow:hidden;
```

#### 案例-盒子边框的小案例:anchor: 

:diamond_shape_with_a_dot_inside: 

**需求**：根据设计图，通过PxCook量取数据，通过代码在网页中完成一致的效果 

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171124279.png" alt="image-20230509150600215" style="zoom: 67%;" />

**代码**：:game_die: 

```html
    <style>
        body {
            margin: 0;
            padding: 0;
        }
        div {
            width: 280px;
            height: 280px;
            border: 7px #3f48cc solid;
            background-color: #ffaec9;
        }
    </style>
</head>
<body>
   <div></div> 
</body>
```

**效果**：

![image-20230509150715717](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171124348.png)

![image-20230509152016625](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171124991.png)

#### 新浪导航案例:angel: 

:diamond_shape_with_a_dot_inside: 

需求：根据设计图，通过PxCook量取数据，通过代码在网页中完成一致的效果

![image-20230509152127880](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171124769.png)

遵循 布局顺序：

1.  从外往内，从上往下

每一个盒子的样式：

1.  宽高
2.  辅助的背景颜色
3.  盒子模型的部分：border,padding,margin
4.  其它样式：color,font-,text-,...

####     border-collapse: collapse

用于决定==表格的边框==是==分开==还是==合并==的

#### border-spacing

**适用元素**：==table==和==inline-block==行内块元素

**是否是继承属性**：是

`border-spacing` 属性指定相邻单元格边框之间的距离（只适用于 [边框分离模式](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-collapse) ）。相当于 HTML 中的 `cellspacing` 属性，但是第二个可选的值可以用来设置不同于水平间距的垂直间距。

`border-spacing` 值也适用于表格的外层边框上，即表格的边框和第一行的、第一列的、最后一行的、最后一列的单元格之间的间距是由表格相应的（水平的或垂直的）边框间距（border-spacing）和相应的（上，右，下或左）内边距之和。

**语法**：

```css
/* <length> */
border-spacing: 2px;

/* horizontal <length> | vertical <length> */
border-spacing: 1cm 2em;

/* Global values */
border-spacing: inherit;
border-spacing: initial;
border-spacing: unset;
```

**值**：

***length*** 

描述单元格之间的水平和垂直距离的一个 `<length>` 值。它只在单值语法下使用。

***horizontal*** 

描述相邻两列的单元格之间的水平距离的一个 `<length>`  值。它只在双值语法下使用。

***vertical*** 

描述相邻两行的单元格之间的垂直距离的一个 `<length>`  值。它只在双值语法下使用。

**inherit** 

一个表示父元素的 `border-spacing` 的计算值的关键字，其父元素必须应用了 `border-spacing` 。

**正式语法**：

```css
table {
  border-spacing: 10px 5px;
}
```

### 4.内边距(padding):pager:

:diamond_shape_with_a_dot_inside: 

<strong style="color:red">padding的大小会撑开盒子的大小</strong>。

==可以复合使用==,**单值**: 上下左右 , **两值**: 上下 , 左右 , **三值**: 上 , 左右 , 下 , **四值**: 四边对应

**使用方向位名词**: <span alt="rainbow">**padding-top**</span>:上 , <span alt="rainbow">**padding-bottom**</span>:下 , <span alt="rainbow">**padding-left**</span>:左 , <span alt="rainbow">**padding-right**</span>:右

#### 四值:ocean: 

![image-20230511194923316](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171126850.png)

#### 三值:oden: 

![image-20230511195031993](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171126817.png)

#### 二值:octopus: 

![image-20230511195050889](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171126999.png)

#### 单值:oil_drum: 

![image-20230511195108997](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171126713.png)

#### CSS3盒子模型(自动內减)box-sizing:border-box:cactus: 

:diamond_shape_with_a_dot_inside: 

<strong style="color:red">CSS2.0之后添加的属性</strong>。

**需求**: 盒子尺寸300*300,背景颜色,边框10px实线黑色,上下左右20px的内边距

-  给盒子设置border或padding时,盒子会被撑大,如果不想盒子被撑大?

**解决方法**: ① : 手动內减

-  操作: 自己计算多余大小,手动在内容中减去
-  缺点: 项目中计算量太大,很麻烦

**解决方法**: ② : 自动內减

-  操作: 给盒子设置属性box-sizing:border-box;即可
-  优点: 浏览器会自动计算多余大小,自动在内容中减去

**建议**: <span alt="wavy" style="color:green">将box-sizing:border-box写到 *{} 通配符选择器中外边距,内边距,自动內减一并设置</span>.

**代码**:

```html
<style>
        div {
            width: 300px;
            height: 300px;
            background-color: pink;
            border: 10px #000 solid;
            padding: 20px;
            /* 将会减掉宽高的大小让盒子的最终大小为300px,300px */
            box-sizing:border-box;
        }
    </style>
</head>
<body>
    <div>文字</div>
</body>
```

**效果**:

![image-20230511201427265](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171127672.png)

![image-20230511201411110](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171127442.png)

### 5.外边距(margin):mahjong: 

**控制平级的盒子与盒子之间的间距** 

可以复合使用和padding的解析方式一样,**margin**: 上 , 右 , 下 , 左

**使用方向位名词**: <span alt="rainbow">**margin-top**</span>: 上 , <span alt="rainbow">**margin-bottom**</span>: 下 , <span alt="rainbow">**margin-left**</span>: 左 , <span alt="rainbow">**margin-right**</span>: 右

**代码**:

```html
<style>
        body {
            padding:0;
            margin:0;
        }
        div {
            width: 200px;
            height: 200px;
            background-color:pink;
            /* margin:上 左 下 右 */
            /* margin: 20px 20px 30px 50px; */
            /* margin-left:10px; */
            /* margin-right:10px; */
            /* margin-bottom:10px; */
            margin-top:10px;
        }
    </style>
</head>
<body>
   <div>内容</div> 
</body>
```

#### 清除默认内外边距:rabbit: 

:diamond_shape_with_a_dot_inside: 

**场景**: 浏览器会默认给部分标签设置默认的margin和padding,但一般在项目开始前需要先清除这些标签默认的margin和padding,后续自己设置

1.  **比如**: body标签默认有 margin: 8px;
2.  **比如**: p标签默认有 上,下的margin
3.  **比如**: ul标签默认由 上,下的margin和padding-left
4.  ...

**解决方法**:

![image-20230511204702464](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171128675.png)

#### 外边距折叠现象-① 合并现象:o: 

**场景**: <font style="color:red">**垂直布局**</font> 的 <font style="color:red">**块级元素**</font> , 上下的margin会合并

**结果**: 最终两者距离为margin的最大值

**解决方法**: 避免就好

-  只给其中一个盒子设置margin即可

代码:

```html
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        div {
            width: 100px;
            height: 100px;
            background-color:pink;
        }
        .one {
            /* margin-bottom: 50px; */
            /* 浏览器会默认选择最大的 ,两个div之间的距离还是60px */
            margin-bottom: 60px;
        }
        .two {
            margin-top: 50px;
        }
        /* 结果两个margin合并,两个div之间的距离还是50px */
    </style>
</head>
<body>
   <div class="one">11</div> 
   <div class="two">22</div>
</body>
```

**效果**:

![image-20230514213602553](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171130774.png)

![image-20230514213451675](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171130158.png)

#### 外边距折叠现象-② 塌陷现象:taco: 

**场景**: <font style="color:red">**互相嵌套**</font> 的 <font style="color:red">**块级元素**</font> , 子元素的<font style="color:red">**margin-top**</font>会作用在父元素上

**结果**: 导致父元素一起往下移动

**解决方法**:

1.  给父元素设置border-top或者padding-top(分隔父子元素的margin-top)
2.  给父元素设置overflow: hidden
3.  转换成行内块元素
4.  设置浮动

**问题**: 定义了两个父子级的div , 父为pink色的 , 子为blue色的,让子div向下移动一点

![image-20230514214919508](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171131210.png)

**思路误区**: 在子div中加上margin-top:50px; 但是结果如下: 

![image-20230514215238221](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171131716.png)

子div中设置的margin-top:50px却连带父div也生效了(坑爹) , 这种现象就是外边距折叠-塌陷现象

#### 行内元素的垂直内外边距:ice_cream: 

**场景**: 如果想要通过margin或padding改变==行内元素==标签的==上下位置==,<strong style="color:red">无法生效</strong>。

**代码**:

```html
<style>
        span {
            /* 左右:生效 , 上下:不生效 */
            /* margin: 100px; */
            /* 左右:生效 , 上下:不生效 */
            padding: 100px;
        }
    </style>
</head>
<body>
    <!-- 如果想要通过margin或padding改变行内标签的上下位置,无法生效 -->
    <!-- 行内标签的margin-top和bottom   不生效 -->
    <!-- 行内标签的padidng-top或bottom  不生效 -->
   <span>span1</span> 
   <span>span2</span>
</body>
```

**结果**:

![image-20230514221438677](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171134708.png)

**解决方法**: ==使用行高 line-height== , 或者==转换显示模式 display: inline-block==。

### 版心居中:lantern: 

:diamond_shape_with_a_dot_inside: 

版心指网页的有效内容

**代码**:

```html
<style>
        * {
            padding:0;
            margin:0;
        }
        div {
            width: 1000px;
            height: 200px;
            background-color:pink;
            /* 上下为0 左右自动统一也就自动(拉伸也)居中了 */
            margin: 0 auto;
        }
    </style>
</head>
<body>
   <div>内容</div> 
</body>
```

**效果**: 

![image-20230511205813810](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171135938.png)

## 消除ul中li前面的原点:o: 

**属性名**: list-style

**属性值**: none

```css
.class {
   list-style:none
}
```

## 结构伪类选择器:weight_lifting_man: 

**目标**: 能够使用<font style="color:red">**结构伪类选择器**</font>在HTML中定位元素

1.**作用与优势**:

1.  **作用**: 根据元素在HTML中的结构关系查找元素
2.  **优势**: 减少对于HTML中类的依赖,有利于保持代码整洁
3.  **场景**: 常用于查找父级选择器中的子元素

2.**选择器**:

| 选择器                  | 说明                                    |
| ----------------------- | --------------------------------------- |
| E:first-child { }       | 匹配父元素中第一个子元素,并且是E元素    |
| E:last-child { }        | 匹配父元素中最后一个子元素,并且是E元素  |
| E:nth-child(n) { }      | 匹配父元素中第n个子元素,并且是E元素     |
| E:nth-last-child(n) { } | 匹配父元素中倒数第n个子元素,并且是E元素 |
| E:first-of-type         | 指定类型E的第一个                       |
| E:last-of-type          | 指定类型E的最后一个                     |
| E:nth-of-type(n)        | 指定类型E的第ng                         |

### nth-child(n)与nth-of-type(n)的区别

#### nth-child(n)

<strong style="color:red">nth-child(n)注意事项</strong>：计算当前选择器的标签下的所有序列号，更详细看如下代码演示：

```css
<style>
   /*nth-child 会把所有的盒子都排列序号
   执行的时候首先看:nth-child(1)之后回去看 前面div
   所以在div中熊大应该是排在第2列的*/
   section div:nth-child(2) {
   	background-color: yellow;
   }
</style>
<section>
   <p>光头强</p>
   <div>熊大</div>
   <div>熊二</div>
</section>
```

效果：

![image-20230719100501571](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307191005810.png)

#### nth-of-type(n)

按照选择标签中指定的类型进行排列序号

```css
<style>
/*nth-of-type 会把指定元素的盒子排列序号
执行的时候首先看 div指定的元素 之后回去看:nth-of-type(1) 第几个孩子*/
section div:nth-of-type(1) {
   background-color: blue;
}
</style>
<section>
<p>光头强</p>
<div>熊大</div>
<div>熊二</div>
</section>
```

效果：

![image-20230719101030850](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307200903271.png)

**n的注意点**：如果小括号填写公式n从0开始，如果是数字从1开始

1.  n为：0,1,2,3,4,5,6,...
2.  通过n可以组成常见公式

| 功能            | 公式                         |
| --------------- | ---------------------------- |
| 偶数            | 2n，even，(2的倍数)          |
| 奇数            | 2n+1，2n-1，odd，(2的倍数+1) |
| 找到前5个       | -n+5                         |
| 找到从第5个往后 | n+5                          |

**代码**：

```html
<style>
   /*选中第一个*/
   li:first-child{
      background-color:green;
   }
   /*选中最后一个*/
   li:last-child{
      background-color:green;
   }
   /*选中第5个*/
   li:nth-child(5){
      background-color:green;
   }
   /*选中倒数第3个*/
   li:nth-last-child(3){
      background-color:green;
   }
</style>
</head>
<body>
   <ul>
      <li>第1个li</li>
      <li>第2个li</li>
      <li>第3个li</li>
      <li>第4个li</li>
      <li>第5个li</li>
      <li>第6个li</li>
      <li>第7个li</li>
      <li>第8个li</li>
   </ul> 
</body>
```

效果：

![image-20230528103455667](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171135426.png)

```html
<style>
   /*偶数选li*/
   li:nth-child(even){
      background-color:green;
   }
   /*偶数选li*/
   li:nth-child(2n){
      background-color:green;
   }
</style>
```

**效果**：

![image-20230528104814607](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171136668.png)

```html
<style>
   /*奇数选li*/
   li:nth-child(odd){
      background-color:green;
   }
   /*奇数选li*/
   li:nth-child(2n-1){
      background-color:green;
   }
   /*奇数选li*/
   li:nth-child(2n-1){
      background-color:green;
   }
</style>
```

**效果**：

![image-20230528105025145](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171136449.png)

```html
<style>
   /*找到前5个*/
   li:nth-child(-n+5){
      background-color:green;
   }
</style>
```

**效果**：

![image-20230528105131120](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171136742.png)

```html
<style>
   /*找到后5个*/
   li:nth-child(n+5){
      background-color:green;
   }
</style>
```

**效果**：

![image-20230528105222330](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171136526.png)

### input:checked

checked都可以在被选中的状态进行一个样式的改变

代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0,
    minimun-scale=1.0,maximun-scale=1.0,user-scalable=no">
    <title>Document</title>
    <link rel="stylesheet" href="./css/选中状态样式.css"/>
</head>
<body>
   <input type="checkbox"/> 
</body>
</html>
```

```less
input {
    &:checked {
        width: 200px;
        height: 200px;
    }
}
```

效果：

![test](./HTML5+CSS3.assets/202307302020351.gif)

### 伪元素:weight_lifting_woman:

**伪元素**：一般页面中的==非主体内容可以使用伪元素==。

**区别**：

1.  元素：HTML设置的标签
2.  伪元素：由CSS模拟出的标签效果

**种类**：

| 伪元素   | 作用                             |
| -------- | -------------------------------- |
| ::before | 在父元素内容的最前添加一个伪元素 |
| ::after  | 在父元素内容的最后添加一个伪元素 |

**注意点**：

1.  <strong style="color:red">必须设置content属性才能生效</strong>。
2.  伪元素默认是==行内元素==。

**代码**：

```html
<style>
   .fu{
      width: 300px;
      height: 300px;
      background-color:rgb(225, 61, 88);
   }
   .fu::before{
      /*添加内容,否则伪元素不生效*/
      content: '老鼠';
      background-color:blue;
      width: 100px;
      height: 100px;
      color:aqua;
      /*伪元素默认为行内显示要设置宽高需要转换为块级元素*/
      display: block;
   }
   .fu::after{
      content: '大米';
      color:rgb(255, 204, 0);
   }
</style>
</head>
<body>
   <div class="fu">
      爱
   </div> 
</body>
```

**效果**：

![image-20230528111625195](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171137699.png)

**控制台查看被解析的代码**：

![image-20230528111712896](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171138699.png)

#### 伪元素选择器使用场景1：伪元素字体图标

需求如下：右侧的字体图标随意更换 能达到效果就行。

![image-20230719102930529](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307191029090.png)

代码：

```html
    <style>
            @font-face {
            font-family: 'icomoon';
            src:  url('fonts/icomoon.eot?nzdx0k');
            src:  url('fonts/icomoon.eot?nzdx0k#iefix') format('embedded-opentype'),
                url('fonts/icomoon.ttf?nzdx0k') format('truetype'),
                url('fonts/icomoon.woff?nzdx0k') format('woff'),
                url('fonts/icomoon.svg?nzdx0k#icomoon') format('svg');
            font-weight: normal;
            font-style: normal;
            font-display: block;
            }
        div {
            position: relative;
            width: 200px;
            height: 35px;
            border: 1px red solid;
        }

        div::after {
            position: absolute;
            top: 10px;
            right: 10px;
            /* content: ''; */
            content: '\e916';
            font-family: 'icomoon';
        }
    </style>
</head>
<body>
   <div></div> 
</body>
```

效果：

![image-20230719103020719](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307191030167.png)

### 标准流:bicyclist: 

**标准流**：又称<font style="color:red">文档流</font>，是浏览器在渲染显示在网页内容时默认采用的一套排版规则，规定了应该以何种方式排列元素

**常见标准流排版规则**：

1.  **块级元素**：从上往下，<font style="color:red">重置布局</font>，独占一行
2.  **行内元素或行内块元素**：从左往右，<font style="color:red">水平布局</font>，空间不够自动折行

### 浮动:fuelpump: 

先看不使用浮动让两个div一行显示的状况：
**特点**：==只会影响浮动后面的元素，不会影响浮动前面的元素==。

<strong style="color:red">注意</strong>：

-  ==块元素==使用<font color=red>浮动</font>，默认==转换==为==行内块元素==.
-  ==行内元素==使用<font color=red>浮动</font>，默认==转换==为==行内块元素==.
   -  浮动转换后的<font color=red>**行内块^display:inline-block也一样^**</font>元素==不会带有默认继承父盒子宽度的特性==，但是<font color=red>**display:block**</font>转换还是==带有继承父盒子宽度的特性的==。
-  使用<font color=red>**浮动**</font>的元素会==脱标==，==不占==有原来==标准流的位置==。

关于这一特点看如下代码：

**验证**：只会==影响浮动后面的元素==，不会影响浮动前面的元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .left {
            float: left;
            width: 100px;
            height: 100px;
            background-color: aqua;
        }
        .right {
            float: right;
            width: 100px;
            height: 100px;
            background-color: aqua;
        }
    </style>
</head>
<body>
    <div>
        <div class="left"></div>
        <div class="right"></div>
        <img src="../imgage/瑞克.png" width="100%" height="500px"/>
    </div>
</body>
</html>
```

结果：

![image-20230718075845677](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307180758670.png)

代码2:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .left {
            float: left;
            width: 100px;
            height: 100px;
            background-color: aqua;
        }
        .right {
            float: right;
            width: 100px;
            height: 100px;
            background-color: aqua;
        }
    </style>
</head>
<body>
    <div>
        <img src="../imgage/瑞克.png" width="100%" height="500px"/>
        <div class="left"></div>
        <div class="right"></div>
    </div>
</body>
</html>
```

结果：

![image-20230718075930490](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307180759987.png)

这就是，前面的元素 不影响但是后面的元素影响

代码：

```html
<style>
   div {
      width: 300px;
      height: 300px;
   }
   .one {
      display: inline-block;
      background-color:pink;
   }
   .two {
      display: inline-block;
      background-color:blue;
   }
</style>
</head>
<body>
   <div class="one">one</div> 
   <div class="two">two</div>
</body>
```

**效果**：

![image-20230528113603700](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171139735.png)

中间会有一个空格隔开，这是因为代码中div标签换行导致的比如如下的解决方式：

```html
<style>
   div {
      width: 300px;
      height: 300px;
   }
   .one {
      display: inline-block;
      background-color:pink;
   }
   .two {
      display: inline-block;
      background-color:blue;
   }
</style>
</head>
<body>
   <div class="one">one</div><div class="two">two</div>
</body>
```

**效果**：

![image-20230528113714809](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171140099.png)

虽然问题解决了，但是这样写在一行的代码风格肯定是不行的。我们就可以使用浮动来解决这种局部问题。

#### 1.浮动的作用:ice_cream: 

**早期的作用**：图文环绕

![image-20230528113847181](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171140465.png)

**现在的作用**：网页布局 (块在一行显示)

**浮动**：<font style="color:red">在一行排列，宽高生效，浮动后的标签具备行内块的特点，但不是行内块因为行内块是有空格的</font>。

#### 2.浮动的代码:ocean: 

**早期用法**：

```html
<style>
   img {
      float: left;
   }
</style>
</head>
<body>
   <img src="./img/星.gif"/ width="500px">
   <p>星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
      星星星星星星星星星星星星星星星星星星星星星星星
   </p>
</body>
```

**效果**：

![image-20230528115251366](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171141825.png)

**现在用法**：

```html
<style>
   div{
      width: 300px;
      height: 300px;
   }
   .one {
      background-color:pink;
      float:left;
   }
   .two {
      background-color:blue;
      float:left;
   }
</style>
</head>
<body>
   <div class="one">one</div>
   <div class="two">two</div>
</body>
```

**效果**：

![image-20230528115717994](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171141050.png)

#### 3.浮动的特点:yellow_heart: 

1.  浮动元素会==脱离标准流==(简称：==脱标==)，在==标准流中不占位置==。

    -  **相当于从地面飘到了空中**。

    **代码**：

    ```html
    <style>
       .one {
          width: 100px;
          height: 100px;
          background-color:pink;
       }
       .two {
          width: 200px;
          height: 200px;
          background-color:blue;
       }
       .three {
          width: 300px;
          height: 300px;
          background-color: orange;
       }
    </style>
    </head>
    <body>
       <div class="one">one</div>
       <div class="two">two</div>
       <div class="three">three</div>
    </body>
    ```

    **效果**：

    ![image-20230528120739581](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171142142.png)

2.  ==浮动元素比标准流高半个级别，可以覆盖标准流中的元素==。

    **将one 左浮动**：

    ```html
    .one {
       width: 100px;
       height: 100px;
       background-color:pink;
       float:left;
    }
    ```

    **效果**：浮动元素在标准流中==不占位置==，==two会占用one的位置但是内容不会占用==。

    ![image-20230528120843473](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171143882.png)

3.  ==浮动找浮动==，==下一个浮动元素==会在==上一个浮动元素后面左右浮动==。

    **代码**：

    ```html
    <style>
       .one {
          width: 100px;
          height: 100px;
          background-color:pink;
          float:left;
       }
       .two {
          width: 200px;
          height: 200px;
          background-color:blue;
          float:left;
       }
       .three {
          width: 300px;
          height: 300px;
          background-color: orange;
       }
    </style>
    </head>
    <body>
       <div class="one">one</div>
       <div class="two">two</div>
       <div class="three">three</div>
    </body>
    ```

    **效果**：

    ![image-20230528121539037](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171144747.png)

4.  **浮动元素有特殊的显示效果** 

    -  ==一行可以显示多个==。
    -  ==可以设置宽高==。

-  **注意点**：

   -  <strong style="color:red">浮动的元素不能通过tet-align:center或者maring:0 auto居中</strong>。

   **代码**：

   ```html
   <style>
      .one {
         width: 100px;
         height: 100px;
         background-color:pink;
         float:left;
      }
      .two {
         width: 200px;
         height: 200px;
         background-color:blue;
         float:left;
         /*浮动级别高，不能生效，盒子无法水平居中*/
         text-align:center;
         margin:0 auto;
      }
      .three {
         width: 300px;
         height: 300px;
         background-color: orange;
      }
   </style>
   </head>
   <body>
      <div class="one">one</div>
      <div class="two">two</div>
      <div class="three">three</div>
   </body>
   ```

   **效果**：

   ![image-20230528122252133](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171145998.png)

#### 4.浮动的案例:cake: 

**需求**：使用浮动，完成设计图中局部效果

![image-20230528122557327](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171145679.png)

```html
<style>
   body{
      margin: 0;
      padding: 0;
   }
   .top{
      width: 100%;
      height: 30px;
      background-color:black;
   }
   .head{
      width:500px;
      height: 150px;
      background-color:pink;
      margin: 0 auto;
   }
   .content{
      width:500px;
      height: 150px;
      background-color:cornsilk;
      margin: 0 auto;
   }
   .one{
      width: 90px;
      height: 150px;
      background-color:darkorange;
      float:left;
   }
   .two{
      /*如果宽度超出了父元素装不下就会换行*/
      width: 410px;
      height: 150px;
      background-color:aqua;
      float:left;
   }
</style>
</head>
<body>
   <div class="top"></div> 
   <div class="head"></div>
   <div class="content">
      <div class="one">123</div>
      <div class="two">456</div>
   </div>
</body>
```

**效果**：

![image-20230528124916098](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171145364.png)

**注意踩坑点**：<font style="color:red">如果蓝色的盒子长度超出父元素就会导致换行如下</font>：

**代码**：

```html
.two{
/*如果宽度超出了父元素装不下就会换行*/
   width: 420px; /*故意增加一点点的宽度*/
   height: 150px;
   background-color:aqua;
   float:left;
}
```

**效果**：

![image-20230528125101730](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171145385.png)

#### 5.小米——案例:ice_cream: 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>练习</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box{
            margin: 0 auto;
            /* background-color:pink; */
            width: 1226px;
            height: 614px;
        }

        .left {
            background-color: blue;
            float: left;
            width: 234px;
            height: 614px;
        }

        .right {
            /* background-color: green; */
            float: right;
            width: 978px;
            height: 614px;
        }

        ul {
            list-style: none;
        }

        .right li{
            margin-right: 14px;
            margin-bottom: 14px;
            width: 234px;
            height: 300px;
            background-color: #87ceeb;
            float: left;
        }

        .right li:nth-child(4n) {
            margin-right: 0;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="left"></div>
        <div class="right">
            <ul>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
                <li></li>
            </ul>
        </div>
    </div>       
</body>
</html>
```

效果：

![image-20230717161514466](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171615021.png)

#### 6.导航栏——案例:kaaba: 

>  该案例中需要主要到的几个点
>
>  1.  CSS的书写顺序
>  2.  li标签中的a标签默认是行内元素我们设置宽高的话需要换为块级元素或者行内块元素
>  3.  不必担心a标签换为块级或者行内块后会独占一行，因为a标签已经在li标签中一块浮动了等

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
        .nav{
            width: 640px;
            height: 50px;
            background-color: #ffc0cb;
            margin: 50px auto;
        }

        ul {
            list-style: none;
        }

        .nav li {
            float: left;
        }

        .nav li a {
            /*注意CSS的书写顺序*/
            /*1.浮动 /display/*/
            /* background-color: green; */
            /* display: inline-block; */
            /*无论使用行内块还是块都可行
             但是这里你可能会产生一个问题
             为什么，块在一行显示，不应该独占一行吗？
             答：因为li标签被浮动了优先级比标准流高, a在li中所以不受其它的a的影响*/
            display: block;

            /*2.盒子模型*/
            width: 80px;
            height: 50px;

            /*3.文字样式*/
            line-height: 1.5;
            text-align: center;
            line-height: 50px;
            text-decoration: none;
            color:aliceblue;
        }

        .nav li a:hover {
            background-color: green;
        }

    </style>
</head>
<body>
    <div class="nav">
        <ul>
            <li><a href="#">新闻</a></li>
            <li><a href="#">新闻</a></li>
            <li><a href="#">新闻</a></li>
            <li><a href="#">新闻</a></li>
            <li><a href="#">新闻</a></li>
            <li><a href="#">新闻</a></li>
            <li><a href="#">新闻</a></li>
            <li><a href="#">新闻</a></li>
        </ul>
    </div>
</body>
</html>
```

效果：

![image-20230717161545761](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171615810.png)

### CSS书写顺序:lantern: 

:rabbit: 

浏览器执行效率更高

**顺序如下**：

0.  有定位先写定位 !!

1.  浮动  / display
2.  盒子模型：margin border padding 宽度高度背景色
3.  文字样式

### 清除浮动:closed_umbrella:

:rabbit: 

**目标**：能够认识<strong style="color:red">清除浮动的目的</strong>，并且能够使用<strong style="color:red">清除浮动的方法</strong>。

#### 清除浮动的介绍:dagger:

:rabbit: 

##### 含义：

<strong style="color:red">清楚浮动带来的影响</strong>。

##### 影响：

-  如果子元素浮动了，此时子元素不能撑开标准流的块级父元素

##### 原因：

-  子元素浮动后脱标 ——> 不占位置

##### 目的：

-  需要父元素有高度，从而不影响其它网页元素的布局

#### 演示浮动的影响性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .top {
            margin: 0 auto;
            width: 1000px;
            /*如果.left和.right都浮动的话就不在标准流中了，div为块级元素没有内容撑开了所以就
            看不到了但是不浮动就会被内容所撑开就能看得到pink的块了*/
            /* height: 300px; */
            background-color: pink;
        }

        .bottom {
            height: 100px;
            background-color: green;
        }

        .left {
            float: left;
            width: 200px;
            height: 300px;
            background-color: #ccc;
        }

        .right {
            float: right;
            width: 790px;
            height: 300px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
    <div class="top">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="bottom"></div>
</body>
</html>
```

解释下面结果, 因为==浮动==的关系==脱离了标准流==，而且div是==块级元素高度由内容撑开==的所以我们看不见父元素的pink块了，其中的green色的div块没有浮动但是也不在top块中所以它掉了上去 ，再看下面的演示就明白了

**结果**：

![image-20230717165833721](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171658169.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .top {
            margin: 0 auto;
            width: 1000px;
            /*如果.left和.right都浮动的话就不在标准流中了，div为块级元素没有内容撑开了所以就
            看不到了但是不浮动就会被内容所撑开就能看得到pink的块了*/
            /* height: 300px; */
            background-color: pink;
        }

        .bottom {
            height: 100px;
            background-color: green;
        }

        .left {
            /* float: left; */
            width: 200px;
            height: 300px;
            background-color: #ccc;
        }

        .right {
            /* float: right; */
            width: 790px;
            height: 300px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
    <div class="top">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="bottom"></div>
</body>
</html>
```

**解释**：当把==浮动给注释掉==了后，==div==里面的==子div==会将当前==父div的高度撑开==所以就==能看到pink块==了。

**结果**：

![image-20230717170055093](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171700561.png)

#### 清除浮动的方法:package: 

:rabbit: 

##### 清除浮动的方法——①直接设置父元素高度

**特点**：

-  **优点**：简单粗暴，方便
-  **缺点**：有些布局中不能固定父元素高度。如：新闻列表，京东推荐模块

**代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .top {
            margin: 0 auto;
            width: 1000px;
            /*如果.left和.right都浮动的话就不在标准流中了，div为块级元素没有内容撑开了所以就
            看不到了但是不浮动就会被内容所撑开就能看得到pink的块了*/
            height: 300px;
            background-color: pink;
        }

        .bottom {
            height: 100px;
            background-color: green;
        }

        .left {
            float: left;
            width: 200px;
            height: 300px;
            background-color: #ccc;
        }

        .right {
            float: right;
            width: 790px;
            height: 300px;
            background-color: skyblue;
        }

        /* .clearfix { */
            /*清楚左右两侧浮动的影响*/
            /* clear: both; */
        /* } */
    </style>
</head>
<body>
    <div class="top">
        <div class="left"></div>
        <div class="right"></div>
        <!-- <div class="clearfix"></div> -->
    </div>
    <div class="bottom"></div>
</body>
</html>
```

**效果**：

![image-20230717171354897](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171713429.png)

##### 清除浮动的方法——②额外标签法

-  **操作**：
   1.  在==父元素内容==的<font color=red>**最后**</font>==添加一个块级元素==.
   2.  给添加的块级元素设置 ==clear:both== 
       1.  clear:left 清除左浮动
       2.  clear:right 清除右浮动
       3.  一般直接使用clear:both省劲又管用。
-  **特点**：
   -  **缺点**：会在页面中添加额外的标签，会让页面的HTML结构变得复杂

**代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .top {
            margin: 0 auto;
            width: 1000px;
            /*如果.left和.right都浮动的话就不在标准流中了，div为块级元素没有内容撑开了所以就
            看不到了但是不浮动就会被内容所撑开就能看得到pink的块了*/
            /* height: 300px; */
            background-color: pink;
        }

        .bottom {
            height: 100px;
            background-color: green;
        }

        .left {
            float: left;
            width: 200px;
            height: 300px;
            background-color: #ccc;
        }

        .right {
            float: right;
            width: 790px;
            height: 300px;
            background-color: skyblue;
        }

        .clearfix {
            /*清楚左右两侧浮动的影响*/
            clear: both;
        }
    </style>
</head>
<body>
    <div class="top">
        <div class="left"></div>
        <div class="right"></div>
        <div class="clearfix"></div>
    </div>
    <div class="bottom"></div>
</body>
</html>
```

**结果**：

![image-20230717170602291](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171706099.png)

##### 清除浮动的方法——③单伪元素清除法

**操作**：用为元素替代了额外标签

1.  基本写法

![image-20230717171056496](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171711057.png)

2.  补充写法

![image-20230717171117658](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171711167.png)

**特点**：

-  **优点**：项目中使用，直接给标签加类即可清除浮动。

**代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .top {
            margin: 0 auto;
            width: 1000px;
            /*如果.left和.right都浮动的话就不在标准流中了，div为块级元素没有内容撑开了所以就
            看不到了但是不浮动就会被内容所撑开就能看得到pink的块了*/
            /* height: 300px; */
            background-color: pink;
        }

        .bottom {
            height: 100px;
            background-color: green;
        }

        .left {
            float: left;
            width: 200px;
            height: 300px;
            background-color: #ccc;
        }

        .right {
            float: right;
            width: 790px;
            height: 300px;
            background-color: skyblue;
        }

        /*单伪元素清除浮动 和 额外标签法原理是一样的*/
        .clearfix::after {
            content: '';
            /*伪元素添加的标签默认是行内元素的，要求块级元素才行*/
            display: block;
            /*清除浮动*/
            clear: both;
            /*为了兼容性*/
            height: 0;
            visibility: hidden;
        }
    </style>
</head>
<body>
    <div class="top clearfix">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="bottom"></div>
</body>
</html>
```

**结果**：

![image-20230717172645492](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171726198.png)

##### 清除浮动的方法——④双伪元素清除法

**操作**：

![image-20230717172841704](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171728235.png)

**特点**：

-  **优点**：项目中使用，直接给标签加类即可清除浮动

**代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .top {
            margin: 0 auto;
            width: 1000px;
            /*如果.left和.right都浮动的话就不在标准流中了，div为块级元素没有内容撑开了所以就
            看不到了但是不浮动就会被内容所撑开就能看得到pink的块了*/
            /* height: 300px; */
            background-color: pink;
        }

        .bottom {
            height: 100px;
            background-color: green;
        }

        .left {
            float: left;
            width: 200px;
            height: 300px;
            background-color: #ccc;
        }

        .right {
            float: right;
            width: 790px;
            height: 300px;
            background-color: skyblue;
        }

        /*.clearfix::before 作用：解决外边距塌陷问题
            外边距塌陷问题： 父子标签，都是块级，子级加margin会影响父级的位置*/
        /*清除浮动*/
        .clearfix::before,
        .clearfix::after {
            content: '';
            display: table;
        }
        /*真正清除浮动的标签*/
        .clearfix::after {
            clear: both;
        }
    </style>
</head>
<body>
    <div class="top clearfix">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="bottom"></div>
</body>
</html>
```

**结果**：

![image-20230717173758945](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171738573.png)

##### 清除浮动的方法——⑤给父元素设置overflow:hidden

**操作**：

-  直接给父元素设置 overflow:hidden

**特点**：

-  **优点**：方便

**代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .top {
            margin: 0 auto;
            width: 1000px;
            /*如果.left和.right都浮动的话就不在标准流中了，div为块级元素没有内容撑开了所以就
            看不到了但是不浮动就会被内容所撑开就能看得到pink的块了*/
            /* height: 300px; */
            background-color: pink;
            overflow: hidden;
        }

        .bottom {
            height: 100px;
            background-color: green;
        }

        .left {
            float: left;
            width: 200px;
            height: 300px;
            background-color: #ccc;
        }

        .right {
            float: right;
            width: 790px;
            height: 300px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
    <div class="top">
        <div class="left"></div>
        <div class="right"></div>
    </div>
    <div class="bottom"></div>
</body>
</html>
```

**结果**：

![image-20230717174319022](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307171743715.png)

## 定位

**定位**：将盒子==定==在某一个==位==置，所以==定位也是在摆放盒子，按照定位的方式移动盒子==。

定位 = ==定位模式== + ==边偏移==。

==定位模式== 用于指定一个元素在文档中的定位方式。==边偏移==则决定了该元素的最终位置。

<strong style="color:red">注意</strong>：块元素和行内元素使用了==绝对定位==和==固定定位==(<font color=red>相对定位不会</font>)以后，会==默认转换成行内块元素==。

但是使用==定位转换==后的==行内块元素默认不会继承父盒子的宽度==，<font style="color:red">display:block转换的是带有的</font>。

### 定位模式

定位模式决定元素的定位方式，它通过CSS的==position==属性来设置，其值可以分为四个：

| 值                               | 语义         |
| -------------------------------- | ------------ |
| static(可以忽略基本不用基本不学) | **静态**定位 |
| relative                         | **相对**定位 |
| absolute                         | **绝对**定位 |
| fixed                            | **固定**定位 |

### 边偏移

边偏移就是定位的盒子移动到最终位置。有==top==，==bottom==，==left==和==right== 4个属性。

| 边偏移属性 | 示例        | 描述                                                     |
| ---------- | ----------- | -------------------------------------------------------- |
| top        | top:80px    | **顶端**偏移量，定义元素相对于其父元素**上边线的距离**。 |
| bottom     | bottom:80px | **底部**偏移量，定义元素相对于其父元素**下边线的距离**。 |
| left       | left:80px   | **左侧**偏移量，定义元素相对于其父元素**左边线的距离**。 |
| right      | right:80px  | **右侧**偏移量，定义元素相对于其父元素**右边线的距离**。 |

### 静态定位static(了解)

静态定位是元素的<strong style="color:red">默认定位方式，无定位的意思</strong>。

**语法**：

```
选择器{position:static;}
```

-  静态定位按照==标准流特性摆放位置==，它==没有边偏移==。
-  静态定位在布局时很少用到

### 相对定位(重要)

<strong style="color:red">相对定位</strong>是元素在移动位置的时候，是相对于它<strong style="color:red">原来的位置</strong>来说的(自恋型)

**语法**：

```
选择器{position:relative;}
```

**相对定位的特点**：(<font color=red>务必记住</font>)

1.  它是相对于自己==原来的位置来移动==的(<strong style="color:red">移动位置的时候参照点是自己原来的位置</strong>)
2.  <strong style="color:red">原来</strong>在标准流的<strong style="color:red">位置</strong>继续占有，后面的盒子仍然==以标准流的方式对待==它(<strong style="color:red">不脱标，继续保留原来位置</strong>)

因此，相对定位==没有脱标==。它最典型的应用是==给绝对定位当爹==的。。。

<font color=red>**子绝父相**</font> (记住这句话)

**代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box1{
            /*相对定位*/
            position: relative;
            top: 100px;
            left: 100px;
            width: 200px;
            height: 200px;
            background-color: pink;
        }

        .box2 {
            width: 200px;
            height: 200px;
            background-color: deeppink;
        }

    </style>
</head>
<body>
    <div class="box1">

    </div>
    <div class="box2">

    </div>
</body>
</html>
```

**结果**：

![image-20230717203652406](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307172039392.png)

### 绝对定位absolute(重要)

<strong style="color:Red">绝对定位</strong>是元素在移动位置的时候，是相对于它的<strong style="color:Red">祖先元素</strong>来说的 (拼爹型)

**语法**：

```
选择器{position:absolute;}
```

**绝对定位的特点**：(务必记住)

-  如果<strong style="color:Red">没有祖先元素</strong>或者<strong style="color:Red">祖先元素没有定位</strong>，则以浏览器为准定位(Document文档)。

-  如果==祖先元素有定位==(相对，绝对，固定定位)，则==以最近一级的有定位祖先元素为参考点移动位置==。

   ![image-20230718122557422](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181225066.png)

-  绝对定位<strong style="color:Red">不再占有原先位置</strong>。(脱标)

![image-20230717205401069](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307172054362.png)

**代码**：

第一种情况没有父元素则以浏览器为准定位

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .son {
            /*绝对定位*/
            position: absolute;
            /* top: 10px;
            left: 10px;
            top: 10px;
            right: 200px; */
            left: 0;
            bottom: 0;
            width: 200px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
   <div class="son">

   </div> 
</body>
</html>
```

第二种情况有父元素并且父元素没有定位

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .father {
            width: 500px;
            height: 500px;
            background-color: skyblue;
        }
        .son {
            /*绝对定位*/
            position: absolute;
            /* top: 10px;
            left: 10px;
            top: 10px;
            right: 200px; */
            left: 0;
            bottom: 0;
            width: 200px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>
<body>
    <div class="father">
        <div class="son">

        </div> 
    </div>
</body>
</html>
```

**结果**：

![image-20230717202627494](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307172039377.png)

如果父元素定位了

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .zuxian {
            position:absolute;
            width: 700px;
            height: 700px;
            background-color: tomato;
        }

        .father {
            position: absolute;
            top: 0;
            width: 500px;
            height: 500px;
            background-color: skyblue;
        }

        .son {
            /*绝对定位*/
            position: absolute;
            /* top: 10px;
            left: 10px;
            top: 10px;
            right: 200px; */
            left: 0;
            bottom: 0;
            width: 200px;
            height: 200px;
            background-color: pink;
        }
    </style>
</head>

<body>
    <div class="zuxian">
        <div class="yeye">
            <div class="father">
                <div class="son">
                </div>
            </div>
        </div>
    </div>
</body>

</html>
```

**结果**：

![image-20230717204421543](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307172044123.png)

### 问题

1 绝对定位和相对定位到底有什么使用场景？

2 为什么说相对定位给绝对定位当爹的呢？

### 子绝父相的由来

弄清楚这个口诀，就明白了绝对定位和想相对定位的使用场景。

这个"子绝父相"太重要了，是我们学习定位的口诀，是定位中最常用的一种方式这句话的意思是：<strong style="color:red">子级是绝对定位的话，父级要用相对定位</strong>。

1.  子级绝对定位，不会占有位置，可以放到父盒子里面的任何一个地方，不会影响其它的兄弟盒子
2.  父盒子需要加定位限制子盒子在父盒子内显示。
3.  父盒子布局时，需要占有位置，因此父亲只能是相对定位。

这就是子绝父相的由来，所以<strong style="color:red">相对定位经常用来作为绝对定位的父级</strong>。

总结：<strong style="color:red">因为父级需要占有位置，因此是相对定位，子盒子不需要占有位置，则是绝对定位</strong>。

### 案例——hot模块

**需求**：在介绍的模块中加入一个Hot图标

![image-20230718084206342](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307180842123.png)

**代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            margin-left: 100px;
            margin-top: 100px;
            width: 300px;
            border: 1px green double;
            border-radius: 5%;
            overflow:hidden;
        }
        .box ul li {
            position: relative;   
            list-style: none;
        }
        .box ul li em {
            position: absolute;
            top: 0;
            right: 0;
        }
        .info {
            text-align: center;
        }
    </style>
</head>
<body>
   <div class="box">
    <ul>
        <li>
            <em>
                <img src="../imgage/hot图标.png" width="30px" height="30px" alt="标签"/>
            </em>
            <img src="../imgage/瑞克.png" width="300px" height="200px" alt="这是图"/>
            <h4>Think PhP 5.0 博客系统实战项目演练</h4>
            <div class="info">
                <span>高级</span> · 1125人正在学习
            </div>
        </li>
    </ul>
   </div> 
</body>
</html>
```

**结果**：

![image-20230718084306941](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307180843488.png)

### 固定定位fixed(重要)

<strong style="color:red">固定定位</strong>是元素<strong style="color:red">固定于浏览器可视区的位置</strong>。

**主要使用场景**：可以在浏览器==页面滚动时元素的位置不会改变==。

**语法**：

```
选择器{position:fixed};
```

**固定定位的特点**：(<strong style="color:red">务必记住</strong>)

1.  以==浏览器的可视窗口==为==参照点移动元素==。

-  跟父元素没有任何关系
-  不随滚动条滚动

2.  固定定位==不在占有原先的位置==.

固定定位也是==脱标==的，其实固定定位也可以看做是一种==特殊的绝对定位==。

**代码**：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .dj {
            position: fixed;
            top: 100px;
            left: 40px;
        }
    </style>
</head>
<body>
    <div class="dj">
        <img src="../imgage/hot图标.png" width="100px" height="100px" alt="图片"/>
    </div>   
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
    <p>111111111111111111111111111</p>
</body>
</html>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307180914067.gif)

#### 固定定位小技巧：固定在版中右侧位置

**算法**：

1.  让固定定位的盒子left:50%，走到浏览器可视区(也可以看作版心)的一半的位置。
2.  让固定定位的盒子margin-left:版心宽度的一半距离。多走 版心宽度的一半位置。

就可以让固定定位的盒子贴着版心右侧对齐了。

**解释**：

给固定定位设置left:50%后就到了中间的位置，在往右走版心的一半的像素就可以了。

left:50%上面已经用过了可以使用margin-left来进行版心的一半的移动。

![image-20230718091841551](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307200905351.png)

**代码**：

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

        .w {
            width: 800px;
            height: 1400px;
            background-color: pink;
            margin: 0 auto;
        }

        .fixed {
            width: 50px;
            height: 50px;
            background-color: skyblue;
            /*固定定位*/
            position: fixed;
            bottom: 59px;
            /*1. 走浏览器宽度的一半*/
            left: 50%;
            /*2. 再往右走到版心的边适合的位置*/
            margin-left: 409px;
        }
    </style>
</head>
<body>
    <div class="fixed"></div> 
    <div class="w">版心盒子 800像素</div>
</body>
</html>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307180935973.gif)

### 粘性定位sticky

<strong style="color:red">粘性定位</strong>可以被认为是相对定位和固定定位的混合。Sticky粘性的。

**语法**：

```
选择器{
		position:sticky; 
		top: 10px;
		}
```

**粘性定位的特点**：

1.  以==浏览器的可视窗口==为参照点==移动元素==(固定定位特点)
2.  粘性定位<strong style="color:red">占有原先的位置</strong>(相对定位特点)
3.  ==必须添加==top，left，right，bottom其中一个才有效。

<strong style="color:red">注意</strong>：跟页面滚动搭配使用。兼容性较差，IE不支持。

**代码**：

```html
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        body {
            height: 2000px;
        }
        div {
            /*粘性定位
            默认为转换为行内块元素,宽度不会默认继承父盒子
            当元素距离浏览器可视区的10px时固定住不再移动*/
            position: sticky;
            top: 10px;
            width: 900px;
            height: 80px;
            background-color: pink;
            margin: 200px auto;
        }
    </style>
</head>
<body>
   <div></div> 
</body>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307200913552.gif)

### 定位的总结

| 定位模式                                            | 是否脱标                                           | 移动位置                                              | 是否常用                                                  |
| --------------------------------------------------- | -------------------------------------------------- | ----------------------------------------------------- | --------------------------------------------------------- |
| static静态定位                                      | 否                                                 | 不能使用边偏移                                        | 很少                                                      |
| <strong style="color:red">relative相对定位</strong> | <strong style="color:red">否(占有位置)</strong>    | <strong style="color:red">相对于自身位置移动</strong> | <strong style="color:red">常用</strong>                   |
| <strong style="color:red">absolute绝对定位</strong> | <strong style="color:red">是(不占有位置)</strong>  | <strong style="color:red">带有定位的父级</strong>     | <strong style="color:red">常用</strong>                   |
| <strong style="color:red">fixed固定定位</strong>    | <strong style="color:red">是(不占有位置)</strong>. | <strong style="color:red">浏览器可视区</strong>       | <strong style="color:red">常用</strong>                   |
| sticky粘性定位                                      | 否(占有位置)                                       | 浏览器可区                                            | 当前阶段少(<strong style="color:red">不建议使用</strong>) |

1.  **一定记住相对定位，固定定位，绝对定位两个大的特点**：
    1.  ==是否占有位置==(脱标否)
    2.  ==以谁为准点移动位置== 
2.  学习定位重点学会  <strong style="color:red">子绝父相</strong>.

### 定位叠放次序 ==z-index== 

在使用定位布局时，可能会出现盒子重叠的情况。此时，可以使用`z-index`来==控制盒子的前后次序== (z轴)

**语法**：

```
选择器{z-index:1;}
```

-  数值可以是==正整数==，==负整数==或==0==，默认是==auto==，数值越==大==，盒子越靠==上==。
-  如果==属性值相同==，则按照书写顺序，==后来居上==.
-  <Strong style="color:red">数字后面不能加单位(千万注意)</strong>.
-  <font color=red>只有定位的盒子才有`z-index`属性</font>.

### 定位的==扩展== 

#### 绝对定位的盒子居中

加了==绝对定位的盒子不能通过`margin: 0 auto`水平居中==，但是可以通过以下计算方法实现水平和垂直居中。

**实现步骤**：

1.  left:50%;：让盒子的左侧移动到父级元素的水平中心位置。
2.  maring-left: -100px;：让盒子向左移动自身宽度的一半

**代码**：

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

        .box {
            width: 200px;
            height: 200px;
            background-color: pink;
            position: absolute;
            left: 50%;
            margin-left: -100px;
            top: 50%;
            margin-top: -100px;
            /*在定位中使用不起效果*/
            /* margin: 0 auto; */
        }
    </style>
</head>

<body>
    <div class="box"></div>
</body>
</html>
```

**效果**：

![image-20230718103529173](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181035341.png)

#### 定位特殊特性

==绝对定位==和==固定定位==也和==浮动类似==。

**类似点**：

-  脱标 (不占有原来位置)
-  默认转换为行内块元素

1.  ==行内元素==添加==绝对定位==或者==固定定位==，可以==直接设置宽度和高度==。

    -  因为子绝父相，绝对定位脱标 转换为行内块元素，相对定位 不脱标不进行转换。

    ```html
        <style>
            span {
                /*加上定位后就可以为行内元素设置宽高了*/
                position: absolute;
                background-color: pink;
                width: 100px;
                height: 100px;
            }
        </style>
    </head>
    <body>
       /*span是行内元素不能设置宽高的*/
       <span>
            123
       </span> 
    </body>
    ```

    **效果**：

    ![image-20230718104148850](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181041392.png)

2.  ==块级元素添加绝对定位==或者==固定定位==，如果==不给宽度==或者==高度==，==默认大小是内容大小==。

    -  因为 绝对定位 和 固定定位 会默认将其它级别元素 转换为 行内块元素。

    ```html
        <style>
            div {
                /*块级元素加上绝对定位后就是按照内容大小的盒子了*/
                position: absolute;
                background-color: skyblue;
            }
        </style>
    </head>
    <body>
       <div>abcd</div>
    </body>
    ```

    **效果**：

    ![image-20230718104547537](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181045587.png)


#### 脱标的盒子不会触发外边距塌陷

==浮动==元素,==绝对定位==(==固定定位==)元素的==都不会触发外边距合并==的问题。

#### 绝对定位(固定定位)会完全压住盒子包括内容

==浮动==元素不同，==只会压住==它下面的标准流的==盒子==，但是==不会压住==下面标准流盒子里面的==文字/图片==.

**代码**：

```html
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            float: left;
            width: 150px;
            height: 150px;
            background-color: pink;
        }
    </style>
</head>
<body>
   <div class="box"></div> 
   <p>阁下何不同风起，扶摇直上九万里</p>
</body>
```

**效果**：

![image-20230718105143246](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181051404.png)

但是==绝对定位(固定定位)==会==压住==下面标准流==所有的内容==.

```html
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            /* float: left; */
            position: absolute;
            width: 150px;
            height: 150px;
            background-color: pink;
        }
    </style>
</head>
<body>
   <div class="box"></div> 
   <p>阁下何不同风起，扶摇直上九万里</p>
</body>
```

**效果**：

![image-20230718105819393](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181058465.png)

浮动之所以不会压住文字，因为浮动产生的目的最初是为了做文字环绕效果的，文字会围绕浮动元素。

<strong style="color:red">注意</strong>：但是==浮动和绝对定位一起使用文字==还是==会被压下面去的==！

```html
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        .box {
            float: left;
            position: absolute;
            width: 150px;
            height: 150px;
            background-color: pink;
        }
    </style>
</head>
<body>
   <div class="box"></div> 
   <p>阁下何不同风起，扶摇直上九万里</p>
</body>
```

**效果**：

![image-20230718105930129](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181059924.png)

### 网页布局总结

通过盒子模型，清楚直到大部分html标签是一个盒子。

通过CSS浮动，定位可以让每个盒子排列到成为网页。

一个完整的网页，是标准流，浮动，定位一起完成布局的，每个都有自己的专门用法。

1 **标准流**

可以让盒子上下排列或者左右排列，<strong style="color:red">垂直的块级盒子显示就用标准流局部</strong>。

2 **浮动**

可以让多个块级元素一行显示或者左右对齐盒子，<strong style="color:red">多个块级盒子水平显示就用浮动布局</strong>。

3 **定位**

定位最大的特点是有层叠的概念，就是可以让多个盒子前后叠压来显示。<strong style="color:red">如果元素自由在某个盒子内移动就用定位局部</strong>。

## 元素的显示与隐藏

类似网站广告，当我们点击关闭就不见了，但是我们重新刷新页面，会重新出现！

本质：<strong style="color:red">让一个元素在页面中隐藏或者显示出来</strong>。

### display属性:star: 

display属性用于设置一个元素对应如何显示。

-  display:none; 隐藏对象
-  display:block; 除了转换为块级元素之外，同时还有显示元素的意思

<strong style="color:red">display隐藏元素后，不再占有原来的位置</strong>。

后面应用及其广泛，搭配JS可以做很多的网页特效。

```html
    <style>
        .peppa {
            display: none;
            /*display:block;*/
            width: 200px;
            height: 200px;
            background-color: pink;
        }

        .george {
            width: 200px;
            height: 200px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
   <div class="peppa">佩奇</div> 
   <div class="george">乔治</div> 
</body>
```

**效果**：

![image-20230718111841243](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181118953.png)

### visibility可见性

<strong style="color:red">visibility</strong>属性用于指定一个元素应可见还是隐藏

-  visibility:visible; 元素可视
-  visibility:hidden; 元素隐藏
-  <strong style="color:red">注意：visibility隐藏元素后，继续占有原来的位置</strong>。

```html
    <style>
        .peppa {
            visibility: hidden;
            width: 200px;
            height: 200px;
            background-color: pink;
        }

        .george {
            width: 200px;
            height: 200px;
            background-color: skyblue;
        }
    </style>
</head>
<body>
   <div class="peppa">佩奇</div> 
   <div class="george">乔治</div> 
</body>
```

**效果**：

![image-20230718112335090](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307181123853.png)

### 总结：

如果隐藏元素==想要原来位置==，就用 visibility:hidden;

如果隐藏元素==不想要原来位置==，就用display:none; (用处更多 重点)

### overflow溢出

overflow属性指定了如果内容溢出一个元素的框(超过其指定高度及宽度)时，会发生什么。

| 属性值  | 描述                                                 |
| ------- | ---------------------------------------------------- |
| visible | 不剪切内容也不添加滚动条                             |
| hidden  | 不显示超过对象尺寸的内容，超出的部分隐藏掉           |
| scroll  | 不管超出内容是否，总是显示滚动条                     |
| auto    | 按需求来决定，超出自动显示滚动条，不超出不显示滚动条 |

一般情况下，我们都不想让溢出的内容显示出来，因为溢出的部分会影响布局。

<font color=red>但是如果有定位的盒子，请慎用overflow:hidden因为它会隐藏</font>.

**代码**：

```html
    <style>
        .peppa {
            /*隐藏溢出部分*/
            /* overflow: hidden; */
            /*滚动条显示溢出部分,不溢出也会显示滚动条*/
            /* overflow: scroll; */
            /*按需显示滚动条*/
            overflow: auto;
            width: 200px;
            height: 200px;
            border: 3px pink solid;
            margin: 100px auto;
        }
    </style>
</head>
<body>
   <div class="peppa">
    《小猪佩奇》（Peppa Pig）是英国动画公司Astley Baker Davies、Entertainment
    One制作的原创欧洲儿童系列电视动画 [11] [13] ，由内维尔·阿斯特利、马克·贝克、
    菲尔·霍尔与乔里斯·范胡尔岑执导 [14] ，于2004年5月31日在英国电视五台首播 [15] ，
    2015年6月，《小猪佩奇》引进中国大陆，并在中央电视台少儿频道首播 [17] 。
   </div> 
</body>
```

**效果**：

![image-20230720091728630](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307200917345.png)

### 隐藏滚动条但保留功能

要隐藏滚动条，但仍能保持滚动，可以使用以下代码：

```css
/* 隐藏 Chrome、Safari 和 Opera 的滚动条 */
.example::-webkit-scrollbar {
  display: none;
}

/* 隐藏 IE、Edge 和 Firefox 的滚动条 */
.example {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
```

**代码**：

```html
<!DOCTYPE html>
<html>
<head>
<style>
.example {
  background-color: #eee;
  width: 200px;
  height: 100px;
  border: 1px dotted black;
  overflow-y: scroll; /* 添加滚动功能 */
}

/* 隐藏 Chrome、Safari 和 Opera 的滚动条 */
.example::-webkit-scrollbar {
    display: none;
}

/* 为 IE、Edge 和 Firefox 隐藏滚动条 */
.example {
  -ms-overflow-style: none;  /* IE 和 Edge */
  scrollbar-width: none;  /* Firefox */
}
</style>
</head>
<body>

<h2>隐藏滚动条但保留功能</h2>
<p>尝试在下面的 div 中滚动：</p>

<div class="example">一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. 一些启用滚动的文本.. </div>

</body>
</html>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281527770.gif)

## 	背景线性渐变

![image-20230725155412855](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307251554063.png)

**语法**：

```css
background: linear-gradient(起始方向，颜色1，颜色2，...);
background: -webkit-linear-gradient(left,red,blue);
background: -webkit-linear-gradient(left top,red,blue);
background: linear-gradient(透明/方向,rgba(0,0,0,.5)); /*使用rgba三原色 和 透明度*/
background: linear-gradient(transparent,rgba(0,0,0,.5));
```

<font color=red>**背景渐变必须添加浏览器私有前缀**</font>. -webkit-

**起始方向可以是**：==访问名词== 或者 ==度数==，如果省==略默认就是top==.

**代码**：

```html
    <style>
        div {
            width: 600px;
            height: 200px;
            /* 背景渐变必须加浏览器私有前缀 */
            background: -webkit-linear-gradient(top left,red,blue);
        }
    </style>
</head>
<body>
   <div></div> 
</body>
```

**效果**：

![image-20230725160102511](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307251601107.png)