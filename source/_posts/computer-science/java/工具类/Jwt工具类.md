---
title: Jwt工具类
categories:
   - [计算机学科,java,工具类]
tags:
   - 计算机学科
   - java
   - 工具类
---

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