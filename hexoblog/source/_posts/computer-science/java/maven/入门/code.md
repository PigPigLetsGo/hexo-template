来自：创建工程引入依赖

```java
public class JDBCUtils {
    private static DataSource druid;
    private static ThreadLocal<Connection> threadlocal = new ThreadLocal<>();

    static{
        try{
            //为了保证程序代码的可移植性,需要基于一个基准来读取这个文件
            InputStream input = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
            Properties pro = new Properties();
            pro.load(input);
            druid = DruidDataSourceFactory.createDataSource(pro);
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
    public static Connection getConnection(){
        Connection connection = null;
        try{
            //1.尝试从当前线程检查是否存在已经绑定的Connection对象
            connection = threadlocal.get();
            //2.检查connection对象是否为Null
            if(connection == null){
                //3.如果为null,则从数据资源获取数据连接
                connection = druid.getConnection();
                //4.获取到数据库连接绑定到当前线程
                threadlocal.set(connection);
            }
        }catch(Exception e){
            throw new RuntimeException(e);
        }
        return connection;
    }
    public static void releaseclose(Connection connection){
       try{
           if(connection != null){
               //关闭连接,释放资源
               connection.close();
               p("数据库已关闭");
               //将当前数据库连接从线程上删除
               threadlocal.remove();
           }
       }catch(Exception e){
           throw new RuntimeException(e);
       }
    }
}
```