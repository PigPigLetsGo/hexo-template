---
title: MongoDB 分页
categories:
  - [计算机学科,database,mongodb,案例Demo]
tags:
  - 计算机学科
  - database
  - mongodb
  - 案例Demo
---

# MongoDB 分页

数据层：

```java
@Repository
public interface ArticleRepository extends MongoRepository<Article,String> {
    /**
     * 分页
     * @param pageable
     * @return
     */
    Page<Article> findAllBy(Pageable pageable);
}
```

业务层接口：

```java
public interface EssayService
{
    R getAll(Pageable pageable);
}
```

业务层实现：

```java
@Service
@AllArgsConstructor
public class EssayServiceImpl implements EssayService
{
	// 查询全部文章并分页
    @Override
    public R getAll(Pageable pageable)
    {
        return R.ok().put("data", repository.findAllBy(pageable));
    }
}
```

视图层：

```java
@RestController
@RequestMapping("/essay")
@AllArgsConstructor
public class EssayController
{
    /**
     * 查询全部文章并分页
     * @return
     */
    @GetMapping("essayAll")
    public R getAll(Pageable pageable)
    {
        return service.getAll(pageable);
    }
}
```

请求结果：

```
{
    "code": 200,
    "data": {
        "content": [
            {
                "id": "660666210c39a27771d075fb",
                "content": "ullamco",
                "tags": [
                    "ea33"
                ],
                "publishtime": "2024-03-29 14:56:33",
                "userid": "1",
                "nickname": "123",
                "createdatatime": "2024-03-29 14:56:33",
                "likenum": 0,
                "state": "0"
            }
        ],
        "pageable": {
            "sort": {
                "empty": true,
                "sorted": false,
                "unsorted": true
            },
            "offset": 0,
            "pageSize": 1,
            "pageNumber": 0,
            "unpaged": false,
            "paged": true
        },
        "last": true,
        "totalPages": 1,
        "totalElements": 1,
        "size": 1,
        "number": 0,
        "sort": {
            "empty": true,
            "sorted": false,
            "unsorted": true
        },
        "numberOfElements": 1,
        "first": true,
        "empty": false
    }
}
```

