---
title: Thymeleaf中SpringSecurity的使用
categories:
    - [计算机学科,java,spring,springsecurity]
tags:
    - 计算机学科
    - springsecurity
    - thymeleaf
---

# Thymeleaf中SpringSecurity的使用:star: 

>  SpringSecurity可以在一些视图技术中进行控制显示效果，例如：JSP或Thymeleaf。在非前后端分离且使用SpringBoot的项目中多使用Thymeleaf作为视图展示技术。

>  Thymeleaf对SpringSecurity的支持都放在Thymeleaf-extras-springsecurityX^X代表某一个版本^中，目前最新版本为5。所以需要在项目中添加此jar包的依赖和thymeleaf的依赖。

```xml
<dependency>
  <groupId>org.thymeleaf.extras</groupId>
  <artifactId>thymeleaf-extras-springsecurity5</artifactId>
  <version>3.0.4.RELEASE</version>
</dependency>
<!--thymeleaf依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

在html页面中引入thymeleaf命名空间和security命名空间。

```html
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org"
      xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity5">
</html>
```

自定义登录页面

```java
@Override
protected void configure(HttpSecurity http) throws Exception {
   http
      //关闭csrf防护，类似于关闭防火墙
      .csrf().disable()
      .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS)
      .and()
      //表单提交
      .formLogin()
      //自定义登录界面,thymeleaf通过接口跳转到对应的页面中直接找会报404
      .loginPage("/login")
      //登录成功后跳转到的页面接口
      .successForwardUrl("/index");
   //授权
   http.authorizeRequests()
      //放行,静态资源
      .antMatchers("/css/**","/js/**","/fonts/**","/img/**","/login.html","/login").permitAll()
      .anyRequest().authenticated();

   http.addFilterBefore(jwtAuthenticationTokenFilter,
                        UsernamePasswordAuthenticationFilter.class);
   //把token校验过滤器添加到过滤器链中
   http.addFilterBefore(jwtAuthenticationTokenFilter,
                        //添加过滤器
                        UsernamePasswordAuthenticationFilter.class);
   http.exceptionHandling()
      .authenticationEntryPoint(authenticationEntryPoint)
      .accessDeniedHandler(accessDeniedHandler);
}
```

## 1 获取属性

>  可以在html页面中通过sec:authentication=""获取UsernamePasswordAuthenticationToken中所有getXXX的内容，包含父类中的getXXX的内容。

**根据源码得出下面属性**：

-  name：登录账号名称
-  principal：登录主体，在自定义登录逻辑中是UserDetails
-  credentials：凭证^密码/证据^
-  authorities：权限和角色
-  details：实际上是WebAuthenticationDetails的实例。可以获取remoteAddress(客户端ip)和sessionId(当前sessionId)

```html
账号: <span sec:authentication="name"></span><br>
登录账号: <span sec:authentication="principal.username"></span><br>
<!--处于安全原因凭证显示不出来-->
凭证: <span sec:authentication="credentials"></span><br>
权限和角色: <span sec:authentication="authorities"></span><br>
客户端地址: <span sec:authentication="details.remoteAddress"></span><br>
<!--SessionId也不显示-->
sessionId: <span sec:authentication="details.sessionId"></span><br>
```

**效果**：

![image-20230523095318581](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230523095318581.png)

==权限==就是在UserDetails实现类中查询数据库所添加到Authentication中的。如下：

```java
@SuppressWarnings("all")
@Service
public class UserDetailsServiceImpl implements UserDetailsService {
    @Autowired
    private UserMapper mapper;
    @Autowired
    private RoleMapper roleMapper;

    @Override
    public UserDetails loadUserByUsername(String s) throws UsernameNotFoundException {
        LambdaQueryWrapper<UserTable> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(UserTable::getUsername,s);
        UserTable user = mapper.selectOne(wrapper);
        if(Objects.isNull(user)){
            throw new RuntimeException("用户名或密码错误");
        }
//        通过username查询用户权限
        List<String> list = roleMapper.selectByRole(user.getUsername());
        return new LoginUser(user,list);
    }
}
```

SQL

```sql
<select id="selectByRole" resultType="String">
    SELECT DISTINCT DISTINCT
        mt.roting
    FROM
        user_table AS u
            LEFT JOIN user_role AS ur ON u.role = ur.user_id
            LEFT JOIN role_menu AS rm ON ur.role_id = rm.role_id
            LEFT JOIN menu_table AS mt ON mt.id = rm.menu_id
    WHERE
        u.STATUS = 0
      AND u.username = #{username};
</select>
```

## 1.2 权限判断:ice_cream: 

### 1.2.1 设置用户角色和权限

设定用户具有admin，/insert，/delete 权限 ROLE_abc 角色。

```java
return new User(username,password,AuthorityUtils.commaSeparatedSpringToAuthorityList("admin,ROLE_abc,/insert,/delete"))
```

### 1.2.2 控制页面显示效果

在页面中根据用户权限和角色判断页面中显示的内容

 **通过权限判断**：

```html
<button sec:authorize="hasAuthority('/insert')">新增</button>
<button sec:authorize="hasAuthority('/delete')">删除</button>
<button sec:authorize="hasAuthority('/update')">修改</button>
<button sec:authorize="hasAuthority('/select')">查看</button>
```

**通过角色判断**：

```html
<button sec:authorize="hasRole('abc')">新增</button>
<button sec:authorize="hasRole('abc')">删除</button>
<button sec:authorize="hasRole('abc')">修改</button>
<button sec:authorize="hasRole('abc')">查看</button>
```

## 2 退出登录:taco: 

用户只需要向Spring Security项目中发送 ==/logout== 退出请求即可。

### 2.1 退出登录:tada: 

实现退出非常简单，只要在页面中添加 ==/logout== 的超链接即可。

```html
<a href="/logout">注销</a>
```

