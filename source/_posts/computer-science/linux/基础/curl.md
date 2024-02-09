---
title: linux - curl的用法指南
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

## curl的用法指南

**简介** 

curl是常用的命令工具,用来请求web服务器,它的名字就是客户端(client)的URL工具的意思
的

它的功能非常强大,命令行参数多达几十种,如果熟练的话,完全可以取代Postman这一类的图形界面工具

![image_2023-02-01-07-44-09](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-01-07-44-09.png)

不带任何参数时,curl就是发出GET请求

`curl https://www.example.com` 

上面命令向<font style="color:red">www.example.com</font>发出GET请求,服务器返回的内容会在命令行输出

**-A** 

<font style="color:red">-A</font>参数指定客户端的用户代理标头,及<font style="color:red">User-Agent</font>.curl的默认用户代理字符串是<font style="color:red">curl/[version]</font>

`curl -A 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36' https://google.com` 

上面个命令将<font style="color:red">User_Agent</font>改成Chrome浏览器

`curl -A '' https://google.com` 

上面个命令会移除<font style="color:red">User-Agent</font>标头

也可以通过<font style="color:red">-H</font>参数直接指定标头,更改<font style="color:red">User-Agent</font>

`curl -H 'User-Agent: php/1.0' https://google.com` 

**-b** 

<font style="color:red">-b</font>参数用来向服务器发送Cookie

`curl -b 'foo=bar' https://google.com` 

上面命令会生成一个标头<font style="color:red">Cookie:foo=bar</font>,向服务器发送一个名为<font style="color:red">foo</font>,值为<font style="color:red">bar</font>的Cookie

`curl -b 'foo1=bar;foo2=bar2' https://google.com` 

上面命令发送两个Cookie

`curl -b cookies.txt https://www.google.com` 

上面命令读取本地文件<font style="color:red">cookies.txt</font>,这里面是服务器设置的Cookie(参见<font style="color:red">-c</font>参数),将其发送到服务器

**-c** 

<font style="color:red">-c</font>参数将服务器设置的Cookie写入一个文件

`curl -c cookies.txt https://www.google.com` 

上面命令将服务器的HTTP回应所设置Cookie写入文本文件<font style="color:red">cookies.txt</font>

**-d** 

<font style="color:red">-d</font>参数用于发送POST请求的数据体

```
curl -d'login=emma＆password=123'-X POST https://google.com/login
# 或者
curl -d 'login=emma' -d 'password=123' -X POST  https://google.com/login
```

使用<font style="color:red">-d</font>参数以后,HTTP请求会自动加上标头

<font style="color:red">Content-Type:application/x-www-form-urlencoded</font>,并且会自动请求转为POST方法,因此可以省略<font style="color:red">-X POST</font>

<font style="color:red">-d</font>参数可以读取本地文本文件的数据,向服务器发送

`curl -d '@data.txt' https://google.com/login` 

上面个命令读取<font style="color:red">data.txt</font>文件的内容,作为数据体向服务器发送

**--data-urlencode** 

<font style="color:red">--data-urlencode</font>参数等同于<font style="color:red">-d</font>,发送POST请求的数据体,区别在于会自动将发送的数据进行URL编码

`curl --data-urlencode 'comment=hello world' https://google.com/login` 

上面改代码中,发送的数据<font style="color:red">hello world</font>之间有一个空格,需要进行URL编码

**-e** 

<font style="color:red">-e</font>参数用来设置HTTP的标头<font style="color:red">Referer</font>,表示请求的来源

`curl -e 'https://google.com?q=example' https://www.example.com` 

上面命令将<font style="color:red">Referer</font>标头设为<font style="color:red">https://google.com?q=example</font>

<font style="color:red">-H</font>参数可以用过直接添加标头<font style="color:red">Referer</font>,达到同样效果

`curl -H 'Referer: https://google.com?q=example' https://www.example.com` 

**-F** 

<font style="color:red">-F</font>参数用来向服务器上传二进制文件

`curl -F 'file=@photo.png' https://google.com/profileH` 

上面命令会给HTTP请求加上标头<font style="color:red">Content-Type: multipart/form-data</font>,返回将文件<font style="color:red">photo.png</font>,作为<font style="color:red">file</font>字段上传

<font style="color:red">-F</font>参数可以指定MIME类型

` curl -F 'file=@photo.png;type=image/png' https://google.com/profile` 

上面命令指定MIME类型为<font style="color:red">image/png</font>,否则curl会把MIME类型设为<font style="color:red">application/octet-stream</font>

<font style="color:red">-F</font>参数也可以指定文件名

`curl -F 'file=@photo.png;filename=me.png' https://google.com/profile` 

上面命令中,原始文件名为<font style="color:red">photo.png</font>,但是服务器接收到的文件名为<font style="color:red">me.png</font>

**-G** 

<font style="color:red">-G</font>参数用来构造URL的查询字符串

`curl -G -d 'q=kitties' -d 'count=20' https://google.com/search` 

上面命令会发出一个GET请求,实际请求的URL为<font style="color:red">https://google.com/search?q=kitties&count=20</font>,如果省略<font style="color:red">-G</font>,会发出一个POST请求

如果数据需要URL编码,可以结合<font style="color:red">--data--urlencodeH</font>参数

`curl -G --data-urlencode 'comment=hello world' https://www.example.com` 

**-H** 

<font style="color:red">-H</font>参数添加HTTP请求的标头

`curl -H 'Accept-Language: en-US' https://google.com` 

上面命令添加HTTP标头<font style="color:red">Accept-Language: en-US</font>

`curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' https://google.com` 

上面命令添加两个HTTP标头

`curl -d '{"login": "emma", "pass": "123"}' -H 'Content-Type: application/json' https://google.com/login` 

上面命令添加HTTP请求的标头是<font style="color:red">Content-Type: application/json</font>,然后用<font style="color:red">-d</font>参数发送JSON数据

**-i** 

<font style="color:red">-i</font>参数打印出服务器回应的HTTP标头

`curl -i https://www.example.com` 

上面命令收到服务器回应后,先输出服务器回应的标头,然后空一行,在输出网页的源码

**-I** 

<font style="color:red">-I</font>参数向服务器发送HEAD请求,然后会将服务器返回的HTTP标头打印出来

`curl -I https://www.example.com` 

上面命令输出服务器对HEAD请求的回应

<font style="color:red">-head</font>参数等同于<font style="color:red">-I</font>

`curl --head https://www.example.com`

**-K** 

<font style="color:red">-K</font>参数指定跳过SSL检测

`curl -k https://www.example.com` 

上面命令不会检查服务器的SSL证书是否正确

**-L** 

<font style="color:red">-L</font>参数会让HTTP请求跟随服务器的重定向,curl默认不跟随重定向

`curl -L -d 'tweet=hi' https://api.twitter.com/tweet` 

**--limit-rate** 

<font style="color:red">--limit-rate</font>用来限制,HTTP请求和回应的宽带,模拟慢网速的环境

`curl --limit-rate 200k https://google.com` 

上面命令将带宽限制在每秒200k字节

**-o** 

<font style="color:red">-o</font>参数将服务器的回应保存成文件,等同于<font style="color:red">wget</font>

上面命令将<font style="color:red">www.example.com</font>保存成<font style="color:red">example.html。</font>

**-O** 

<font style="color:red">-O</font>参数将服务器回应保存成文件,并将URL的最后部分当作文件名

`curl -O https://www.example.com/foo/bar.html` 

上面命令将服务器回应保存成文件,文件名为<font style="color:red">bar.html</font>

**-s** 

<font style="color:red">-s</font>参数将不输出错误和进度信息

`curl -s https://www.example.com` 

上面命令一旦发生错误,不会显示错误信息,不发生错误的话,会正常显示运行结果

如果想让curl不产生任何输出,可以使用下面的命令

`curl -s -o /dev/null https://google.com` 

**-S** 

<font style="color:red">-S</font>参数指定只输出错误的信息,通常与<font style="color:red">-s一起使用</font>

`curl -s -o /dev/null https://google.com` 

上面命令没有任何输出,除非发生错误

**-u** 

<font style="color:red">-u</font>参数用来设置服务器认证的用户名和密码

` curl -u 'bob:12345' https://google.com/login` 

上面命令设置用户名为<font style="color:red">bob</font>,密码为<font style="color:red">12345</font>,然后将其转为HTTP标头<font style="color:red">Authorization: Basic Ym9iOjEyMzQ1</font>.

curl能够识别URL里面的用户名和密码

`curl https://bob:12345@google.com/login` 

上面命令能够识别URL里面的用户名和密码,将其转为上个例子里面的HTTP标头

`curl -u 'bob' https://google.com/login` 

上面命令只设置了用户名,执行后,curl会提示用户输入密码

**-v** 

<font style="color:red">-v</font>参数输出通信的整个过程,用于调式

` curl -v https://www.example.com` 

<font style="color:red">-trace</font>参数也可以用于调式,还会输出原始的二进制数据

` curl --trace - https://www.example.com` 

**-x** 

<font style="color:red">-x</font>参数指定HTTP请求的代理

`curl -x socks5://james:cats@myproxy.com:8080 https://www.example.com` 

上面命令指定HTTP请求通过<font style="color:red">myproxy.com:8081</font>cks
5代理发出

如果没有指定代理协议,默认为HTTP

`curl -x james:cats@myproxy.com:8080 https://www.example.com` 

上面命令中,请求的代理使用HTTP协议

**-X** 

<font style="color:red">-X</font>参数指定HTTP请求的方法

`curl -X POST https://www.example.com` 

上面命令对<font style="color:red">https://www.example.com</font>发出POST请求
