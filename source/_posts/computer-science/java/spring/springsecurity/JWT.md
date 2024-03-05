---
title: JWT
categories:
    - [计算机学科,java,spring,springsecurity]
tags:
    - 计算机学科
    - springsecurity
    - JWT
---

## JWT

#### 介绍

JWT是JSON Web Token 的缩写，即JSON Web令牌，是一种自包含令牌(自己能包含重要信息)，是为了在网络引用环境间传递声明而执行的一种基于JSON的开发标准

JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源。比如用在用户登录上。

JWT最重要的作用就是对token的信息的防伪作用。

### JWT令牌的组成

一个JWT由三个部分组成：JWT头，有效载荷，签名哈希

最后由这三者组合进行base64url编码得到JWT

典型的，一个JWT看起来如下图：该对象为一个很长的字符串，字符之间通过 “.” 分隔符分为三个子串。

https://jwt.io/

### JWT头

JWT头部分是一个描述JWT元数据的JSON对象，通常如下所示：

```json
{
   "alg": "HS256",
   "typ": "JWT"
}
```

在上面的代码中，alg属性表示签名使用的算法，默认为HMAC SHA256 (写为HS256)

typ属性表示令牌的类型，JWT令牌统一写为JWT。

最后，使用Base64URL算法将上述JSON对象转换为字符串保存。

### 有效载荷

有效载荷部分，是JWT的主体内容部分，也是一个JSON对象，包含需要传递的数据，JWT指定七个默认字段供选择。

iss：jwt签发者

sub：主题

aud：接收jwt的一方

exp：jwt的过期时间，这个过期事件必须要大于签发时间

nbf：定义在什么时间之前，该jwt都是不可用的。

jat：jwt的签发时间

jti：jwt的唯一身份标识，主要用来作为一次性token，从而回避重放攻击。

**除以上默认字段外，我们还可以自定义私有字段，如下例**：

```json
{
   "name": "Helen",
   "role": "editor",
   "avatar": "helen.jpg"
}
```

请注意，默认情况下JWT是未加密的，任何人都可以解读其内容，因此不要构建隐私信息字段，存放保密信息，以防止信息泄露

JSON对象也是用Base64URL算法转换为字符串保存。

### 签名哈希

签名哈希部分是对上面两部分数据签名，通过指定的算法生成哈希，以确保数据不会被篡改。

首先，需要指定一个密码 (secret)。改密码仅仅为保存在服务器中，并且不能向用户公开。然后，使用标头中指定的签名算法 (默认情况下为HMAC SHA256) 根据以下公式生成签名。

```
HMACSHA256(base64urlEncode(header) + "." + base64UrlEncode(claims).secret) ==> 签名 hash
```

在计算出签名哈希后，JWT头，有效载荷和签名哈希的三个部分组合成一个字符串，每个部分用 “.” 分隔，就构成整个JWT对象。

### Base64URL算法

如前所述，JWT头和有效载荷序列化的算法都用到了Base64URL。该算法和常见Base64算法类似，稍有差别。

作为令牌的JWT可以放在URL中 (例如api.example/?token=xxx)。Base64中用的三个字符是 “+” , “/” 和 “=” ，由于在URL中有特殊含义，因此Base64URL中对他们做了替换： “=” 去掉，“+” 用 “-” 替换，“/” 用 “_” 替换，这就是Base64URL算法

### JWT工具类代码

具体可以根据自己的

```java
import io.jsonwebtoken.*;

import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;
import java.util.Date;
import java.util.UUID;

public class JWTHelper {
    // 设置有效期 一个小时
    public static final Long JWT_TTL = 60 * 60 * 1000L;
    // 设置密钥 设置长度不能大于11
    public static final String JWT_KEY = "liusangbaoyo";

    public static String getUUID() {
        return UUID.randomUUID().toString().replaceAll("-", "");
    }

    /**
     * 生成JWT
     *
     * @param subject token中要存放的数据(JSON格式)
     * @return
     */
    public static String createJwt(String subject) {
        return getJwtBuilder(subject, null, getUUID(), null, null).compact();
    }

    public static String createJwt(Long id, String name) {
        return getJwtBuilder(null, null, getUUID(), id, name).compact();
    }

    public static String createJwt(String subject, Long ttlMillis) {
        // 设置过期时间
        return getJwtBuilder(subject, ttlMillis, getUUID(), null, null).compact();
    }

    private static JwtBuilder getJwtBuilder(String subject, Long ttlMillis, String uuid, Long id, String name) {
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;
        SecretKey secretKey = generalKey();
        long nowMillis = System.currentTimeMillis();
        Date now = new Date(nowMillis);
        if (ttlMillis == null) {
            ttlMillis = JWTHelper.JWT_TTL;
        }
        long expMills = nowMillis + ttlMillis;
        Date expDate = new Date(expMills);
        return Jwts.builder()
                .setId(uuid) // 唯一的ID
                .setSubject(subject) // 主题，可以是JSON数据
                .claim("userId", id)
                .claim("username", name)
                .setIssuer("dkx") // 签发者
                .setIssuedAt(now) // 签发时间
                .signWith(signatureAlgorithm, secretKey) // 使用 HS256对称加密算法签名，第二个参数为密钥
                .setExpiration(expDate);
    }

    public static String createJwt(String id, String subject, Long ttlMillis) {
        return getJwtBuilder(subject, ttlMillis, id, null, null).compact();
    }

    private static SecretKey generalKey() {
        byte[] encodedKey = Base64.getDecoder().decode(JWTHelper.JWT_KEY);
        return new SecretKeySpec(encodedKey, 0, encodedKey.length, "AES");
    }

    public static Claims parseJwt(String jwt) {
        Claims claims;
        SecretKey secretKey = generalKey();
        try {
            return Jwts.parser()
                    .setSigningKey(secretKey)
                    .parseClaimsJws(jwt)
                    .getBody();
        } catch (ExpiredJwtException e) {
            claims = e.getClaims();
        }

        return claims;
    }
}
```

### JWT中的Claims

Claims有索赔，声称，要求或者权利要求的含义，但是笔者觉得任一个翻译都不怎么合乎语义，这里保留Claims关键字直接作为命名。JWT的核心作用就是保护Claims的完整性 (或者数据加密) ，保证JWT传输的过程中Claims不被篡改 (或者不被破解)。Claims在JWT原始内容中是一个JSON格式的字符串，其中单个Claims是k-v结构，作为JsonNode中的一个find-value，这里列出了常用的规范中预定义好的Claim；

| 简称 | 全称            | 含义                                |
| ---- | --------------- | ----------------------------------- |
| iss  | Issuer          | 发行方                              |
| sub  | Subject         | 主体                                |
| aud  | Audience        | (接收) 目标方                       |
| exp  | Expiration Time | 过期时间                            |
| nbf  | Not Before      | 早于该定义的时间的JWT不能被接受处理 |
| iat  | Issued At       | JWT发行时的时间戳                   |
| jti  | JWT ID          | JWT的唯一标识                       |

这些预定义的Claim并不要求强制使用，何时选用何种Claim完全由使用者决定，而为了使JWT更加紧凑，这些Claim都是用了简短的命名方式去定义。在不和内建的Claim冲突的前提下，使用者可以自定义新的公共Claim，如：

| 简称 | 全称        | 含义    |
| ---- | ----------- | ------- |
| cid  | Customer ID | 客户ID  |
| rid  | Role ID     | 角色 ID |

一定要注意，在JWT实现中，Claims会作为payload部分进行Base64编码，明文会直接暴漏，敏感信息一般不应该设计为一个自定义 Claim。

<span alt='solid'>取出上述JWT工具类中设置的userId与username的值</span>.

```java
@Test
public void test() {
   String jwt = JWTHelper.createJwt(1233L, "jiashujv");
   System.out.println(jwt);
   Claims claims = JWTHelper.parseJwt(jwt);
   Integer id = claims.get("userId", Integer.class);
   String username = claims.get("username", String.class);
   System.out.println("id: "+id+", name: "+username);
}
```

