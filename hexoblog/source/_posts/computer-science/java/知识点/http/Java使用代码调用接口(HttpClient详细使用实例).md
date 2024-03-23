---
title: Java使用代码调用接口(HttpClient详细使用实例)
categories:
    - [计算机学科,java,知识点,http]
tags:
    - http
    - 知识点
---

# Java使用代码调用接口(HttpClient详细使用实例)

**了解什么是HttpClient**：

Java中的HttpClient是用于进行HTTP请求和响应的类库。它提供了一种==简单，灵活，可扩展的方式来发送HTTP请求====，并处理HTTP响应==。HttpClient的==核心==功能包括==创建HTTP请求，设置请求参数，发送请求，接收响应，处理响应数据以及处理异常情况==。它使得在Java应用程序中实现==HTTP通信==变得更加==方便和高效==。

**HttpClient的主要功能**：

实现了所有HTTP的方法(GET,POST,PUT,DELETE,HEAD,OPTIONS)等

支持HTTPS协议

支持自动(跳转)转向

. . .

## 1 引入依赖

### 1.1 引入HttpClient的依赖

```xml
<dependency>
   <groupId>org.apache.httpcomponents</groupId>
   <artifactId>httpclient</artifactId>
   <version>4.5.5</version>
</dependency>
```

### 1.2 引入fastjson依赖

```xml
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>fastjson</artifactId>
   <version>1.2.47</version>
</dependency>
```

PS：本人引入依赖的目的是，在后续实例中，会用到 "将对象转化为json字符串的功能"，也可以引其它有此功能的依赖

PS：使用的是SpringBoot工程通过HttpClient发送请求到 controller接口，并将controller接口的响应返回到HttpClient中来接收。

## 2 详细使用示例

声明：此示例中，以Java发送HttpClient(在test里面单元测试发送的)；也是以Java接收的(在controller里面接收的)

### 2.1 GET无参

HttpClient发送示例：

```java
@Test
void contextLoads() {
   // 通过建造者模式 获取HttpClient对象
   CloseableHttpClient httpClient = HttpClientBuilder.create().build();
   // 创建Get请求指定请求路径
   HttpGet httpGet = new HttpGet("http://localhost/doGetControllerOne");
   CloseableHttpResponse response = null;
   try {
      // 发送Get请求 得到响应对象
      response = httpClient.execute(httpGet);
      // 从响应对象中后去响应实体
      HttpEntity responseEntity = response.getEntity();
      System.out.println("响应状态为：" + response.getStatusLine());
      if (responseEntity != null) {
         System.out.println("响应内容长度为：" + responseEntity.getContentLength());
         System.out.println("响应内容为：" + EntityUtils.toString(responseEntity));
      }
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (response != null) {
            response.close();
         }
         if (httpClient != null) {
            httpClient.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

对应接收示例：

```java
@RestController
public class TestDemo {
    @GetMapping("doGetControllerOne")
    public String doGetControllerOne() {
        return "hello";
    }
}
```

打印结果：

```
响应状态为：HTTP/1.1 200 
响应内容长度为：5
响应内容为：hello
```

### 2.2 GET有参(方式一：直接拼接URL)

HttpClient发送示例：

```java
@Test
public void test1() {
   // 通过建造者模式 获取HttpClient对象
   CloseableHttpClient httpClient = HttpClientBuilder.create().build();
   // 使用SpringBuffer来进行参数的拼接
   StringBuffer params = new StringBuffer();
   try {
      // 使用URLEncoder进行编码防止中文乱码
      params.append("name=" + URLEncoder.encode("Xxx", "utf-8"));
      params.append("&");
      params.append("age=24");
   } catch (UnsupportedEncodingException e) {
      throw new RuntimeException(e);
   }
   // 创建Get请求
   HttpGet httpGet = new HttpGet("http://localhost/doGetControllerTwo?" + params);
   CloseableHttpResponse response = null;
   try {
      // 配置请求信息
      RequestConfig requestConfig = RequestConfig.custom()
         // 链接超时时间
         .setConnectTimeout(5000)
         // 请求超时时间
         .setConnectionRequestTimeout(5000)
         // socket读写超时时间
         .setSocketTimeout(5000)
         // 是否允许重定向
         .setRedirectsEnabled(true)
         .build();
      // 将配置信息添加到Get请求中
      httpGet.setConfig(requestConfig);
      // 执行Get请求获取响应对象
      response = httpClient.execute(httpGet);
      // 获取响应实体
      HttpEntity entity = response.getEntity();
      System.out.println("响应状态为：" + response.getStatusLine());
      if (entity != null) {
         System.out.println("响应内容长度为：" + entity.getContentLength());
         System.out.println("响应内容为：" + EntityUtils.toString(entity));
      }
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      try {
         // 释放资源
         if (response != null) {
            response.close();
         }
         if (httpClient != null) {
            httpClient.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

对应接收示例：

```java
@GetMapping("doGetControllerTwo")
public String doGetControllerOne(String name, Integer age) {
   return "今年" + name + ", 已经" + age + " 岁了";
}
```

打印结果：

```
响应状态为：HTTP/1.1 200 
响应内容长度为：26
响应内容为：今年Xxx, 已经24 岁了
```

### 2.3 GET有参(方式二：使用URL获得HttpGet)

HttpClient发送示例：

```java
@Test
public void test2() {
   // 通过建造者模式获取HttpClient对象
   CloseableHttpClient httpClient = HttpClientBuilder.create().build();
   URI uri = null;
   // 创建List集合类型为 用于表示键值对的接口通常用于HTTP请求和响应的参数
   List<NameValuePair> params = new ArrayList<>();
   //  设置参数
   params.add(new BasicNameValuePair("name", "Xxx"));
   params.add(new BasicNameValuePair("age", "18"));
   try {
      // 构建URI对象的信息
      uri = new URIBuilder()
         .setScheme("http")
         .setHost("localhost")
         .setPath("/doGetControllerTwo")
         .setParameters(params)
         .build();
   } catch (URISyntaxException e) {
      throw new RuntimeException(e);
   }
   // 创建Get请求传入请求信息
   HttpGet httpGet = new HttpGet(uri);
   // 配置信息
   RequestConfig requestConfig = RequestConfig.custom()
      .setConnectTimeout(5000)
      .setConnectionRequestTimeout(5000)
      .setSocketTimeout(5000)
      .setRedirectsEnabled(true)
      .build();
   // 将配置信息传入请求中
   httpGet.setConfig(requestConfig);
   CloseableHttpResponse response = null;
   try {
      // 执行请求获得响应对象
      response = httpClient.execute(httpGet);
      // 通过响应对象获取实体
      HttpEntity entity = response.getEntity();
      System.out.println("响应状态为：" + response.getStatusLine());
      if (entity != null) {
         System.out.println("响应内容长度为：" + entity.getContentLength());
         System.out.println("响应内容为：" + EntityUtils.toString(entity));
      }
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (response != null) {
            response.close();
         }
         if (httpClient != null) {
            httpClient.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

对应接收示例：

```java
@GetMapping("doGetControllerTwo")
public String doGetControllerOne(String name, Integer age) {
   return "今年" + name + ", 已经" + age + " 岁了";
}
```

打印结果：

```
响应状态为：HTTP/1.1 200 
响应内容长度为：26
响应内容为：今年Xxx, 已经18 岁了
```

### 2.4 POST无参

HttpClient发送示例：

```java
@Test
public void test3() {
   CloseableHttpClient httpClient = HttpClientBuilder.create().build();
   HttpPost httpPost = new HttpPost("http://localhost/doPostControllerOne");
   CloseableHttpResponse response = null;
   try {
      response = httpClient.execute(httpPost);
      HttpEntity entity = response.getEntity();
      System.out.println("状态为：" + response.getStatusLine());
      if (entity != null) {
         System.out.println("获取响应长度为：" + entity.getContentLength());
         System.out.println("获取响应内容为：" + EntityUtils.toString(entity));
      }
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      try {
         if (response != null) {
            response.close();
         }
         if (httpClient != null) {
            httpClient.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

对应接收示例：

```java
@PostMapping("doPostControllerOne")
public String doPostControllerOne() {
   return "这个post请求没有任何参数";
}
```

打印结果：

```
状态为：HTTP/1.1 200 
获取响应长度为：34
获取响应内容为：这个post请求没有任何参数
```

### 2.5 POST有参(普通参数)

PS：POST传递普通参数时，方式与GET一样即可，这里以直接在URL后缀上参数的方式示例

HttpClient发送示例：

```java
@Test
public void test4() {
   // 通过构造者模式获取HttpClient对象
   CloseableHttpClient httpClient = HttpClientBuilder.create().build();
   // 创建字符缓冲区用于拼接参数
   StringBuffer params = new StringBuffer();
   try {
      params.append("name=" + URLEncoder.encode("Xxx", "utf-8"));
      params.append("&");
      params.append("age=24");
   } catch (UnsupportedEncodingException e) {
      throw new RuntimeException(e);
   }
   // 创建Post请求
   HttpPost httpPost = new HttpPost("http://localhost/doPostControllerTwo?" + params);
   CloseableHttpResponse response = null;
   try {
      // 执行请求并返回响应对象
      response = httpClient.execute(httpPost);
      // 获取实体
      HttpEntity entity = response.getEntity();
      System.out.println("状态为：" + response.getStatusLine());
      if (entity != null) {
         System.out.println("响应内容长度为：" + entity.getContentLength());
         System.out.println("响应内容为：" + EntityUtils.toString(entity));
      }
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (response != null) {
            response.close();
         }
         if (httpClient != null) {
            httpClient.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

对应接收示例：

```java
@PostMapping("doPostControllerTwo")
public String doPostControllerTwo(String name, Integer age) {
   return "今年" + name + ", 已经" + age + " 岁了";
}
```

打印结果：

```
状态为：HTTP/1.1 200 
响应内容长度为：26
响应内容为：今年Xxx, 已经24 岁了
```

### 2.6 POST有参(对象参数)

HttpClient发送示例：

```java
@Test
public void test5() {
   // 通过构建者模式获取HttpClient对象
   CloseableHttpClient httpClient = HttpClientBuilder.create().build();
   // 创建Post请求
   HttpPost httpPost = new HttpPost("http://localhost/doPostControllerThree");
   // 创建对象并设置属性信息
   User user = new User();
   user.setName("刘桑");
   user.setAge(20);
   // 将对象序列化为JSON数据
   String jsonString = JSON.toJSONString(user);
   // 设置json数据为utf-8返回json实体
   StringEntity entity = new StringEntity(jsonString, "utf-8");
   // 设置请求的实体数据
   httpPost.setEntity(entity);
   // 设置请求的请求头信息为json数据格式
   httpPost.setHeader("Content-Type", "application/json;charset=utf8");
   CloseableHttpResponse response = null;
   try {
      // 执行请求并返回响应对象
      response = httpClient.execute(httpPost);
      // 获取实体
      HttpEntity responseEntity = response.getEntity();
      System.out.println("响应状态为：" + response.getStatusLine());
      if (responseEntity != null) {
         System.out.println("响应内容长度为：" + responseEntity.getContentLength());
         System.out.println("响应内容为" + EntityUtils.toString(responseEntity));
      }
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (response != null) {
            response.close();
         }
         if (httpClient != null) {
            httpClient.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

对应接收示例：

```java
@PostMapping("doPostControllerThree")
public User doPostControllerThree(@RequestBody User user) {
   return user;
}
```

打印结果：

```
响应状态为：HTTP/1.1 200 
响应内容长度为：-1
响应内容为{"name":"刘桑","age":20}
```

### 2.7 POST有参(普通参数 + 对象参数)

PS：POST传递普通参数时，方式与GET一样即可，这里以通过URI获取HttpPost的方式为例

HttpClient发送示例：

```java
@Test
public void test6() {
   // 通过构造者模式获取HttpClient对象
   CloseableHttpClient httpClient = HttpClientBuilder.create().build();
   // 创建List集合泛型为 键值对的对象用常用于HTTP请求的参数和响应的参数
   List<NameValuePair> params = new ArrayList<>();
   params.add(new BasicNameValuePair("flag", "4"));
   params.add(new BasicNameValuePair("meaning", "这是什么鬼？"));
   URI uri = null;
   try {
      // 创建URI对象构建URI的请求信息
      uri = new URIBuilder()
         .setScheme("http")
         .setHost("localhost")
         .setPath("doPostControllerFour")
         .setParameters(params)
         .build();
   } catch (URISyntaxException e) {
      throw new RuntimeException(e);
   }
   // 创建Post请求对象并传入请求信息
   HttpPost httpPost = new HttpPost(uri);
   // 创建User对象并设置属性信息
   User user = new User();
   user.setName("刘桑");
   user.setAge(20);
   // 将User对象序列化为JSON数据并转换为utf-8格式的实体数据
   StringEntity entity = new StringEntity(JSON.toJSONString(user), "utf-8");
   // 将实体数据设置到请求实体中
   httpPost.setEntity(entity);
   // 设置请求头信息为json格式
   httpPost.setHeader("Content-Type", "application/json;charset=utf8");
   CloseableHttpResponse response = null;
   try {
      // 执行请求并获取响应对象
      response = httpClient.execute(httpPost);
      // 获取实体
      HttpEntity responseEntity = response.getEntity();
      System.out.println("状态为：" + response.getStatusLine());
      if (responseEntity != null) {
         System.out.println("响应内容长度为：" + responseEntity.getContentLength());
         System.out.println("响应内容为：" + EntityUtils.toString(responseEntity));
      }
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (response != null) {
            response.close();
         }
         if (httpClient != null) {
            httpClient.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }

}
```

对应接收示例：

```java
@PostMapping("doPostControllerFour")
public UserDto doPostControllerFour(@RequestBody User user, Integer flag, String meaning) {
   UserDto userDto = new UserDto();
   userDto.setName(user.getName());
   userDto.setAge(user.getAge());
   userDto.setFlag(flag);
   userDto.setMeaning(meaning);
   return userDto;
}
```

打印结果：

```
状态为：HTTP/1.1 200 
响应内容长度为：-1
响应内容为：{"name":"刘桑","age":20,"flag":4,"meaning":"这是什么鬼？"}
```

