---
title: RouteService
categories:
   - [计算机学科,java,案例Demo,纯java分页]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - 分页
---

```java
@SuppressWarnings("all")
public interface RouteService {
    /**
     * 根据类别进行分页查询
     * @param cid
     * @param currentPage
     * @param pageSize
     * @return
     */
    public PageBean<Route> pageQuery(int cid,int currentPage,int pageSize);

}
```

