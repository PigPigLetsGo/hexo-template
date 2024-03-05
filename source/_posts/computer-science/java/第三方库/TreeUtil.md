---
title: Hutool-TreeUtil
categories:
   - [计算机学科,java,第三方库]
tags:
   - 计算机学科
   - java
   - 第三方库
   - hutool
---

# TreeUtil

导入依赖：

```xml
<dependency>
   <groupId>cn.hutool</groupId>
   <artifactId>hutool-all</artifactId>
   <version>5.7.22</version>
</dependency>
```

创建数据库：

![image-20240216193508088](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240216193508088.png)

实体类：

```java
@Data
@EqualsAndHashCode(callSuper = false)
public class Managebar {
    private String id;
    private String name;
    private String parentId;
    private Integer orderNum;
    private Integer level;
}
```

service：

```java
public interface ManagebarService extends IService<Managebar> {

    List<Tree<String>> treeNode();
}
```

ServiceImpl：

```java
@Service(value = "managebarService")
public class ManagebarServiceImpl extends ServiceImpl<ManagebarMapper, Managebar> implements ManagebarService {

    @Override
    public List<Tree<String>> treeNode() {
        //1.获取所有资料分类
        List<Managebar> dataList = this.lambdaQuery().getBaseMapper().selectList(Wrappers.lambdaQuery(Managebar.class));
        //2.配置
        TreeNodeConfig config = new TreeNodeConfig();
        config.setIdKey("id");                              //默认id，可以不设置
        config.setParentIdKey("parentId");                  //父id
        config.setNameKey("Name");                          //分类名称
        config.setDeep(3);                                  //最大递归深度
        config.setChildrenKey("childrenList");              //孩子节点
        config.setWeightKey("sort");                        //排序字段
        //3.转树
        List<Tree<String>> treeList = TreeUtil.build(dataList, "0", config, ((object, treeNode) -> {
            treeNode.putExtra("id", object.getId());
            treeNode.putExtra("parentId", object.getParentId());
            treeNode.putExtra("Name", object.getName());
            treeNode.putExtra("level", object.getLevel());
            treeNode.putExtra("orderNum", object.getOrderNum());
            //扩展属性...
        }));
        return treeList;
    }
}
```

Controller：

```java
@RestController
public class TreeController {
    @Autowired
    @Qualifier(value = "managebarService")
    private ManagebarService managebarService;

    @GetMapping("all")
    public Result<List<Tree<String>>> getAll() {
        return Result.success(managebarService.treeNode());
    }
}
```

访问地址返回的数据效果如下：

![image-20240216193655576](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240216193655576.png)

这样看着不明显我们通过工具进行解析一下：

![image-20240216193729157](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240216193729157.png)

完毕！