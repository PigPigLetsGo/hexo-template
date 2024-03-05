---
title: jedis连接池工具类
categories:
   - [计算机学科,java,工具类]
tags:
   - 计算机学科
   - java
   - 工具类
---

需要使用到的jar包

![image-20240303115223087](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303115223087.png)

```java
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

import java.io.InputStream;
import java.util.Properties;

public class JedisUtils {
    private static JedisPool jedispool;
    static{
        InputStream input = JedisUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        Properties properties = new Properties();
        //关联文件
        try{
            properties.load(input);
            System.out.println(properties);
        }catch(Exception e){
             throw new RuntimeException();
        }
        //获取数据,设置到jedispoolconfig中
        JedisPoolConfig jedispoolconfig = new JedisPoolConfig();
        jedispoolconfig.setMaxTotal(Integer.parseInt(properties.getProperty("maxTotal")));
        jedispoolconfig.setMaxIdle(Integer.parseInt(properties.getProperty("maxIdle")));
        //初始化jedispool
        jedispool = new JedisPool(jedispoolconfig,properties.getProperty("host")
                ,Integer.parseInt(properties.getProperty("port")),5000,properties.getProperty("password"));
    }
    /**
     * 获取连接方法
     */
    public static Jedis getJedis(){
        return jedispool.getResource();
    }
}
```

## properties配置信息

```properties
host=127.0.0.1
port=6379
password=dkx
maxTotal=50000
maxIdle=5000
```