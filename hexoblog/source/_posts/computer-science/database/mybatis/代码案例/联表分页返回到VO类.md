---
title: 联表分页返回到VO类
categories:
  - [计算机学科,database,mybatis,代码案例]
tags:
  - 计算机学科
  - database
  - mybatis
  - 案例Demo
  - 分页
---

# 联表分页返回到VO类

创建数据库：

class

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : windows
 Source Server Type    : MySQL
 Source Server Version : 80028
 Source Host           : localhost:3306
 Source Schema         : mpj_test

 Target Server Type    : MySQL
 Target Server Version : 80028
 File Encoding         : 65001

 Date: 30/03/2024 10:56:43
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tab_class
-- ----------------------------
DROP TABLE IF EXISTS `tab_class`;
CREATE TABLE `tab_class`  (
  `id` int NOT NULL AUTO_INCREMENT COMMENT '课程表id',
  `name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '课程名称',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tab_class
-- ----------------------------
INSERT INTO `tab_class` VALUES (1, '数学');
INSERT INTO `tab_class` VALUES (2, '语文');
INSERT INTO `tab_class` VALUES (3, '英语');
INSERT INTO `tab_class` VALUES (4, '化学');
INSERT INTO `tab_class` VALUES (5, '生物');
INSERT INTO `tab_class` VALUES (6, '物理');
INSERT INTO `tab_class` VALUES (7, '政治');
INSERT INTO `tab_class` VALUES (8, '历史');
INSERT INTO `tab_class` VALUES (9, '地理');

SET FOREIGN_KEY_CHECKS = 1;
```

student

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : windows
 Source Server Type    : MySQL
 Source Server Version : 80028
 Source Host           : localhost:3306
 Source Schema         : mpj_test

 Target Server Type    : MySQL
 Target Server Version : 80028
 File Encoding         : 65001

 Date: 30/03/2024 10:56:55
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tab_studenttwo
-- ----------------------------
DROP TABLE IF EXISTS `tab_studenttwo`;
CREATE TABLE `tab_studenttwo`  (
  `id` int NOT NULL AUTO_INCREMENT COMMENT '学生id',
  `name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NULL DEFAULT NULL COMMENT '学生名称',
  `age` int NULL DEFAULT NULL COMMENT '学生年龄',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tab_studenttwo
-- ----------------------------
INSERT INTO `tab_studenttwo` VALUES (1, '张三', 18);
INSERT INTO `tab_studenttwo` VALUES (2, '李四', 19);
INSERT INTO `tab_studenttwo` VALUES (3, '王五', 20);

SET FOREIGN_KEY_CHECKS = 1;
```

cs

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : windows
 Source Server Type    : MySQL
 Source Server Version : 80028
 Source Host           : localhost:3306
 Source Schema         : mpj_test

 Target Server Type    : MySQL
 Target Server Version : 80028
 File Encoding         : 65001

 Date: 30/03/2024 10:56:49
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tab_cs
-- ----------------------------
DROP TABLE IF EXISTS `tab_cs`;
CREATE TABLE `tab_cs`  (
  `id` int NOT NULL AUTO_INCREMENT COMMENT '中间表id(查询学生与课程的关系)',
  `c_id` int NULL DEFAULT NULL COMMENT '课程表id',
  `s_id` int NULL DEFAULT NULL COMMENT '学生表id',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_0900_ai_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tab_cs
-- ----------------------------
INSERT INTO `tab_cs` VALUES (1, 1, 1);
INSERT INTO `tab_cs` VALUES (2, 2, 1);
INSERT INTO `tab_cs` VALUES (3, 3, 1);
INSERT INTO `tab_cs` VALUES (4, 1, 2);
INSERT INTO `tab_cs` VALUES (5, 2, 2);
INSERT INTO `tab_cs` VALUES (6, 3, 2);
INSERT INTO `tab_cs` VALUES (7, 4, 2);
INSERT INTO `tab_cs` VALUES (8, 1, 3);
INSERT INTO `tab_cs` VALUES (9, 6, 3);

SET FOREIGN_KEY_CHECKS = 1;
```

可参考sql语句：

```sql
SELECT
	ts.`name` student_name,
	ts.age student_age,
	tcs.`name` class_name
FROM
	tab_studenttwo ts
	LEFT JOIN tab_cs tc ON tc.s_id = ts.id
	LEFT JOIN tab_class tcs ON tcs.id = tc.c_id
WHERE
	ts.`name` = '李四'
```

查询结果：

![image-20240330150505499](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240330150505499.png)

## java代码

vo类：

```java
@Data
public class StudentClassVO
{
    private String studentName;
    private Integer studentAge;
    private String className;
}
```

dao:

```java
@Mapper
public interface StudenttwoDao extends MPJBaseMapper<StudenttwoEntity>
{
    Page<StudentClassVO> getAll(Page<StudentClassVO> page);
}
```

daoxml:

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dkx.mpj.dao.StudenttwoDao">
    <select id="getAll" resultType="com.dkx.mpj.entity.vo.StudentClassVO">
        SELECT
            ts.`name` student_name,
            ts.age student_age,
            tcs.`name` class_name
        FROM
            tab_studenttwo ts
                LEFT JOIN tab_cs tc ON tc.s_id = ts.id
                LEFT JOIN tab_class tcs ON tcs.id = tc.c_id
        WHERE
            ts.`name` = '李四'
    </select>
</mapper>
```

serviceImpl:

```java
@Service("studenttwoService")
@AllArgsConstructor
public class StudenttwoServiceImpl extends ServiceImpl<StudenttwoDao, StudenttwoEntity> implements StudenttwoService {
    private StudenttwoDao studenttwoDao;
    @Override
    public PageResult<StudentClassVO> getAll(PageParams pageParams)
    {
        Page<StudentClassVO> page = new Page<>(pageParams.getPageNo(), pageParams.getPageSize());
        Page<StudentClassVO> all = studenttwoDao.getAll(page);
        List<StudentClassVO> records = all.getRecords();
        long total = all.getTotal();
        return new PageResult<>(records, total, pageParams.getPageNo(), pageParams.getPageSize());
    }
}
```

controller:

```java
@GetMapping("list")
public PageResult getAll(PageParams pageParams)
{
   return studenttwoService.getAll(pageParams);
}
```

请求结果：

![image-20240330151203163](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240330151203163.png)

