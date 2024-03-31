---
title: 电商项目中的SKU与SPU的数据库表设计
categories:
    - [计算机学科,database,知识点]
tags:
    - 计算机学科
    - database
    - 知识点
---

# 电商项目中的SKU与SPU的数据库表设计

## 简介：

一般情况下我们使用5张表就可以解决基本的需求了:

1.  商品分类：category
2.  商品表(即SPU表)：product
3.  商品规格表(即SKU表)：product_specs
4.  属性Key表：attribute_key
5.  属性Value表：attribute_value

## 具体设计

spu表和sku表实现不同商品的存储：spu表使用attribute_list字段保存属性集合，查询时使用product_id和product_spacs去sku表中获取的具体的单品信息。spu表中可以增加一些商品的公共信息字段，例如名称，发布的商家，发布日期，上架状态；sku表中增加一些每个单品不同的字段，比如不同的单品有不同图片和名称或者详情说明等等，反正根据业务进行扩展。

核心思路就是把弹性字段使用json存储，这样设计的有点是数据表结构稳定，不用在商品增加属性后增加字段，缺点是商品数据的解析复杂，弹性字段需要在业务代码中进行处理，增加了业务代码的复杂度。

表结构示意图：

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/ebc5536255314e829293b6bbbdfccb18.png)

## 商品分类表：tb_product_category

该表主要是描述商品的分类。此表采用无限层级树状数据结构，程序使用递归算法来遍历分类下的所有子类，pid是父级分类，pid=null时说明是根节点，属于一级类别；

属性：id，pid，name等

## 商品表(即SPU表)：tb_product

存储商品信息。表中关键字段是category_id和attribute_list两个字段

-  category_id：记录商品属于哪个分类，用于通过分类进行商品搜索
-  attribute_list：记录的是所有属性的集合，这个字段采用json格式存储，便于前端解析；前端解析后可以在页面显示出商品的所有属性，用户点击选择出属性组合后，前端可以拼接形成如：{"内存":"2G","颜色":"红色","尺寸":"20cm"} 的json格式数据，加上商品id(商品规格表tb_product_specs)就可以查询到具体的商品，随机获取到具体单品的库存和价格等信息；

属性：id，category_id，name，attribute_list等

## 商品规格表(即SKU表)：tb_product_specs

存储商品的具体规格单品，即sku，sku表保存的是具体的单品信息，比如具体规格的库存和价格等，核心字段是product_id和product_specs

-  product_id：记录的是spu表中的商品id
-  product_specs：记录的是该单品具体的属性值(规格值)

属性：id，product_id，product_specs等

## 属性Key表：tb_attribute_key

用于存储sku的属性，比如：机身颜色，存储容量

属性key表和属性value表仅用于管理后台页面生成属性选项，管理员在发布新商品时勾选属性，方便规则的录入和保证正确性

## 属性Value表：tb_attribute_value

用于存储sku的属性值，比如：6+128G，8+128G，8+256G

## 总结

上述数据表设计方案用于商品类别差异不是很大情形，通过表的字段可以发现不同的商品之间变化的信息只有attribute_list字段，而这个字段通过json来存储各种不同的属性集合，同样sku表中变化的字段只有product_specs也是通过json来存储各种不同属性组合

## 改进方案

将tb_product，tb_brand，tb_product_specs表中的info字段分别抽取到单独的表中

## sql

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : localhost
 Source Server Type    : MySQL
 Source Server Version : 80025
 Source Host           : localhost:3306
 Source Schema         : test

 Target Server Type    : MySQL
 Target Server Version : 80025
 File Encoding         : 65001

 Date: 07/12/2021 11:19:44
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for tb_attribute_key
-- ----------------------------
DROP TABLE IF EXISTS `tb_attribute_key`;
CREATE TABLE `tb_attribute_key`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '编号',
  `attribute_name` varchar(22) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '属性名',
  `priority` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '优先级',
  `category_id` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '类别编号',
  `create_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  `update_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0) COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `category_id`(`category_id`) USING BTREE,
  CONSTRAINT `tb_attribute_key_ibfk_1` FOREIGN KEY (`category_id`) REFERENCES `tb_product_category` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '属性key表' ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_attribute_key
-- ----------------------------

-- ----------------------------
-- Table structure for tb_attribute_value
-- ----------------------------
DROP TABLE IF EXISTS `tb_attribute_value`;
CREATE TABLE `tb_attribute_value`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '编号',
  `attribute_id` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '属性编号',
  `priority` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '优先级',
  `attribute_value` varchar(22) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '属性值',
  `create_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  `update_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0) COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `category_id`(`attribute_id`) USING BTREE,
  CONSTRAINT `tb_attribute_value_ibfk_1` FOREIGN KEY (`attribute_id`) REFERENCES `tb_attribute_key` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '属性value表' ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_attribute_value
-- ----------------------------

-- ----------------------------
-- Table structure for tb_brand
-- ----------------------------
DROP TABLE IF EXISTS `tb_brand`;
CREATE TABLE `tb_brand`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '品牌编号',
  `name` varchar(18) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '品牌名称',
  `logo` varchar(180) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '品牌LOGO',
  `info` varchar(220) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '品牌描述',
  `status` tinyint(0) UNSIGNED NULL DEFAULT 1 COMMENT '状态 1上架  2下架',
  `create_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  `update_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0) COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '品牌表' ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_brand
-- ----------------------------

-- ----------------------------
-- Table structure for tb_product
-- ----------------------------
DROP TABLE IF EXISTS `tb_product`;
CREATE TABLE `tb_product`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '商品编号',
  `name` varchar(28) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '商品名称',
  `attribute_list` json NULL COMMENT '商品属性列表',
  `category_id` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '类别编号',
  `brand_id` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '品牌编号',
  `status` tinyint(0) UNSIGNED NULL DEFAULT NULL COMMENT '状态 1上架  2下架',
  `info` varchar(180) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '备注',
  `create_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT ' 创建时间',
  `update_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0) COMMENT ' 更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `category_id`(`category_id`) USING BTREE,
  INDEX `brand_id`(`brand_id`) USING BTREE,
  CONSTRAINT `tb_product_ibfk_1` FOREIGN KEY (`category_id`) REFERENCES `tb_product_category` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT,
  CONSTRAINT `tb_product_ibfk_2` FOREIGN KEY (`brand_id`) REFERENCES `tb_brand` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '商品表（SPU）' ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_product
-- ----------------------------
INSERT INTO `tb_product` VALUES (1, 'IPHONE8', '{\"商品简介\": {\"品牌\": \"Apple\", \"系统\": \"iOS\", \"充电器\": \"其他\", \"分辨率\": \"其他\", \"支持IPv6\": \"支持IPv6\", \"商品产地\": \"中国大陆\", \"商品名称\": \"AppleiPhone 13\", \"商品毛重\": \"320.00g\", \"商品编号\": \"100014352501\", \"机身存储\": \"256GB\", \"运行内存\": \"其他\"}, \"规格与包装\": {\"主体\": {\"品牌\": \"Apple\", \"上市年份\": \"2021年\", \"上市月份\": \"9月\", \"产品名称\": \"Apple iPhone 13 (A2634) 256GB 午夜色 支持移动联通电信5G 双卡双待手机\", \"入网型号\": \"A2634\", \"首销日期\": \"24日\"}, \"基本信息\": {\"机身材质\": \"以官网信息为准\", \"机身重量（g）\": \"173g\", \"机身厚度（mm）\": \"7.65mm\", \"机身宽度（mm）\": \"71.5mm\", \"机身长度（mm）\": \"146.7mm\"}}}', NULL, NULL, NULL, NULL, '2021-12-07 10:52:15', '2021-12-07 10:52:15');

-- ----------------------------
-- Table structure for tb_product_category
-- ----------------------------
DROP TABLE IF EXISTS `tb_product_category`;
CREATE TABLE `tb_product_category`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '类别编号',
  `name` varchar(12) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '类别名称',
  `status` tinyint(0) UNSIGNED NULL DEFAULT 1 COMMENT '状态：1上架 2下架',
  `priority` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '优先级',
  `pid` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '父类别编号',
  `create_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  `update_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0) COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `pid`(`pid`) USING BTREE,
  CONSTRAINT `tb_product_category_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `tb_product_category` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB AUTO_INCREMENT = 1 CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '商品类别表' ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_product_category
-- ----------------------------
INSERT INTO `tb_product_category` VALUES (1, 'aa', 1, NULL, NULL, '2021-12-07 10:34:29', '2021-12-07 10:34:29');
INSERT INTO `tb_product_category` VALUES (2, 'bb', 1, NULL, 1, '2021-12-07 10:34:36', '2021-12-07 10:34:36');
INSERT INTO `tb_product_category` VALUES (3, 'cc', 1, NULL, 2, '2021-12-07 10:34:42', '2021-12-07 10:34:42');
INSERT INTO `tb_product_category` VALUES (4, 'dd', 1, NULL, 3, '2021-12-07 10:34:51', '2021-12-07 10:34:51');

-- ----------------------------
-- Table structure for tb_product_specs
-- ----------------------------
DROP TABLE IF EXISTS `tb_product_specs`;
CREATE TABLE `tb_product_specs`  (
  `id` int(0) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '规格编号',
  `product_id` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '商品编号',
  `name` varchar(24) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '规格名称',
  `sell_point` varchar(48) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '卖点',
  `product_specs` json NULL COMMENT '规格列表',
  `price` decimal(10, 2) NULL DEFAULT NULL COMMENT '售价',
  `barcode` varchar(120) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '条形码',
  `img` varchar(120) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '首图',
  `pics` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '图片列表',
  `amount` int(0) UNSIGNED NULL DEFAULT NULL COMMENT '库存',
  `status` tinyint(0) UNSIGNED NULL DEFAULT 1 COMMENT '状态 1上架  2下架',
  `info` varchar(180) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '备注信息',
  `create_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) COMMENT '创建时间',
  `update_time` datetime(0) NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0) COMMENT '更新时间',
  PRIMARY KEY (`id`) USING BTREE,
  INDEX `product_id`(`product_id`) USING BTREE,
  CONSTRAINT `tb_product_specs_ibfk_1` FOREIGN KEY (`product_id`) REFERENCES `tb_product` (`id`) ON DELETE RESTRICT ON UPDATE RESTRICT
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '商品规格表（sku表）' ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of tb_product_specs
-- ----------------------------
INSERT INTO `tb_product_specs` VALUES (1, 1, 'IPHONE8 PLUS', '贵,信号不好', '{\"版本\": \"128GB\", \"颜色\": \"红色\", \"购买方式\": \"公开版\"}', 5999.00, NULL, NULL, NULL, 1000, 1, NULL, '2021-12-07 10:59:18', '2021-12-07 10:59:18');
INSERT INTO `tb_product_specs` VALUES (2, 1, 'IPHONE8 MINI', '轻，信号好', '{\"版本\": \"256GB\", \"颜色\": \"玫瑰金色\", \"增值保障\": \"2年259\", \"购买方式\": \"官方标配\"}', 4200.00, NULL, NULL, NULL, 12, 1, NULL, '2021-12-07 10:59:30', '2021-12-07 11:01:37');

SET FOREIGN_KEY_CHECKS = 1;

```

## sql案例：

```sql
-- 用户点击分类比如：手机，请求数据展示手机这个商品的所有的基本信息
SELECT
	tp.id 商品id,
	tp.`name` 商品名称,
	tp.attribute_list 商品简介
FROM
	tb_product_category tpc
	LEFT JOIN tb_product tp ON tp.category_id = tpc.id
WHERE
	tpc.id = 1
	
-- 用户点击了类别查询所当前类别的所有商品的基本信息
SELECT
	tps.id 商品id,
	tps.`name` 商品名称,
	tp.attribute_list 商品简介,
	tps.price 商品价格
FROM
	tb_product tp
	LEFT JOIN tb_product_specs tps ON tps.product_id = tp.id 
WHERE
	tp.id = 1

-- 用户点击了某个基本信息的手机，请求数据查询出详细信息，用户选择类型时前端获取 数据以json传递后端，后端根据当前商品id和选择的类型(json数据)进行查询数据库
SELECT
	tps.`name` 商品名称,
	tps.price 商品价格,
	tps.img 首图,
	tps.pics 图片列表,
	tp.attribute_list 商品详细信息 
FROM
	tb_product_specs tps
	LEFT JOIN tb_product tp ON tp.id = tps.product_id 
WHERE
	tp.category_id = 1
	AND tps.id = 1
	AND tps.product_specs LIKE '%{"版本": "128GB", "颜色": "红色", "购买方式": "公开版"}%'
```

