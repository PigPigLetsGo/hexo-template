---
title: 实现bilibili点击小滑块移动效果
categories:
    - [计算机学科,web,js,案例代码]
tags:
    - 计算机学科
    - web
    - js
    - 案例代码
---

## 实现bilibili点击小滑块移动效果

**需求**：当点击链接，下面红色滑块跟着移动

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308011603296.gif)

**分析**：

1.  用到事件委托
2.  点击链接得到当前元素的offsetLeft值
3.  修改Line颜色块的transform值 = 点击链接的offsetLeft
4.  添加过渡效果

**代码**：

html

```html
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
   <script src="./js/index.js"></script>
</body>
```

less

```less
@import "base";
// 声明基准值变量
@baseSize: 3.75vmin;
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
        position: relative;
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
                    // &:hover {
                    //     border-bottom: (3 / @baseSize) #fb7299 solid;
                    //     width: 100%;
                    //     height: (37 / @baseSize);
                    // }
                }
                // 如果使用则解除上面的相对定位的注释然后将hover注释掉
                .line {
                    position: absolute;
                    bottom: 0;
                    left: (16 / @baseSize);
                    width: (28 / @baseSize);
                    height: (2 / @baseSize);
                    background-color: #fb7299;
                    transition: all .3s
                }
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

base.less

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

js

```js
// 1.获取元素对象 添加事件委托
const list = document.querySelector('.tabs-list')
const line = document.querySelector('.line')
// 给a注册事件
list.addEventListener('click',function(e) {
    // 判断点击的标签是否为a标签
    if(e.target.tagName === 'A')
    // 将点击后的target的offsetLeft值给向Y轴移动函数进行操作,后面记得带上单位
    line.style.transform = `translateX(${e.target.offsetLeft}px)`
})
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308012328825.gif)

