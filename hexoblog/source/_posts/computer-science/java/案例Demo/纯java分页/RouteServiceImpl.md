---
title: RouteServiceImpl
categories:
   - [计算机学科,java,案例Demo,纯java分页]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - 分页
---

```java
public class RouteServiceImpl implements RouteService {
    private RouteDao d = new RouteDaoImpl();
    @Override
    public PageBean<Route> pageQuery(int cid,int currentPage,int pageSize){
        PageBean<Route> pb = new PageBean<>();
//        设置当前页码
        pb.setCurrentPage(currentPage);
//        设置每页显示条数
        pb.setPageSize(pageSize);
//        设置总记录数
        int totalCount = d.findTotalCount(cid);
        pb.setTotalCount(totalCount);
//        设置当前页显示的数据集合
        int start = (currentPage - 1) * pageSize;
        List<Route> list = d.findByPage(cid,start,pageSize);
        pb.setList(list);
//        设置总页数
        int totalPage = totalCount % pageSize == 0 ? totalCount/pageSize : (totalCount/pageSize) + 1;
        pb.setTotalPage(totalPage);
        return pb;
    }
}
```

