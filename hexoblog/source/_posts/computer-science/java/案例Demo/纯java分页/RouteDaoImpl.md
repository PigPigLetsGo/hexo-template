---
title: RouteDaoImpl
categories:
   - [计算机学科,java,案例Demo,纯java分页]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - 分页
---

```java
public class RouteDaoImpl implements RouteDao {
    private JdbcTemplate j = new JdbcTemplate(JDBCUtils.getDataSource());
    @Override
    public int findTotalCount(int cid){
        String sql = "select count(*) from tab_route where cid = ?";
        return j.queryForObject(sql,Integer.class,cid);
    }
    @Override
    public List<Route> findByPage(int cid, int start, int pageSize){
        String sql = "select * from tab_route where cid = ? limit ? , ?";
        return j.query(sql,new BeanPropertyRowMapper<Route>(Route.class),cid,start,pageSize);
    }
}
```

