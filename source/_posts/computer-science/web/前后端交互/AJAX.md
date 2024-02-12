---
title: AJAX
categories:
   - [计算机学科,web,前后端交互]
tags:
   - 计算机学科
   - web
   - 前后端交互
---

## AJAX

### 概念:ASynchronous JavaScript And XML 异步的JavaScript和XML

作用: 提升用户的体验

1. 异步和同步:客户端和服务器端相互通信的基础上
    - 同步: 客户端必须等待服务器端的响应,在等待期间客户端不能做其它事情
    - 异步: 客户端不需要等待服务器端的响应,在服务器端处理请求的过程中,客户端可以进行其它的操作

![image_2022-12-26-19-23-35](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2022-12-26-19-23-35.png)

Ajax是一种用于创建快速动态网页的技术

Ajax是一种在无需重新加载整个网页的情况下,能够更新部分网页的技术

通过在后台与服务器进行少量数据交换,Ajax可以使网页实现异步更新,这意味着可以在不重新加载整个网页的情况下,对网页的某部分进行更新

传统的网页(不使用Ajax)如果需要更新内容,必须重载整个网页页面

### 实现方式

1. 原生的js实现方式(了解)

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/jquery-3.3.1.min.js"></script>
    <script src="./js/jquery-migrate-1.2.1.js"></script>
    <script>
        //定义方法
        function fun(){
            //发送异步请求
            //创建核心对象
            var xmlhttp;
            if(window.XMLHttpRequest){
                xmlhttp = new XMLHttpRequest();
            }else{
                xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
            }
            //建立连接
            /**
             * 参数:
             *  1.请求方式:GET,POST
             *      - get方式,请求参数在URL后边拼接,send方法为空参
             *      - post方法,请求参数在send方法中定义
             *  2.请求的URL:
             *  3.同步或异步请求:true(异步)或false(同步)
             */  
            xmlhttp.open("GET","/ajaxServlet?username=tom",true);
            //发送请求
            xmlhttp.send();
            //接受并处理来自服务器的响应结果
            //获取方式:xmlhttp.responseText
            //什么时候获取?当服务器响应成功后再获取
            //当xmlhttp对象的就绪状态改变时,触发事件 onreadystatechange
            xmlhttp.onreadystatechange = function(){
                //判断readyState就绪状态是否为4,判断status响应状态码是否为200
                if(xmlhttp.readyState==4 && xmlhttp.status==200){
                    //获取服务器的响应结果
                    var responseText = xmlhttp.responseText;
                    alert(responseText);
                }
            }
        }
    </script>
</head>
<body>
<input type="button" value="发送异步请求" onclick="fun()">
<input type="text">
</body>
</html>
```

2. JQuery实现方式
    1. $.ajax()
        -  每个键值对尾都需要加上逗号,否则会报错而最后一行尾建议不要加逗号
        -  参数:
           1.  url:请求路径
               -  格式:url:"路径"
           2.  data:请求参数
               -  格式:data:{"username":"jack","age":"123"}
           3.  callback:回调函数
               -  格式:success:function( ){  }
           4.  type:响应结果的类型
               -  格式:type:"GET"
    
    ```js
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script src="./js/jquery-3.3.1.min.js"></script>
        <script src="./js/jquery-migrate-1.2.1.js"></script>
        <script>
            //定义方法
            function fun(){
            let username = $('#username').val()
            let password = $('#password').val()
                //使用$.ajax()方式来发送异步请求
            let user = {
              'username':username,
              'password':password
            }
                $.ajax({
                    //每个键值对后需要加上逗号否则会报错,但是最后一个键值对不要加逗号否则浏览器会报错           //请求路径
                    url:"/user/login",
                    //请求方式
                    type:"POST",
                    // data:"username=jack&age=123"
                    // http://localhost/user/login?username=jack&age=123
                    data:{"username":"jack","age":"123"},//请求参数
           		    //或者使用以下发送数据
                    data: JSON.stringify(user),
                    //设置接收到的响应数据的格式
                    dataType:"json",
                    //设置发送给服务器的数据格式
                    contentType: 'application/json',
                    //设置用异步或同步方式执行脚本true为异步，false为同步
                    async: true/false,
                    //响应成功后的回调函数
                    success:function(a){
                        alert(a);
                    },
                    //如果请求响应出错,会执行的回调函数
                    error:function(){
                        alert("出错啦");
                    }
                });
            }
        </script>
    </head>
    <body>
    <input type="button" value="异步发送请求" onclick="fun()">
    </body>
    </html>
    ```
    
    
    
    2.  $.get():发送get请求
    
        - 语法:$.get(url,[data],[callback],[type])
            - 参数:
            1. url:请求路径
            2. data:请求参数
            3. callback:回调函数
            4. type:响应结果的类型
    
    3.  $.post():发送post请求
    
        -  语法:$.post(url,[data],[callback],[type])
    
           - 参数:
    
           1. url:请求路径
    
           2.  data:请求参数
    
           3.  callback:回调函数
    
           4.  type:响应结果的类型

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="./js/jquery-3.3.1.min.js"></script>
    <script src="./js/jquery-migrate-1.2.1.js"></script>
    <script>
        //定义方法
        // function fun(){
        //     $.get("ajaxServlet",{username:"tom",age:"18"},function(a){
        //         alert(a);
        //     },"text");
        // }
        //定义方法
        function fun(){
            //                路径                         参数                  回调函数
            $.post("ajaxServlet",{username:"tom",age:"18"},function(a){
                //弹出窗口
                alert(a);
                //类型
            },"text");
        }
    </script>
</head>
<body>
<input type="button" value="异步发送请求" onclick="fun()">
</body>
</html>
```

