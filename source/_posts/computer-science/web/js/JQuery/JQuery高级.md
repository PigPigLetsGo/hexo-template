---
title: jQuery高级部分
categories:
    - [计算机学科,web,js,JQuery]
tags:
    - 计算机学科
    - jquery
---

## JQuery高级部分

### 动画

1. 三种方式显示和隐藏元素
    1. 默认显示和隐藏方式
        1. show([speed],[easing],[fn]):显示
            - 参数:
                1. speed:动画速度, 三个预定义的值("slow","normal","fast")或表示动画时长的毫秒数值(如:1000)
                2. easing:用来指定切换效果,默认是"swing", 可用参数"linear"
                    - swing:动画执行时效果为:缓入缓出,中间快
                    - linear:动画执行时效果为:匀速的
                3. fn:在动画完成时执行的函数, 每个元素执行一次
        2. hide([speed],[easing],[fn]):隐藏
        3. toggle([speed],[easing],[fn]):显示则隐藏,隐藏则显示
        ```js
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="UTF-8">
            <title>Insert title here</title>
            <script type="text/javascript" src="../js/jquery-3.3.1.min.js"></script>
            <script>
                //隐藏
                // function hideFn(){
                //     $("#showDiv").hide("5000","swing",function(){
                //         alert("隐藏了...");
                //     });
                // }
                //隐藏
                function hideFn(){
                    $("#showDiv").hide("5000","swing",function(){
                        alert("隐藏了...");
                    });
                }
                //显示
                function showFn(){
                    $("#showDiv").show("fast","linear");
                }
                //隐藏/显示
                function toggleFn(){
                    $("#showDiv").toggle("fast","swing");
                }
            </script>
        </head>
        <body>
        <input type="button" value="点击按钮隐藏div" onclick="hideFn()">
        <input type="button" value="点击按钮显示div" onclick="showFn()">
        <input type="button" value="点击按钮切换div显示和隐藏" onclick="toggleFn()">
        
        <div id="showDiv" style="width:300px;height:300px;background:pink">
            div显示和隐藏
        </div>
        </body>
        </html>
        ```
    2. 滑动显示和隐藏方式
        1. slideDown([speed],[easing],[fn]):显示
            - 参数:
                1. speed:动画速度,三个预定义的值("slow","normal","fast")或表示动画时长的毫秒数值(如:1000)
                2. easing:用来指定切换效果,默认是"swing",可用参数"linear"
                    - swing:动画执行时效果为:缓入缓出,中间快
                    - linear:动画执行时效果为:匀速的
                3. fn:在动画完成时执行的函数,每个元素执行一次
        2. slideUp([speed],[easing],[fn]):隐藏
        3. slideToggle([speed],[easing],[fn]):显示则隐藏,隐藏则显示
        ```js
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="UTF-8">
            <title>Insert title here</title>
            <script type="text/javascript" src="../js/jquery-3.3.1.min.js"></script>
            <script>
                function showFn(){//speed:动画速度,easing:参数swing:缓入缓出,中间快 linear:线性的,fn:在动画执行完后执行函数
                    $("#showDiv").slideDown("slow","swing");
                }
                function hideFn(){//speed:动画速度,easing:参数swing:缓入缓出,中间快  linear:线性的,fn:在动画执行完后执行函数
                    $("#showDiv").slideUp("slow");
                }
                function toggleFn(){//speed:动画速度,easing:参数swing:缓入缓出,中间块  linear:线性的,fn:在动画执行完后执行函数
                    $("#showDiv").slideToggle("slow");
                }
            </script>
        </head>
        <body>
        <input type="button" value="点击按钮隐藏div" onclick="hideFn()">
        <input type="button" value="点击按钮显示div" onclick="showFn()">
        <input type="button" value="点击按钮切换div显示和隐藏" onclick="toggleFn()">
        
        <div id="showDiv" style="width:300px;height:300px;background:pink">
            div显示和隐藏
        </div>
        </body>
        </html>
        ```
    3. 淡入淡出显示和隐藏方式
        1. fadeIn([speed],[easing],[fn]):显示
            - 参数:
                1. speed:动画速度,三个预定义的值("slow","normal","fast")或表示动画时长的毫秒数值
                2. easing:用来指定切换效果,默认是"swing",可用参数"linear"
                    - swing:动画执行时效果为:缓入缓出,中间快
                    - linear:动画执行时效果为:匀速的
                3. fn:在动画完完成时执行的函数,每个元素执行一次
        2. fadeOut([speed],[easing],[fn]):隐藏
        3. fadeToggle([speed],[easing],[fn]):显示则隐藏,隐藏则显示
        ```js
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="UTF-8">
            <title>Insert title here</title>
            <script type="text/javascript" src="../js/jquery-3.3.1.min.js"></script>
            <script>
                function showFn(){//speed:动画速度,easing:参数 swing:缓入缓出,中间快  linear:线性的,fn:动画执行完时执行函数
                    $("#showDiv").fadeIn("slow");//显示
                }
                function hideFn(){//speed:动画速度,easing:参数 swing:缓入缓出,中间快 linear:线性的,fn:动画执行完时执行函数
                    $("#showDiv").fadeOut("slow");//隐藏
                }
                function toggleFn(){//speed:动画速度,easing:参数 swing:缓入缓出,中间快 linear:现行的,fn:动画执行完时执行函数
                    $("#showDiv").fadeToggle("slow");//显示则隐藏,隐藏则显示
                }
            </script>
        </head>
        <body>
        <input type="button" value="点击按钮隐藏div" onclick="hideFn()">
        <input type="button" value="点击按钮显示div" onclick="showFn()">
        <input type="button" value="点击按钮切换div显示和隐藏" onclick="toggleFn()">
        
        <div id="showDiv" style="width:300px;height:300px;background:pink">
            div显示和隐藏
        </div>
        </body>
        </html>
        ```

JQuery的显示和隐藏动画效果其实就是控制`display` 属性来完成的

- 解释:`display:显示/展示 none:没有一个/无` display:none:没有显示/无展示

当我们点击隐藏后的div标签

![image_2022-12-25-18-31-18](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/image_2022-12-25-18-31-18.png)

当我们点击显示后的div标签

![image_2022-12-25-18-31-37](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/image_2022-12-25-18-31-37.png)

可以看到`display` 属性在我们点击显示后就不见了,在我们点击隐藏后就会出现

### 遍历

1. js的遍历方式
    - for(初始化值;循环结束条件;自增数)
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script src="./js/jquery-3.3.1.min.js"></script>
        <script>
            $(function(){
                //city下有多个li返回是一个集合
               var citys = $("#city li");
               //遍历citys
               for(var i = 0;i < citys.length;i ++){
                   //获取标签体内容
                   alert(i+":"+citys[i].innerHTML);
               }
            });
        </script>
    </head>
    <body>
        <ul id="city">
            <li>北京</li>
            <li>上海</li>
            <li>河北</li>
            <li>天津</li>
        </ul>
    </body>
    </html>
    ```
2. jq的遍历方式
    1. jq对象.each(callback)
    2. $.each(object,[callback])
    3. for..of:JQuery 3.0 版本之后提供的方式
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/jquery-3.3.1.min.js"></script>
    <script>
        $(function(){
            //city下有多个li返回是一个集合
           var citys = $("#city li");
           //遍历citys
           for(var i = 0;i < citys.length;i ++){
               //获取标签体内容
               if("上海" == citys[i].innerHTML){
                   // break;
                   continue;
               }
               alert(i+":"+citys[i].innerHTML);
           }
            //jq对象.each(callback)
            var citys = $("#city li");
            citys.each(function(index,element){
                //1.获取li对象,第一种方式:this
                // alert(this.innerHTML);
                //JQuery中提供的方法比js多很多
                if("上海" == $(this).html()){
                    //如果当前function返回为false,则结束循环(break)
                    // return false;
                    //如果返回为true,则结束本次循环,执行下次循环(continue)
                    return true;
                }
                alert($(this).html());
                //2.获取li对象 第二种方式,在回调函数中定义参数 index(索引) element(元素对象)
                // alert(index+":"+element.innerHTML);
                // alert($(element).html());
            });
            for(li of citys){
                alert($(li).html());
            }            
        });
    </script>
</head>
<body>
    <ul id="city">
        <li>北京</li>
        <li>上海</li>
        <li>河北</li>
        <li>天津</li>
    </ul>
</body>
</html>
```

### 事件绑定

1. JQuery标准的绑定方式
    - Jq对象.事件方法(回调函数);
```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/jquery-3.3.1.min.js"></script>
    <script>
        $(function(){
            //链式编程方式
            //      mouseover:鼠标移动到元素上触发事件
           $("#name").mouseover(function(){
               alert("鼠标来了...");
           //   mouseout:鼠标移除元素内触发事件
           }).mouseout(function(){
               alert("鼠标走了...");
           });
           alert("确定让输入框获得焦点?");
           //页面加载完成后让文本输入框获得焦点
           $("#name").focus();//让输入框获得焦点
        });
    </script>
</head>
<body>
<input type="text" id="name" value="绑定事件">
</body>
</html>
```
2. on绑定事件/off解除绑定
    - jq对象.on("事件名称",回调函数);
    - jq对象.off("事件名称");
        - 如果off方法不传递任何参数,则将组件上的所有事件全部解绑

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/jquery-3.3.1.min.js"></script>
    <script>
        $(function(){
            //1.使用on给按钮绑定单击事件 click
           $("#btn").on("click",function(){
              alert("我被点击了...");
           });
           //2.使用off解除btn按钮的单击事件,首先绑定按钮2的单击事件来解除按钮1的单击事件
           $("#btn1").click(function(){
               //解除按钮1的单击事件
               $("#btn").off("click");
               $("#btn"),off();//如果不指定解除的事件名称,将组件上的所有事件全部解除
              alert("按钮1单击事件被解除了...不信你点击试试? 没反应的");
           });
        });
    </script>
</head>
<body>
<input type="button" id="btn" value="使用on绑定事件">
<input type="button" id="btn1" value="使用off解绑事件">
</body>
</html>
```

3. 事件切换:toggle
    - jq对象.toggle(fn1,fn2...);
    - 注意: 此方法在JQuery-1.9版本中被删除了使用的话会被当作为是动作事件我们需要导入相应的jar包来弥补

并引入这个jar包

![image_2022-12-25-17-59-10](https://gitee.com/Doukaixin/note_image/raw/master/https://gitee.com/doukaixin/note_image/image_2022-12-25-17-59-10.png)

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/jquery-3.3.1.min.js" type="text/javascript" charset="UTF-8"></script>
    <script src="./js/jquery-migrate-1.2.1.js" type="text/javascript" charset="UTF-8"></script>
    <script>
        $(function(){
            //给btn按钮绑定事件切换
           $("#btn").toggle(function(){
              $("#myDiv").css("backgroundColor","green");
           },function(){
               $("#myDiv").css("backgroundColor","pink");
           });
        });//切换事件与显示隐藏两种使用方式并不相互冲突
        function toggleFn(){
            $("#myDiv").toggle("slow");
        }
    </script>
</head>
<body>
<input type="button" value="事件切换" id="btn" onclick="toggleFn()"><br>
<div id="myDiv" style="width:300px;height:300px;background:pink">点击按钮变成绿色,再次点击变成红色</div>
</body>
</html>
```

### 案例

[点击查看](.\案例) 

### 插件

插件:增强JQuery的功能

1. 实现方式:
    1. $.fn.extend(object)
        - 增强通过JQuery获取的对象的功能$("#id")
    - JQuery对象进行方法扩展
    ```java
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>01-jQuery对象进行方法扩展</title>
        <script src="../js/jquery-3.3.1.min.js" type="text/javascript" charset="utf-8"></script>
        <script>
           //使用jquery插件 给jq对象添加2个方法 check()选中所有复选框，uncheck()取消选中所有复选框
            //1.定义JQuery的对象插件
            $.fn.extend({
                //定义了一个chck方法,所有的jq对象都可以调用该方法
                check:function(){
                    //让复选框选中
                    //this:代表了调用该方法的jq对象
                    this.prop("checked",true);
                },
                uncheck:function(){
                    //让复选框不选中
                    this.prop("checked",false);
                }
            });
            //DOM文档加载完成之后执行函数
            $(function(){
                //获取按钮绑定单击事件
                $("#btn-check").click(function(){
                    //通过属性选择器获取input标签来调用check方法执行指定函数
                   $("input[type='checkbox']").check();
                });//获取按钮绑定单击事件
                $("#btn-uncheck").click(function(){
                    //通过属性选择器获取input标签来调用uncheck方法执行指定函数
                   $("input[type='checkbox']").uncheck();
                });
            });
        </script>
    </head>
    <body>
    <input id="btn-check" type="button" value="点击选中复选框" onclick="checkFn()">
    <input id="btn-uncheck" type="button" value="点击取消复选框选中" onclick="uncheckFn()">
    <br/>
    <input type="checkbox" value="football">足球
    <input type="checkbox" value="basketball">篮球
    <input type="checkbox" value="volleyball">排球
    </body>
    </html>
    ```
    2. $.extend(object)
        - 增强JQuery对象自身的功能 $/JQuery
    - JQuery全局进行方法扩展
    ```java
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="UTF-8">
        <title>01-jQuery对象进行方法扩展</title>
        <script src="../js/jquery-3.3.1.min.js" type="text/javascript" charset="utf-8"></script>
        <script type="text/javascript">
            //对全局方法扩展2个方法，扩展min方法：求2个值的最小值；扩展max方法：求2个值最大值
            $.extend({
                max:function(a,b){
                    //返回两数中较大值
                    return a >= b? a:b;
                },
                min:function(a,b){
                    //返回两数中较小值
                    return a <= b? a:b;
                }
            });
            //调用全局方法
            //获取较大值
            var max = $.max(2,3);
            alert(max);
            //获取较小值
            var min = $.min(2,3);
            alert(min);
        </script>
    </head>
    <body>
    </body>
    </html>
    ```

