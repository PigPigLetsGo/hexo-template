---
title: RouteServlet
categories:
   - [计算机学科,java,案例Demo,纯java分页]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - 分页
---

```java
@WebServlet("/route/*")
public class RouteServlet extends BaseServlet {
    private RouteService service = new RouteServiceImpl();
    /**
     * 分页查询
     * @param request
     * @param response
     * @throws ServletException
     * @throws IOException
     */
    public void pageQuery(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
//        1.接收参数
        String currentPagestr = request.getParameter("currentPage");
        String pageSizestr = request.getParameter("pageSize");
        String cidstr = request.getParameter("cid");
        int cid = 0;//类别id
//        2.处理参数
        if(cidstr != null && cidstr.length() > 0){
            cid = Integer.parseInt(cidstr);
        }
        int pageSize = 0;
        if(pageSizestr != null && pageSizestr.length() >= 0){
            pageSize = Integer.parseInt(pageSizestr);
        }else{
            pageSize = 5;//每页显示条数,如果不传递默认显示5条
        }
        int currentPage = 0;
        if(currentPagestr != null && currentPagestr.length() >= 0){
            currentPage = Integer.parseInt(currentPagestr);
        }else{
            currentPage = 1;//当前页码,如果不传递默认显示为1
        }
//        3.调用service查询PageBean对象
        PageBean<Route> pd = service.pageQuery(cid,currentPage,pageSize);
//        4.将pageBean对象序列化为json返回
        writeValue(pd,response);
    }
}
```

获取字段getParameter("xxx"),获取后判断条件并转换为int类型
