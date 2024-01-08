---
title: form-serialize插件，快速收集表单元素的值
categories:
    - [tools]
tags:
    - 工具
    - web
---

## form-serialize插件

**作用**：==快速==收集表单元素的值

![image-20230813085042213](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308130850440.png)

这么多的用户填写的表单信息我们如果一个一个获取的话会很麻烦，可以使用form-serialize插件来完成。

##### 语法：

```js
//获取表单对象
const form = ducment.querySelector('.example-form')
//serialize函数 参数1：指定获取哪个表单范围内的值传入表单对象，参数2：传入配置对象
const data = serialize(form,{hash: true , empty: true})
```

![image-20230813085338699](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308130853093.png)

无论有多少个表单元素都可以快速的一步到位的全部收集出来。

```js
<body>
    <form action="javaScript:;" class="form-data" method="POST">
        <input type="text" name="username" />
        <br>
        <input type="password" name="password" />
        <br>
        <input type="submit" class="btn" value="提交" />
    </form>
    <script type="text/javaScript" src="./js/form-serialize.js"></script>
    <script>
        document.querySelector('.btn').addEventListener('click', function () {
            // 使用form-serialize函数，快速收集表单元素值
            // 参数1：要获取哪个表单的数据
            //      表单元素设置name属性，值会作为对象的属性名
            //      建议name属性的值，最好和接口文档参数名一致，否则后端接收不不到数据就会报错
            // 参数2：配置对象
            //      hash: 设置获取数据结构
            //           -  true ：JSON格式的JS对象数据
            //           -  false：查询字符串 url ？后面的
            //      empty: 设置是否获取空值
            const form = document.querySelector('.form-data')
            const data = serialize(form, { hash: false, empty: true })
            console.log(data)
        })
    </script>
</body>
```

**效果**：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308150957805.gif)

### hash：参数的true跟false的区别

-  true ：JSON格式的JS对象数据
-  false：查询字符串 url ？后面的

```js
const data = serialize(form, { hash: true, empty: true })
```

**结果**：

![image-20230815100345134](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151003940.png)

```js
const data = serialize(form, { hash: false, empty: true })
```

**结果**：

![image-20230815100416896](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308151004712.png)
