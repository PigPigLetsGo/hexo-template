---
title: org-springframework-http-converter-HttpMessageNotWritableException-No-converter-found-for-retur
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - SpringBoot
---

问题描述

```
org.springframework.http.converter.HttpMessageNotWritableException: No converter found for return
```

![![image_2023-03-06-11-17-13](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-06-11-17-13_20230309195930.png)](org-springframework-http-converter-HttpMessageNotWritableException-No-converter-found-for-retur_md_files/image_2023-03-06-11-17-13_20230309195930.png?v=1&type=image&token=V1:hGc8ZOpix6zwkKJ3uTZ21rs8QqklDC2nQmYZZb6pG1c)

- Result

```java
@SuppressWarnings("all")
public class Result {
    private Object data;
    private Integer code;
    private String msg;

    public Result() {
    }

    public Result(Object data, Integer code) {
        this.data = data;
        this.code = code;
    }

    public Result(Object data, Integer code, String msg) {
        this.data = data;
        this.code = code;
        this.msg = msg;
    }
}
```

问题原因:实体类缺少getter,setter方法

解决方案:重写getter,setter方法即可

