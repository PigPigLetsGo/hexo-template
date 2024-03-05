---
title: SpringBoot+vue前后端对接，解决跨域问题。
categories:
   - [计算机学科,java,spring,springboot]
tags:
   - 计算机学科
   - springboot
   - 基础
---

# SpringBoot+vue前后端对接，解决跨域问题。

1 启动vue项目

打开终端到vue项目工程的目录下执行命令

```bash
npm run serve
```

2 打开SpringBoot项目,配置跨域Config类

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@SuppressWarnings("all")
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    //解决跨域问题
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**").allowedOriginPatterns("*")
                .allowedMethods("GET","HEAD","POST","PUT","DELETE","OPTIONS")
                .allowCredentials(true).maxAge(3600).allowedHeaders("*");
    }
}
```

3 启动SpringBoot项目请求一下Controller路径,查看是否有问题

![image-20230420155511006](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040859705.png)

4 没问题后在vue项目中填写如下代码来获取SpringBoot请求路径响应的Json数据

```js
created(){
  const _this = this							//then:回调
  axios.get('http://localhost/user/all').then(function(resp){
      for(let i = 0,end = resp.data.data.length;i < end;i ++){
          _this.studentId = resp.data.data[i].studentNumber
          _this.Athome = resp.data.data[i].name
      }
      console.log(resp.data.data[0].name);
     	console.log(resp);
    });
}
```

**跨域失败情况** 

-  如果跨域失败SpringBoot启动访问路径可能会报错500
-  下面是跨域不成功访问vue项目所报的错

![image-20230420160350631](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040859396.png)

**跨域成功情况** 

-  跨域成功请求vue项目所相应的数据

![image-20230420160620159](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040859314.png)
