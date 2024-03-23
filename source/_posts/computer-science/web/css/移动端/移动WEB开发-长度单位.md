---
title: 移动端WEB-长度单位vw/vh
categories: 
    - [计算机学科,web,css,移动端]
tags:
    - web
    - 计算机学科
---

## 长度单位

-  vw/vh

设计稿标准为375px 分成100等份 每1vw就是 3.75 ，如果是540就是5.4 根据视口的大小不同进行*5.4来适应当前视口 达到了自适应的效果，不需要rem和flexible.js了 

vw不需要检测视口宽度 rem必须要检测视口宽度再改html文字大小 而vw不必像rem那样麻烦 为什么呢？ 因为vw就是当前视口的百分之一。

不管在什么屏幕下，我们把屏幕分为平均的100等份。

>  1vw = 1 / 100 屏幕的宽度

##### 1vw和1%

width: 1vw;

width: 1%;

1.  vw永远是以视口的宽度为准，在375设计稿下，1vw永远是3.75px
2.  百分比以父盒子为准，假如父盒子是200px，则1% 是2px

![image-20230728113841921](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281138772.png)

##### px如何转为vw

我们设计稿是iphone6/7/8 是375px，vw把屏幕划分了100等份，则1vw = 3.75px

有个盒子，它的宽度是3.75px * 3.75px，则写成vw为 1vw * 1vw

又有一个盒子，宽度和高度分别是 37.5px和37.5px，则转换为vw 为 10vw * 10vw

有一个盒子 68px * 29px，则代码如下：

```less
width: (68 / 3.75vw); //前面不带单位，因为带了单位就以第一个值为准结果就不是vw单位了
height: (29 / 3.75vw);
```

##### 相对单位

相对<font color=red>视口的尺寸</font>计算结果

vw：viewport width

-  1vw = <font color=red>1/100</font> 视口宽度

vh：viewport height

-  1vh = <font color=red>1/100</font> 视口高度

vw：将整个视口分成100等份，每一份就是1vw

##### vw单位尺寸

1.  确定设计稿<font color=red>对应</font>的vw尺寸(1/100视口宽度)
    -  查看<font color=red>设计稿宽度</font> -> 确定参考<font color=red>设备宽度</font>(视口宽度) -> 确定<font color=red>vw尺寸</font>(1/100视口宽度)
2.  vw单位的尺寸 = <font color=red>px单位数值</font> / (1/100视口宽度)

**比如说设计稿是 375px 的 那么1vw就是 3.75px宽度** 

测量的如下盒子是68px 那么就是 68px / 3.75vw

![image-20230728105227707](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281052341.png)

代码：

```html
<body>
   <div class="box"></div> 
</body>
```

less

```less
.box {
    // width: 100px;
    // height: 100px;
    width: (100 / 3.75vw); 
    height: (100 / 3.75vw);
    background-color: pink;
}
```

**效果**：

100等份 / 在375像素1vw就是3.75 所以100除以 3.75 等于如下结果 26.66666...vw

![image-20230728110613909](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281106946.png)

在iphone 6/7/8的效果下就是 以 100 / 3.75来显示的

![image-20230728110352186](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281103793.png)

设备换成Plus 就是 100 / 3.75 * 4.14 来显示的。

如果视口又变大了比如说 540 那么就是 26.666...vw * 5.4 前面 计算出来 26.666...vw是死的。

![image-20230728110721307](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281107973.png)

##### vh单位尺寸

1.  确定设计稿<font color=red>对应</font>的vh尺寸(1 / 100视口宽度)
    -  查看<font color=red>设计稿宽度</font> -> 确定参考<font color=red>设备宽度</font>(视口高度) -> 确定<font color=red>vh尺寸</font>(1 / 100视口宽度)
2.  vh单位的尺寸 = <font color=red>px单位数值</font> / (1 / 100视口高度)

-  **思考**：开发中，会不会vw和vh混用呢？
   -  答：<span alt='solid'>不会</span>.
   -  原因：vh是1 / 100视口宽度，<font color=red>全面屏视口宽度尺寸大</font>，如果混用可能会导致<font color=red>盒子变形</font>。

### bilibili案例

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
    <link rel="shortcut icon" href="./favicon.ico" type="image/x-icon"/>
    <link rel="stylesheet" href="./css/normalize.css"/>
    <link rel="stylesheet" href="./fonts/iconfont.css"/>
    <link rel="stylesheet" href="./css/index.css"/>
</head>
<body>
    <!--头部部分-->
   <div class="suspension">
   <!--头部模块-->
    <div class="m-navbar">
        <!--logo模块-->
        <a href="#" class="logo">
            <i class="iconfont Navbar_logo"></i>
        </a>
        <!--右侧-->
        <div class="right">
            <!-- 搜索 -->
            <a href="#" class="search">
                <i class="iconfont ic_search_tab"></i>
            </a>
            <!-- 头像 -->
            <a href="#" class="face">
                <img src="./image/login.png" alt=""/>
            </a>
            <!-- 下载 -->
            <div class="app-btn">
                <img src="./image/download.png" alt=""/>
            </div>
        </div>
    </div>
    <!--频道模块-->
    <div class="channel-menu">
        <div class="tabs">
            <!-- 很宽的盒子 -->
            <div class="tabs-list">
                <a href="#">首页</a>
                <a href="#">动画</a>
                <a href="#">番剧</a>
                <a href="#">果蔬</a>
                <a href="#">音乐</a>
                <a href="#">舞蹈</a>
                <a href="#">鬼畜</a>
                <a href="#">游戏</a>
                <!-- 红色线 -->
                <div class="line"></div>
            </div>
        </div>
        <div class="after">
            <i class="iconfont general_pulldown_s"></i>
        </div>
    </div>
   </div> 
   <!--主体部分-->
   <div class="m-home">
    <div class="video-list">
        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>

        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>
        
        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>

        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>

        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>

        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>

        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>

        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>

        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>

        <a href="#" class="vide-item">
            <div class="card">
                <img src="./image/dog.jpg@480w_270h_1c" alt=""/>
                <!--播放量-->
                <div class="count">
                    <span>
                        <i class="iconfont icon_shipin_bofangshu"></i>
                        播放量
                    </span>
                    <span>
                        <i class="iconfont icon_shipin_danmushu"></i>
                        评论数
                    </span>
                </div>
            </div>
            <p class="title ellipsis-2">
                315晚会能不能曝光下智能电视？N重广告、套娃会员、操作反人类，当代年轻人是怎么被智能电视逼疯的？【商业B面&amp;牛顿】
            </p>
        </a>
    </div>
   </div>
   <!-- 底部部分 -->
   <footer class="app">
    <div class="btn-app">
        <i class="iconfont Navbar_logo"></i>
        打开APP,看你感兴趣的视频
    </div>
   </footer>
</body>
</html>
```

less

```less
@import "base";
// 声明基准值变量
@baseSize: 3.75vw;
.suspension {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background-color: #fff;
    z-index: 1;
    // 头部模块
    .m-navbar {
        padding: 0 (12 / @baseSize) 0 (18 / @baseSize);
        display: flex;
        height: (44 / @baseSize);
        justify-content: space-between;
        align-items: center;
        // logo模块
        .logo {
            i {
                color: #fb7299;
                font-size: (28 / @baseSize);
            }
        }
        // 右侧
        .right {
            display: flex;
            // 搜索
            .search {
                i {
                    font-size: (22 / @baseSize);
                    color: #ccc;
                }
            }
            // 头像
            .face {
                overflow: hidden;
                width: (24 / @baseSize);
                height: (24 / @baseSize);
                // background-color: red;
                border-radius: 50%;
                margin: 0 (20 / @baseSize);
            }
            // 下载
            .app-btn {
                width: (72 / @baseSize);
                height: (24 / @baseSize);
                // background-color: purple;
            }
        }
    }
    // 频道模块
    .channel-menu {
        // position: relative;
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: (44 / @baseSize);
        border-bottom: 1px #ccc solid;
        // background-color: purple;
        // 左侧频道
        .tabs {
            overflow: hidden;
            flex: 1;
            .tabs-list {
                display: flex;
                overflow-x: scroll;
                a {
                    padding: 0 (16 / @baseSize);
                    white-space: nowrap;
                    height: (40 / @baseSize);
                    line-height: (40 / @baseSize);
                    &:hover {
                        border-bottom: (3 / @baseSize) #fb7299 solid;
                        width: 100%;
                        height: (37 / @baseSize);
                    }
                }
                // 如果使用则解除上面的相对定位的注释然后将hover注释掉
                // .line {
                //     position: absolute;
                //     bottom: 0;
                //     left: (16 / @baseSize);
                //     width: (28 / @baseSize);
                //     height: (2 / @baseSize);
                //     background-color: #fb7299;
                // }
            }
        }
        // 右侧三角
        .after {
            width: (40 / @baseSize);
            height: (40 / @baseSize);
            text-align: center;
            line-height: (40 / @baseSize);
            color: #ccc;
            i {
                font-size: (20 / @baseSize);
            }
        }
    }
}
// 主体部分
.m-home {
    padding: (91 / @baseSize) 0 0 0;
    .video-list {
        display: flex;
        flex-wrap: wrap;
        .vide-item {
            width: 50%;
            // height: 150px;
            padding: (8 / @baseSize) (5 / @baseSize);
            // background-color: pink;
            .card {
                position: relative; 
                height: (97 / @baseSize);
                .count {
                    position: absolute;
                    bottom: 0;
                    left: 0;
                    height: (27 / @baseSize);
                    width: 100%;
                    background-color: pink;
                    padding: 0 (5 / @baseSize);
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    font-size: (12 / @baseSize);
                    color: #fff;
                    background: linear-gradient(transparent,rgba(0,0,0,.5));
                }
            }
            .title {
                margin-top: (5 / @baseSize);
                font-size: (12 / @baseSize);
            }
        }
    }
}
// 底部模块
.app {
    position: fixed;
    left: 0;
    bottom: (30 / @baseSize);
    width: 100%;
    height: (38 / @baseSize);
    // background-color: pink;
    .btn-app {
        margin: 0 (20 / @baseSize);
        height: (38 / @baseSize);
        background-color: purple;
        border-radius: (19 / @baseSize);
        text-align: center;
        line-height: (38 / @baseSize);
        background-color: #fb7299;
        color: #fff;
        font-size: (17 / @baseSize);
    }
}
```

base.less 做一些基本的初始化

```less
img {
    vertical-align: middle;
    width: 100%;
    height: 100%;
}
a {
    text-decoration: none;
    color: black;
}
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -webkit-tap-highlight-color: rgba(0,0,0,0);
}
.ellipsis-2 {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
  }

/* 隐藏 Chrome、Safari 和 Opera 的滚动条 */
.tabs-list::-webkit-scrollbar {
  display: none !important;
}
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281549941.gif)

查看项目：[点击查看](./案例/m-bilibili) 

### vw和vmin/vmax的区别

-  vmin可以照顾手机端 ==横屏==和==竖屏==的显示效果

![image-20230728153753577](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281537853.png)

#### 对比案例

**自己写的bilibili网页 横屏查看** 

![image-20230728154135433](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281541797.png)

**再看bilibili官网的横屏效果** 

![image-20230728154200950](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281542778.png)

可以看出使用vmin的话高度小于宽度的以高度来进行布局这么做的好处就是上面导航栏变小我们想看的内容变得更大更多

使用vmin就是看当前屏幕宽度和高度谁更小，谁更小就以谁来进行分配适配布局

如何使用呢 其实跟vw是一样的我们只需要更换下项目的单位即可操作如下：

ctrl+ h进行替换单位

![image-20230728154656334](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281546016.png)

**效果**：

![image-20230728160522752](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202307281605210.png)
