---
title: WebSocket
categories:
    - [计算机学科,web,websocket]
tags:
    - 计算机学科
    - web
    - websocket
---

## 1 WebSocket:star: 

#### 1.1 WebSocket介绍:christmas_tree: 

WebSocket是一种网络通信协议。RFC6455定义了它的通信标准

WebSocket是HTML5开始提供的一种在单个TCP连接上进行双工通讯的协议

HTTP协议是一种无状态的，无连接的，单向的应用层协议。它采用了请求/响应模型。通信请求只能由客户端发起，服务端对请求做出答应处理。

这种通信模型有一个弊端：HTTP协议无法实现服务器主动向客户端发起消息。

这种单向请求的特点，注定了如果服务器有连续的变化，客户端要获知就非常麻烦。大多数Web应用程序将通过频繁的异步AJAX请求实现长轮询。轮询的效率低，非常浪费资源 (因为必须不停链接，或者HTTP连接始终打开)

http协议：

![image-20230916214845144](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309162148534.png)

websocket协议

![image-20230916214919593](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309162149776.png)

#### 1.2 websokcet协议:christmas_tree: 

本协议有两种部分：握手和数据传输。

握手是基于http协议的。

来自客户端的握手看起来像如下形式：

```
// ws协议，如果使用的是http那么就是http协议
GET ws://localhost/chat HTTP/1.1
Host: localhost
// 升级连接为websocket
Upgrade: websocket
// 表示连接为升级连接
Connection: Upgrade
Sec-websocket-Key: dGhlIHNhbxBsZSBub25jZQ==
Sec-websocket-Extensions: permessage-deflate
Sec-websocket-Version: 13
```

来自服务器的握手看起来像如下形式：

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-websocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbk+XOo=
Sec-websocket-Extensions: permessage-deflate
```

字段说明：

| 头名称                   | 说明                                                         |
| ------------------------ | ------------------------------------------------------------ |
| Connection:Upgrade       | 标识该HTTP请求是一个协议升级请求                             |
| Upgrade:WebSocket        | 协议升级为websocket协议                                      |
| Sec-websocket-version:13 | 客户端支持websocket的版本                                    |
| Sec-websocket-key:       | 客户端采用base64编码的24位==随机==字符序列，服务器==接收客户端==HTTP协议升级的==证明==。要求服务端响应一个对应加密的Sec-websocket-Accept头信息作为应答 |
| Sec-websocket-Extensions | 协议扩展类型                                                 |

#### 1.3 客户端 (浏览器) 实现:christmas_tree: 

##### 1.3.1 websocket对象:deciduous_tree: 

实现websocket的web浏览器将通过websocket对象公开所有必需的客户端功能 (主要指支持Html5的浏览器)。

<span alt='solid'>以API用于创建websocket对象</span>：

```js
let ws = new WebSocket(url)
```

>  参数url格式说明：ws://ip地址:端口号/资源名称
>
>  url：请求的地址
>
>  ws：协议从http协议变成了ws协议
>
>  端口号：如果是80默认可以省略不写

##### 1.3.2 websocket事件:deciduous_tree: 

websocket对象的相关事件：

| 事件    | 事件处理程序            | 描述                       |
| ------- | ----------------------- | -------------------------- |
| open    | websocket对象.open      | 连接建立时触发             |
| message | websocket对象.onmessage | 客户端接收服务端数据时触发 |
| error   | websocket对象.onerror   | 通信发生错误时触发         |
| close   | websocket对象.onclose   | 连接关闭时触发             |

##### 1.3.3 websocket方法:deciduous_tree: 

websocket对象的相关方法：

| 方法   | 描述             |
| ------ | ---------------- |
| send() | 使用连接发送数据 |

#### 1.4 服务端实现:christmas_tree: 

Tomcat的7.0.5版本开始支持websocket，并且实现了Java websocket规范 (JSR356)。

Java websocket应用由一系列的websocketEndpoint组成。Endpoint还一个Java对象，代表websocket链接的一端，对于服务端，我们可以视为处理具体websocket消息的接口，就像Servlet与http请求一样。

服务端，我们可以视为处理具体websocket消息的接口，就像Servlet之与http请求一样。

我们可以通过两种方式定义Endpoint

-  第一种是编程式，即继承类 javax.websocket.Endpoint并实现其方法。
-  第二种是注解式，即定义一个POJO，并添加@ServerEndpoint相关注解。

Endpoint实例在websocket握手时创建，并在客户端与服务端链接过程中有效，最后在连接关闭时结束。在Endpoint接口中明确定义了与其生命周期相关的方法，规范实现者确保生命周期的各个阶段调用实例的相关方法。生命周期方法如下：

| 方法    | 含义描述                                                     | 注解     |
| ------- | ------------------------------------------------------------ | -------- |
| onClose | 当会话关闭时调用                                             | @onClose |
| onOpen  | 当开启一个新的会话时调用，该方法是客户端与服务端握手成功后调用的方法 | @onOpen  |
| onError | 当连接过程中异常时调用                                       | @onError |

##### 服务端如何 接收客户端发送的数据呢？

通过为session添加MessageHandler消息处理器来接收消息，当采用注解方式定义Endpoint时，我们还可以通过@onMessage注解指定接收消息的方法。

<blockquote alt='danger'>
	<div>
      <p>
         <span><span alt='solid'>注意</span></span>：这里的session不是http中的session而是onClose,onOpen,onError重写的函数中的session对象</span>
      </p>
      <p>
         <span>这个session可以通过一个对象，而这个对象就可以给客户端发送数据了</span>
      </p>
   </div>
</blockquote>

##### 服务端如何推送数据给客户端呢？

发送消息则由RemoteEndpoint完成，其实例由Session维护，根据使用情况，我们可以通过Session.getBasicRemote获取同步消息发送的实例，然后调用其sedXxx()方法就可以发送消息，可以通过Session.getAsyncRemote获取异步消息发送实例。

<span alt='solid'>服务端代码</span>： 

```java
/**
 * @version v1.0
 * @ClassName: ChatEndpoint
 * @Description: TODO(一句话描述该类的功能)
 * @Author: 黑马程序员
 */
@ServerEndpoint(value = "/chat",configurator = GetHttpSessionConfigurator.class)
@Component
public class ChatEndpoint {

    //用来存储每一个客户端对象对应的ChatEndpoint对象
    private static Map<String,ChatEndpoint> onlineUsers = new ConcurrentHashMap<>();

    //声明session对象，通过该对象可以发送消息给指定的用户
    private Session session;

    //声明一个HttpSession对象，我们之前在HttpSession对象中存储了用户名
    private HttpSession httpSession;

    @OnOpen
    //连接建立时被调用
    public void onOpen(Session session, EndpointConfig config) {
        
    }

    private void broadcastAllUsers(String message) {
        
    }

    private Set<String> getNames() {
        
    }

    @OnMessage
    //接收到客户端发送的数据时被调用
    public void onMessage(String message,Session session) {
        
    }

    @OnClose
    //连接关闭时被调用
    public void onClose(Session session) {

    }
}
```



## 2 基于websocket的网页聊天室:star: 

#### 2.1 实现流程:christmas_tree: 

![image-20230916225558256](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309162256391.png)

发送websocket请求就是“打开会话” 和服务端建立连接，其中有一个对象就是Endpoint对象，而Endpoint对象有三个注解标注的三个方法。

打开会话的话首先@onOpen注解标记的这个方法它会被自动调用，而在这个方法里面我们首先要做的就是记录Session，因为到时候可以通过Session给客户端主动的推送消息。

第二个就是HttpSession，为什么要获取HttpSession呢？这是Http里面的啊。因为我们登录的时候已经把用户名存储在了HttpSession里面所以我们通过HttpSession可以获取当前登录这个用户名。

最后要广播这个消息，为什么要广播呢？因为实现还有登录后自己页面中显示哪个好友登录了这个就需要用到广播消息。把这些已经登录了的用户名广播给(或者说推送给) 客户端，客户端进行一个展示。展示的话当成两部分，第一部分：好友列表展示。第二部分：系统广播的一个展示。

给一个好用发送消息之后@onMessage注解标记的方法就会被调用，调用之后要做的事儿就是解析这个消息，然后判断接收这个消息的人是给谁发的。比如：发送人张三-》接收人 李四

当关闭连接后就会调用@onClose注解标记的方法，然后将某人关闭连接的消息 广播给其他的用户。

#### 2.2 消息格式:christmas_tree: 

-  客户端  - - > 服务端

   `{"toName":"张三", "message":"你好"}` 

-  服务端 - - > 客户端

   系统消息格式：`{"isSystem":true, "forName":null, "message":["李四", "王五"]}` 

   推送给某一个的消息格式：`{"isSystem":false, "forName":"张三", "message":"你好"}` 

#### 2.3 功能实现:christmas_tree: 

##### 2.3.1 创建项目，导入相关jar包:deciduous_tree: 

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.dkx</groupId>
    <artifactId>it-chatDemo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>it-chatDemo</name>
    <description>it-chatDemo</description>
    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-websocket</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

#### 后端代码

解决跨域问题

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

/**
 * @author Dkx
 * @version 1.0
 * @2023/9/1820:31
 * @function
 * @comment
 */

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

message类

```java
public class Message {
    private String toName;
    private String message;

    public String getToName() {
        return toName;
    }

    public void setToName(String toName) {
        this.toName = toName;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

}
```

result类

```java
public class Result {
    private boolean flag;
    private String message;

    public String getToken() {
        return token;
    }

    public void setToken(String token) {
        this.token = token;
    }

    private String token;

    public boolean isFlag() {
        return flag;
    }

    public void setFlag(boolean flag) {
        this.flag = flag;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}
```

resultmessage类

```java
public class ResultMessage {
    private boolean isSystem;
    private String formName;
    private Object message;// 如果是系统消息是数组

    public boolean isSystem() {
        return isSystem;
    }

    public void setSystem(boolean system) {
        isSystem = system;
    }

    public String getFormName() {
        return formName;
    }

    public void setFormName(String formName) {
        this.formName = formName;
    }

    public Object getMessage() {
        return message;
    }

    public void setMessage(Object message) {
        this.message = message;
    }
}
```

user类

```java
public class User {
    private String username;
    private String password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```

messageUtils类

```java
import com.dkx.pojo.ResultMessage;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;

/**
 * @author Dkx
 * @version 1.0
 * @2023/9/1819:46
 * @function
 * @comment
 */
public class MessageUtils {
    public static String getMessage(boolean isSystemMessage, String formName, Object message) {
        try {
            ResultMessage result = new ResultMessage();
            result.setSystem(isSystemMessage);
            result.setMessage(message);
            if(formName != null) {
                result.setFormName(formName);
            }
            ObjectMapper mapper = new ObjectMapper();
            return mapper.writeValueAsString(result);
        } catch (JsonProcessingException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

chatendpoint类

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.dkx.pojo.Message;
import com.dkx.utils.MessageUtils;
import jakarta.servlet.http.HttpSession;
import jakarta.websocket.*;
import jakarta.websocket.server.ServerEndpoint;
import org.springframework.stereotype.Component;

import java.util.Map;
import java.util.Set;
import java.util.concurrent.ConcurrentHashMap;

/**
 * @version v1.0
 * @ClassName: ChatEndpoint
 * @Description: TODO(一句话描述该类的功能)
 * @Author: 黑马程序员
 */
@ServerEndpoint(value = "/chat",configurator = GetHttpSessionConfigurator.class)
@Component
public class ChatEndpoint {

    //用来存储每一个客户端对象对应的ChatEndpoint对象
    private static Map<String,ChatEndpoint> onlineUsers = new ConcurrentHashMap<>();

    //声明session对象，通过该对象可以发送消息给指定的用户
    private Session session;

    //声明一个HttpSession对象，我们之前在HttpSession对象中存储了用户名
    private HttpSession httpSession;

    @OnOpen
    //连接建立时被调用
    public void onOpen(Session session, EndpointConfig config) {
        //将局部的session对象赋值给成员session
        this.session = session;
        //获取httpSession对象
        HttpSession httpSession = (HttpSession) config.getUserProperties().get(HttpSession.class.getName());
        this.httpSession = httpSession;

        //从httpSession对象中获取用户名
        String username = (String) httpSession.getAttribute("user");

        //将当前对象存储到容器中
        onlineUsers.put(username,this);

        //将当前在线用户的用户名推送给所有的客户端
        //1,获取消息
        String message = MessageUtils.getMessage(true, null, getNames());
        //2,调用方法进行系统消息的推送
        broadcastAllUsers(message);
    }

    private void broadcastAllUsers(String message) {
        try {
            //要将该消息推送给所有的客户端
            Set<String> names = onlineUsers.keySet();
            for (String name : names) {
                ChatEndpoint chatEndpoint = onlineUsers.get(name);
                chatEndpoint.session.getBasicRemote().sendText(message);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private Set<String> getNames() {
        return onlineUsers.keySet();
    }

    @OnMessage
    //接收到客户端发送的数据时被调用
    public void onMessage(String message,Session session) {
        try {
            //将message转换成message对象
            ObjectMapper mapper = new ObjectMapper();
            Message mess = mapper.readValue(message, Message.class);
            //获取要将数据发送给的用户
            String toName = mess.getToName();
            //获取消息数据
            String data = mess.getMessage();
            //获取当前登陆的用户
            String username = (String) httpSession.getAttribute("user");
            //获取推送给指定用户的消息格式的数据
            String resultMessage = MessageUtils.getMessage(false, username, data);
            //发送数据
            onlineUsers.get(toName).session.getBasicRemote().sendText(resultMessage);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @OnClose
    //连接关闭时被调用
    public void onClose(Session session) {

        String username = (String) httpSession.getAttribute("user");
        //从容器中删除指定的用户
        onlineUsers.remove(username);
        //获取推送的消息
        String message = MessageUtils.getMessage(true, null, getNames());
        broadcastAllUsers(message);
    }
}
```

 GetHttpSessionConfigurator 类

```java
import jakarta.servlet.http.HttpSession;
import jakarta.websocket.HandshakeResponse;
import jakarta.websocket.server.HandshakeRequest;
import jakarta.websocket.server.ServerEndpointConfig;
import org.springframework.stereotype.Component;

/**
 * @author Dkx
 * @version 1.0
 * @2023/9/1811:39
 * @function
 * @comment
 */

// 继承ServerEndpointConfig里面的内部类Configurator
@Component
public class GetHttpSessionConfigurator extends ServerEndpointConfig.Configurator {

    // 重写modifyHandshake方法里面有一个参数HandshakeRequest有Request的话我们就可以拿到HttpSession对象了
    @Override
    public void modifyHandshake(ServerEndpointConfig sec, HandshakeRequest request, HandshakeResponse response) {
        HttpSession httpSession = (HttpSession) request.getHttpSession();
        // 将HttpSession对象存储到配置对象中
        //ServerEndpointConfig 对象 里面继承了EndpointConfig对象
        // 而在ChatEndpoint类中的OnOpen函数中参数就有一个EndpointConfig对象
        sec.getUserProperties().put(HttpSession.class.getName(), httpSession);
    }
}
```

websocketconfig类

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;

/**
 * @author Dkx
 * @version 1.0
 * @2023/9/1811:17
 * @function
 * @comment
 */
@Configuration
public class WebSocketConfig {
    // 注入对象ServerEndpointExporter
    // ServerEndpointExporter对象作用：可以自动注册使用了@ServerEndpoint注解的Bean
    @Bean
    public ServerEndpointExporter serverEndpointExporter() {
        return new ServerEndpointExporter();
    }
}
```

pathcontroller类

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

/**
 * @author Dkx
 * @version 1.0
 * @2023/9/199:08
 * @function
 * @comment
 */
@Controller
public class PathController {
    @RequestMapping("login")
    public String login() {
        return "/login";
    }

    @RequestMapping("/")
    public String u() {
        return login();
    }

    @RequestMapping("index")
    public String index() {
        return "/index";
    }

    @RequestMapping("404")
    public String error() {
        return "/404";
    }
}
```

userController类

```java
import com.dkx.pojo.Result;
import com.dkx.pojo.User;
import jakarta.servlet.http.HttpSession;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

/**
 * @author Dkx
 * @version 1.0
 * @2023/9/1819:53
 * @function
 * @comment
 */
@RestController
public class UserController {
    @PostMapping("login")
    public Result login(@RequestBody User user, HttpSession session) {
        Result result = new Result();
        if(user != null && "123".equals(user.getPassword())) {
            result.setFlag(true);
            result.setToken("oaisjidonaogboibnoanougbn");
            session.setAttribute("user", user.getUsername());
        } else {
            result.setFlag(false);
            result.setMessage("login error");
        }
        return result;
    }

    @GetMapping("getUsername")
    public String getUserName(HttpSession session) {
        String username = (String) session.getAttribute("user");
        return username;
    }
}
```

#### 前端页面

login

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
    <script th:src="@{/Utils/form-serialize.js}"></script>
</head>
<body style="padding: 0; margin: 0 auto; max-width: 750px;">
    <div style="margin-top: 190px; text-align: center">
        <form class="form" action="javaScript:" method="post" autocomplete="on">
            <label>
                账号: <input type="text" name="username" placeholder="请输入账号..."/>
            </label>
            <br />
            <br />
            <label>
                密码: <input type="password" name="password" placeholder="请输入密码..."/>
            </label>
            <br />
            <br />
            &nbsp;&nbsp;&nbsp;&nbsp;
            <input style="width: 128px" type="button" class="submit" value="登录" />
        </form>
    </div>
<script type="module" th:src="@{/js/login.js}"></script>
</body>
</html>
```

index

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="format-detection" content="telephone=no">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=0, minimum-scale=1.0, maximum-scale=1.0">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta content="yes" name="apple-mobile-web-app-capable">
    <meta content="yes" name="apple-touch-fullscreen">
    <meta name="full-screen" content="yes">
    <meta content="default" name="apple-mobile-web-app-status-bar-style">
    <meta name="screen-orientation" content="portrait">
    <meta name="browsermode" content="application">
    <meta name="msapplication-tap-highlight" content="no">
    <meta name="x5-orientation" content="portrait">
    <meta name="x5-fullscreen" content="true">
    <meta name="x5-page-mode" content="app">
    <base target="_blank">
    <title>黑马畅聊-聊天室</title>
    <script th:src="@{/Utils/jquery-1.9.1.min.js}"></script>
    <link th:href="@{/bootstrap/bootstrap.min.css}" rel="stylesheet"/>
    <script th:src="@{/bootstrap/bootstrap.min.js}"></script>
    <link rel="stylesheet" th:href="@{/css/chat.css}">
</head>
<body>
<img style="width:100%;height:100%" th:src="@{/img/chat_bg.jpg}">

<div class="abs cover contaniner">
    <div class="abs cover pnl">
        <div class="top pnl-head" style="padding: 20px ; color: white;" >
            <div id="userName"> 用户：张三<span style='float: right;color: green'>在线</span></div>
            <div id="chatMes" style="text-align: center;color: #6fbdf3;font-family: 新宋体">
                <!--正在和 <font face="楷体">张三</font> 聊天-->
            </div>
        </div>
        <!--聊天区开始-->
        <div class="abs cover pnl-body" id="pnlBody" >
            <div class="abs cover pnl-left" id="initBackground" style="background-color: white; width: 100%">
                <div class="abs cover pnl-left" id="chatArea" style="display: none">
                    <div class="abs cover pnl-msgs scroll" id="show">
                        <div class="pnl-list" id="hists"><!-- 历史消息 --></div>
                        <div class="pnl-list" id="msgs">

                            <!-- 消息这展示区域 -->
                            <!-- <div class="msg guest"><div class="msg-right"><div class="msg-host headDefault"></div><div class="msg-ball">你好</div></div></div>
                             <div class="msg robot"><div class="msg-left" worker=""><div class="msg-host photo" style="background-image: url(img/avatar/Member002.jpg)"></div><div class="msg-ball">你好</div></div></div>-->
                        </div>
                    </div>

                    <div class="abs bottom pnl-text">
                        <div class="abs cover pnl-input">
                            <textarea class="scroll" id="context_text" wrap="hard" placeholder="在此输入文字信息..."></textarea>
                            <div class="abs atcom-pnl scroll hide" id="atcomPnl">
                                <ul class="atcom" id="atcom"></ul>
                            </div>
                        </div>

                        <div class="abs br pnl-btn" id="submit" style="background-color: rgb(32, 196, 202); color: rgb(255, 255, 255);">
                            发送
                        </div>
                    </div>
                </div>

                <!--聊天区 结束-->

                <div class="abs right pnl-right">
                    <div class="slider-container hide"></div>
                    <div class="pnl-right-content">
                        <div class="pnl-tabs">
                            <div class="tab-btn active" id="hot-tab">好友列表</div>
                        </div>
                        <div class="pnl-hot">
                            <ul class="rel-list unselect" id="userlist">
<!--                                &lt;!&ndash;<li class="rel-item"><a onclick='showChat("张三")'>张三</a></li>-->
<!--                                <li class="rel-item"><a onclick='showChat("李四")'>李四</a></li>-->
                            </ul>
                        </div>
                    </div>

                    <div class="pnl-right-content">
                        <div class="pnl-tabs">
                            <div class="tab-btn active">系统广播</div>
                        </div>
                        <div class="pnl-hot">
                            <ul class="rel-list unselect" id="broadcastList">
<!--                                &lt;!&ndash;<li class="rel-item" style="color: #9d9d9d;font-family: 宋体">您的好友 张三 已上线</li>-->
<!--                                <li class="rel-item" style="color: #9d9d9d;font-family: 宋体">您的好友 李四 已上线</li>&ndash;&gt;-->
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
<script th:src="@{/js/index.js}"></script>
</body>
</html>
```

#### javaScript代码

login

```js
const from = document.querySelector('.form')
const submit = document.querySelector('.submit')
submit.addEventListener('click', function (e) {
    e.preventDefault();
    const data = serialize(from, {hash: true, empty: true})
    const jsonData = JSON.stringify(data)
    const xhl = new XMLHttpRequest()
    xhl.open('post', `http://localhost:8080/login`)
    xhl.setRequestHeader('Content-Type', 'application/json')
    xhl.send(jsonData)
    xhl.addEventListener('loadend', function () {
        if (xhl.status === 200) {
            const jsonRes = JSON.parse(xhl.response)
            localStorage.setItem('token', jsonRes.token)
            location = '/index'
        } else {
            location = '/404'
        }
    })
})
```

index

```js
const token = localStorage.getItem('token')

if (!token) {
    location = '/login'
}

let toName
let username

function showChat(name) {
    toName = name
    document.querySelector('#chatArea').style.display = 'block'
    // 清空聊天区
    document.querySelector('#msgs').innerHTML = ''
    document.querySelector('#chatMes').innerHTML = `正在和<font face="楷体"> [${toName}] </font>聊天中`
    const chatData = sessionStorage.getItem(toName)
    if (chatData != null) {
        document.querySelector('#msgs').innerHTML = chatData
    }
}

const zaixian = document.querySelector('#userName')
~function () {
    const xhl = new XMLHttpRequest()
    xhl.open('get', '/getUsername')
    xhl.send()
    xhl.addEventListener('loadend', function (e) {
        e.preventDefault()
        if (xhl.status === 200) {
            const name = xhl.response
            username = name
        }
    })
}()

let i = 1
// 创建webSocket对象
let ws = new WebSocket('ws://localhost:8080/chat')
// 为ws绑定事件
ws.onopen = function () {
    i++
    // 在建立连接后需要做什么事儿?
    // 显示在线信息
    zaixian.innerHTML = "用户：" + username + "<span style='float: right; color: green'>在线</span>"
}

// 接收到服务器推送的消息后触发
ws.onmessage = function (evt) {
    // 获取服务端推送过来的消息
    const dataStr = evt.data
    // 将dataStr转换为JSON对象
    const res = JSON.parse(dataStr)
    if (res.system) {
        // 系统消息
        let names = res.message
        // 1.好友列表展示
        let userListStr = ''
        // 2.系统广播展示
        let broadcastListStr = ''
        for (let i = 0; i < names.length; i++) {
            if (names[i] != username) {
                userListStr += "<li class=\"rel-item\"><a onclick='showChat(\"" + names[i] + "\")'>" + names[i] + "</a></li>"
                broadcastListStr += "<li class=\"rel-item\" style=\"color: #9d9d9d;font-family: 宋体\">您的好友 " + names[i] + " 已上线</li>"
            }
        }
        // 渲染好友列表和系统广播
        document.querySelector('#userlist').innerHTML = userListStr
        document.querySelector('#broadcastList').innerHTML = broadcastListStr
    } else {
        // 不是系统消息
        // 将服务器推送的消息进行展示
        let str = '<div class="msg robot"><div class="msg-left" worker=""><div class="msg-host photo" style="background-image: url(../img/avatar/Member00' + i + '.jpg)"></div><div class="msg-ball">' + res.message + '</div></div></div>'
        document.querySelector('#msgs').innerHTML += str

        if (toName === res.formName) {
            document.querySelector('#msgs').innerHTML += str
        }
        const chatData = sessionStorage.getItem(res.formName)
        if (chatData != null) {
            str = chatData + str
        }
        sessionStorage.setItem(res.formName, str)
    }
}

ws.onclose = function () {
    // 显示离线信息
    zaixian.innerHTML = "用户：" + username + "<span style='float: right; color: red'>离线</span>"
}

const show = document.querySelector('#show')
const sendMsg = document.querySelector('#submit')
sendMsg.addEventListener('click', function () {
    // 获取输入内容
    const content_text = document.querySelector('#context_text')
    const data = content_text.value
    // 清空输入区的内容
    content_text.value = ''
    let json = {
        'toName': toName,
        'message': data
    }
    // 将消息展示在聊天区
    let str = '<div class="msg guest"><div class="msg-right"><div class="msg-host headDefault"></div><div class="msg-ball">' + data + '</div></div></div>'
    document.querySelector('#msgs').innerHTML += str
    let chatData = sessionStorage.getItem(toName)
    if (chatData != null) {
        str = chatData + str
    }

    sessionStorage.setItem(toName, str)
    // 发送数据给服务端
    ws.send(JSON.stringify(json))
    show.scrollTop = show.scrollHeight
})
```



