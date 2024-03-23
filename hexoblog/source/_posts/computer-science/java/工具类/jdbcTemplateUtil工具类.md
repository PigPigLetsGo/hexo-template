---
title: jdbcTemplate连接池工具类
categories:
   - [计算机学科,java,工具类]
tags:
   - 计算机学科
   - java
   - 工具类
---

配置jdbcTempalte使用

```java
public class JUtils {

    private static DataSource source;
    static{
        Properties pro = null;
        try{
            pro = new Properties();
            InputStream in = JUtils.class.getClassLoader().getResourceAsStream("druid.properties");
            pro.load(in);
            source = DruidDataSourceFactory.createDataSource(pro);
        }catch(IOException e){
            throw new RuntimeException(e);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    public static Connection getConnection() throws SQLException {
        return source.getConnection();
    }
    public static DataSource getSource() {
        return source;
    }
}
```