---
title: BaseServlet的抽取，简化开发
categories:
   - [计算机学科,java,servlet]
tags:
   - 计算机学科
   - java
   - servlet
   - 基础
---

# 优化Servlet

> 减少Servlet的数量,现在是一个功能一个Servlet,将其优化为一个模块一个Servlet,相当于在数据库中一张表对应一个Servlet,在Servlet中提供不同的方法,完成用户的请求

![image_2023-01-22-14-22-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-22-14-22-05.png)

> 将所有相关的操作写到一个类中来减少代码的冗余度,比如一个模块如:用户登录,用户查询是一个模块,而商品查询,旅游景点则是另一个模块

- Servlet继承自HttpServlet而HttpServlet根据请求方式不同完成doPost或doGet的分发

代码案例:

1. 创建一个`BaseServlet` 类,类中定义一个`service` 方法

```java
@SuppressWarnings("all")
public class BaseServlet extends HttpServlet {
    protected void service(HttpServletRequest req,HttpServletResponse resp){
//        System.out.println("baseServlet的service方法被执行了!");
//        完成方法分发
//        1.获取请求路径
        String uri = req.getRequestURI();// /dkx/user/add : 截取字符串最后一个/就可以拿到add了
//        2.获取方法名称
        String add = uri.substring(uri.lastIndexOf("/")+1);//截取包头不含尾,lastIndexOf+1来去掉/
//        3.获取方法对象method
        /*
        this:谁调用我,我就代表谁,而这个service又是被UserServlet调用的所以this代表的是UserServlet对象
         */
        try{
            Method method = this.getClass().getMethod(add,HttpServletRequest.class,HttpServletResponse.class);
//        4.执行方法
            method.invoke(this,req,resp);
        }catch(Exception e){
            e.printStackTrace();
            
        }
    }
}
```

2. 创建`UserServlet` 类而让该类继承`BaseServlet` 类,该类虚拟目录为user/*

```java
@WebServlet("/user/*")
public class UserServlet extends BaseServlet {
    public void add(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        p("UerServlet的add方法");
    }

    public void find(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        p("UerServlet的find方法");
    }
}
```

> UserServlet继承了BaseServlet当我们访问UserServlet的方法时就会调用BaseServlet中的service方法,我们可以创建多个模块来进行管理

3. 创建 `CategoryServlet` 类,让该类也继承`BaseServlet` 类

```java
@WebServlet("/category/*")
public class CategoryServlet extends BaseServlet {
    public void add(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        p("CategoryServlet的find方法");
    }

    public void find(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        p("CategoryServlet的find方法");
    }
}
```

- 当访问虚拟目录/category/*时调用的是CategoryServlet的方法对象

> 添加一些使用频繁的操作比如:`json序列化` 

```java
/**
 * 直接将传入对象序列化为json,并且写回客户端
 * @param obj
 */
public void writeValue(Object obj,HttpServletResponse response) throws IOException {
    try{
        ObjectMapper mapper = new ObjectMapper();
        response.setContentType("application/json;charset=utf-8");
        mapper.writeValue(response.getOutputStream(),obj);
    }catch(Exception e){
        e.printStackTrace();
        p(e);//* 无限递归,已经解决
    }
}
/**
 * 将传入的对象序列化为json,返回
 * @param obj
 */
public String writeAsString(Object obj) throws JsonProcessingException {
    ObjectMapper mapper = new ObjectMapper();
    return mapper.writeValueAsString(obj);
}
```

调用方法类

```java
public void login(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    //1.获取用户名和密码数据
    Map<String, String[]> map = request.getParameterMap();
    //2.封装User对象
    User user = new User();
    try {
        BeanUtils.populate(user,map);
    } catch (IllegalAccessException e) {
        e.printStackTrace();
    } catch (InvocationTargetException e) {
        e.printStackTrace();
    }
    //3.调用Service查询
    User u  = service.login(user);
    ResultInfo info = new ResultInfo();
    //4.判断用户对象是否为null
    if(u == null){
        //用户名密码或错误
        info.setFlag(false);
        info.setErrorMsg("用户名密码或错误");
    }
    //5.判断用户是否激活
    if(u != null && !"Y".equals(u.getStatus())){
        //用户尚未激活
        info.setFlag(false);
        info.setErrorMsg("您尚未激活，请激活");
    }
    //6.判断登录成功
    if(u != null && "Y".equals(u.getStatus())){
        request.getSession().setAttribute("user",u);//登录成功标记
        //登录成功
        info.setFlag(true);
    }
    //响应数据
    writeValue(info,response);
}
```

