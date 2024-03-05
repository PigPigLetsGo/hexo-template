---
title: Result工具类
categories:
   - [计算机学科,java,工具类]
tags:
   - 计算机学科
   - java
   - 工具类
---

## 平常返回数据给前端展示的响应类

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Result<T> {

    private int code;
    private String message;
    private T data;

    public Result(T data) {
        this.code = 200;
        this.message = "success";
        this.data = data;
    }

    public Result(T data, boolean success, String message) {
        if (success) {
            this.code = 200;
            this.message = "success";
        } else {
            this.code = 500;
            this.message = message;
        }
        this.data = data;
    }

    public Result(int code, String message) {
        this.code = code;
        this.message = message;
        this.data = null;
    }

    public static <T> Result<T> success(T data) {
        return new Result<>(data);
    }

    public static <T> Result<T> fail(String message) {
        return new Result<>(500, message);
    }

    public static <T> Result<T> fail(int code, String message) {
        return new Result<>(code, message);
    }
}
```

## 分页响应类

```java
@Data
@ToString
public class PageResult<T> implements Serializable {

    // 数据列表
    private List<T> items;

    //总记录数
    private long counts;

    //当前页码
    private long page;

    //每页记录数
    private long pageSize;

    public PageResult(List<T> items, long counts, long page, long pageSize) {
        this.items = items;
        this.counts = counts;
        this.page = page;
        this.pageSize = pageSize;
    }
}
```

分页接收参数类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class PageParams {
    // 当前页码
    private long pageNo;
    // 每页显示记录数
    private long pageSize = Integer.MAX_VALUE;
}
```