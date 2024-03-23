---
title: 微信扫码登录-PC
categories:
   - [计算机学科,java,第三方库]
tags:
   - 计算机学科
   - java
   - 第三方库
---

# 一、微信扫码登录-PC

## 1.1 理论基础-OAtuh2.0

OAuth(Open Authorization)是一个关于授权(authorization)的开放网络标准，允许用户授权第三方应用访问它们存储在另外的服务提供者上的信息，而不需要将用户名和密码提供给第三方。

OAuth在全世界得到广泛应用，目前的版本是2.0版。

## 1.2 协议特点：

**简单**：不管是OAuth服务提供者还是应用开发者，都很易于理解与使用

**安全**：没有涉及到用户秘钥等信息，更安全更灵活

**开放**：任何服务提供商都可以实现OAuth，任何软件开发商都可以使用OAuth

# 二、 OAuth2.0-角色说明

| 角色                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| 客户端                 | 本身不存储资源，需要通过资源拥有者授权去请求资源服务器的资源(APP游戏，影视网站等) |
| 资源拥有者             | 通常为用户，也可以是应用程序，即资源的拥有者                 |
| 授权服务器(认证服务器) | 用于服务提供商对资源拥有者的身份进行认证，对访问资源进行授权，认证成功后会给客户端发放令牌<br />作为客户端访问资源服务器的凭据 |
| 资源服务器             | 存储资源的服务器，比如微信端存储的用户信息                   |

# 三、四种授权模式

授权码模式(Authorization Code Grant)

隐式授权模式(Implicit Grant)

用户名密码模式(Resource Owner Password Credentials Grant)

客户端模式(Client Gredentials Grant)

# 四、授权码模式是OAuth2目前最安全最复杂也是最常用的授权流程

![image-20240320150104046](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320150104046.png)

**如上图解释**：

有四个角色，资源拥有者(自己)，客户端(游戏，影视网站)，授权服务器(微信，QQ)，受保护资源(账号信息)

**第一步**：资源拥有者委托客户端访问受保护资源 (委托游戏，影视应用我要微信，QQ登录)

**第二步**：客户端请求受保护的资源，这时会生成一个二维码，这个二维码就是从微信拿到用户授权

**第三步**：重定向到授权服务器，进行 身份验证

**第四步**：进行授权，同意授权后到第五步

**第五步**：返回授权码，重定向到客户端请求的URL

**第六步**：从浏览器重定向到客户端

**第七步**：通过授权码到授权服务器中请求令牌，授权服务器进行校验授权码的有效性，如果授权码没问题就到第八步

**第八步**：返回令牌到客户端

**第九步**：拿到令牌访问受保护的资源

**第十步**：返回受保护的资源信息 (非敏感)

# 五、二维码

二维码(dimensional barcode)，又称二维条码，是在一维条码的基础上扩展出的一种具有可读性的条码。设备扫描二维条码，通过识别条码的长度和宽度中所记载的二进制数据，可获取其中所包含的信息。

总之：二维码是信息的载体

例如：黑马程序员的网址写入二维码，扫描二维码就可以打开官网。

# 六、Java生成二维码

Hutool是一个Java工具类库，对文件，流，加密解密，转码，正则，线程，XML等JDK方法进行封装，组成各种Util工具类

### 6.1 导入maven依赖

```xml
<dependency>
   <groupId>cn.hutool</groupId>
   <artifactId>hutool-all</artifactId>
   <version>5.7.5</version>
</dependency>
<dependency>
   <groupId>com.google.zxing</groupId>
   <artifactId>core</artifactId>
   <version>3.3.3</version>
</dependency>
```

### 6.2 生成二维码代码：

```java
QrCodeUtil.generate("www.baidu.com", 300, 300, FileUtil.file("D:\\qrcode.jpg"));
```

![image-20240320152446989](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320152446989.png)

当我们扫描二维码后就会进入到代码中指定的网站

### 6.3 Java生成二维码-纠错级别

L，M，Q，H 由低到高

低级别的像素块更大，可以远距离识别，但是遮挡就会造成无法识别

高级别则相反，像素块小，允许遮挡一定范围，但是像素块更密集

```java
/**
 * 生成一个600 * 600 的二维码
 * 纠错级别设置为H
 * L,M,Q,H几个参数，由低到高，低级别的像素块更大
 */
QrConfig config = new QrConfig(600, 600);
// 设置纠错级别
config.setErrorCorrection(ErrorCorrectionLevel.H);
// 设置二维码的颜色
config.setBackColor(Color.BLUE);
QrCodeUtil.generate("www.baidu.com", config, FileUtil.file("D:\\qrcode.jpg"));
```

# 七、 实现微信登录-准备工作 申请账号

## 7.1 扫码登录微信有两种实现方式

1.  基于微信公众平台的扫码登录

    让第三方应用投入微信的怀抱而设计的，这第三方应用指的是比如android, ios, 网站, 系统等

2.  基于微信开放平台的扫码登录

    为了让程序员小伙伴利用微信自家技术(公众号，小程序)开发公众号，小程序而准备的

## 7.2 区别

微信开放平台需要开企业认证才能注册

微信公众平台需要认证微信服务号，才能进行扫码登录开发。只需要申请一个公众号

## 7.3 测试公众号申请

https://mp.weixin.qq.com/debug/cgi-bin/sandbox?t=sandbox/login

# 八、 基本配置

## 8.1 接口信息配置

==PS==：下图只做演示，并不是真正配置

![image-20240320154005612](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320154005612.png)

URL：填写自己服务器的接口地址

token：随意填写

==注意==：需要内网穿透否则微信访问不了自己的服务器接口就会报错

内网穿透可以使用natapp使用步骤如下：

1.  打开控制台网站：配置隧道信息

    ![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320155204514.png)

2.  在客户端目录下打开终端输入命令：natapp.exe --authtoken=[从控制台配置中复制的token]

    ![image-20240320155334477](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320155334477.png)

3.  启动后我们将地址换一下就可以了

    将地址配置到接口配置信息中点击提交就可以看到上面提示我们配置成功，就说明成功了！

    ![image-20240320155608541](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320155608541.png)

4.  这里是需要结合下面的测试接口代码来的，否则微信找不到服务器的接口会报错

接口配置信息的文档详细说明如下：

开发者提交信息后，微信服务器将发送GET请求到填写的服务器地址URL上，GET请求携带参数如下表所示：

| 参数      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| signature | 微信加密签名，signature结合了开发者填写的token参数和请求中的timestamp参数，nonce参数 |
| timestamp | 时间戳                                                       |
| nonce     | 随机数                                                       |
| echostr   | 随机字符串                                                   |

开发者通过检验signature对请求进行校验(下面有校验方式)。若确认此次GET请求来自微信服务器，请原样返回echostr参数内容，则接入生效，成为开发者成功，否则接入失败。加密/校验流程如下：

1.  将token，timestamp，nonce三个参数进行字典排序
2.  将三个参数字符串拼接成一个字符串进行sha1加密
3.  开发者获得加密后的字符串可与signature对比，标识该请求来源于微信

## 8.2 测试接口代码：

```java
@GetMapping("wxCheck")
public String er(String signature, String timestamp, String nonce, String echostr) {
   System.out.println("echostr 微信给的随机字符串 = " + echostr);
   // 如果这里不返回 那么在点击提交时就会报错提示配置失败的
   return echostr;
}
```

打印结果

```
echostr 微信给的随机字符串 = 4752244468755445686
```

## 8.3 OAuth2.0 网页授权

在微信公众号请求用户网页授权之前，开发者需要先到公众平台官网中的 [设置于开发] - [功能设置] - [网页授权域名]的配置选项中，修改授权回调域名

请注意，这里填写的是域名 (是一个字符串)，而不是URL，因此请勿加 http:// 等协议头

==PS==：下图只做演示并不是真正配置

![image-20240320160349974](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320160349974.png)

下面是自己设置的回调域名就是内网穿透的域名

![image-20240320160656885](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320160656885.png)

### 8.3.1 测试二维码

我们还需要扫描如下图中的二维码进行关注该公众号才能使用

![image-20240320160529502](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240320160529502.png)

# 九、 代码实现

## 9.1 第一步：用户同意授权，获取code

在确保微信公众账号拥有授权作用域(scope参数)的权限的前提下(已认证服务号，默认拥有scope参数中的snsapi_base和snsapi_userinfo权限)，引导关注者打开如下页面：

https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect

将上面的链接生成一个二维码引导用户扫码进行授权登录

| 参数             | 是否必须 | 说明                                                         |
| ---------------- | -------- | ------------------------------------------------------------ |
| appid            | 是       | 公众号的唯一标识                                             |
| redirect_uri     | 是       | 授权后重定向的回调链接地址，请使用urlEncode对链接进行处理    |
| response_type    | 是       | 返回类型，请填写code                                         |
| scope            | 是       | 应用授权作用域，snsapi_base(不弹出授权页面，直接跳转，只能获取用户openId)<br />snsapi_userinfo(弹出授权页面，可通过openid拿到昵称，性别，所在地。并且<br />即使在未关注的情况下，只要用户授权，也能获取其信息) |
| state            | 否       | 重定向后会带上state参数，开发者可以填写a-z A-Z 0-9的参数值，最多128字节 |
| #wechat_redirect | 是       | 无论直接打开还是做页面302重定向时候，必须带此参数            |
| forcePopup       | 否       | 强制此授权需要用户弹窗确认；默认为false；需要注意的是，若用户命中了特殊<br />场景下的静默授权逻辑，则此参数不生效 |

### 9.1.1 二维码登录接口

```java
// 将测试号管理页面的appid放到此处，后续可以配置到yml中进行读取
private static final String appid = "wx0777296eb82be4f2";

@GetMapping("wxLogin")
public void wxLoginPage(HttpServletResponse response) {
   try
   {
      // 回调地址
      String redirectUrl = URLEncoder.encode("http://z7yqny.natappfree.cc/wxCallback", "UTF-8");
      // 用户同意授权获取code其中拼接上appid和回调地址
      String url = "https://open.weixin.qq.com/connect/oauth2/authorize?appid=" + appid + "&redirect_uri=" + redirectUrl
         + "&response_type=code&scope=snsapi_userinfo&state=STATE#wechat_redirect";
      // 设置响应格式为图片
      response.setContentType("image/png");
      // 生成二维码写出到页面中
      QrCodeUtil.generate(url, 300, 300, "jpg", response.getOutputStream());
   } catch (Exception e)
   {
      throw new RuntimeException(e);
   }
}
```

用户通过这个接口进行扫码后就到第二步了

## 第二步：用户同意授权后，通过code换取一个授权的token

拿到code后请求以下链接获取access_token

https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code

当我们把链接中几个参数拿到以后请求就可以拿到如下表所示：

| 参数            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| access_token    | 网页授权接口调用凭证，注意：此access_token与基础支持的access_token不同 |
| expires_in      | access_token接口调用凭证超时时间，单位(秒)                   |
| refresh_token   | 用户刷新access_token                                         |
| openid          | 用户唯一标识，请注意，在未关注公众号时，用户访问公众号的网页，也会产生一个用户<br />和公众号唯一的OpenId |
| scope           | 用户授权的作用域，使用逗号(,)分隔                            |
| is_snapshotuser | 是否为快照页模式模拟账号，只有当用户账号是快照页模式模拟账号时返回，值为1 |
| unionid         | 用户统一标识(针对一个微信开放平台账号下的应用，同一用户的unionid是唯一的)只有当scope为"snsapi_userinfo"时返回 |

### 9.2.1 定义类用于接收上述表中返回的属性值

```java
@Data
public class TokenInfo
{
    private String access_token;
    private String expires_in;
    private String refresh_token;
    private String openid;
    private String scope;
}
```

### 9.2.2 回调接口

当用户扫码授权后就会请求如下回调接口

这个回调地址会接受到微信给的code码，通过code码获取用户

```java
@GetMapping("wxCallback")
public String pcCallback(String code, String state, HttpServletRequest request, HttpServletResponse response, HttpSession session) throws IOException
{
   return JSON.toJSONString(WechatUtils.getUserInfo(code));
}
```

```java
public class WechatUtils
{
    private static final String appId = "wx0777296eb82be4f2";
    private static final String secret = "8f93fb4fb289037d1ea0775ad33cccd0";
    public static WeChatUser getUserInfo(String code) throws IOException
    {
        HttpClient httpClient = HttpClients.createDefault();
        // 用code交换token访问的地址，需要给appid还有(秘钥)secret以及刚刚微信返回的code码，以及指定方式授权类型为授权码模式
        String tokenUrl = "https://api.weixin.qq.com/sns/oauth2/access_token?appid=" + appId + "&SECRET=" + secret + "&code=" + code + "&grant_type=authorization_code";
        // 通过HttpGet对象构造发起GET请求，请求上面的连接
        HttpGet httpGet = new HttpGet(tokenUrl);
        // 存储响应结果的变量
        String responseResult = "";
        // 发起GET请求
        HttpResponse response = httpClient.execute(httpGet);
        // 如果状态码为200及成功就将结果赋值给 存储结果变量
        if (response.getStatusLine().getStatusCode() == 200)
        {
            responseResult = EntityUtils.toString(response.getEntity(), "UTF-8");
        }
        return null;
    }
}
```

## 第三步：刷新access_token(如果需要)

由于access_token拥有较短的有效期，当access_token超时后，可以使用refresh_token进行刷新，refresh_token有效期为30天，当refresh_token失效之后，需要用户重新授权

请求方法：

获取第二步的refresh_token后，请求以下链接获取access_token

https://api.weixin.qq.com/sns/oauth2/refresh_token?appid=APPID&grant_type=refresh_token&refresh_token=REFRESH_TOKEN

如果我们需要刷新token的话微信给了我们一个接口

| 参数          | 是否必须 | 说明                                          |
| ------------- | -------- | --------------------------------------------- |
| appid         | 是       | 公众号的唯一标识                              |
| grant_type    | 是       | 填写为refresh_token                           |
| refresh_token | 是       | 填写通过access_token获取到的refresh_token参数 |

## 第四步：拉取用户信息(需scope为snsapi_userinfo)

如果网页授权作用域为snsapi_userinfo，则此时开发者可以通过access_token和openid拉取用户信息了。

请求方法：

http: GET (请使用http协议)

https://api.weixin.qq.com/sns/userinfo?access_token=ACCESS_TOKEN&&openid=OPENID&lang=zh_CN

| 参数         | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| access_token | 网页授权接口调用凭证。注意：此access_token与基础支持的access_token不同 |
| openid       | 用户的唯一标识                                               |
| lang         | 返回国家地区语言版本，zh_CN简体，zh_TW繁体，en英语           |

### 9.4.1 创建封装用户信息的类

```java
@Data
public class WeChatUser
{
    private String openid;
    private String nickname;
    private Integer sex;
    private String country;
    private String city;
    private String province;
    private String headimgurl;
    private String privilege;
    private String unionid;
}
```



```java
public class WechatUtils
{
    private static final String appId = "wx0777296eb82be4f2";
    private static final String secret = "8f93fb4fb289037d1ea0775ad33cccd0";
    public static WeChatUser getUserInfo(String code) throws IOException
    {
        HttpClient httpClient = HttpClients.createDefault();
        // 用code交换token访问的地址，需要给appid还有(秘钥)secret以及刚刚微信返回的code码，以及指定方式授权类型为授权码模式
        String tokenUrl = "https://api.weixin.qq.com/sns/oauth2/access_token?appid=" + appId + "&SECRET=" + secret + "&code=" + code + "&grant_type=authorization_code";
        // 通过HttpGet对象构造发起GET请求，请求上面的连接
        HttpGet httpGet = new HttpGet(tokenUrl);
        // 存储响应结果的变量
        String responseResult = "";
        // 发起GET请求
        HttpResponse response = httpClient.execute(httpGet);
        // 如果状态码为200及成功就将结果赋值给 存储结果变量
        if (response.getStatusLine().getStatusCode() == 200)
        {
            responseResult = EntityUtils.toString(response.getEntity(), "UTF-8");
        }
        // 拿到token信息将信息封装到TokenInfo对象中
        TokenInfo tokenInfo = JSON.parseObject(responseResult, TokenInfo.class);
        // 通过连接加上token还有openId获取个人信息
        String userInfoUrl = "https://api.weixin.qq.com/sns/userinfo?access_token=" + tokenInfo.getAccess_token() + "&openid=" + tokenInfo.getOpenid() + "&lang=zh_CN";
        HttpGet httpGetOne = new HttpGet(userInfoUrl);
        HttpResponse responseOne = httpClient.execute(httpGetOne);
        if (responseOne.getStatusLine().getStatusCode() == 200)
        {
            responseResult = EntityUtils.toString(responseOne.getEntity(), "UTF-8");
        }
        // 将请求到的用户信息封装到 用户信息类中并返回这个对象
        WeChatUser weChatuser = JSON.parseObject(responseResult, WeChatUser.class);
        return weChatuser;
    }
}
```

请求接口后可以返回的参数信息表如下所示：

上面的用户信息类就是根据下面的信息来创建的字段用来接收微信给我们的这些信息

| 参数       | 描述                                                       |
| ---------- | ---------------------------------------------------------- |
| openid     | 用户的唯一标识                                             |
| nickname   | 用户昵称                                                   |
| sex        | 用户的性别，值为1是男性，2是女性，0是未知                  |
| province   | 用户个人资料填写的省份                                     |
| city       | 普通用户个人资料填写的城市                                 |
| country    | 国家，例如中国为CN                                         |
| headimgurl | 用户头像                                                   |
| privilege  | 用户特权信息                                               |
| unionid    | 只有在用户将公众号绑定到微信开放平台账号后，才会出现该字段 |