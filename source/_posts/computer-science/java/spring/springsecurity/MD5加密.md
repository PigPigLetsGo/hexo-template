---
title: MD5加密代码
categories:
    - [计算机学科,java,spring,springsecurity]
tags:
    - 计算机学科
    - springsecurity
    - MD5
---

## MD5加密代码

```java
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public final class MD5 {

    public static String encrypt(String strSrc) {
        try {
            char hexChars[] = { '0', '1', '2', '3', '4', '5', '6', '7', '8',
                    '9', 'a', 'b', 'c', 'd', 'e', 'f' };
            byte[] bytes = strSrc.getBytes();
            MessageDigest md = MessageDigest.getInstance("MD5");
            md.update(bytes);
            bytes = md.digest();
            int j = bytes.length;
            char[] chars = new char[j * 2];
            int k = 0;
            for (int i = 0; i < bytes.length; i++) {
                byte b = bytes[i];
                chars[k++] = hexChars[b >>> 4 & 0xf];
                chars[k++] = hexChars[b & 0xf];
            }
            return new String(chars);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
            throw new RuntimeException("MD5加密出错！！+" + e);
        }
    }
}
```

这个加密的结果与MySQL中的结果是一摸一样的

验证：

![image-20230910161535468](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309101615100.png)

![image-20230910161547664](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309101615139.png)