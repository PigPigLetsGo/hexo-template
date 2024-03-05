---
title: REST风格
categories:
    - [计算机学科,java,spring,springmvc]
tags:
    - 计算机学科
    - springmvc
---

# REST

### 概述 

- REST简介(Repersentational State Transfer),表现形式状态转换
    - 传统风格资源描述形式
    - http://loclahost/user/getById?id=1
    - http://localhost/user/saveUser

- REST风格描述形式
    - http://localhost/user/1 
    - http://localhost/user

- 优点:
    - 隐藏资源的访问行为,无法通过地址得知对资源是何种操作
    - 书写简化

- 按照REST风格访问资源时使用<font style="color:red">**行为动作**</font>区分对资源进行了何种操作
    - http://localhost/users         查询全部用户信息    GET(查询)
    - http://localhost/users/1      查询指定用户信息    GET(查询)
    - http://localhost/users         添加用户信息           POST(新增/保存)
    - http://localhost/users         修改用户信息           PUT(修改/更新)
    - http://localhost/users/1      删除用户信息           DELETE(删除)

![![image_2023-03-03-11-03-53](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061103778.png)](REST%E9%A3%8E%E6%A0%BC_md_files/image_2023-03-03-11-03-53_20230307150309.png?v=1&type=image&token=V1:zQ12RX8ah08KPGpWyX_M1Djk4mXiqtwHcxoY3zJstAo)

<font style="color:red">**注意事项:**</font> 

上述行为是约定方式,约定不是规范,可以打破,所以称REST风格,而不是REST规范

描述模块的名称通常使用复数,也就是加s的格式描述,表示此类资源,而非单个资源,例如:users,books,accounts...

- 根据REST风格对资源进行访问称为<font style="color:red">**RESTful**</font> 

### 操作步骤:

1.设定http请求动作

method = RequestMethod.XXX

```java
//    使用REST风格设置save方法为POST请求
    @RequestMapping(value = "/users",method = RequestMethod.POST)
    @ResponseBody
    public String save(){
        System.out.println("springmvc user save running...");
        return "save";
    }
    @RequestMapping(value = "/users/{id}",method = RequestMethod.DELETE)
    @ResponseBody
//    @PathVariable:这个变量的值来自于路径,但是不清楚来自于路径的哪里
    public String delete(@PathVariable Integer id){
        System.out.println("springmvc user delete running..."+id);
        return "delete";
    }
```

2.设定请求参数(路径变量)

@RequestMapping(value = "/users/<font style="color:red">{id}</font>",method = RequestMethod.XXXX)

<font style="color:red">@PathVariabl</font> 

```java
    @RequestMapping( value = "/users/{id}",method = RequestMethod.GET)
    @ResponseBody
    public String UserById(@PathVariable Integer id){
        System.out.println("springmvc user UserById running...");
        return "userbyid";
    }
```

### @RequestMapping

- 类型:方法注解

- 位置:SpringMvc控制器方法定义上方

- 作用:设置当前控制器方法请求访问路径

- 属性:
    - value(默认):请求访问路径
    - method:http请求动作,标准动作(GET/POST/PUT/DELETE)

### @PathVariable

- 类型:形参注解

- 位置:SpringMvc控制器方法形参定义前面

- 作用:绑定路径参数与处理器方法形参间的关系,要求路径参数名与形参名一一对应

**@RequestBody @RequestParam @PathVariable** 

- 区别
    - @RequestParam 用于接收url地址传参或表单传参
    - @RequestBody 用于接收json数据
    - @PathVariable 用于接收路径参数,使用{参数名称}描述路径参数

- <font style="color:red">应用</font> 
    - 后期开发中,发送请求参数超过1个时,以json格式为主,@RequestBody引用较广
    - 如果发送非json格式数据,选用@RequestParam接收请求参数
    - 采用RESTful进行开发,当参数数量较少时,例如1个,可以采用@PathVariable接收请求路径变量,通常用于传递id值

### RESTful快速开发

```java
    @RequestMapping(value = "/users/{id}",method = RequestMethod.DELETE)
    @ResponseBody
//    @PathVariable:这个变量的值来自于路径,但是不清楚来自于路径的哪里
    public String delete(@PathVariable Integer id){
        System.out.println("springmvc user delete running..."+id);
        return "delete";
    }
    @RequestMapping(value = "/users/{u}",method = RequestMethod.PUT)
    @ResponseBody
    public String update(@RequestBody User u){
        System.out.println("springmvc user update running..."+u);
        return "update";
    }
```

简化重复在方法上写@Response和/users路径名

![![image_2023-03-03-18-36-15](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061109617.png)](REST%E9%A3%8E%E6%A0%BC_md_files/image_2023-03-03-18-36-15_20230307150329.png?v=1&type=image&token=V1:oejCepTy_6hcgPfchIWBnq7Ta1iUEofW-gRbLdJWc-c)

- 整体Controller代码

```java
//配置controller
//@Controller
//定义类上整个类生效
//@ResponseBody
//将controller和responsebody两个注解功能合二为一
@RestController
@RequestMapping("/users")
public class UserController {
    //    使用REST风格设置save方法为POST请求
//    @RequestMapping(method = RequestMethod.POST)
//    简化上面的注解功能与上面的method = RequestMethod.POST是对等的
    @PostMapping
    public String save(){
        System.out.println("springmvc user save running...");
        return "save";
    }
//    @RequestMapping(value = "/{id}",method = RequestMethod.DELETE)
//    简化上面注解,有参数加上参数
    @DeleteMapping("/{id}")
//    @PathVariable:这个变量的值来自于路径,但是不清楚来自于路径的哪里
    public String delete(@PathVariable Integer id){
        System.out.println("springmvc user delete running..."+id);
        return "delete";
    }
//    @RequestMapping(value = "/{user}",method = RequestMethod.PUT)
//    简化上面注解
    @PutMapping
    public String update(@RequestBody User user){
        System.out.println("springmvc user update running..."+user);
        return "update";
    }
//    @RequestMapping( value = "/{id}",method = RequestMethod.GET)
//    简化上面注解
    @GetMapping("/{id}")
    public String UserById(@PathVariable Integer id){
        System.out.println("springmvc user UserById running...");
        return "userbyid";
    }
//    @RequestMapping(method = RequestMethod.GET)
//    简化上面注解
    @GetMapping
    public String getAll(){
        System.out.println("springmvc user getAll running...");
        return "getAll";
    }
}
```

### @RestController

- 类型:类注解

- 位置:基于SpringMvc的RESTful开发控制器类定义上方

- 作用:设置当前控制器类为RESTful风格,等同于@Controller与@ResponseBody两个注解组合功能

### @GetMapping,@PostMapping,@PutMapping,@DeleteMapping

- 类型:方法注解

- 位置:基于SpringMvc的RESTful开发控制器方法定义上方

- 作用:设置当前控制器方法请求访问路径与请求动作,每种对应一个请求动作,例如:@GetMapping对应GET请求

- 属性
    - value(默认):请求访问路径

```java
    @DeleteMapping("/{id}")
//    @PathVariable:这个变量的值来自于路径,但是不清楚来自于路径的哪里
    public String delete(@PathVariable Integer id){
        System.out.println("springmvc user delete running..."+id);
        return "delete";
    }
```

### 设置对静态资源的访问放行

- 由于SpringMvcConfig中查找资源设置了`/` 会查找所有的资源作为SpringMvc资源,会导致静态页面资源被SpringMvc拦截走导致访问不到,所以我们需要定义一个类来过滤

```java
/**
 * @author Dkx
 * @version 1.0
 * @3/3/20239:03 PM
 * @function
 * @comment
 * 过滤web静态资源不让Spring拦截走,因为在SpringMvcConfig中的查找资源使用了/为找全部
 */
@SuppressWarnings("all")
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
//        当访问/page/???时不要走MVC，走/page目录下的内容
//        如果访问了/page/**的请求就访问/page/下的内容
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
//        如果访问了/css/**的请求就访问/css/下的内容
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
//        如果访问了/fonts/**的请求就访问/fonts/下的内容
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
//        如果访问了/js/**的请求就访问/js/下的内容
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
    }
}
```

