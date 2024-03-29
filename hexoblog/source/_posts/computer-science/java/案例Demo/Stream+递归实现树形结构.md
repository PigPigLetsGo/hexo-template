---
title: Stream+递归实现树形结构
categories:
   - [计算机学科,java,案例Demo]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - 菜单数据结构
---

# Stream+递归实现树形结构

```java
// 构建树形结构
@Override
public List<CategoryEntity> listWithTree()
{
   // 1. 查出所有分类
   List<CategoryEntity> entities = baseMapper.selectList(null);
   // 2. 组装成父子的树形结构
   // 2.1 找到所有的一级分类
   List<CategoryEntity> levelMenus = entities.stream()
      // 过滤所有父级菜单
      .filter(c -> c.getParentCid() == 0)
      .map(m ->
           {
              // 递归查找所有菜单的子级菜单
              m.setChildren(getChildrens(m, entities));
              return m;
           }).sorted((m, m2) ->
                     {
                        // 根据sort字段进行排序
                        return ((m.getSort() == null ? 0 : m.getSort()) - (m2.getSort() == null ? 0 : m2.getSort()));
                        //return ((m.getCreateTime() == null ? "a" : m.getCreateTime()).compareTo(m2.getCreateTime() == null ? "a" : m2.getCreateTime()));
                     }).collect(Collectors.toList());
   // 返回构建完成的树形结构
   return levelMenus;
}

// 递归查找所有菜单的子菜单
private List<CategoryEntity> getChildrens(CategoryEntity root, List<CategoryEntity> all)
{
   List<CategoryEntity> children = all.stream().filter(c ->
                                                       {
                                                          // 过滤所有子级菜单的数据
                                                          return c.getParentCid() == root.getCatId();
                                                       }).map(m -> {
      // 递归查找子级菜单
      m.setChildren(getChildrens(m, all));
      return m;
   }).sorted((m, m2) -> {
      // 对自己菜单进行排序
      return ((m.getSort() == null ? 0 : m.getSort()) - (m2.getSort() == null ? 0 : m2.getSort()));
      //return ((m.getCreateTime() == null ? "a" : m.getCreateTime()).compareTo(m2.getCreateTime() == null ? "a" : m2.getCreateTime()));
   }).collect(Collectors.toList());
   return children;
}
```

