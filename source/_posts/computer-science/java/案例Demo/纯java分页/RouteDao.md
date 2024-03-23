---
title: RouteDao
categories:
   - [计算机学科,java,案例Demo,纯java分页]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - 分页
---

```java
public interface RouteDao {
//    根据cid查询总记录数
    public int findTotalCount(int cid);
//    根据cid,start,pageSize查询当前页的数据集合
    public List<Route> findByPage(int cid,int start,int pageSize);
}
```
